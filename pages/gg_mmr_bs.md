<Grid cols = 3>
 


<div class="relative font-bold mt-3">  
    <h1 class="text-lg m-0">📊 Balance Sheet</h1>
</div>


<div class = "relative relative mb-5 mt-1">

<Dropdown data={prev_year_column} name=prev_year_column value=prev_year_column title="Start" defaultValue="Dec-23" order="date_sort desc">
    <DropdownOption value="Dec-23" valueLabel="Dec-23" />
</Dropdown>

<Dropdown data={next_year_of_prev} name=next_year_of_prev value=next_year_of_prev title="End" defaultValue="Dec-24">
    <DropdownOption value="Dec-24" valueLabel="Dec-24" />
</Dropdown>

</div>

<div class= "relative mt-5 ml-30">
<!-- Last Updated Text on the Right -->
    <p class="text-sm text-grey ml-auto">
        📅 Last Updated: <Value data={max_date} />
    </p>
</div>
</Grid>

<div class="flex items-center justify-between w-full">
    <!-- Button Group on the Left -->
    <ButtonGroup name="matric" display="tabs">
        <ButtonGroupItem valueLabel="Global Green India" value="GGCL" default />
        <ButtonGroupItem valueLabel="Global Green Europe" value="GGE" />
    </ButtonGroup>

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
    title = "Values are in Million USD ($)">

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
        name="${inputs.next_year_of_prev.value}"
        fmt="$0.00"
        align="center" 
        totalAgg="sum" 
        subtotalAgg="sum"
        title = '{inputs.next_year_of_prev.value}'
    />
            
   <Column 
        id="Selected_Date_2"
        name="${inputs.prev_year_column.value}" 
        fmt="$0.00" 
        totalAgg="sum"
        align="center" 
        subtotalAgg="sum"
        title = '{inputs.prev_year_column.value}'
    />

     <Column 
        id="Variance" 
        name={"Variance (${inputs.next_year_of_prev.value} vs ${inputs.prev_year_column.value})"} 
        fmt="$0.00" 
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
        "${inputs.next_year_of_prev.value}" AS Selected_Date_1,  -- Backticks replaced with double quotes
        "${inputs.prev_year_column.value}" AS Selected_Date_2
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

```sql prev_year_column
WITH AvailableDates AS (
    SELECT 
        column_name,
        STRPTIME(column_name, '%b-%y') AS date_sort
    FROM information_schema.columns
    WHERE table_name = '${inputs.matric}_BS'  
      AND column_name NOT IN ('sno', 'category', 'subcategory', 'no', 'particulars')
),
WithPreviousYear AS (
    SELECT 
        ad.column_name,
        ad.date_sort,
        -- Format prev_year_column as 'Dec-23' (Title Case)
        UPPER(LEFT(ad.column_name, 1)) || LOWER(SUBSTRING(ad.column_name FROM 2 FOR 2)) || '-' || 
        LPAD(CAST(CAST(RIGHT(ad.column_name, 2) AS INTEGER) - 1 AS TEXT), 2, '0') AS prev_year_column
    FROM AvailableDates ad
),
MatchedPrevious AS (
    SELECT 
        wp.prev_year_column,
        wp.date_sort,
        CASE 
            WHEN LOWER(ad2.column_name) = LOWER(wp.prev_year_column) THEN 'Valid Date'
            ELSE 'Invalid Date'
        END AS status
    FROM WithPreviousYear wp
    LEFT JOIN AvailableDates ad2 ON LOWER(ad2.column_name) = LOWER(wp.prev_year_column) -- Ensure case-insensitive match
)
SELECT DISTINCT
    prev_year_column, -- Already in Title Case (e.g., Dec-23)
    date_sort
FROM MatchedPrevious
WHERE status = 'Valid Date'
ORDER BY date_sort DESC;

```

```sql next_year_of_prev
WITH AvailableDates AS (
    SELECT 
        column_name,
        STRPTIME(column_name, '%b-%y') AS date_sort
    FROM information_schema.columns
    WHERE table_name = '${inputs.matric}_BS'  
      AND column_name NOT IN ('sno', 'category', 'subcategory', 'no', 'particulars')
),
WithPreviousYear AS (
    SELECT 
        ad.column_name,
        ad.date_sort,
        -- Format prev_year_column as 'Dec-23' (Title Case)
        UPPER(LEFT(ad.column_name, 1)) || LOWER(SUBSTRING(ad.column_name FROM 2 FOR 2)) || '-' || 
        LPAD(CAST(CAST(RIGHT(ad.column_name, 2) AS INTEGER) - 1 AS TEXT), 2, '0') AS prev_year_column
    FROM AvailableDates ad
),
MatchedPrevious AS (
    SELECT 
        wp.prev_year_column,
        wp.date_sort,
        CASE 
            WHEN LOWER(ad2.column_name) = LOWER(wp.prev_year_column) THEN 'Valid Date'
            ELSE 'Invalid Date'
        END AS status
    FROM WithPreviousYear wp
    LEFT JOIN AvailableDates ad2 ON LOWER(ad2.column_name) = LOWER(wp.prev_year_column) -- Ensure case-insensitive match
),
WithNextYearOfPrev AS (
    SELECT 
        mp.prev_year_column,
        mp.date_sort,
        mp.status,
        -- Format next_year_of_prev as 'Dec-24' (Title Case)
        UPPER(LEFT(mp.prev_year_column, 1)) || LOWER(SUBSTRING(mp.prev_year_column FROM 2 FOR 2)) || '-' || 
        LPAD(CAST(CAST(RIGHT(mp.prev_year_column, 2) AS INTEGER) + 1 AS TEXT), 2, '0') AS next_year_of_prev
    FROM MatchedPrevious mp
    LEFT JOIN AvailableDates ad3 ON 
        LOWER(ad3.column_name) = LOWER(
            UPPER(LEFT(mp.prev_year_column, 1)) || LOWER(SUBSTRING(mp.prev_year_column FROM 2 FOR 2)) || '-' || 
            LPAD(CAST(CAST(RIGHT(mp.prev_year_column, 2) AS INTEGER) + 1 AS TEXT), 2, '0')
        )
    WHERE mp.prev_year_column = '${inputs.prev_year_column.value}'
)
SELECT DISTINCT
    next_year_of_prev, -- Now formatted in Title Case (e.g., Dec-24)
    date_sort
FROM WithNextYearOfPrev
WHERE status = 'Valid Date'
ORDER BY date_sort DESC;

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




