<Grid cols = 2>
 


<div class="relative">  
    <h1 class="text-lg m-0">📊 Balance Sheet</h1>
</div>


<div class>

<Dropdown data={date_filter} name=date_filter value=date_filter title="Start" defaultValue="Dec-24">
    <DropdownOption value="Dec-24" valueLabel="Dec-24" />
</Dropdown>

<Dropdown data={date_filter_1} name=date_filter_1 value=date_filter_1 title="End" defaultValue="Dec-23">
    <DropdownOption value="Dec-23" valueLabel="Dec-23" />
</Dropdown>

</div>

</Grid>

<div class="flex items-center justify-between w-full">
    <!-- Button Group on the Left -->
    <ButtonGroup name="matric" display="tabs">
        <ButtonGroupItem valueLabel="Global Green India" value="GGCL" default />
        <ButtonGroupItem valueLabel="Global Green Europe" value="GGE" />
    </ButtonGroup>

    <!-- Last Updated Text on the Right -->
    <p class="text-sm text-grey ml-auto">
        📅 Last Updated: <Value data={max_date} />
    </p>
</div>

<div class="bg-gray-800 text-white p-6 shadow-lg rounded-lg mb-10">
    <!-- Display Comments Dynamically -->
    <Details title='Balance Sheet Commentary' open = true>
        {#each BS_comm as comment}
            {#if inputs.matric === "GGE"}  <!-- Match with ButtonGroupItem value -->
                <p class="text-gray-300 text-sm">{comment.global_green_europe}</p>
            {:else}
                <p class="text-gray-300 text-sm">{comment.global_green_india}</p>
            {/if}
        {/each}
    </Details>
</div>

<DataTable 
    data={subcategories} 
    groupBy="subcategory" 
    subtotals=true 
    totalRow=true
    groupsOpen=true
    totalLabel="Total"
    rowshadowing={true}
    headerFontColor=Bold
    headerColor=#FFD700
    title = "Values are in Million USD">

   <Column id="subcategory" 
        name="Sub-Category"  
        totalFmt="Total" 
        totalAgg=""
        subtotalFmt='@value' />
         

    
    <Column id="particulars" 
            name="Particulars"
            totalFmt='0 "Line Items"'
            totalAgg=""/>

    
     <Column 
        id="Selected_Date_1"
        name="${inputs.date_filter.value}"
        fmt="0.00"
        align="center" 
        totalAgg="sum" 
        subtotalAgg="sum"
        title = '{inputs.date_filter.value}'
    />
            
   <Column 
        id="Selected_Date_2"
        name="${inputs.date_filter_1.value}" 
        fmt="0.00" 
        totalAgg="sum"
        align="center" 
        subtotalAgg="sum"
        title = '{inputs.date_filter_1.value}'
    />

     <Column 
        id="Variance" 
        name={"Variance (${inputs.date_filter_1.value} vs ${inputs.date_filter.value})"} 
        fmt="0.00" 
        totalAgg="sum" 
        subtotalAgg="sum"
        title="Variance"
        align="center"
        contentType="delta"
        wrapTitle={true}
    />      

</DataTable>




```sql subcategories
WITH mapped_data AS (
    SELECT 
        CASE 
            WHEN category IN ('Fixed Assets', 'Investments', 'Property') THEN 'Non Current Assets'
            WHEN category IN ('Inventory', 'Receivables', 'Cash') THEN 'Current Assets'
            WHEN category IN ('Equity Capital', 'Reserves', 'Retained Earnings') THEN 'Shareholder''s Funds'
            WHEN category IN ('Long Term Loan', 'Bonds Payable') THEN 'Non Current Liabilities'
            WHEN category IN ('Short Term Loan', 'Payables', 'Tax Payable') THEN 'Current Liabilities'
            ELSE category  
        END AS category_group,
        subcategory,  
        particulars,
        "${inputs.date_filter.value}" AS Selected_Date_1,  -- Backticks replaced with double quotes
        "${inputs.date_filter_1.value}" AS Selected_Date_2
    FROM "${inputs.matric}_BS"  -- Backticks replaced with double quotes
)

-- Calculate variance
SELECT 
    subcategory,  
    particulars,
    Selected_Date_1,
    Selected_Date_2,
    (Selected_Date_2::NUMERIC - Selected_Date_1::NUMERIC) AS Variance
FROM mapped_data
ORDER BY subcategory, particulars;
```

```sql date_filter
SELECT 
    UPPER(LEFT(column_name, 1)) || LOWER(SUBSTRING(column_name FROM 2)) AS date_filter
FROM information_schema.columns
WHERE table_name = '${inputs.matric}_BS'  
AND column_name NOT IN ('sno', 'category', 'subcategory', 'no', 'particulars')
ORDER BY column_name;
```

```sql date_filter_1
WITH AvailableDates AS (
    SELECT column_name 
    FROM information_schema.columns 
    WHERE table_name = '${inputs.matric}_BS'  
    AND column_name NOT IN ('sno', 'category', 'subcategory', 'no', 'particulars')
),
SelectedDate AS (
    SELECT 
        COALESCE('${inputs.date_filter.value}', 
            (SELECT MAX(column_name) FROM AvailableDates)
        ) AS selected_date
), 
PreviousYear AS (
    SELECT 
        selected_date,
        LEFT(selected_date, 3) || '-' || CAST(CAST(RIGHT(selected_date, 2) AS INTEGER) - 1 AS TEXT) AS previous_date
    FROM SelectedDate
)
SELECT 
    selected_date AS date_filter,
    UPPER(LEFT(previous_date, 1)) || LOWER(SUBSTRING(previous_date FROM 2)) AS date_filter_1
FROM PreviousYear;
```


```sql BS_comm
SELECT DISTINCT global_green_india, global_green_europe 
FROM Supabase.GG_BS_comment 
```

```sql max_date
SELECT STRFTIME(MAX(STRPTIME(date_filter, '%b-%y')), '%b-%y') AS max_date
FROM (
    SELECT 
        UPPER(LEFT(column_name, 1)) || SUBSTRING(column_name FROM 2) AS date_filter
    FROM information_schema.columns
    WHERE table_name = '${inputs.matric}_BS'
    AND column_name NOT IN ('category', 'sno', 'no', 'subcategory', 'particulars') -- Exclude non-date columns
) AS subquery;
```

```sql test
WITH AvailableDates AS (
    SELECT column_name 
    FROM information_schema.columns 
    WHERE table_name = '${inputs.matric}_BS'  
    AND column_name NOT IN ('sno', 'category', 'subcategory', 'no', 'particulars')
),

SelectedDate AS (
    SELECT 
        '${inputs.date_filter.value}' AS selected_date,
        -- Compute previous year date dynamically (e.g., Aug-23 → Aug-22)
        LEFT('${inputs.date_filter.value}', 3) || '-' || 
        CAST(CAST(RIGHT('${inputs.date_filter.value}', 2) AS INTEGER) - 1 AS TEXT) 
        AS previous_year_date
),

Validation AS (
    SELECT 
        s.selected_date,
        s.previous_year_date,
        CASE 
            WHEN NOT EXISTS (
                SELECT 1 FROM AvailableDates WHERE column_name = s.previous_year_date
            ) 
            THEN 'Invalid Date for ' || s.previous_year_date
            ELSE NULL 
        END AS error_message
    FROM SelectedDate s
)

SELECT * FROM Validation;
```

