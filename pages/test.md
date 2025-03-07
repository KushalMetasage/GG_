<div style="position: relative; margin-bottom: 40px;">  
    <h1 style="font-weight: bold; font-size: 30px; margin: 0;">Test</h1>
</div>

<center>
<Dropdown data={date_filter} name=date_filter value=date_filter title="Date" defaultValue="Apr-19 to Mar-20">
</Dropdown>
</center>

```sql date_filter
WITH FiscalYears AS (
    SELECT DISTINCT 
        'Apr-' || RIGHT(column_name, 2) || ' to Mar-' || LPAD(CAST(RIGHT(column_name, 2)::INTEGER + 1 AS TEXT), 2, '0') AS date_filter
    FROM information_schema.columns
    WHERE table_name = 'GGCL_Actual'  
    AND column_name NOT IN ('particulars', 'sno') -- Exclude non-date columns
    AND column_name LIKE 'apr-%'  -- Select only April columns to form fiscal year ranges
    AND RIGHT(column_name, 2)::INTEGER < 24  -- Ensures "Apr-24" does NOT get included in Fiscal Year ranges
)
SELECT date_filter FROM FiscalYears ORDER BY date_filter;

```

```sql ytd
WITH FiscalYearMap AS (
    -- Defines the valid fiscal year ranges
    SELECT 'Apr-19' AS start_month, 'Mar-20' AS end_month UNION ALL
    SELECT 'Apr-20', 'Mar-21' UNION ALL
    SELECT 'Apr-21', 'Mar-22' UNION ALL
    SELECT 'Apr-22', 'Mar-23' UNION ALL
    SELECT 'Apr-23', 'Mar-24'
),

SelectedDate AS (
    -- Extract start and end months from selected range
    SELECT 
        start_month, 
        end_month 
    FROM FiscalYearMap
    WHERE ('${inputs.date_filter.value}' = start_month || ' to ' || end_month)
),

PreviousYear AS (
    -- Generate last year's corresponding range for LY calculations
    SELECT 
        start_month,
        end_month,
        LEFT(start_month, 3) || '-' || CAST(CAST(RIGHT(start_month, 2) AS INTEGER) - 1 AS TEXT) AS ly_start_month,
        LEFT(end_month, 3) || '-' || CAST(CAST(RIGHT(end_month, 2) AS INTEGER) - 1 AS TEXT) AS ly_end_month
    FROM SelectedDate
),

UnpivotedActual AS (
    -- Unpivot GGCL_Actual while ensuring correct column-to-value mapping
    SELECT 
        particulars, 
        date_column, 
        SUM(CAST(value AS NUMERIC)) AS CY_Actual  -- Ensure numeric summation
    FROM (
        SELECT 
            a.particulars,
            UNNEST(ARRAY[
                'Apr-19', 'May-19', 'Jun-19', 'Jul-19', 'Aug-19', 'Sep-19', 'Oct-19', 'Nov-19', 'Dec-19',
                'Jan-20', 'Feb-20', 'Mar-20', 'Apr-20', 'May-20', 'Jun-20', 'Jul-20', 'Aug-20', 'Sep-20', 'Oct-20', 'Nov-20', 'Dec-20', 
                'Jan-21', 'Feb-21', 'Mar-21', 'Apr-21', 'May-21', 'Jun-21', 'Jul-21', 'Aug-21', 'Sep-21', 'Oct-21', 'Nov-21', 'Dec-21',
                'Jan-22', 'Feb-22', 'Mar-22', 'Apr-22', 'May-22', 'Jun-22', 'Jul-22', 'Aug-22', 'Sep-22', 'Oct-22', 'Nov-22', 'Dec-22',
                'Jan-23', 'Feb-23', 'Mar-23', 'Apr-23', 'May-23', 'Jun-23', 'Jul-23', 'Aug-23', 'Sep-23', 'Oct-23', 'Nov-23', 'Dec-23',
                'Jan-24', 'Feb-24', 'Mar-24', 'Apr-24', 'May-24', 'Jun-24', 'Jul-24', 'Aug-24', 'Sep-24', 'Oct-24', 'Nov-24', 'Dec-24'
            ]) AS date_column,
            UNNEST(ARRAY[
                a."Apr-19", a."May-19", a."Jun-19", a."Jul-19", a."Aug-19", a."Sep-19", a."Oct-19", a."Nov-19", a."Dec-19",
                a."Jan-20", a."Feb-20", a."Mar-20", a."Apr-20", a."May-20", a."Jun-20", a."Jul-20", a."Aug-20", a."Sep-20", a."Oct-20", a."Nov-20", a."Dec-20", 
                a."Jan-21", a."Feb-21", a."Mar-21", a."Apr-21", a."May-21", a."Jun-21", a."Jul-21", a."Aug-21", a."Sep-21", a."Oct-21", a."Nov-21", a."Dec-21",
                a."Jan-22", a."Feb-22", a."Mar-22", a."Apr-22", a."May-22", a."Jun-22", a."Jul-22", a."Aug-22", a."Sep-22", a."Oct-22", a."Nov-22", a."Dec-22",
                a."Jan-23", a."Feb-23", a."Mar-23", a."Apr-23", a."May-23", a."Jun-23", a."Jul-23", a."Aug-23", a."Sep-23", a."Oct-23", a."Nov-23", a."Dec-23",
                a."Jan-24", a."Feb-24", a."Mar-24", a."Apr-24", a."May-24", a."Jun-24", a."Jul-24", a."Aug-24", a."Sep-24", a."Oct-24", a."Nov-24", a."Dec-24"
            ]) AS value
        FROM GGCL_Actual a
    ) AS CombinedActual
    WHERE date_column >= (SELECT start_month FROM SelectedDate) 
      AND date_column <= (SELECT end_month FROM SelectedDate)  -- Ensuring both Apr & Mar are included
    GROUP BY particulars, date_column
),

UnpivotedAOP AS (
    -- Unpivot GGCL_AOP ensuring alignment with Actuals
    SELECT 
        particulars, 
        date_column, 
        SUM(CAST(value AS NUMERIC)) AS CY_AOP  
    FROM (
        SELECT 
            a.particulars,
            UNNEST(ARRAY[
                'Apr-19', 'May-19', 'Jun-19', 'Jul-19', 'Aug-19', 'Sep-19', 'Oct-19', 'Nov-19', 'Dec-19',
                'Jan-20', 'Feb-20', 'Mar-20', 'Apr-20', 'May-20', 'Jun-20', 'Jul-20', 'Aug-20', 'Sep-20', 'Oct-20', 'Nov-20', 'Dec-20', 
                'Jan-21', 'Feb-21', 'Mar-21', 'Apr-21', 'May-21', 'Jun-21', 'Jul-21', 'Aug-21', 'Sep-21', 'Oct-21', 'Nov-21', 'Dec-21',
                'Jan-22', 'Feb-22', 'Mar-22', 'Apr-22', 'May-22', 'Jun-22', 'Jul-22', 'Aug-22', 'Sep-22', 'Oct-22', 'Nov-22', 'Dec-22',
                'Jan-23', 'Feb-23', 'Mar-23', 'Apr-23', 'May-23', 'Jun-23', 'Jul-23', 'Aug-23', 'Sep-23', 'Oct-23', 'Nov-23', 'Dec-23',
                'Jan-24', 'Feb-24', 'Mar-24', 'Apr-24', 'May-24', 'Jun-24', 'Jul-24', 'Aug-24', 'Sep-24', 'Oct-24', 'Nov-24', 'Dec-24'
            ]) AS date_column,
            UNNEST(ARRAY[
                a."Apr-19", a."May-19", a."Jun-19", a."Jul-19", a."Aug-19", a."Sep-19", a."Oct-19", a."Nov-19", a."Dec-19",
                a."Jan-20", a."Feb-20", a."Mar-20", a."Apr-20", a."May-20", a."Jun-20", a."Jul-20", a."Aug-20", a."Sep-20", a."Oct-20", a."Nov-20", a."Dec-20", 
                a."Jan-21", a."Feb-21", a."Mar-21", a."Apr-21", a."May-21", a."Jun-21", a."Jul-21", a."Aug-21", a."Sep-21", a."Oct-21", a."Nov-21", a."Dec-21",
                a."Jan-22", a."Feb-22", a."Mar-22", a."Apr-22", a."May-22", a."Jun-22", a."Jul-22", a."Aug-22", a."Sep-22", a."Oct-22", a."Nov-22", a."Dec-22",
                a."Jan-23", a."Feb-23", a."Mar-23", a."Apr-23", a."May-23", a."Jun-23", a."Jul-23", a."Aug-23", a."Sep-23", a."Oct-23", a."Nov-23", a."Dec-23",
                a."Jan-24", a."Feb-24", a."Mar-24", a."Apr-24", a."May-24", a."Jun-24", a."Jul-24", a."Aug-24", a."Sep-24", a."Oct-24", a."Nov-24", a."Dec-24"
            ]) AS value
        FROM GGCL_AOP a
    ) AS CombinedAOP
    WHERE date_column >= (SELECT start_month FROM SelectedDate) 
      AND date_column <= (SELECT end_month FROM SelectedDate)
    GROUP BY particulars, date_column
),

LY_Actual AS (
    -- Fetch Last Year Actuals for proper variance calculation
    SELECT 
        ua.particulars,
        SUM(ua.CY_Actual) AS LY_Actual
    FROM UnpivotedActual ua
    JOIN PreviousYear py 
    ON ua.date_column >= py.ly_start_month 
   AND ua.date_column <= py.ly_end_month
    GROUP BY ua.particulars
)

SELECT 
    ua.particulars,
    SUM(ua.CY_Actual) AS "CY Actual",
    SUM(COALESCE(uAOP.CY_AOP, 0)) AS "CY AOP",
    SUM(COALESCE(ly.LY_Actual, 0)) AS "LY Actual",
    SUM(ua.CY_Actual - COALESCE(uAOP.CY_AOP, 0)) AS "Variance vs AOP",
    SUM(ua.CY_Actual - COALESCE(ly.LY_Actual, 0)) AS "Variance vs LY"
FROM UnpivotedActual ua
LEFT JOIN UnpivotedAOP uAOP 
    ON ua.particulars = uAOP.particulars 
    AND ua.date_column = uAOP.date_column
LEFT JOIN LY_Actual ly 
    ON ua.particulars = ly.particulars
WHERE ua.date_column >= (SELECT start_month FROM SelectedDate) 
  AND ua.date_column <= (SELECT end_month FROM SelectedDate)  
GROUP BY ua.particulars;
```