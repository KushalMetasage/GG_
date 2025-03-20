




<Grid cols = 3>

<!-- <div class="relative mt-3">  
    <h1 class="text-lg m-0 font-bold">💵 Consolidated Income</h1>
</div> -->

## 💵 Consolidated Income


<div class="relative ml-25 mb-5 mt-1">
<Dropdown data={date_filter} name=date_filter value=date_filter title="Date" defaultValue="Jan-25" order = "date_sort desc">
    <DropdownOption value="Jan-25" valueLabel="Jan-25" />
</Dropdown>
</div>

<div class = "relative mt-5 ml-30">
 <!-- Last Updated Text on the Right -->
    <p class="text-sm text-grey ml-auto">
        📅 Last Updated: <Value data={max_date} />
    </p>
</div>

</Grid>



<DataTable data={gg_cons} rows = 20 rowshadowing={true} headerFontColor=Bold headerColor=#FFD700 title = "Values are in Million USD ($)">
 <Column id= "particulars" fmt=0/>
 <Column id= "CY Actual" fmt='$0.00' align="center"/>
 <Column id= "CY AOP" fmt='$0.00' align="center"/>
 <Column id= "LY Actual" fmt='$0.00' align="center"/>
 <Column id= "Variance vs AOP" fmt='$0.00' align="center"/>
 <Column id= "Variance vs LY" fmt='$0.00' align="center"/>
</DataTable>

<DataTable data={gg_margins_cons} rows = 20 rowshadowing={true} headerFontColor=Bold headerColor=#FFD700>
<Column id= "particulars" fmt=0/>
 <Column id= "CY Actual" fmt='0.00"%"' align="center"/>
 <Column id= "CY AOP" fmt='0.00"%"' align="center"/>
 <Column id= "LY Actual" fmt='0.00"%"' align="center"/>
 <Column id= "Variance vs AOP" fmt='0.00"%"' align="center"/>
 <Column id= "Variance vs LY" fmt='0.00"%"' align="center"/>
</DataTable>

<Grid cols = 3>

<!-- <div class="relative mt-20">  
    <h1 class="text-lg m-0 font-bold">💵 Income Statement</h1>
</div>
 -->


 ## 💵 Income Statement

<div class = "relative mt-1 ml-25">

<Dropdown data={date_filter_inc} name=date_filter_inc value=date_filter_inc title="Date" defaultValue="Jan-25" order = "date_sort desc">
    <DropdownOption value="Jan-25" valueLabel="Jan-25" />
</Dropdown>

</div>

<div class = "relative mt-4 ml-30">
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

<DataTable data={gg_data} rows = 20 rowshadowing={true} headerFontColor=Bold headerColor=#FFD700 title = "Values are in Million USD ($)">
 <Column id= "particulars" fmt=0/>
 <Column id= "CY Actual" fmt='$0.00' align="center"/>
 <Column id= "CY AOP" fmt='$0.00' align="center"/>
 <Column id= "LY Actual" fmt='$0.00' align="center"/>
 <Column id= "Variance vs AOP" fmt='$0.00' align="center"/>
 <Column id= "Variance vs LY" fmt='$0.00' align="center"/>
</DataTable>



<DataTable data={gg_margins} rowshadowing={true} headerFontColor=Bold headerColor=#FFD700>
 <Column id= "particulars" fmt=0 />
 <Column id= "CY Actual" fmt='0.00"%"' align="center"/>
 <Column id= "CY AOP" fmt='0.00"%"' align="center"/>
 <Column id= "LY Actual" fmt='0.00"%"' align="center"/>
 <Column id= "Variance vs AOP" fmt='0.00"%"' align="center"/>
 <Column id= "Variance vs LY" fmt='0.00"%"' align="center"/>
</DataTable>

<Grid cols = 3>

<!-- <div class="relative mt-20">  
    <h1 class="text-lg m-0 font-bold">🏦 YTD Income Statement</h1>
</div> -->

## 🏦 YTD Income Statement


<div class="relative ml-25 mt-1">
<Dropdown data={date_filter_ytd} name=date_filter_ytd value=date_filter_ytd title="Year" defaultValue="2025" order = "date_sort desc">
</Dropdown>
<Info description="For the year 2019 data is from Apr-19 to Dec-19" color="red" />
</div>

<div class = "relative mt-4 ml-30">
 <!-- Last Updated Text on the Right -->
    <p class="text-sm text-grey ml-auto">
        📅 Last Updated: <Value data={max_date_ytd} />
    </p>
</div>

</Grid>

<div class="flex items-center justify-between w-full">
   
    <!-- Button Group on the Left -->
    <ButtonGroup name="matric_ytd" display="tabs">
        <ButtonGroupItem valueLabel="Global Green India" value="GGCL" default />
        <ButtonGroupItem valueLabel="Global Green Europe" value="GGE" />
    </ButtonGroup>
</div>

<DataTable data={ytd} rows = 20 rowshadowing={true} headerFontColor=Bold headerColor=#FFD700 title = "Values are in Million USD ($)">
<Column id= "particulars" fmt=0 />
<Column id= "CY Actual" fmt='$0.00' align="center"/>
<Column id= "YTD AOP" fmt='$0.00' align="center"/>
<Column id= "LY Actual YTD" fmt='$0.00' align="center"/>
<Column id= "Variance vs AOP YTD" fmt='$0.00' align="center"/>
<Column id= "Variance vs LY YTD" fmt='$0.00' align="center"/>
</DataTable>

<DataTable data={ytd_margins} rows = 20 rowshadowing={true} headerFontColor=Bold headerColor=#FFD700>
<Column id= "particulars" fmt=0 />
<Column id= "CY Actual" fmt='0.00"%"' align="center"/>
<Column id= "YTD AOP" fmt='0.00"%"' align="center"/>
<Column id= "LY Actual YTD" fmt='0.00"%"' align="center"/>
<Column id= "Variance vs AOP YTD" fmt='0.00"%"' align="center"/>
<Column id= "Variance vs LY YTD" fmt='0.00"%"' align="center"/>
</DataTable>


```sql date_filter
SELECT 
    UPPER(LEFT(column_name, 1)) || SUBSTRING(column_name FROM 2) AS date_filter,
    STRPTIME(column_name, '%b-%y') AS date_sort
FROM information_schema.columns
WHERE table_name = 'GGE_Actual'  
AND column_name NOT IN ('particulars', 'sno') -- Exclude non-date columns
GROUP BY ALL
ORDER BY date_sort DESC;

```

```sql gg_cons
WITH SelectedDate AS (
    SELECT LOWER('${inputs.date_filter.value}') AS selected_month_year
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
    -- Unpivot GGCL_Actual (India) & GGE_Actual (Europe)
    SELECT
        sno, 
        particulars, 
        date_column, 
        SUM(value) AS CY_Actual
    FROM (
        SELECT 
            a.sno,
            a.particulars,
            UNNEST(ARRAY[
                'apr-19', 'may-19', 'jun-19', 'jul-19', 'aug-19', 'sep-19', 'oct-19', 'nov-19', 'dec-19',
                'jan-20', 'feb-20', 'mar-20', 'apr-20', 'may-20', 'jun-20', 'jul-20', 'aug-20', 'sep-20', 'oct-20', 'nov-20', 'dec-20', 
                'jan-21', 'feb-21', 'mar-21', 'apr-21', 'may-21', 'jun-21', 'jul-21', 'aug-21', 'sep-21', 'oct-21', 'nov-21', 'dec-21',
                'jan-22', 'feb-22', 'mar-22', 'apr-22', 'may-22', 'jun-22', 'jul-22', 'aug-22', 'sep-22', 'oct-22', 'nov-22', 'dec-22',
                'jan-23', 'feb-23', 'mar-23', 'apr-23', 'may-23', 'jun-23', 'jul-23', 'aug-23', 'sep-23', 'oct-23', 'nov-23', 'dec-23',
                'jan-24', 'feb-24', 'mar-24', 'apr-24', 'may-24', 'jun-24', 'jul-24', 'aug-24', 'sep-24', 'oct-24', 'nov-24', 'dec-24',
                'jan-25'
            ]) AS date_column,
            UNNEST(ARRAY[
                a."apr-19", a."may-19", a."jun-19", a."jul-19", a."aug-19", a."sep-19", a."oct-19", a."nov-19", a."dec-19",
                a."jan-20", a."feb-20", a."mar-20", a."apr-20", a."may-20", a."jun-20", a."jul-20", a."aug-20", a."sep-20", a."oct-20", a."nov-20", a."dec-20", 
                a."jan-21", a."feb-21", a."mar-21", a."apr-21", a."may-21", a."jun-21", a."jul-21", a."aug-21", a."sep-21", a."oct-21", a."nov-21", a."dec-21",
                a."jan-22", a."feb-22", a."mar-22", a."apr-22", a."may-22", a."jun-22", a."jul-22", a."aug-22", a."sep-22", a."oct-22", a."nov-22", a."dec-22",
                a."jan-23", a."feb-23", a."mar-23", a."apr-23", a."may-23", a."jun-23", a."jul-23", a."aug-23", a."sep-23", a."oct-23", a."nov-23", a."dec-23",
                a."jan-24", a."feb-24", a."mar-24", a."apr-24", a."may-24", a."jun-24", a."jul-24", a."aug-24", a."sep-24", a."oct-24", a."nov-24", a."dec-24", a."jan-25"
            ]) AS value
        FROM GGCL_Actual a
        
        UNION ALL

        SELECT
            b.sno, 
            b.particulars,
            UNNEST(ARRAY[
                'apr-19', 'may-19', 'jun-19', 'jul-19', 'aug-19', 'sep-19', 'oct-19', 'nov-19', 'dec-19',
                'jan-20', 'feb-20', 'mar-20', 'apr-20', 'may-20', 'jun-20', 'jul-20', 'aug-20', 'sep-20', 'oct-20', 'nov-20', 'dec-20', 
                'jan-21', 'feb-21', 'mar-21', 'apr-21', 'may-21', 'jun-21', 'jul-21', 'aug-21', 'sep-21', 'oct-21', 'nov-21', 'dec-21',
                'jan-22', 'feb-22', 'mar-22', 'apr-22', 'may-22', 'jun-22', 'jul-22', 'aug-22', 'sep-22', 'oct-22', 'nov-22', 'dec-22',
                'jan-23', 'feb-23', 'mar-23', 'apr-23', 'may-23', 'jun-23', 'jul-23', 'aug-23', 'sep-23', 'oct-23', 'nov-23', 'dec-23',
                'jan-24', 'feb-24', 'mar-24', 'apr-24', 'may-24', 'jun-24', 'jul-24', 'aug-24', 'sep-24', 'oct-24', 'nov-24', 'dec-24',
                'jan-25'
            ] ) AS date_column, 
            UNNEST(ARRAY[
                b."apr-19", b."may-19", b."jun-19", b."jul-19", b."aug-19", b."sep-19", b."oct-19", b."nov-19", b."dec-19",
                b."jan-20", b."feb-20", b."mar-20", b."apr-20", b."may-20", b."jun-20", b."jul-20", b."aug-20", b."sep-20", b."oct-20", b."nov-20", b."dec-20", 
                b."jan-21", b."feb-21", b."mar-21", b."apr-21", b."may-21", b."jun-21", b."jul-21", b."aug-21", b."sep-21", b."oct-21", b."nov-21", b."dec-21",
                b."jan-22", b."feb-22", b."mar-22", b."apr-22", b."may-22", b."jun-22", b."jul-22", b."aug-22", b."sep-22", b."oct-22", b."nov-22", b."dec-22",
                b."jan-23", b."feb-23", b."mar-23", b."apr-23", b."may-23", b."jun-23", b."jul-23", b."aug-23", b."sep-23", b."oct-23", b."nov-23", b."dec-23",
                b."jan-24", b."feb-24", b."mar-24", b."apr-24", b."may-24", b."jun-24", b."jul-24", b."aug-24", b."sep-24", b."oct-24", b."nov-24", b."dec-24", b."jan-25"
            ]) AS value
        FROM GGE_Actual b
    ) AS CombinedActual
    GROUP BY sno,particulars, date_column
),

UnpivotedAOP AS (
    -- Unpivot GGCL_AOP (India) & GGE_AOP (Europe)
    SELECT
        sno, 
        particulars, 
        date_column, 
        SUM(value) AS CY_AOP
    FROM (
        SELECT 
            a.sno,
            a.particulars,
            UNNEST(ARRAY[
                'apr-19', 'may-19', 'jun-19', 'jul-19', 'aug-19', 'sep-19', 'oct-19', 'nov-19', 'dec-19',
                'jan-20', 'feb-20', 'mar-20', 'apr-20', 'may-20', 'jun-20', 'jul-20', 'aug-20', 'sep-20', 'oct-20', 'nov-20', 'dec-20', 
                'jan-21', 'feb-21', 'mar-21', 'apr-21', 'may-21', 'jun-21', 'jul-21', 'aug-21', 'sep-21', 'oct-21', 'nov-21', 'dec-21',
                'jan-22', 'feb-22', 'mar-22', 'apr-22', 'may-22', 'jun-22', 'jul-22', 'aug-22', 'sep-22', 'oct-22', 'nov-22', 'dec-22',
                'jan-23', 'feb-23', 'mar-23', 'apr-23', 'may-23', 'jun-23', 'jul-23', 'aug-23', 'sep-23', 'oct-23', 'nov-23', 'dec-23',
                'jan-24', 'feb-24', 'mar-24', 'apr-24', 'may-24', 'jun-24', 'jul-24', 'aug-24', 'sep-24', 'oct-24', 'nov-24', 'dec-24',
                'jan-25'
            ]) AS date_column, 
            UNNEST(ARRAY[
                a."apr-19", a."may-19", a."jun-19", a."jul-19", a."aug-19", a."sep-19", a."oct-19", a."nov-19", a."dec-19",
                a."jan-20", a."feb-20", a."mar-20", a."apr-20", a."may-20", a."jun-20", a."jul-20", a."aug-20", a."sep-20", a."oct-20", a."nov-20", a."dec-20", 
                a."jan-21", a."feb-21", a."mar-21", a."apr-21", a."may-21", a."jun-21", a."jul-21", a."aug-21", a."sep-21", a."oct-21", a."nov-21", a."dec-21",
                a."jan-22", a."feb-22", a."mar-22", a."apr-22", a."may-22", a."jun-22", a."jul-22", a."aug-22", a."sep-22", a."oct-22", a."nov-22", a."dec-22",
                a."jan-23", a."feb-23", a."mar-23", a."apr-23", a."may-23", a."jun-23", a."jul-23", a."aug-23", a."sep-23", a."oct-23", a."nov-23", a."dec-23",
                a."jan-24", a."feb-24", a."mar-24", a."apr-24", a."may-24", a."jun-24", a."jul-24", a."aug-24", a."sep-24", a."oct-24", a."nov-24", a."dec-24", a."jan-25"
            ]) AS value
        FROM GGCL_AOP a
        
        UNION ALL

        SELECT
            b.sno, 
            b.particulars,
            UNNEST(ARRAY[
                'apr-19', 'may-19', 'jun-19', 'jul-19', 'aug-19', 'sep-19', 'oct-19', 'nov-19', 'dec-19',
                'jan-20', 'feb-20', 'mar-20', 'apr-20', 'may-20', 'jun-20', 'jul-20', 'aug-20', 'sep-20', 'oct-20', 'nov-20', 'dec-20', 
                'jan-21', 'feb-21', 'mar-21', 'apr-21', 'may-21', 'jun-21', 'jul-21', 'aug-21', 'sep-21', 'oct-21', 'nov-21', 'dec-21',
                'jan-22', 'feb-22', 'mar-22', 'apr-22', 'may-22', 'jun-22', 'jul-22', 'aug-22', 'sep-22', 'oct-22', 'nov-22', 'dec-22',
                'jan-23', 'feb-23', 'mar-23', 'apr-23', 'may-23', 'jun-23', 'jul-23', 'aug-23', 'sep-23', 'oct-23', 'nov-23', 'dec-23',
                'jan-24', 'feb-24', 'mar-24', 'apr-24', 'may-24', 'jun-24', 'jul-24', 'aug-24', 'sep-24', 'oct-24', 'nov-24', 'dec-24',
                'jan-25'
            ]) AS date_column, 
            UNNEST(ARRAY[
                b."apr-19", b."may-19", b."jun-19", b."jul-19", b."aug-19", b."sep-19", b."oct-19", b."nov-19", b."dec-19",
                b."jan-20", b."feb-20", b."mar-20", b."apr-20", b."may-20", b."jun-20", b."jul-20", b."aug-20", b."sep-20", b."oct-20", b."nov-20", b."dec-20", 
                b."jan-21", b."feb-21", b."mar-21", b."apr-21", b."may-21", b."jun-21", b."jul-21", b."aug-21", b."sep-21", b."oct-21", b."nov-21", b."dec-21",
                b."jan-22", b."feb-22", b."mar-22", b."apr-22", b."may-22", b."jun-22", b."jul-22", b."aug-22", b."sep-22", b."oct-22", b."nov-22", b."dec-22",
                b."jan-23", b."feb-23", b."mar-23", b."apr-23", b."may-23", b."jun-23", b."jul-23", b."aug-23", b."sep-23", b."oct-23", b."nov-23", b."dec-23",
                b."jan-24", b."feb-24", b."mar-24", b."apr-24", b."may-24", b."jun-24", b."jul-24", b."aug-24", b."sep-24", b."oct-24", b."nov-24", b."dec-24", b."jan-25"
            ]) AS value
        FROM GGE_AOP b
    ) AS CombinedAOP
    GROUP BY sno,particulars, date_column
),

LY_Actual AS (
    -- Fetch Last Year Actuals
    SELECT
        ua.sno, 
        ua.particulars,
        SUM(ua.CY_Actual) AS LY_Actual
    FROM UnpivotedActual ua
    JOIN PreviousYear py 
    ON ua.date_column = py.ly_month_year
    GROUP BY ua.sno,ua.particulars
)

SELECT
    ua.sno, 
    ua.particulars,
    SUM(ua.CY_Actual) AS "CY Actual",
    SUM(COALESCE(uAOP.CY_AOP, 0)) AS "CY AOP",
    SUM(COALESCE(ly.LY_Actual, 0)) AS "LY Actual",
    SUM(ua.CY_Actual - COALESCE(uAOP.CY_AOP, 0)) AS "Variance vs AOP",
    SUM(ua.CY_Actual - COALESCE(ly.LY_Actual, 0)) AS "Variance vs LY"
FROM UnpivotedActual ua
LEFT JOIN UnpivotedAOP uAOP 
    ON ua.sno = uAOP.sno AND ua.particulars = uAOP.particulars AND ua.date_column = uAOP.date_column
LEFT JOIN LY_Actual ly 
    ON ua.sno = ly.sno AND ua.particulars = ly.particulars
WHERE ua.date_column = (SELECT selected_month_year FROM SelectedDate)
AND ua.particulars NOT IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')
GROUP BY ua.sno, ua.particulars
HAVING 
    NOT (
        TRIM(ua.particulars) = ''  -- Ensures the 'Particulars' column is not empty
        AND SUM(ua.CY_Actual) = 0 
        AND SUM(COALESCE(uAOP.CY_AOP, 0)) = 0 
        AND SUM(COALESCE(ly.LY_Actual, 0)) = 0 
        AND SUM(ua.CY_Actual - COALESCE(uAOP.CY_AOP, 0)) = 0 
        AND SUM(ua.CY_Actual - COALESCE(ly.LY_Actual, 0)) = 0
    )
ORDER BY ua.sno ASC;
```

```sql gg_margins_cons
WITH SelectedDate AS (
    SELECT LOWER('${inputs.date_filter.value}') AS selected_month_year
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
    -- Unpivot GGCL_Actual (India) & GGE_Actual (Europe)
    SELECT
        sno, 
        particulars, 
        date_column, 
        SUM(value) AS CY_Actual
    FROM (
        SELECT 
            a.sno,
            a.particulars,
            UNNEST(ARRAY[
                'apr-19', 'may-19', 'jun-19', 'jul-19', 'aug-19', 'sep-19', 'oct-19', 'nov-19', 'dec-19',
                'jan-20', 'feb-20', 'mar-20', 'apr-20', 'may-20', 'jun-20', 'jul-20', 'aug-20', 'sep-20', 'oct-20', 'nov-20', 'dec-20', 
                'jan-21', 'feb-21', 'mar-21', 'apr-21', 'may-21', 'jun-21', 'jul-21', 'aug-21', 'sep-21', 'oct-21', 'nov-21', 'dec-21',
                'jan-22', 'feb-22', 'mar-22', 'apr-22', 'may-22', 'jun-22', 'jul-22', 'aug-22', 'sep-22', 'oct-22', 'nov-22', 'dec-22',
                'jan-23', 'feb-23', 'mar-23', 'apr-23', 'may-23', 'jun-23', 'jul-23', 'aug-23', 'sep-23', 'oct-23', 'nov-23', 'dec-23',
                'jan-24', 'feb-24', 'mar-24', 'apr-24', 'may-24', 'jun-24', 'jul-24', 'aug-24', 'sep-24', 'oct-24', 'nov-24', 'dec-24',
                'jan-25'
            ]) AS date_column,
            UNNEST(ARRAY[
                a."apr-19", a."may-19", a."jun-19", a."jul-19", a."aug-19", a."sep-19", a."oct-19", a."nov-19", a."dec-19",
                a."jan-20", a."feb-20", a."mar-20", a."apr-20", a."may-20", a."jun-20", a."jul-20", a."aug-20", a."sep-20", a."oct-20", a."nov-20", a."dec-20", 
                a."jan-21", a."feb-21", a."mar-21", a."apr-21", a."may-21", a."jun-21", a."jul-21", a."aug-21", a."sep-21", a."oct-21", a."nov-21", a."dec-21",
                a."jan-22", a."feb-22", a."mar-22", a."apr-22", a."may-22", a."jun-22", a."jul-22", a."aug-22", a."sep-22", a."oct-22", a."nov-22", a."dec-22",
                a."jan-23", a."feb-23", a."mar-23", a."apr-23", a."may-23", a."jun-23", a."jul-23", a."aug-23", a."sep-23", a."oct-23", a."nov-23", a."dec-23",
                a."jan-24", a."feb-24", a."mar-24", a."apr-24", a."may-24", a."jun-24", a."jul-24", a."aug-24", a."sep-24", a."oct-24", a."nov-24", a."dec-24", a."jan-25"
            ]) AS value
        FROM GGCL_Actual a
        
        UNION ALL

        SELECT 
            b.sno,
            b.particulars,
            UNNEST(ARRAY[
                'apr-19', 'may-19', 'jun-19', 'jul-19', 'aug-19', 'sep-19', 'oct-19', 'nov-19', 'dec-19',
                'jan-20', 'feb-20', 'mar-20', 'apr-20', 'may-20', 'jun-20', 'jul-20', 'aug-20', 'sep-20', 'oct-20', 'nov-20', 'dec-20', 
                'jan-21', 'feb-21', 'mar-21', 'apr-21', 'may-21', 'jun-21', 'jul-21', 'aug-21', 'sep-21', 'oct-21', 'nov-21', 'dec-21',
                'jan-22', 'feb-22', 'mar-22', 'apr-22', 'may-22', 'jun-22', 'jul-22', 'aug-22', 'sep-22', 'oct-22', 'nov-22', 'dec-22',
                'jan-23', 'feb-23', 'mar-23', 'apr-23', 'may-23', 'jun-23', 'jul-23', 'aug-23', 'sep-23', 'oct-23', 'nov-23', 'dec-23',
                'jan-24', 'feb-24', 'mar-24', 'apr-24', 'may-24', 'jun-24', 'jul-24', 'aug-24', 'sep-24', 'oct-24', 'nov-24', 'dec-24',
                'jan-25'
            ] ) AS date_column, 
            UNNEST(ARRAY[
                b."apr-19", b."may-19", b."jun-19", b."jul-19", b."aug-19", b."sep-19", b."oct-19", b."nov-19", b."dec-19",
                b."jan-20", b."feb-20", b."mar-20", b."apr-20", b."may-20", b."jun-20", b."jul-20", b."aug-20", b."sep-20", b."oct-20", b."nov-20", b."dec-20", 
                b."jan-21", b."feb-21", b."mar-21", b."apr-21", b."may-21", b."jun-21", b."jul-21", b."aug-21", b."sep-21", b."oct-21", b."nov-21", b."dec-21",
                b."jan-22", b."feb-22", b."mar-22", b."apr-22", b."may-22", b."jun-22", b."jul-22", b."aug-22", b."sep-22", b."oct-22", b."nov-22", b."dec-22",
                b."jan-23", b."feb-23", b."mar-23", b."apr-23", b."may-23", b."jun-23", b."jul-23", b."aug-23", b."sep-23", b."oct-23", b."nov-23", b."dec-23",
                b."jan-24", b."feb-24", b."mar-24", b."apr-24", b."may-24", b."jun-24", b."jul-24", b."aug-24", b."sep-24", b."oct-24", b."nov-24", b."dec-24", b."jan-25"
            ]) AS value
        FROM GGE_Actual b
    ) AS CombinedActual
    GROUP BY sno,particulars, date_column
),

UnpivotedAOP AS (
    -- Unpivot GGCL_AOP (India) & GGE_AOP (Europe)
    SELECT 
        sno,
        particulars, 
        date_column, 
        SUM(value) AS CY_AOP
    FROM (
        SELECT 
            a.sno,
            a.particulars,
            UNNEST(ARRAY[
                'apr-19', 'may-19', 'jun-19', 'jul-19', 'aug-19', 'sep-19', 'oct-19', 'nov-19', 'dec-19',
                'jan-20', 'feb-20', 'mar-20', 'apr-20', 'may-20', 'jun-20', 'jul-20', 'aug-20', 'sep-20', 'oct-20', 'nov-20', 'dec-20', 
                'jan-21', 'feb-21', 'mar-21', 'apr-21', 'may-21', 'jun-21', 'jul-21', 'aug-21', 'sep-21', 'oct-21', 'nov-21', 'dec-21',
                'jan-22', 'feb-22', 'mar-22', 'apr-22', 'may-22', 'jun-22', 'jul-22', 'aug-22', 'sep-22', 'oct-22', 'nov-22', 'dec-22',
                'jan-23', 'feb-23', 'mar-23', 'apr-23', 'may-23', 'jun-23', 'jul-23', 'aug-23', 'sep-23', 'oct-23', 'nov-23', 'dec-23',
                'jan-24', 'feb-24', 'mar-24', 'apr-24', 'may-24', 'jun-24', 'jul-24', 'aug-24', 'sep-24', 'oct-24', 'nov-24', 'dec-24',
                'jan-25'
            ]) AS date_column, 
            UNNEST(ARRAY[
                a."apr-19", a."may-19", a."jun-19", a."jul-19", a."aug-19", a."sep-19", a."oct-19", a."nov-19", a."dec-19",
                a."jan-20", a."feb-20", a."mar-20", a."apr-20", a."may-20", a."jun-20", a."jul-20", a."aug-20", a."sep-20", a."oct-20", a."nov-20", a."dec-20", 
                a."jan-21", a."feb-21", a."mar-21", a."apr-21", a."may-21", a."jun-21", a."jul-21", a."aug-21", a."sep-21", a."oct-21", a."nov-21", a."dec-21",
                a."jan-22", a."feb-22", a."mar-22", a."apr-22", a."may-22", a."jun-22", a."jul-22", a."aug-22", a."sep-22", a."oct-22", a."nov-22", a."dec-22",
                a."jan-23", a."feb-23", a."mar-23", a."apr-23", a."may-23", a."jun-23", a."jul-23", a."aug-23", a."sep-23", a."oct-23", a."nov-23", a."dec-23",
                a."jan-24", a."feb-24", a."mar-24", a."apr-24", a."may-24", a."jun-24", a."jul-24", a."aug-24", a."sep-24", a."oct-24", a."nov-24", a."dec-24", a."jan-25"
            ]) AS value
        FROM GGCL_AOP a
        
        UNION ALL

        SELECT 
            b.sno,
            b.particulars,
            UNNEST(ARRAY[
                'apr-19', 'may-19', 'jun-19', 'jul-19', 'aug-19', 'sep-19', 'oct-19', 'nov-19', 'dec-19',
                'jan-20', 'feb-20', 'mar-20', 'apr-20', 'may-20', 'jun-20', 'jul-20', 'aug-20', 'sep-20', 'oct-20', 'nov-20', 'dec-20', 
                'jan-21', 'feb-21', 'mar-21', 'apr-21', 'may-21', 'jun-21', 'jul-21', 'aug-21', 'sep-21', 'oct-21', 'nov-21', 'dec-21',
                'jan-22', 'feb-22', 'mar-22', 'apr-22', 'may-22', 'jun-22', 'jul-22', 'aug-22', 'sep-22', 'oct-22', 'nov-22', 'dec-22',
                'jan-23', 'feb-23', 'mar-23', 'apr-23', 'may-23', 'jun-23', 'jul-23', 'aug-23', 'sep-23', 'oct-23', 'nov-23', 'dec-23',
                'jan-24', 'feb-24', 'mar-24', 'apr-24', 'may-24', 'jun-24', 'jul-24', 'aug-24', 'sep-24', 'oct-24', 'nov-24', 'dec-24',
                'jan-25'
            ]) AS date_column, 
            UNNEST(ARRAY[
                b."apr-19", b."may-19", b."jun-19", b."jul-19", b."aug-19", b."sep-19", b."oct-19", b."nov-19", b."dec-19",
                b."jan-20", b."feb-20", b."mar-20", b."apr-20", b."may-20", b."jun-20", b."jul-20", b."aug-20", b."sep-20", b."oct-20", b."nov-20", b."dec-20", 
                b."jan-21", b."feb-21", b."mar-21", b."apr-21", b."may-21", b."jun-21", b."jul-21", b."aug-21", b."sep-21", b."oct-21", b."nov-21", b."dec-21",
                b."jan-22", b."feb-22", b."mar-22", b."apr-22", b."may-22", b."jun-22", b."jul-22", b."aug-22", b."sep-22", b."oct-22", b."nov-22", b."dec-22",
                b."jan-23", b."feb-23", b."mar-23", b."apr-23", b."may-23", b."jun-23", b."jul-23", b."aug-23", b."sep-23", b."oct-23", b."nov-23", b."dec-23",
                b."jan-24", b."feb-24", b."mar-24", b."apr-24", b."may-24", b."jun-24", b."jul-24", b."aug-24", b."sep-24", b."oct-24", b."nov-24", b."dec-24", b."jan-25"
            ]) AS value
        FROM GGE_AOP b
    ) AS CombinedAOP
    GROUP BY sno,particulars, date_column
),

LY_Actual AS (
    -- Fetch Last Year Actuals
    SELECT 
        ua.sno,
        ua.particulars,
        SUM(ua.CY_Actual) AS LY_Actual
    FROM UnpivotedActual ua
    JOIN PreviousYear py 
    ON ua.date_column = py.ly_month_year
    GROUP BY ua.sno,ua.particulars
)

SELECT
    ua.sno, 
    ua.particulars,
    SUM(ua.CY_Actual) * 100 AS "CY Actual",
    SUM(COALESCE(uAOP.CY_AOP, 0)) * 100 AS "CY AOP",
    SUM(COALESCE(ly.LY_Actual, 0)) * 100 AS "LY Actual",
    SUM(ua.CY_Actual - COALESCE(uAOP.CY_AOP, 0)) * 100 AS "Variance vs AOP",
    SUM(ua.CY_Actual - COALESCE(ly.LY_Actual, 0)) * 100 AS "Variance vs LY"
FROM UnpivotedActual ua
LEFT JOIN UnpivotedAOP uAOP 
    ON ua.sno = uAOP.sno AND ua.particulars = uAOP.particulars AND ua.date_column = uAOP.date_column
LEFT JOIN LY_Actual ly 
    ON ua.sno = ly.sno AND ua.particulars = ly.particulars
WHERE ua.date_column = (SELECT selected_month_year FROM SelectedDate)
AND ua.particulars IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')
GROUP BY ua.sno, ua.particulars;
```

```sql date_filter_inc
SELECT 
    UPPER(LEFT(column_name, 1)) || SUBSTRING(column_name FROM 2) AS date_filter_inc,
    STRPTIME(column_name, '%b-%y') AS date_sort
FROM information_schema.columns
WHERE table_name = 'GGE_Actual'  
AND column_name NOT IN ('particulars', 'sno') -- Exclude non-date columns
GROUP BY ALL
ORDER BY date_sort DESC;

```

```sql gg_data
WITH SelectedDate AS (
    SELECT LOWER('${inputs.date_filter_inc.value}') AS selected_month_year
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
    AND NOT (
        TRIM(ua.particulars) = ''  -- Ensures the 'Particulars' column is not empty
        AND ua.value = 0 
        AND COALESCE(uAOP.value, 0) = 0 
        AND COALESCE(ly.value, 0) = 0 
        AND (ua.value - COALESCE(uAOP.value, 0)) = 0 
        AND (ua.value - COALESCE(ly.value, 0)) = 0
    )
ORDER BY ua.sno ASC;
```

```sql gg_margins
WITH SelectedDate AS (
    SELECT LOWER('${inputs.date_filter_inc.value}') AS selected_month_year
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
    ua.particulars,
    ua.value * 100 AS "CY Actual",
    uAOP.value * 100 AS "CY AOP",
    (ua.value - COALESCE(uAOP.value, 0)) * 100 AS "Variance vs AOP",
    COALESCE(ly.value, 0) * 100 AS "LY Actual",
    (ua.value - COALESCE(ly.value, 0)) * 100 AS "Variance vs LY"
FROM UnpivotedActual ua
LEFT JOIN UnpivotedAOP uAOP ON ua.particulars = uAOP.particulars AND ua.date_column = uAOP.date_column
LEFT JOIN UnpivotedActual ly ON ua.particulars = ly.particulars AND ly.date_column = (SELECT ly_month_year FROM PreviousYear)
WHERE ua.date_column = (SELECT selected_month_year FROM SelectedDate)
AND ua.particulars IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %');
```

```sql date_filter_ytd
WITH CalendarYears AS (
    SELECT DISTINCT 
        CASE 
            WHEN RIGHT(column_name, 2) = '19' THEN '2019'  -- Convert 'Apr-19 to Dec-19' to '2019'
            ELSE '20' || RIGHT(column_name, 2)  -- Convert 'Jan-20 to Dec-20' into '2020'
        END AS year_label,  -- Store year values as labels
        STRPTIME('01-' || RIGHT(column_name, 2), '%d-%y') AS date_sort -- Convert to proper date for sorting
    FROM information_schema.columns
    WHERE table_name = 'GGCL_Actual'  
    AND (column_name LIKE 'jan-%' OR column_name LIKE 'apr-19')  -- Include 'apr-19' for the special case
    AND RIGHT(column_name, 2) ~ '^[0-9]+$'  -- Ensure only numeric years are processed
    AND RIGHT(column_name, 2)::INTEGER   -- Ensures only valid years
)
SELECT year_label AS date_filter_ytd, date_sort  -- Use year as the final label
FROM CalendarYears 
ORDER BY date_sort DESC;  -- Sorting in descending order

```

```sql ytd
SELECT 
  A.sno,
  'Apr-19 to Dec-19' AS YTD,
  A.particulars AS particulars,
  COALESCE(A."apr-19", 0) + COALESCE(A."may-19", 0) + COALESCE(A."jun-19", 0) + 
  COALESCE(A."jul-19", 0) + COALESCE(A."aug-19", 0) + COALESCE(A."sep-19", 0) + 
  COALESCE(A."oct-19", 0) + COALESCE(A."nov-19", 0) + COALESCE(A."dec-19", 0) AS "CY Actual",
  COALESCE(B."apr-19", 0) + COALESCE(B."may-19", 0) + COALESCE(B."jun-19", 0) + 
  COALESCE(B."jul-19", 0) + COALESCE(B."aug-19", 0) + COALESCE(B."sep-19", 0) + 
  COALESCE(B."oct-19", 0) + COALESCE(B."nov-19", 0) + COALESCE(B."dec-19", 0) AS "YTD AOP",
  0 AS "LY Actual YTD",  -- First year has no LY Actual YTD
  COALESCE(A."apr-19", 0) + COALESCE(A."may-19", 0) + COALESCE(A."jun-19", 0) + 
  COALESCE(A."jul-19", 0) + COALESCE(A."aug-19", 0) + COALESCE(A."sep-19", 0) + 
  COALESCE(A."oct-19", 0) + COALESCE(A."nov-19", 0) + COALESCE(A."dec-19", 0) -
  (COALESCE(B."apr-19", 0) + COALESCE(B."may-19", 0) + COALESCE(B."jun-19", 0) + 
   COALESCE(B."jul-19", 0) + COALESCE(B."aug-19", 0) + COALESCE(B."sep-19", 0) + 
   COALESCE(B."oct-19", 0) + COALESCE(B."nov-19", 0) + COALESCE(B."dec-19", 0)) AS "Variance vs AOP YTD",
   0 AS "Variance vs LY YTD"  -- First year has no previous LY Actual YTD
FROM '${inputs.matric_ytd}_Actual' A
JOIN '${inputs.matric_ytd}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter_ytd.value}' = '2019'
AND A.particulars NOT IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')
AND NOT (
    TRIM(A.particulars) = ''  -- Ensures the 'Particulars' column is not empty
    AND ( 
        COALESCE(A."apr-19", 0) + COALESCE(A."may-19", 0) + COALESCE(A."jun-19", 0) + 
        COALESCE(A."jul-19", 0) + COALESCE(A."aug-19", 0) + COALESCE(A."sep-19", 0) + 
        COALESCE(A."oct-19", 0) + COALESCE(A."nov-19", 0) + COALESCE(A."dec-19", 0)
    ) = 0
)


UNION ALL

SELECT 
   A.sno,
  'Jan-20 to Dec-20' AS YTD,
  A.particulars AS particulars,
  
  -- CY Actual YTD
COALESCE(A."jan-20", 0) + COALESCE(A."feb-20", 0) + COALESCE(A."mar-20", 0) + 
COALESCE(A."apr-20", 0) + COALESCE(A."may-20", 0) + COALESCE(A."jun-20", 0) + 
COALESCE(A."jul-20", 0) + COALESCE(A."aug-20", 0) + COALESCE(A."sep-20", 0) + 
COALESCE(A."oct-20", 0) + COALESCE(A."nov-20", 0) + COALESCE(A."dec-20", 0) AS "CY Actual",


  -- YTD AOP
COALESCE(B."jan-20", 0) + COALESCE(B."feb-20", 0) + COALESCE(B."mar-20", 0) + 
COALESCE(B."apr-20", 0) + COALESCE(B."may-20", 0) + COALESCE(B."jun-20", 0) + 
COALESCE(B."jul-20", 0) + COALESCE(B."aug-20", 0) + COALESCE(B."sep-20", 0) + 
COALESCE(B."oct-20", 0) + COALESCE(B."nov-20", 0) + COALESCE(B."dec-20", 0) AS "YTD AOP",

-- LY Actual YTD (Previous Year CY Actual)
(SELECT 
   COALESCE(A_prev."apr-19", 0) + COALESCE(A_prev."may-19", 0) + COALESCE(A_prev."jun-19", 0) + 
   COALESCE(A_prev."jul-19", 0) + COALESCE(A_prev."aug-19", 0) + COALESCE(A_prev."sep-19", 0) + 
   COALESCE(A_prev."oct-19", 0) + COALESCE(A_prev."nov-19", 0) + COALESCE(A_prev."dec-19", 0)
 FROM '${inputs.matric_ytd}_Actual' A_prev
 WHERE A_prev.particulars = A.particulars) AS "LY Actual YTD",

-- Variance vs AOP YTD (CY Actual - YTD AOP)
(COALESCE(A."jan-20", 0) + COALESCE(A."feb-20", 0) + COALESCE(A."mar-20", 0) + 
 COALESCE(A."apr-20", 0) + COALESCE(A."may-20", 0) + COALESCE(A."jun-20", 0) + 
 COALESCE(A."jul-20", 0) + COALESCE(A."aug-20", 0) + COALESCE(A."sep-20", 0) + 
 COALESCE(A."oct-20", 0) + COALESCE(A."nov-20", 0) + COALESCE(A."dec-20", 0)) -

(COALESCE(B."jan-20", 0) + COALESCE(B."feb-20", 0) + COALESCE(B."mar-20", 0) + 
 COALESCE(B."apr-20", 0) + COALESCE(B."may-20", 0) + COALESCE(B."jun-20", 0) + 
 COALESCE(B."jul-20", 0) + COALESCE(B."aug-20", 0) + COALESCE(B."sep-20", 0) + 
 COALESCE(B."oct-20", 0) + COALESCE(B."nov-20", 0) + COALESCE(B."dec-20", 0)) AS "Variance vs AOP YTD",

-- Variance vs LY YTD (CY Actual - LY Actual YTD)
(COALESCE(A."jan-20", 0) + COALESCE(A."feb-20", 0) + COALESCE(A."mar-20", 0) + 
 COALESCE(A."apr-20", 0) + COALESCE(A."may-20", 0) + COALESCE(A."jun-20", 0) + 
 COALESCE(A."jul-20", 0) + COALESCE(A."aug-20", 0) + COALESCE(A."sep-20", 0) + 
 COALESCE(A."oct-20", 0) + COALESCE(A."nov-20", 0) + COALESCE(A."dec-20", 0)) -

(SELECT  
   COALESCE(A_prev."apr-19", 0) + COALESCE(A_prev."may-19", 0) + COALESCE(A_prev."jun-19", 0) + 
   COALESCE(A_prev."jul-19", 0) + COALESCE(A_prev."aug-19", 0) + COALESCE(A_prev."sep-19", 0) + 
   COALESCE(A_prev."oct-19", 0) + COALESCE(A_prev."nov-19", 0) + COALESCE(A_prev."dec-19", 0)
 FROM '${inputs.matric_ytd}_Actual' A_prev
 WHERE A_prev.particulars = A.particulars) AS "Variance vs LY YTD"


FROM '${inputs.matric_ytd}_Actual' A
JOIN '${inputs.matric_ytd}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter_ytd.value}' = '2020'
AND A.particulars NOT IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')
AND NOT (
    TRIM(A.particulars) = ''  -- Ensures the 'Particulars' column is not empty
    AND (
        COALESCE(A."jan-20", 0) + COALESCE(A."feb-20", 0) + COALESCE(A."mar-20", 0) + 
        COALESCE(A."apr-20", 0) + COALESCE(A."may-20", 0) + COALESCE(A."jun-20", 0) + 
        COALESCE(A."jul-20", 0) + COALESCE(A."aug-20", 0) + COALESCE(A."sep-20", 0) + 
        COALESCE(A."oct-20", 0) + COALESCE(A."nov-20", 0) + COALESCE(A."dec-20", 0)
    ) = 0
)

UNION ALL

SELECT
   A.sno, 
  'Jan-21 to Dec-21' AS YTD,
  A.particulars AS particulars,
  
-- CY Actual Calculation
COALESCE(A."jan-21", 0) + COALESCE(A."feb-21", 0) + COALESCE(A."mar-21", 0) + 
COALESCE(A."apr-21", 0) + COALESCE(A."may-21", 0) + COALESCE(A."jun-21", 0) + 
COALESCE(A."jul-21", 0) + COALESCE(A."aug-21", 0) + COALESCE(A."sep-21", 0) + 
COALESCE(A."oct-21", 0) + COALESCE(A."nov-21", 0) + COALESCE(A."dec-21", 0) AS "CY Actual",
  
-- YTD AOP Calculation
COALESCE(B."jan-21", 0) + COALESCE(B."feb-21", 0) + COALESCE(B."mar-21", 0) + 
COALESCE(B."apr-21", 0) + COALESCE(B."may-21", 0) + COALESCE(B."jun-21", 0) + 
COALESCE(B."jul-21", 0) + COALESCE(B."aug-21", 0) + COALESCE(B."sep-21", 0) + 
COALESCE(B."oct-21", 0) + COALESCE(B."nov-21", 0) + COALESCE(B."dec-21", 0) AS "YTD AOP",
  
-- LY Actual YTD (Previous Year)
(SELECT 
   COALESCE(A_prev."jan-20", 0) + COALESCE(A_prev."feb-20", 0) + COALESCE(A_prev."mar-20", 0) + 
   COALESCE(A_prev."apr-20", 0) + COALESCE(A_prev."may-20", 0) + COALESCE(A_prev."jun-20", 0) + 
   COALESCE(A_prev."jul-20", 0) + COALESCE(A_prev."aug-20", 0) + COALESCE(A_prev."sep-20", 0) + 
   COALESCE(A_prev."oct-20", 0) + COALESCE(A_prev."nov-20", 0) + COALESCE(A_prev."dec-20", 0)
 FROM GGCL_Actual A_prev
 WHERE A_prev.particulars = A.particulars) AS "LY Actual YTD",
  
-- Variance vs AOP YTD (CY Actual - YTD AOP)
(COALESCE(A."jan-21", 0) + COALESCE(A."feb-21", 0) + COALESCE(A."mar-21", 0) + 
 COALESCE(A."apr-21", 0) + COALESCE(A."may-21", 0) + COALESCE(A."jun-21", 0) + 
 COALESCE(A."jul-21", 0) + COALESCE(A."aug-21", 0) + COALESCE(A."sep-21", 0) + 
 COALESCE(A."oct-21", 0) + COALESCE(A."nov-21", 0) + COALESCE(A."dec-21", 0)) -

(COALESCE(B."jan-21", 0) + COALESCE(B."feb-21", 0) + COALESCE(B."mar-21", 0) + 
 COALESCE(B."apr-21", 0) + COALESCE(B."may-21", 0) + COALESCE(B."jun-21", 0) + 
 COALESCE(B."jul-21", 0) + COALESCE(B."aug-21", 0) + COALESCE(B."sep-21", 0) + 
 COALESCE(B."oct-21", 0) + COALESCE(B."nov-21", 0) + COALESCE(B."dec-21", 0)) AS "Variance vs AOP YTD",
  
-- Variance vs LY YTD (CY Actual - LY Actual YTD)
(COALESCE(A."jan-21", 0) + COALESCE(A."feb-21", 0) + COALESCE(A."mar-21", 0) + 
 COALESCE(A."apr-21", 0) + COALESCE(A."may-21", 0) + COALESCE(A."jun-21", 0) + 
 COALESCE(A."jul-21", 0) + COALESCE(A."aug-21", 0) + COALESCE(A."sep-21", 0) + 
 COALESCE(A."oct-21", 0) + COALESCE(A."nov-21", 0) + COALESCE(A."dec-21", 0)) - 

(SELECT 
   COALESCE(A_prev."jan-20", 0) + COALESCE(A_prev."feb-20", 0) + COALESCE(A_prev."mar-20", 0) + 
   COALESCE(A_prev."apr-20", 0) + COALESCE(A_prev."may-20", 0) + COALESCE(A_prev."jun-20", 0) + 
   COALESCE(A_prev."jul-20", 0) + COALESCE(A_prev."aug-20", 0) + COALESCE(A_prev."sep-20", 0) + 
   COALESCE(A_prev."oct-20", 0) + COALESCE(A_prev."nov-20", 0) + COALESCE(A_prev."dec-20", 0)
 FROM '${inputs.matric_ytd}_Actual' A_prev
 WHERE A_prev.particulars = A.particulars) AS "Variance vs LY YTD"


FROM '${inputs.matric_ytd}_Actual' A
JOIN '${inputs.matric_ytd}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter_ytd.value}' = '2021'
AND A.particulars NOT IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')
AND NOT (
    TRIM(A.particulars) = ''  -- Ensures the 'Particulars' column is not empty
    AND (
        COALESCE(A."jan-21", 0) + COALESCE(A."feb-21", 0) + COALESCE(A."mar-21", 0) + 
        COALESCE(A."apr-21", 0) + COALESCE(A."may-21", 0) + COALESCE(A."jun-21", 0) + 
        COALESCE(A."jul-21", 0) + COALESCE(A."aug-21", 0) + COALESCE(A."sep-21", 0) + 
        COALESCE(A."oct-21", 0) + COALESCE(A."nov-21", 0) + COALESCE(A."dec-21", 0)
    ) = 0
)


UNION ALL

SELECT 
   A.sno,
  'Jan-22 to Dec-22' AS YTD,
  A.particulars AS particulars,
-- CY Actual YTD Calculation (Jan-22 to Dec-22)
COALESCE(A."jan-22", 0) + COALESCE(A."feb-22", 0) + COALESCE(A."mar-22", 0) + 
COALESCE(A."apr-22", 0) + COALESCE(A."may-22", 0) + COALESCE(A."jun-22", 0) + 
COALESCE(A."jul-22", 0) + COALESCE(A."aug-22", 0) + COALESCE(A."sep-22", 0) + 
COALESCE(A."oct-22", 0) + COALESCE(A."nov-22", 0) + COALESCE(A."dec-22", 0) AS "CY Actual",

-- YTD AOP Calculation (Jan-22 to Dec-22)
COALESCE(B."jan-22", 0) + COALESCE(B."feb-22", 0) + COALESCE(B."mar-22", 0) + 
COALESCE(B."apr-22", 0) + COALESCE(B."may-22", 0) + COALESCE(B."jun-22", 0) + 
COALESCE(B."jul-22", 0) + COALESCE(B."aug-22", 0) + COALESCE(B."sep-22", 0) + 
COALESCE(B."oct-22", 0) + COALESCE(B."nov-22", 0) + COALESCE(B."dec-22", 0) AS "YTD AOP",

-- LY Actual YTD Calculation (Jan-21 to Dec-21)
(SELECT 
   COALESCE(A_prev."jan-21", 0) + COALESCE(A_prev."feb-21", 0) + COALESCE(A_prev."mar-21", 0) + 
   COALESCE(A_prev."apr-21", 0) + COALESCE(A_prev."may-21", 0) + COALESCE(A_prev."jun-21", 0) + 
   COALESCE(A_prev."jul-21", 0) + COALESCE(A_prev."aug-21", 0) + COALESCE(A_prev."sep-21", 0) + 
   COALESCE(A_prev."oct-21", 0) + COALESCE(A_prev."nov-21", 0) + COALESCE(A_prev."dec-21", 0)
 FROM '${inputs.matric_ytd}_Actual' A_prev
 WHERE A_prev.particulars = A.particulars) AS "LY Actual YTD",

-- Variance vs AOP YTD Calculation (Jan-22 to Dec-22)
(
   COALESCE(A."jan-22", 0) + COALESCE(A."feb-22", 0) + COALESCE(A."mar-22", 0) + 
   COALESCE(A."apr-22", 0) + COALESCE(A."may-22", 0) + COALESCE(A."jun-22", 0) + 
   COALESCE(A."jul-22", 0) + COALESCE(A."aug-22", 0) + COALESCE(A."sep-22", 0) + 
   COALESCE(A."oct-22", 0) + COALESCE(A."nov-22", 0) + COALESCE(A."dec-22", 0)
) 
- 
(
   COALESCE(B."jan-22", 0) + COALESCE(B."feb-22", 0) + COALESCE(B."mar-22", 0) + 
   COALESCE(B."apr-22", 0) + COALESCE(B."may-22", 0) + COALESCE(B."jun-22", 0) + 
   COALESCE(B."jul-22", 0) + COALESCE(B."aug-22", 0) + COALESCE(B."sep-22", 0) + 
   COALESCE(B."oct-22", 0) + COALESCE(B."nov-22", 0) + COALESCE(B."dec-22", 0)
) 
AS "Variance vs AOP YTD",

-- Variance vs LY YTD Calculation (Jan-22 to Dec-22)
(
   COALESCE(A."jan-22", 0) + COALESCE(A."feb-22", 0) + COALESCE(A."mar-22", 0) + 
   COALESCE(A."apr-22", 0) + COALESCE(A."may-22", 0) + COALESCE(A."jun-22", 0) + 
   COALESCE(A."jul-22", 0) + COALESCE(A."aug-22", 0) + COALESCE(A."sep-22", 0) + 
   COALESCE(A."oct-22", 0) + COALESCE(A."nov-22", 0) + COALESCE(A."dec-22", 0)
)
- 
(
   SELECT 
     COALESCE(A_prev."jan-21", 0) + COALESCE(A_prev."feb-21", 0) + COALESCE(A_prev."mar-21", 0) + 
     COALESCE(A_prev."apr-21", 0) + COALESCE(A_prev."may-21", 0) + COALESCE(A_prev."jun-21", 0) + 
     COALESCE(A_prev."jul-21", 0) + COALESCE(A_prev."aug-21", 0) + COALESCE(A_prev."sep-21", 0) + 
     COALESCE(A_prev."oct-21", 0) + COALESCE(A_prev."nov-21", 0) + COALESCE(A_prev."dec-21", 0)
   FROM '${inputs.matric_ytd}_Actual' A_prev
   WHERE A_prev.particulars = A.particulars
) 
AS "Variance vs LY YTD"


FROM '${inputs.matric_ytd}_Actual' A
JOIN '${inputs.matric_ytd}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter_ytd.value}' = '2022'
AND A.particulars NOT IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')
AND NOT (
    TRIM(A.particulars) = ''  -- Ensures the 'Particulars' column is not empty
    AND (
        COALESCE(A."jan-22", 0) + COALESCE(A."feb-22", 0) + COALESCE(A."mar-22", 0) + 
        COALESCE(A."apr-22", 0) + COALESCE(A."may-22", 0) + COALESCE(A."jun-22", 0) + 
        COALESCE(A."jul-22", 0) + COALESCE(A."aug-22", 0) + COALESCE(A."sep-22", 0) + 
        COALESCE(A."oct-22", 0) + COALESCE(A."nov-22", 0) + COALESCE(A."dec-22", 0)
    ) = 0
)

UNION ALL

SELECT
   A.sno, 
  'Jan-23 to Dec-23' AS YTD,
  A.particulars AS particulars,
-- CY Actual (Jan-23 to Dec-23)
COALESCE(A."jan-23", 0) + COALESCE(A."feb-23", 0) + COALESCE(A."mar-23", 0) + 
COALESCE(A."apr-23", 0) + COALESCE(A."may-23", 0) + COALESCE(A."jun-23", 0) + 
COALESCE(A."jul-23", 0) + COALESCE(A."aug-23", 0) + COALESCE(A."sep-23", 0) + 
COALESCE(A."oct-23", 0) + COALESCE(A."nov-23", 0) + COALESCE(A."dec-23", 0) AS "CY Actual",

-- YTD AOP (Jan-23 to Dec-23)
COALESCE(B."jan-23", 0) + COALESCE(B."feb-23", 0) + COALESCE(B."mar-23", 0) + 
COALESCE(B."apr-23", 0) + COALESCE(B."may-23", 0) + COALESCE(B."jun-23", 0) + 
COALESCE(B."jul-23", 0) + COALESCE(B."aug-23", 0) + COALESCE(B."sep-23", 0) + 
COALESCE(B."oct-23", 0) + COALESCE(B."nov-23", 0) + COALESCE(B."dec-23", 0) AS "YTD AOP",

-- LY Actual YTD (Jan-22 to Dec-22)
(SELECT 
   COALESCE(A_prev."jan-22", 0) + COALESCE(A_prev."feb-22", 0) + COALESCE(A_prev."mar-22", 0) + 
   COALESCE(A_prev."apr-22", 0) + COALESCE(A_prev."may-22", 0) + COALESCE(A_prev."jun-22", 0) + 
   COALESCE(A_prev."jul-22", 0) + COALESCE(A_prev."aug-22", 0) + COALESCE(A_prev."sep-22", 0) + 
   COALESCE(A_prev."oct-22", 0) + COALESCE(A_prev."nov-22", 0) + COALESCE(A_prev."dec-22", 0)
 FROM GGCL_Actual A_prev
 WHERE A_prev.particulars = A.particulars) AS "LY Actual YTD",

-- Variance vs AOP YTD (CY Actual - YTD AOP) for Jan-23 to Dec-23
(
   COALESCE(A."jan-23", 0) + COALESCE(A."feb-23", 0) + COALESCE(A."mar-23", 0) + 
   COALESCE(A."apr-23", 0) + COALESCE(A."may-23", 0) + COALESCE(A."jun-23", 0) + 
   COALESCE(A."jul-23", 0) + COALESCE(A."aug-23", 0) + COALESCE(A."sep-23", 0) + 
   COALESCE(A."oct-23", 0) + COALESCE(A."nov-23", 0) + COALESCE(A."dec-23", 0)
)
- 
(
   COALESCE(B."jan-23", 0) + COALESCE(B."feb-23", 0) + COALESCE(B."mar-23", 0) + 
   COALESCE(B."apr-23", 0) + COALESCE(B."may-23", 0) + COALESCE(B."jun-23", 0) + 
   COALESCE(B."jul-23", 0) + COALESCE(B."aug-23", 0) + COALESCE(B."sep-23", 0) + 
   COALESCE(B."oct-23", 0) + COALESCE(B."nov-23", 0) + COALESCE(B."dec-23", 0)
) 
AS "Variance vs AOP YTD",

-- Variance vs LY YTD (CY Actual - LY Actual YTD) for Jan-23 to Dec-23
(
   COALESCE(A."jan-23", 0) + COALESCE(A."feb-23", 0) + COALESCE(A."mar-23", 0) + 
   COALESCE(A."apr-23", 0) + COALESCE(A."may-23", 0) + COALESCE(A."jun-23", 0) + 
   COALESCE(A."jul-23", 0) + COALESCE(A."aug-23", 0) + COALESCE(A."sep-23", 0) + 
   COALESCE(A."oct-23", 0) + COALESCE(A."nov-23", 0) + COALESCE(A."dec-23", 0)
) 
- 
(
   SELECT 
     COALESCE(A_prev."jan-22", 0) + COALESCE(A_prev."feb-22", 0) + COALESCE(A_prev."mar-22", 0) + 
     COALESCE(A_prev."apr-22", 0) + COALESCE(A_prev."may-22", 0) + COALESCE(A_prev."jun-22", 0) + 
     COALESCE(A_prev."jul-22", 0) + COALESCE(A_prev."aug-22", 0) + COALESCE(A_prev."sep-22", 0) + 
     COALESCE(A_prev."oct-22", 0) + COALESCE(A_prev."nov-22", 0) + COALESCE(A_prev."dec-22", 0)
   FROM '${inputs.matric_ytd}_Actual' A_prev
   WHERE A_prev.particulars = A.particulars
) 
AS "Variance vs LY YTD"


FROM '${inputs.matric_ytd}_Actual' A
JOIN '${inputs.matric_ytd}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter_ytd.value}' = '2023'
AND A.particulars NOT IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')
AND NOT (
    TRIM(A.particulars) = ''  -- Ensures the 'Particulars' column is not empty
    AND (
        COALESCE(A."jan-23", 0) + COALESCE(A."feb-23", 0) + COALESCE(A."mar-23", 0) + 
        COALESCE(A."apr-23", 0) + COALESCE(A."may-23", 0) + COALESCE(A."jun-23", 0) + 
        COALESCE(A."jul-23", 0) + COALESCE(A."aug-23", 0) + COALESCE(A."sep-23", 0) + 
        COALESCE(A."oct-23", 0) + COALESCE(A."nov-23", 0) + COALESCE(A."dec-23", 0)
    ) = 0
)

UNION ALL

SELECT
   A.sno, 
  'Jan-24 to Dec-24' AS YTD,
  A.particulars AS particulars,
-- CY Actual (Jan-24 to Dec-24)
COALESCE(A."jan-24", 0) + COALESCE(A."feb-24", 0) + COALESCE(A."mar-24", 0) + 
COALESCE(A."apr-24", 0) + COALESCE(A."may-24", 0) + COALESCE(A."jun-24", 0) + 
COALESCE(A."jul-24", 0) + COALESCE(A."aug-24", 0) + COALESCE(A."sep-24", 0) + 
COALESCE(A."oct-24", 0) + COALESCE(A."nov-24", 0) + COALESCE(A."dec-24", 0) AS "CY Actual",

-- YTD AOP (Jan-24 to Dec-24)
COALESCE(B."jan-24", 0) + COALESCE(B."feb-24", 0) + COALESCE(B."mar-24", 0) + 
COALESCE(B."apr-24", 0) + COALESCE(B."may-24", 0) + COALESCE(B."jun-24", 0) + 
COALESCE(B."jul-24", 0) + COALESCE(B."aug-24", 0) + COALESCE(B."sep-24", 0) + 
COALESCE(B."oct-24", 0) + COALESCE(B."nov-24", 0) + COALESCE(B."dec-24", 0) AS "YTD AOP",

-- LY Actual YTD (Jan-23 to Dec-23)
(SELECT 
   COALESCE(A_prev."jan-23", 0) + COALESCE(A_prev."feb-23", 0) + COALESCE(A_prev."mar-23", 0) + 
   COALESCE(A_prev."apr-23", 0) + COALESCE(A_prev."may-23", 0) + COALESCE(A_prev."jun-23", 0) + 
   COALESCE(A_prev."jul-23", 0) + COALESCE(A_prev."aug-23", 0) + COALESCE(A_prev."sep-23", 0) + 
   COALESCE(A_prev."oct-23", 0) + COALESCE(A_prev."nov-23", 0) + COALESCE(A_prev."dec-23", 0)
 FROM GGCL_Actual A_prev
 WHERE A_prev.particulars = A.particulars) AS "LY Actual YTD",

-- Variance vs AOP YTD (CY Actual - YTD AOP) for Jan-24 to Dec-24
(
   COALESCE(A."jan-24", 0) + COALESCE(A."feb-24", 0) + COALESCE(A."mar-24", 0) + 
   COALESCE(A."apr-24", 0) + COALESCE(A."may-24", 0) + COALESCE(A."jun-24", 0) + 
   COALESCE(A."jul-24", 0) + COALESCE(A."aug-24", 0) + COALESCE(A."sep-24", 0) + 
   COALESCE(A."oct-24", 0) + COALESCE(A."nov-24", 0) + COALESCE(A."dec-24", 0)
)
- 
(
   COALESCE(B."jan-24", 0) + COALESCE(B."feb-24", 0) + COALESCE(B."mar-24", 0) + 
   COALESCE(B."apr-24", 0) + COALESCE(B."may-24", 0) + COALESCE(B."jun-24", 0) + 
   COALESCE(B."jul-24", 0) + COALESCE(B."aug-24", 0) + COALESCE(B."sep-24", 0) + 
   COALESCE(B."oct-24", 0) + COALESCE(B."nov-24", 0) + COALESCE(B."dec-24", 0)
) 
AS "Variance vs AOP YTD",

-- Variance vs LY YTD (CY Actual - LY Actual YTD) for Jan-24 to Dec-24
(
   COALESCE(A."jan-24", 0) + COALESCE(A."feb-24", 0) + COALESCE(A."mar-24", 0) + 
   COALESCE(A."apr-24", 0) + COALESCE(A."may-24", 0) + COALESCE(A."jun-24", 0) + 
   COALESCE(A."jul-24", 0) + COALESCE(A."aug-24", 0) + COALESCE(A."sep-24", 0) + 
   COALESCE(A."oct-24", 0) + COALESCE(A."nov-24", 0) + COALESCE(A."dec-24", 0)
) 
- 
(
   SELECT 
     COALESCE(A_prev."jan-23", 0) + COALESCE(A_prev."feb-23", 0) + COALESCE(A_prev."mar-23", 0) + 
     COALESCE(A_prev."apr-23", 0) + COALESCE(A_prev."may-23", 0) + COALESCE(A_prev."jun-23", 0) + 
     COALESCE(A_prev."jul-23", 0) + COALESCE(A_prev."aug-23", 0) + COALESCE(A_prev."sep-23", 0) + 
     COALESCE(A_prev."oct-23", 0) + COALESCE(A_prev."nov-23", 0) + COALESCE(A_prev."dec-23", 0)
   FROM '${inputs.matric_ytd}_Actual' A_prev
   WHERE A_prev.particulars = A.particulars
) 
AS "Variance vs LY YTD"



FROM '${inputs.matric_ytd}_Actual' A
JOIN '${inputs.matric_ytd}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter_ytd.value}' = '2024'
AND A.particulars NOT IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')
AND NOT (
    TRIM(A.particulars) = ''
        AND (
        COALESCE(A."jan-24", 0) + COALESCE(A."feb-24", 0) + COALESCE(A."mar-24", 0) + 
        COALESCE(A."apr-24", 0) + COALESCE(A."may-24", 0) + COALESCE(A."jun-24", 0) + 
        COALESCE(A."jul-24", 0) + COALESCE(A."aug-24", 0) + COALESCE(A."sep-24", 0) + 
        COALESCE(A."oct-24", 0) + COALESCE(A."nov-24", 0) + COALESCE(A."dec-24", 0)
    ) = 0 
)

UNION ALL

SELECT 
  A.sno, 
  'Jan-25' AS YTD,
  A.particulars AS particulars,

  -- CY Actual (Jan-25)
  COALESCE(A."jan-25", 0) AS "CY Actual",

  -- YTD AOP (Jan-25)
  COALESCE(B."jan-25", 0) AS "YTD AOP",

  -- LY Actual YTD (Jan-24)
  (SELECT COALESCE(A_prev."jan-24", 0) 
   FROM GGCL_Actual A_prev
   WHERE A_prev.particulars = A.particulars) AS "LY Actual YTD",

  -- Variance vs AOP YTD (CY Actual - YTD AOP) for Jan-25
  (COALESCE(A."jan-25", 0) - COALESCE(B."jan-25", 0)) AS "Variance vs AOP YTD",

  -- Variance vs LY YTD (CY Actual - LY Actual YTD) for Jan-25
  (COALESCE(A."jan-25", 0) - 
    (SELECT COALESCE(A_prev."jan-24", 0) 
     FROM GGCL_Actual A_prev
     WHERE A_prev.particulars = A.particulars)) AS "Variance vs LY YTD"

FROM '${inputs.matric_ytd}_Actual' A
JOIN '${inputs.matric_ytd}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter_ytd.value}' = '2025'
AND A.particulars NOT IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')
AND NOT (
    TRIM(A.particulars) = ''
        AND (COALESCE(A."jan-25", 0)) = 0
)

ORDER BY A.sno ASC;
```

```sql ytd_margins
SELECT 
  A.sno,
  'Apr-19 to Dec-19' AS YTD,
  A.particulars AS particulars,
  COALESCE(A."apr-19", 0) + COALESCE(A."may-19", 0) + COALESCE(A."jun-19", 0) + 
  COALESCE(A."jul-19", 0) + COALESCE(A."aug-19", 0) + COALESCE(A."sep-19", 0) + 
  COALESCE(A."oct-19", 0) + COALESCE(A."nov-19", 0) + COALESCE(A."dec-19", 0) AS "CY Actual",
  COALESCE(B."apr-19", 0) + COALESCE(B."may-19", 0) + COALESCE(B."jun-19", 0) + 
  COALESCE(B."jul-19", 0) + COALESCE(B."aug-19", 0) + COALESCE(B."sep-19", 0) + 
  COALESCE(B."oct-19", 0) + COALESCE(B."nov-19", 0) + COALESCE(B."dec-19", 0) AS "YTD AOP",
  0 AS "LY Actual YTD",  -- First year has no LY Actual YTD
  COALESCE(A."apr-19", 0) + COALESCE(A."may-19", 0) + COALESCE(A."jun-19", 0) + 
  COALESCE(A."jul-19", 0) + COALESCE(A."aug-19", 0) + COALESCE(A."sep-19", 0) + 
  COALESCE(A."oct-19", 0) + COALESCE(A."nov-19", 0) + COALESCE(A."dec-19", 0) -
  (COALESCE(B."apr-19", 0) + COALESCE(B."may-19", 0) + COALESCE(B."jun-19", 0) + 
   COALESCE(B."jul-19", 0) + COALESCE(B."aug-19", 0) + COALESCE(B."sep-19", 0) + 
   COALESCE(B."oct-19", 0) + COALESCE(B."nov-19", 0) + COALESCE(B."dec-19", 0)) AS "Variance vs AOP YTD",
   0 AS "Variance vs LY YTD"  -- First year has no previous LY Actual YTD
FROM '${inputs.matric_ytd}_Actual' A
JOIN '${inputs.matric_ytd}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter_ytd.value}' = '2019'
AND A.particulars IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')


UNION ALL

SELECT 
   A.sno,
  'Jan-20 to Dec-20' AS YTD,
  A.particulars AS particulars,
  
  -- CY Actual YTD
COALESCE(A."jan-20", 0) + COALESCE(A."feb-20", 0) + COALESCE(A."mar-20", 0) + 
COALESCE(A."apr-20", 0) + COALESCE(A."may-20", 0) + COALESCE(A."jun-20", 0) + 
COALESCE(A."jul-20", 0) + COALESCE(A."aug-20", 0) + COALESCE(A."sep-20", 0) + 
COALESCE(A."oct-20", 0) + COALESCE(A."nov-20", 0) + COALESCE(A."dec-20", 0) AS "CY Actual",


  -- YTD AOP
COALESCE(B."jan-20", 0) + COALESCE(B."feb-20", 0) + COALESCE(B."mar-20", 0) + 
COALESCE(B."apr-20", 0) + COALESCE(B."may-20", 0) + COALESCE(B."jun-20", 0) + 
COALESCE(B."jul-20", 0) + COALESCE(B."aug-20", 0) + COALESCE(B."sep-20", 0) + 
COALESCE(B."oct-20", 0) + COALESCE(B."nov-20", 0) + COALESCE(B."dec-20", 0) AS "YTD AOP",

-- LY Actual YTD (Previous Year CY Actual)
(SELECT 
   COALESCE(A_prev."apr-19", 0) + COALESCE(A_prev."may-19", 0) + COALESCE(A_prev."jun-19", 0) + 
   COALESCE(A_prev."jul-19", 0) + COALESCE(A_prev."aug-19", 0) + COALESCE(A_prev."sep-19", 0) + 
   COALESCE(A_prev."oct-19", 0) + COALESCE(A_prev."nov-19", 0) + COALESCE(A_prev."dec-19", 0)
 FROM '${inputs.matric_ytd}_Actual' A_prev
 WHERE A_prev.particulars = A.particulars) AS "LY Actual YTD",

-- Variance vs AOP YTD (CY Actual - YTD AOP)
(COALESCE(A."jan-20", 0) + COALESCE(A."feb-20", 0) + COALESCE(A."mar-20", 0) + 
 COALESCE(A."apr-20", 0) + COALESCE(A."may-20", 0) + COALESCE(A."jun-20", 0) + 
 COALESCE(A."jul-20", 0) + COALESCE(A."aug-20", 0) + COALESCE(A."sep-20", 0) + 
 COALESCE(A."oct-20", 0) + COALESCE(A."nov-20", 0) + COALESCE(A."dec-20", 0)) -

(COALESCE(B."jan-20", 0) + COALESCE(B."feb-20", 0) + COALESCE(B."mar-20", 0) + 
 COALESCE(B."apr-20", 0) + COALESCE(B."may-20", 0) + COALESCE(B."jun-20", 0) + 
 COALESCE(B."jul-20", 0) + COALESCE(B."aug-20", 0) + COALESCE(B."sep-20", 0) + 
 COALESCE(B."oct-20", 0) + COALESCE(B."nov-20", 0) + COALESCE(B."dec-20", 0)) AS "Variance vs AOP YTD",

-- Variance vs LY YTD (CY Actual - LY Actual YTD)
(COALESCE(A."jan-20", 0) + COALESCE(A."feb-20", 0) + COALESCE(A."mar-20", 0) + 
 COALESCE(A."apr-20", 0) + COALESCE(A."may-20", 0) + COALESCE(A."jun-20", 0) + 
 COALESCE(A."jul-20", 0) + COALESCE(A."aug-20", 0) + COALESCE(A."sep-20", 0) + 
 COALESCE(A."oct-20", 0) + COALESCE(A."nov-20", 0) + COALESCE(A."dec-20", 0)) -

(SELECT  
   COALESCE(A_prev."apr-19", 0) + COALESCE(A_prev."may-19", 0) + COALESCE(A_prev."jun-19", 0) + 
   COALESCE(A_prev."jul-19", 0) + COALESCE(A_prev."aug-19", 0) + COALESCE(A_prev."sep-19", 0) + 
   COALESCE(A_prev."oct-19", 0) + COALESCE(A_prev."nov-19", 0) + COALESCE(A_prev."dec-19", 0)
 FROM '${inputs.matric_ytd}_Actual' A_prev
 WHERE A_prev.particulars = A.particulars) AS "Variance vs LY YTD"


FROM '${inputs.matric_ytd}_Actual' A
JOIN '${inputs.matric_ytd}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter_ytd.value}' = '2020'
AND A.particulars IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')

UNION ALL

SELECT
   A.sno, 
  'Jan-21 to Dec-21' AS YTD,
  A.particulars AS particulars,
  
-- CY Actual Calculation
COALESCE(A."jan-21", 0) + COALESCE(A."feb-21", 0) + COALESCE(A."mar-21", 0) + 
COALESCE(A."apr-21", 0) + COALESCE(A."may-21", 0) + COALESCE(A."jun-21", 0) + 
COALESCE(A."jul-21", 0) + COALESCE(A."aug-21", 0) + COALESCE(A."sep-21", 0) + 
COALESCE(A."oct-21", 0) + COALESCE(A."nov-21", 0) + COALESCE(A."dec-21", 0) AS "CY Actual",
  
-- YTD AOP Calculation
COALESCE(B."jan-21", 0) + COALESCE(B."feb-21", 0) + COALESCE(B."mar-21", 0) + 
COALESCE(B."apr-21", 0) + COALESCE(B."may-21", 0) + COALESCE(B."jun-21", 0) + 
COALESCE(B."jul-21", 0) + COALESCE(B."aug-21", 0) + COALESCE(B."sep-21", 0) + 
COALESCE(B."oct-21", 0) + COALESCE(B."nov-21", 0) + COALESCE(B."dec-21", 0) AS "YTD AOP",
  
-- LY Actual YTD (Previous Year)
(SELECT 
   COALESCE(A_prev."jan-20", 0) + COALESCE(A_prev."feb-20", 0) + COALESCE(A_prev."mar-20", 0) + 
   COALESCE(A_prev."apr-20", 0) + COALESCE(A_prev."may-20", 0) + COALESCE(A_prev."jun-20", 0) + 
   COALESCE(A_prev."jul-20", 0) + COALESCE(A_prev."aug-20", 0) + COALESCE(A_prev."sep-20", 0) + 
   COALESCE(A_prev."oct-20", 0) + COALESCE(A_prev."nov-20", 0) + COALESCE(A_prev."dec-20", 0)
 FROM GGCL_Actual A_prev
 WHERE A_prev.particulars = A.particulars) AS "LY Actual YTD",
  
-- Variance vs AOP YTD (CY Actual - YTD AOP)
(COALESCE(A."jan-21", 0) + COALESCE(A."feb-21", 0) + COALESCE(A."mar-21", 0) + 
 COALESCE(A."apr-21", 0) + COALESCE(A."may-21", 0) + COALESCE(A."jun-21", 0) + 
 COALESCE(A."jul-21", 0) + COALESCE(A."aug-21", 0) + COALESCE(A."sep-21", 0) + 
 COALESCE(A."oct-21", 0) + COALESCE(A."nov-21", 0) + COALESCE(A."dec-21", 0)) -

(COALESCE(B."jan-21", 0) + COALESCE(B."feb-21", 0) + COALESCE(B."mar-21", 0) + 
 COALESCE(B."apr-21", 0) + COALESCE(B."may-21", 0) + COALESCE(B."jun-21", 0) + 
 COALESCE(B."jul-21", 0) + COALESCE(B."aug-21", 0) + COALESCE(B."sep-21", 0) + 
 COALESCE(B."oct-21", 0) + COALESCE(B."nov-21", 0) + COALESCE(B."dec-21", 0)) AS "Variance vs AOP YTD",
  
-- Variance vs LY YTD (CY Actual - LY Actual YTD)
(COALESCE(A."jan-21", 0) + COALESCE(A."feb-21", 0) + COALESCE(A."mar-21", 0) + 
 COALESCE(A."apr-21", 0) + COALESCE(A."may-21", 0) + COALESCE(A."jun-21", 0) + 
 COALESCE(A."jul-21", 0) + COALESCE(A."aug-21", 0) + COALESCE(A."sep-21", 0) + 
 COALESCE(A."oct-21", 0) + COALESCE(A."nov-21", 0) + COALESCE(A."dec-21", 0)) - 

(SELECT 
   COALESCE(A_prev."jan-20", 0) + COALESCE(A_prev."feb-20", 0) + COALESCE(A_prev."mar-20", 0) + 
   COALESCE(A_prev."apr-20", 0) + COALESCE(A_prev."may-20", 0) + COALESCE(A_prev."jun-20", 0) + 
   COALESCE(A_prev."jul-20", 0) + COALESCE(A_prev."aug-20", 0) + COALESCE(A_prev."sep-20", 0) + 
   COALESCE(A_prev."oct-20", 0) + COALESCE(A_prev."nov-20", 0) + COALESCE(A_prev."dec-20", 0)
 FROM '${inputs.matric_ytd}_Actual' A_prev
 WHERE A_prev.particulars = A.particulars) AS "Variance vs LY YTD"


FROM '${inputs.matric_ytd}_Actual' A
JOIN '${inputs.matric_ytd}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter_ytd.value}' = '2021'
AND A.particulars IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')

UNION ALL

SELECT 
   A.sno,
  'Jan-22 to Dec-22' AS YTD,
  A.particulars AS particulars,
-- CY Actual YTD Calculation (Jan-22 to Dec-22)
COALESCE(A."jan-22", 0) + COALESCE(A."feb-22", 0) + COALESCE(A."mar-22", 0) + 
COALESCE(A."apr-22", 0) + COALESCE(A."may-22", 0) + COALESCE(A."jun-22", 0) + 
COALESCE(A."jul-22", 0) + COALESCE(A."aug-22", 0) + COALESCE(A."sep-22", 0) + 
COALESCE(A."oct-22", 0) + COALESCE(A."nov-22", 0) + COALESCE(A."dec-22", 0) AS "CY Actual",

-- YTD AOP Calculation (Jan-22 to Dec-22)
COALESCE(B."jan-22", 0) + COALESCE(B."feb-22", 0) + COALESCE(B."mar-22", 0) + 
COALESCE(B."apr-22", 0) + COALESCE(B."may-22", 0) + COALESCE(B."jun-22", 0) + 
COALESCE(B."jul-22", 0) + COALESCE(B."aug-22", 0) + COALESCE(B."sep-22", 0) + 
COALESCE(B."oct-22", 0) + COALESCE(B."nov-22", 0) + COALESCE(B."dec-22", 0) AS "YTD AOP",

-- LY Actual YTD Calculation (Jan-21 to Dec-21)
(SELECT 
   COALESCE(A_prev."jan-21", 0) + COALESCE(A_prev."feb-21", 0) + COALESCE(A_prev."mar-21", 0) + 
   COALESCE(A_prev."apr-21", 0) + COALESCE(A_prev."may-21", 0) + COALESCE(A_prev."jun-21", 0) + 
   COALESCE(A_prev."jul-21", 0) + COALESCE(A_prev."aug-21", 0) + COALESCE(A_prev."sep-21", 0) + 
   COALESCE(A_prev."oct-21", 0) + COALESCE(A_prev."nov-21", 0) + COALESCE(A_prev."dec-21", 0)
 FROM '${inputs.matric_ytd}_Actual' A_prev
 WHERE A_prev.particulars = A.particulars) AS "LY Actual YTD",

-- Variance vs AOP YTD Calculation (Jan-22 to Dec-22)
(
   COALESCE(A."jan-22", 0) + COALESCE(A."feb-22", 0) + COALESCE(A."mar-22", 0) + 
   COALESCE(A."apr-22", 0) + COALESCE(A."may-22", 0) + COALESCE(A."jun-22", 0) + 
   COALESCE(A."jul-22", 0) + COALESCE(A."aug-22", 0) + COALESCE(A."sep-22", 0) + 
   COALESCE(A."oct-22", 0) + COALESCE(A."nov-22", 0) + COALESCE(A."dec-22", 0)
) 
- 
(
   COALESCE(B."jan-22", 0) + COALESCE(B."feb-22", 0) + COALESCE(B."mar-22", 0) + 
   COALESCE(B."apr-22", 0) + COALESCE(B."may-22", 0) + COALESCE(B."jun-22", 0) + 
   COALESCE(B."jul-22", 0) + COALESCE(B."aug-22", 0) + COALESCE(B."sep-22", 0) + 
   COALESCE(B."oct-22", 0) + COALESCE(B."nov-22", 0) + COALESCE(B."dec-22", 0)
) 
AS "Variance vs AOP YTD",

-- Variance vs LY YTD Calculation (Jan-22 to Dec-22)
(
   COALESCE(A."jan-22", 0) + COALESCE(A."feb-22", 0) + COALESCE(A."mar-22", 0) + 
   COALESCE(A."apr-22", 0) + COALESCE(A."may-22", 0) + COALESCE(A."jun-22", 0) + 
   COALESCE(A."jul-22", 0) + COALESCE(A."aug-22", 0) + COALESCE(A."sep-22", 0) + 
   COALESCE(A."oct-22", 0) + COALESCE(A."nov-22", 0) + COALESCE(A."dec-22", 0)
)
- 
(
   SELECT 
     COALESCE(A_prev."jan-21", 0) + COALESCE(A_prev."feb-21", 0) + COALESCE(A_prev."mar-21", 0) + 
     COALESCE(A_prev."apr-21", 0) + COALESCE(A_prev."may-21", 0) + COALESCE(A_prev."jun-21", 0) + 
     COALESCE(A_prev."jul-21", 0) + COALESCE(A_prev."aug-21", 0) + COALESCE(A_prev."sep-21", 0) + 
     COALESCE(A_prev."oct-21", 0) + COALESCE(A_prev."nov-21", 0) + COALESCE(A_prev."dec-21", 0)
   FROM '${inputs.matric_ytd}_Actual' A_prev
   WHERE A_prev.particulars = A.particulars
) 
AS "Variance vs LY YTD"


FROM '${inputs.matric_ytd}_Actual' A
JOIN '${inputs.matric_ytd}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter_ytd.value}' = '2022'
AND A.particulars IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')

UNION ALL

SELECT
   A.sno, 
  'Jan-23 to Dec-23' AS YTD,
  A.particulars AS particulars,
-- CY Actual (Jan-23 to Dec-23)
COALESCE(A."jan-23", 0) + COALESCE(A."feb-23", 0) + COALESCE(A."mar-23", 0) + 
COALESCE(A."apr-23", 0) + COALESCE(A."may-23", 0) + COALESCE(A."jun-23", 0) + 
COALESCE(A."jul-23", 0) + COALESCE(A."aug-23", 0) + COALESCE(A."sep-23", 0) + 
COALESCE(A."oct-23", 0) + COALESCE(A."nov-23", 0) + COALESCE(A."dec-23", 0) AS "CY Actual",

-- YTD AOP (Jan-23 to Dec-23)
COALESCE(B."jan-23", 0) + COALESCE(B."feb-23", 0) + COALESCE(B."mar-23", 0) + 
COALESCE(B."apr-23", 0) + COALESCE(B."may-23", 0) + COALESCE(B."jun-23", 0) + 
COALESCE(B."jul-23", 0) + COALESCE(B."aug-23", 0) + COALESCE(B."sep-23", 0) + 
COALESCE(B."oct-23", 0) + COALESCE(B."nov-23", 0) + COALESCE(B."dec-23", 0) AS "YTD AOP",

-- LY Actual YTD (Jan-22 to Dec-22)
(SELECT 
   COALESCE(A_prev."jan-22", 0) + COALESCE(A_prev."feb-22", 0) + COALESCE(A_prev."mar-22", 0) + 
   COALESCE(A_prev."apr-22", 0) + COALESCE(A_prev."may-22", 0) + COALESCE(A_prev."jun-22", 0) + 
   COALESCE(A_prev."jul-22", 0) + COALESCE(A_prev."aug-22", 0) + COALESCE(A_prev."sep-22", 0) + 
   COALESCE(A_prev."oct-22", 0) + COALESCE(A_prev."nov-22", 0) + COALESCE(A_prev."dec-22", 0)
 FROM GGCL_Actual A_prev
 WHERE A_prev.particulars = A.particulars) AS "LY Actual YTD",

-- Variance vs AOP YTD (CY Actual - YTD AOP) for Jan-23 to Dec-23
(
   COALESCE(A."jan-23", 0) + COALESCE(A."feb-23", 0) + COALESCE(A."mar-23", 0) + 
   COALESCE(A."apr-23", 0) + COALESCE(A."may-23", 0) + COALESCE(A."jun-23", 0) + 
   COALESCE(A."jul-23", 0) + COALESCE(A."aug-23", 0) + COALESCE(A."sep-23", 0) + 
   COALESCE(A."oct-23", 0) + COALESCE(A."nov-23", 0) + COALESCE(A."dec-23", 0)
)
- 
(
   COALESCE(B."jan-23", 0) + COALESCE(B."feb-23", 0) + COALESCE(B."mar-23", 0) + 
   COALESCE(B."apr-23", 0) + COALESCE(B."may-23", 0) + COALESCE(B."jun-23", 0) + 
   COALESCE(B."jul-23", 0) + COALESCE(B."aug-23", 0) + COALESCE(B."sep-23", 0) + 
   COALESCE(B."oct-23", 0) + COALESCE(B."nov-23", 0) + COALESCE(B."dec-23", 0)
) 
AS "Variance vs AOP YTD",

-- Variance vs LY YTD (CY Actual - LY Actual YTD) for Jan-23 to Dec-23
(
   COALESCE(A."jan-23", 0) + COALESCE(A."feb-23", 0) + COALESCE(A."mar-23", 0) + 
   COALESCE(A."apr-23", 0) + COALESCE(A."may-23", 0) + COALESCE(A."jun-23", 0) + 
   COALESCE(A."jul-23", 0) + COALESCE(A."aug-23", 0) + COALESCE(A."sep-23", 0) + 
   COALESCE(A."oct-23", 0) + COALESCE(A."nov-23", 0) + COALESCE(A."dec-23", 0)
) 
- 
(
   SELECT 
     COALESCE(A_prev."jan-22", 0) + COALESCE(A_prev."feb-22", 0) + COALESCE(A_prev."mar-22", 0) + 
     COALESCE(A_prev."apr-22", 0) + COALESCE(A_prev."may-22", 0) + COALESCE(A_prev."jun-22", 0) + 
     COALESCE(A_prev."jul-22", 0) + COALESCE(A_prev."aug-22", 0) + COALESCE(A_prev."sep-22", 0) + 
     COALESCE(A_prev."oct-22", 0) + COALESCE(A_prev."nov-22", 0) + COALESCE(A_prev."dec-22", 0)
   FROM '${inputs.matric_ytd}_Actual' A_prev
   WHERE A_prev.particulars = A.particulars
) 
AS "Variance vs LY YTD"


FROM '${inputs.matric_ytd}_Actual' A
JOIN '${inputs.matric_ytd}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter_ytd.value}' = '2023'
AND A.particulars IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')

UNION ALL

SELECT
   A.sno, 
  'Jan-24 to Dec-24' AS YTD,
  A.particulars AS particulars,
-- CY Actual (Jan-24 to Dec-24)
COALESCE(A."jan-24", 0) + COALESCE(A."feb-24", 0) + COALESCE(A."mar-24", 0) + 
COALESCE(A."apr-24", 0) + COALESCE(A."may-24", 0) + COALESCE(A."jun-24", 0) + 
COALESCE(A."jul-24", 0) + COALESCE(A."aug-24", 0) + COALESCE(A."sep-24", 0) + 
COALESCE(A."oct-24", 0) + COALESCE(A."nov-24", 0) + COALESCE(A."dec-24", 0) AS "CY Actual",

-- YTD AOP (Jan-24 to Dec-24)
COALESCE(B."jan-24", 0) + COALESCE(B."feb-24", 0) + COALESCE(B."mar-24", 0) + 
COALESCE(B."apr-24", 0) + COALESCE(B."may-24", 0) + COALESCE(B."jun-24", 0) + 
COALESCE(B."jul-24", 0) + COALESCE(B."aug-24", 0) + COALESCE(B."sep-24", 0) + 
COALESCE(B."oct-24", 0) + COALESCE(B."nov-24", 0) + COALESCE(B."dec-24", 0) AS "YTD AOP",

-- LY Actual YTD (Jan-23 to Dec-23)
(SELECT 
   COALESCE(A_prev."jan-23", 0) + COALESCE(A_prev."feb-23", 0) + COALESCE(A_prev."mar-23", 0) + 
   COALESCE(A_prev."apr-23", 0) + COALESCE(A_prev."may-23", 0) + COALESCE(A_prev."jun-23", 0) + 
   COALESCE(A_prev."jul-23", 0) + COALESCE(A_prev."aug-23", 0) + COALESCE(A_prev."sep-23", 0) + 
   COALESCE(A_prev."oct-23", 0) + COALESCE(A_prev."nov-23", 0) + COALESCE(A_prev."dec-23", 0)
 FROM GGCL_Actual A_prev
 WHERE A_prev.particulars = A.particulars) AS "LY Actual YTD",

-- Variance vs AOP YTD (CY Actual - YTD AOP) for Jan-24 to Dec-24
(
   COALESCE(A."jan-24", 0) + COALESCE(A."feb-24", 0) + COALESCE(A."mar-24", 0) + 
   COALESCE(A."apr-24", 0) + COALESCE(A."may-24", 0) + COALESCE(A."jun-24", 0) + 
   COALESCE(A."jul-24", 0) + COALESCE(A."aug-24", 0) + COALESCE(A."sep-24", 0) + 
   COALESCE(A."oct-24", 0) + COALESCE(A."nov-24", 0) + COALESCE(A."dec-24", 0)
)
- 
(
   COALESCE(B."jan-24", 0) + COALESCE(B."feb-24", 0) + COALESCE(B."mar-24", 0) + 
   COALESCE(B."apr-24", 0) + COALESCE(B."may-24", 0) + COALESCE(B."jun-24", 0) + 
   COALESCE(B."jul-24", 0) + COALESCE(B."aug-24", 0) + COALESCE(B."sep-24", 0) + 
   COALESCE(B."oct-24", 0) + COALESCE(B."nov-24", 0) + COALESCE(B."dec-24", 0)
) 
AS "Variance vs AOP YTD",

-- Variance vs LY YTD (CY Actual - LY Actual YTD) for Jan-24 to Dec-24
(
   COALESCE(A."jan-24", 0) + COALESCE(A."feb-24", 0) + COALESCE(A."mar-24", 0) + 
   COALESCE(A."apr-24", 0) + COALESCE(A."may-24", 0) + COALESCE(A."jun-24", 0) + 
   COALESCE(A."jul-24", 0) + COALESCE(A."aug-24", 0) + COALESCE(A."sep-24", 0) + 
   COALESCE(A."oct-24", 0) + COALESCE(A."nov-24", 0) + COALESCE(A."dec-24", 0)
) 
- 
(
   SELECT 
     COALESCE(A_prev."jan-23", 0) + COALESCE(A_prev."feb-23", 0) + COALESCE(A_prev."mar-23", 0) + 
     COALESCE(A_prev."apr-23", 0) + COALESCE(A_prev."may-23", 0) + COALESCE(A_prev."jun-23", 0) + 
     COALESCE(A_prev."jul-23", 0) + COALESCE(A_prev."aug-23", 0) + COALESCE(A_prev."sep-23", 0) + 
     COALESCE(A_prev."oct-23", 0) + COALESCE(A_prev."nov-23", 0) + COALESCE(A_prev."dec-23", 0)
   FROM '${inputs.matric_ytd}_Actual' A_prev
   WHERE A_prev.particulars = A.particulars
) 
AS "Variance vs LY YTD"



FROM '${inputs.matric_ytd}_Actual' A
JOIN '${inputs.matric_ytd}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter_ytd.value}' = '2024'
AND A.particulars IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')

UNION ALL

SELECT 
  A.sno, 
  'Jan-25' AS YTD,
  A.particulars AS particulars,

  -- CY Actual (Jan-25)
  COALESCE(A."jan-25", 0) AS "CY Actual",

  -- YTD AOP (Jan-25)
  COALESCE(B."jan-25", 0) AS "YTD AOP",

  -- LY Actual YTD (Jan-24)
  (SELECT COALESCE(A_prev."jan-24", 0) 
   FROM GGCL_Actual A_prev
   WHERE A_prev.particulars = A.particulars) AS "LY Actual YTD",

  -- Variance vs AOP YTD (CY Actual - YTD AOP) for Jan-25
  (COALESCE(A."jan-25", 0) - COALESCE(B."jan-25", 0)) AS "Variance vs AOP YTD",

  -- Variance vs LY YTD (CY Actual - LY Actual YTD) for Jan-25
  (COALESCE(A."jan-25", 0) - 
    (SELECT COALESCE(A_prev."jan-24", 0) 
     FROM GGCL_Actual A_prev
     WHERE A_prev.particulars = A.particulars)) AS "Variance vs LY YTD"

FROM '${inputs.matric_ytd}_Actual' A
JOIN '${inputs.matric_ytd}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter_ytd.value}' = '2025'
AND A.particulars IN ('GROSS %', 'EBITDA %', 'EBT %', 'EBIT %')
AND NOT (
    TRIM(A.particulars) = ''
        AND (COALESCE(A."jan-25", 0)) = 0
)

ORDER BY A.sno ASC;
```

```sql max_date
SELECT STRFTIME(MAX(STRPTIME(date_filter, '%b-%y')), '%b-%y') AS max_date
FROM (
    SELECT 
        UPPER(LEFT(column_name, 1)) || SUBSTRING(column_name FROM 2) AS date_filter
    FROM information_schema.columns
    WHERE table_name = 'GGE_Actual'
    AND column_name NOT IN ('particulars', 'sno') -- Exclude non-date columns
) AS subquery;
```

```sql max_date_ytd
WITH CalendarYears AS (
    SELECT DISTINCT 
        'Jan-' || RIGHT(column_name, 2) || ' to Dec-' || RIGHT(column_name, 2) AS date_filter_ytd,
        RIGHT(column_name, 2)::INTEGER AS YTD  -- Extract numeric year for sorting
    FROM information_schema.columns
    WHERE table_name = 'GGCL_Actual'  
    AND column_name LIKE 'jan-%'  -- Change filter to Jan-based columns
    AND RIGHT(column_name, 2)::INTEGER  -- Ensures only valid years
)
SELECT 'Jan-' || LPAD(CAST(MAX(YTD) AS TEXT), 2, '0') AS max_date_ytd
FROM CalendarYears;

```