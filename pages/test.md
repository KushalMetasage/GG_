<Grid cols = 2>

<div class="relative">  
    <h1 class="text-lg m-0">📑 Income Statement Overview</h1>
</div>


<div>
{#if inputs.financials == "income_statement"}
<Dropdown data={date_filter_income} name=date_filter_income value=date_filter_income title="Date" defaultValue="Jan-25">
    <DropdownOption value="Jan-25" valueLabel="Jan-25" />
</Dropdown>


{:else if inputs.financials == "balance_sheet"}

<Dropdown data={date_filter_bs} name=date_filter_bs value=date_filter_bs title="Start" defaultValue="Dec-24">
    <DropdownOption value="Dec-24" valueLabel="Dec-24" />
</Dropdown>

<Dropdown data={date_filter_bs_1} name=date_filter_bs_1 value=date_filter_bs_1 title="End" defaultValue="Dec-23">
    <DropdownOption value="Dec-23" valueLabel="Dec-23" />
</Dropdown>

{/if}
</div>

</Grid>




<!-- Toggle Between Global Green India & Europe -->
<div class="flex items-center justify-between w-full">
    <!-- Button Group on the Left -->
    {#if inputs.financials !== "cash_flow"}
    <ButtonGroup name="matric" display="tabs">
        <ButtonGroupItem valueLabel="Global Green India" value="GGCL" default />
        <ButtonGroupItem valueLabel="Global Green Europe" value="GGE" />
    </ButtonGroup>

    {:else}
    <ButtonGroup name="matric" display="tabs">
         <ButtonGroupItem valueLabel="Global Green India" value="Global Green India" default />
         <ButtonGroupItem valueLabel="Global Green Europe" value="Global Green Europe" />
  </ButtonGroup>
   {/if}
   

{#if inputs.financials == "income_statement"}
  <p class="text-sm text-grey ml-auto">
        📅 Last Updated: 
        <Value data={max_date_income} />
    </p>

{:else if inputs.financials !== "cash_flow"}
  <p class="text-sm text-grey ml-auto">
        📅 Last Updated: 
        <Value data={max_date_bs} />
    </p>
{/if}


   
</div>


<div class = "flex justify-center items-center">
<ButtonGroup name="financials" display="tabs">
    <ButtonGroupItem valueLabel="💵 Income Statement" value="income_statement" default />
    <ButtonGroupItem valueLabel="💰 Cash Flow" value="cash_flow" />
    <ButtonGroupItem valueLabel="📊 Balance Sheet" value="balance_sheet" />
</ButtonGroup>
</div>

{#if inputs.financials == "income_statement"}
<DataTable data={gg_data} rows = 20 rowshadowing={true} headerFontColor=Bold headerColor=#FFD700 title = "Values are in Million USD">
 <Column id= "particulars" fmt=0/>
 <Column id= "CY Actual" fmt='0.00' align="center"/>
 <Column id= "CY AOP" fmt='0.00' align="center"/>
 <Column id= "LY Actual" fmt='0.00' align="center"/>
 <Column id= "Variance vs AOP" fmt='0.00' align="center"/>
 <Column id= "Variance vs LY" fmt='0.00' align="center"/>
</DataTable>


<DataTable data={gg_margins} rowshadowing={true} headerFontColor=Bold headerColor=#FFD700>
 <Column id= "particulars" fmt=0 />
 <Column id= "CY Actual" fmt='0.00' align="center"/>
 <Column id= "CY AOP" fmt='0.00' align="center"/>
 <Column id= "LY Actual" fmt='0.00' align="center"/>
 <Column id= "Variance vs AOP" fmt='0.00' align="center"/>
 <Column id= "Variance vs LY" fmt='0.00' align="center"/>
</DataTable>


{:else if inputs.financials == "balance_sheet"}

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
        name="${inputs.date_filter_bs.value}"
        fmt="0.00"
        align="center" 
        totalAgg="sum" 
        subtotalAgg="sum"
        title = '{inputs.date_filter_bs.value}'
    />
            
   <Column 
        id="Selected_Date_2"
        name="${inputs.date_filter_bs_1.value}" 
        fmt="0.00" 
        totalAgg="sum"
        align="center" 
        subtotalAgg="sum"
        title = '{inputs.date_filter_bs_1.value}'
    />

     <Column 
        id="Variance" 
        name={"Variance (${inputs.date_filter_bs_1.value} vs ${inputs.date_filter_bs.value})"} 
        fmt="0.00" 
        totalAgg="sum" 
        subtotalAgg="sum"
        title="Variance"
        align="center"
        contentType="delta"
        wrapTitle={true}
    />      

</DataTable>

{:else}
  <!-- Right Column: Cash Flow Comments -->
    <div class="bg-gray-800 text-white p-6 shadow-lg rounded-lg mb-10">

        <!-- Display Comments Dynamically -->
        <Details title='Cash Flow Commentary' open = true>
            {#each cashflow_comm as comment}
                <p class="text-gray-300 text-sm">
                    {inputs.matric === "Global Green India" ? comment.global_green_india : comment.global_green_europe}
                </p>
            {/each}
        </Details>
    </div>


    <!-- Left Column: Cash Flow Table -->
    <div>
        <DataTable data={cashflow} rowshadowing={true} headerFontColor=Bold headerColor=#FFD700 title="Values are in Million USD">
            <Column id="particular" title="Metric"/>
            <Column id="Dec 2023" align="center" title="Dec 2023" fmt="0.00"/>
            <Column id="Dec 2024" align="center" title="Dec 2024" fmt="0.00"/>
            <Column id="Net Change" align="center" title="Net Change" contentType="delta" fmt="0.00"/>
        </DataTable>
    </div>
{/if}

```sql date_filter_income
SELECT 
    UPPER(LEFT(column_name, 1)) || SUBSTRING(column_name FROM 2) AS date_filter_income
FROM information_schema.columns
WHERE table_name = 'GGE_Actual'  
AND column_name NOT IN ('particulars', 'sno'); -- Exclude non-date columns
```


```sql gg_data
WITH SelectedDate AS (
    SELECT LOWER('${inputs.date_filter_income.value}') AS selected_month_year
),

PreviousYear AS (
    SELECT 
        selected_month_year, 
        CASE 
            WHEN selected_month_year = 'apr-19' THEN NULL  -- No LY for Apr-19
            ELSE LEFT(selected_month_year, 3) || '-' || CAST(CAST(RIGHT(selected_month_year, 2) AS INTEGER) - 1 AS TEXT) 
        END AS ly_month_year
    FROM SelectedDate
),

UnpivotedActual AS (
    SELECT
        a.sno, 
        a.particulars,
        unnest(ARRAY[
            'apr-19', 'may-19', 'jun-19', 'jul-19', 'aug-19', 'sep-19', 'oct-19', 'nov-19', 'dec-19',
            'jan-20', 'feb-20', 'mar-20', 'apr-20', 'may-20', 'jun-20', 'jul-20', 'aug-20', 'sep-20', 'oct-20', 'nov-20', 'dec-20', 
            'jan-21', 'feb-21', 'mar-21', 'apr-21', 'may-21', 'jun-21', 'jul-21', 'aug-21', 'sep-21', 'oct-21', 'nov-21', 'dec-21',
            'jan-22', 'feb-22', 'mar-22', 'apr-22', 'may-22', 'jun-22', 'jul-22', 'aug-22', 'sep-22', 'oct-22', 'nov-22', 'dec-22',
            'jan-23', 'feb-23', 'mar-23', 'apr-23', 'may-23', 'jun-23', 'jul-23', 'aug-23', 'sep-23', 'oct-23', 'nov-23', 'dec-23',
            'jan-24', 'feb-24', 'mar-24', 'apr-24', 'may-24', 'jun-24', 'jul-24', 'aug-24', 'sep-24', 'oct-24', 'nov-24', 'dec-24',
            'jan-25'
        ]) AS date_column,
        unnest(array[
            a."apr-19", a."may-19", a."jun-19", a."jul-19", a."aug-19", a."sep-19", a."oct-19", a."nov-19", a."dec-19",
            a."jan-20", a."feb-20", a."mar-20", a."apr-20", a."may-20", a."jun-20", a."jul-20", a."aug-20", a."sep-20", a."oct-20", a."nov-20", a."dec-20", 
            a."jan-21", a."feb-21", a."mar-21", a."apr-21", a."may-21", a."jun-21", a."jul-21", a."aug-21", a."sep-21", a."oct-21", a."nov-21", a."dec-21",
            a."jan-22", a."feb-22", a."mar-22", a."apr-22", a."may-22", a."jun-22", a."jul-22", a."aug-22", a."sep-22", a."oct-22", a."nov-22", a."dec-22",
            a."jan-23", a."feb-23", a."mar-23", a."apr-23", a."may-23", a."jun-23", a."jul-23", a."aug-23", a."sep-23", a."oct-23", a."nov-23", a."dec-23",
            a."jan-24", a."feb-24", a."mar-24", a."apr-24", a."may-24", a."jun-24", a."jul-24", a."aug-24", a."sep-24", a."oct-24", a."nov-24", a."dec-24", a."jan-25"
        ]) AS value
    FROM Supabase."${inputs.matric}_Actual" a
),

UnpivotedAOP AS (
    SELECT 
        b.sno,
        b.particulars,
        unnest(ARRAY[
            'apr-19', 'may-19', 'jun-19', 'jul-19', 'aug-19', 'sep-19', 'oct-19', 'nov-19', 'dec-19',
            'jan-20', 'feb-20', 'mar-20', 'apr-20', 'may-20', 'jun-20', 'jul-20', 'aug-20', 'sep-20', 'oct-20', 'nov-20', 'dec-20', 
            'jan-21', 'feb-21', 'mar-21', 'apr-21', 'may-21', 'jun-21', 'jul-21', 'aug-21', 'sep-21', 'oct-21', 'nov-21', 'dec-21',
            'jan-22', 'feb-22', 'mar-22', 'apr-22', 'may-22', 'jun-22', 'jul-22', 'aug-22', 'sep-22', 'oct-22', 'nov-22', 'dec-22',
            'jan-23', 'feb-23', 'mar-23', 'apr-23', 'may-23', 'jun-23', 'jul-23', 'aug-23', 'sep-23', 'oct-23', 'nov-23', 'dec-23',
            'jan-24', 'feb-24', 'mar-24', 'apr-24', 'may-24', 'jun-24', 'jul-24', 'aug-24', 'sep-24', 'oct-24', 'nov-24', 'dec-24',
            'jan-25'
        ]) AS date_column,
        unnest(array[
            b."apr-19", b."may-19", b."jun-19", b."jul-19", b."aug-19", b."sep-19", b."oct-19", b."nov-19", b."dec-19",
            b."jan-20", b."feb-20", b."mar-20", b."apr-20", b."may-20", b."jun-20", b."jul-20", b."aug-20", b."sep-20", b."oct-20", b."nov-20", b."dec-20", 
            b."jan-21", b."feb-21", b."mar-21", b."apr-21", b."may-21", b."jun-21", b."jul-21", b."aug-21", b."sep-21", b."oct-21", b."nov-21", b."dec-21",
            b."jan-22", b."feb-22", b."mar-22", b."apr-22", b."may-22", b."jun-22", b."jul-22", b."aug-22", b."sep-22", b."oct-22", b."nov-22", b."dec-22",
            b."jan-23", b."feb-23", b."mar-23", b."apr-23", b."may-23", b."jun-23", b."jul-23", b."aug-23", b."sep-23", b."oct-23", b."nov-23", b."dec-23",
            b."jan-24", b."feb-24", b."mar-24", b."apr-24", b."may-24", b."jun-24", b."jul-24", b."aug-24", b."sep-24", b."oct-24", b."nov-24", b."dec-24", b."jan-25"
        ]) AS value
    FROM Supabase."${inputs.matric}_AOP" b
)

SELECT 
    ua.sno,
    ua.particulars,
    ua.value AS "CY Actual",
    uAOP.value AS "CY AOP",
    COALESCE(ly.value, 0) AS "LY Actual",
    (ua.value - COALESCE(uAOP.value, 0)) AS "Variance vs AOP",
    (ua.value - COALESCE(ly.value, 0)) AS "Variance vs LY"  -- New Column
FROM UnpivotedActual ua
LEFT JOIN UnpivotedAOP uAOP 
    ON ua.sno = uAOP.sno AND ua.particulars = uAOP.particulars AND ua.date_column = uAOP.date_column
LEFT JOIN UnpivotedActual ly 
    ON ua.sno = ly.sno AND ua.particulars = ly.particulars AND ly.date_column = (SELECT ly_month_year FROM PreviousYear)
WHERE ua.date_column = (SELECT selected_month_year FROM SelectedDate)
AND ua.particulars NOT IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')
AND (ua.particulars IS NOT NULL AND ua.particulars <> '')  
---AND (ua.value != 0 OR uAOP.value != 0 OR COALESCE(ly.value, 0) != 0)
ORDER BY ua.sno ASC;
```

```sql gg_margins
WITH SelectedDate AS (
    SELECT LOWER('${inputs.date_filter_income.value}') AS selected_month_year
),

PreviousYear AS (
    SELECT 
        selected_month_year, 
        CASE 
            WHEN selected_month_year = 'apr-19' THEN NULL  -- No LY for Apr-19
            ELSE LEFT(selected_month_year, 3) || '-' || CAST(CAST(RIGHT(selected_month_year, 2) AS INTEGER) - 1 AS TEXT) 
        END AS ly_month_year
    FROM SelectedDate
),

UnpivotedActual AS (
    SELECT
        a.sno, 
        a.particulars,
        unnest(ARRAY[
            'apr-19', 'may-19', 'jun-19', 'jul-19', 'aug-19', 'sep-19', 'oct-19', 'nov-19', 'dec-19',
            'jan-20', 'feb-20', 'mar-20', 'apr-20', 'may-20', 'jun-20', 'jul-20', 'aug-20', 'sep-20', 'oct-20', 'nov-20', 'dec-20', 
            'jan-21', 'feb-21', 'mar-21', 'apr-21', 'may-21', 'jun-21', 'jul-21', 'aug-21', 'sep-21', 'oct-21', 'nov-21', 'dec-21',
            'jan-22', 'feb-22', 'mar-22', 'apr-22', 'may-22', 'jun-22', 'jul-22', 'aug-22', 'sep-22', 'oct-22', 'nov-22', 'dec-22',
            'jan-23', 'feb-23', 'mar-23', 'apr-23', 'may-23', 'jun-23', 'jul-23', 'aug-23', 'sep-23', 'oct-23', 'nov-23', 'dec-23',
            'jan-24', 'feb-24', 'mar-24', 'apr-24', 'may-24', 'jun-24', 'jul-24', 'aug-24', 'sep-24', 'oct-24', 'nov-24', 'dec-24',
            'jan-25'
        ]) AS date_column,
        unnest(array[
            a."apr-19", a."may-19", a."jun-19", a."jul-19", a."aug-19", a."sep-19", a."oct-19", a."nov-19", a."dec-19",
            a."jan-20", a."feb-20", a."mar-20", a."apr-20", a."may-20", a."jun-20", a."jul-20", a."aug-20", a."sep-20", a."oct-20", a."nov-20", a."dec-20", 
            a."jan-21", a."feb-21", a."mar-21", a."apr-21", a."may-21", a."jun-21", a."jul-21", a."aug-21", a."sep-21", a."oct-21", a."nov-21", a."dec-21",
            a."jan-22", a."feb-22", a."mar-22", a."apr-22", a."may-22", a."jun-22", a."jul-22", a."aug-22", a."sep-22", a."oct-22", a."nov-22", a."dec-22",
            a."jan-23", a."feb-23", a."mar-23", a."apr-23", a."may-23", a."jun-23", a."jul-23", a."aug-23", a."sep-23", a."oct-23", a."nov-23", a."dec-23",
            a."jan-24", a."feb-24", a."mar-24", a."apr-24", a."may-24", a."jun-24", a."jul-24", a."aug-24", a."sep-24", a."oct-24", a."nov-24", a."dec-24", a."jan-25"
        ]) AS value
    FROM Supabase."${inputs.matric}_Actual" a
),

UnpivotedAOP AS (
    SELECT
        b.sno, 
        b.particulars,
        unnest(ARRAY[
            'apr-19', 'may-19', 'jun-19', 'jul-19', 'aug-19', 'sep-19', 'oct-19', 'nov-19', 'dec-19',
            'jan-20', 'feb-20', 'mar-20', 'apr-20', 'may-20', 'jun-20', 'jul-20', 'aug-20', 'sep-20', 'oct-20', 'nov-20', 'dec-20', 
            'jan-21', 'feb-21', 'mar-21', 'apr-21', 'may-21', 'jun-21', 'jul-21', 'aug-21', 'sep-21', 'oct-21', 'nov-21', 'dec-21',
            'jan-22', 'feb-22', 'mar-22', 'apr-22', 'may-22', 'jun-22', 'jul-22', 'aug-22', 'sep-22', 'oct-22', 'nov-22', 'dec-22',
            'jan-23', 'feb-23', 'mar-23', 'apr-23', 'may-23', 'jun-23', 'jul-23', 'aug-23', 'sep-23', 'oct-23', 'nov-23', 'dec-23',
            'jan-24', 'feb-24', 'mar-24', 'apr-24', 'may-24', 'jun-24', 'jul-24', 'aug-24', 'sep-24', 'oct-24', 'nov-24', 'dec-24',
            'jan-25'
        ]) AS date_column,
        unnest(array[
            b."apr-19", b."may-19", b."jun-19", b."jul-19", b."aug-19", b."sep-19", b."oct-19", b."nov-19", b."dec-19",
            b."jan-20", b."feb-20", b."mar-20", b."apr-20", b."may-20", b."jun-20", b."jul-20", b."aug-20", b."sep-20", b."oct-20", b."nov-20", b."dec-20", 
            b."jan-21", b."feb-21", b."mar-21", b."apr-21", b."may-21", b."jun-21", b."jul-21", b."aug-21", b."sep-21", b."oct-21", b."nov-21", b."dec-21",
            b."jan-22", b."feb-22", b."mar-22", b."apr-22", b."may-22", b."jun-22", b."jul-22", b."aug-22", b."sep-22", b."oct-22", b."nov-22", b."dec-22",
            b."jan-23", b."feb-23", b."mar-23", b."apr-23", b."may-23", b."jun-23", b."jul-23", b."aug-23", b."sep-23", b."oct-23", b."nov-23", b."dec-23",
            b."jan-24", b."feb-24", b."mar-24", b."apr-24", b."may-24", b."jun-24", b."jul-24", b."aug-24", b."sep-24", b."oct-24", b."nov-24", b."dec-24", b."jan-25"
        ]) AS value
    FROM Supabase."${inputs.matric}_AOP" b
)

SELECT
    ua.sno, 
    ua.particulars,
    ua.value * 100 AS "CY Actual",
    uAOP.value * 100 AS "CY AOP",
    (ua.value - COALESCE(uAOP.value, 0)) * 100 AS "Variance vs AOP",
    COALESCE(ly.value, 0) * 100 AS "LY Actual",
    (ua.value - COALESCE(ly.value, 0)) * 100 AS "Variance vs LY"
FROM UnpivotedActual ua
LEFT JOIN UnpivotedAOP uAOP ON ua.sno = uAOP.sno AND ua.particulars = uAOP.particulars AND ua.date_column = uAOP.date_column
LEFT JOIN UnpivotedActual ly ON ua.sno = ly.sno AND ua.particulars = ly.particulars AND ly.date_column = (SELECT ly_month_year FROM PreviousYear)
WHERE ua.date_column = (SELECT selected_month_year FROM SelectedDate)
AND ua.particulars IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')
ORDER BY ua.sno ASC;
```

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
        sno,  
        particulars,
        "${inputs.date_filter_bs.value}" AS Selected_Date_1,  -- Backticks replaced with double quotes
        "${inputs.date_filter_bs_1.value}" AS Selected_Date_2
    FROM "${inputs.matric}_BS"  -- Backticks replaced with double quotes
)

-- Calculate variance
SELECT 
    subcategory, 
    sno, 
    particulars,
    Selected_Date_1,
    Selected_Date_2,
    (Selected_Date_2::NUMERIC - Selected_Date_1::NUMERIC) AS Variance
FROM mapped_data
ORDER BY sno ASC;
```

```sql date_filter_bs
SELECT 
    UPPER(LEFT(column_name, 1)) || LOWER(SUBSTRING(column_name FROM 2)) AS date_filter_bs
FROM information_schema.columns
WHERE table_name = '${inputs.matric}_BS'  
AND column_name NOT IN ('sno', 'category', 'subcategory', 'no', 'particulars')
ORDER BY column_name;
```

```sql date_filter_bs_1
WITH AvailableDates AS (
    SELECT column_name 
    FROM information_schema.columns 
    WHERE table_name = '${inputs.matric}_BS'  
    AND column_name NOT IN ('sno', 'category', 'subcategory', 'no', 'particulars')
),
SelectedDate AS (
    SELECT 
        COALESCE('${inputs.date_filter_bs.value}', 
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
    selected_date AS date_filter_bs,
    UPPER(LEFT(previous_date, 1)) || LOWER(SUBSTRING(previous_date FROM 2)) AS date_filter_bs_1
FROM PreviousYear;
```


```sql BS_comm
SELECT DISTINCT global_green_india, global_green_europe 
FROM Supabase.GG_BS_comment 
```

```sql cashflow
SELECT 
    particular, 
    dec23 AS "Dec 2023", 
    dec24 AS "Dec 2024", 
    (dec24 - dec23) AS "Net Change",  
    company 
FROM Supabase.GG_cashflow
WHERE company = '${inputs.matric}'
ORDER BY "Dec 2023" DESC;
```

```sql cashflow_comm
SELECT DISTINCT global_green_india, global_green_europe 
FROM Supabase.GG_cashflow_comment 
```

```sql max_date_income
SELECT STRFTIME(MAX(STRPTIME(date_filter_income, '%b-%y')), '%b-%y') AS max_date_income
FROM (
    SELECT 
        UPPER(LEFT(column_name, 1)) || SUBSTRING(column_name FROM 2) AS date_filter_income
    FROM information_schema.columns
    WHERE table_name = 'GGE_Actual'
    AND column_name NOT IN ('particulars', 'sno') -- Exclude non-date columns
) AS subquery;
```

```sql max_date_bs
SELECT STRFTIME(MAX(STRPTIME(date_filter_bs, '%b-%y')), '%b-%y') AS max_date_bs
FROM (
    SELECT 
        UPPER(LEFT(column_name, 1)) || SUBSTRING(column_name FROM 2) AS date_filter_bs
    FROM information_schema.columns
    WHERE table_name = '${inputs.matric}_BS'
    AND column_name NOT IN ('category', 'sno', 'no', 'subcategory', 'particulars') -- Exclude non-date columns
) AS subquery;
```

