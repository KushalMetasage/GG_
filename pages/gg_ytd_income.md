<Grid cols = 2>

<div style="position: relative; margin-bottom: 40px;">  
    <h1 style="font-weight: bold; font-size: 30px; margin: 0;">🏦 YTD Income Statement</h1>
</div>

<div>
<Dropdown data={date_filter} name=date_filter value=date_filter title="Fiscal Year" defaultValue="Apr-23 to Mar-24">
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
    <p class="text-[14px] font-small text-white ml-auto">
        📅 Last Updated: <Value data={max_date} />
    </p>
</div>

<DataTable data={ytd} rows = 20 rowshadowing={true} headerFontColor=Bold headerColor=#FFD700 title = "Values are in Million USD">
<Column id= "fiscal_year" fmt=0/>
<Column id= "metric_name" fmt=0/>
<Column id= "CY Actual" fmt='0.00'/>
<Column id= "YTD AOP" fmt='0.00'/>
<Column id= "LY Actual YTD" fmt='0.00'/>
<Column id= "Variance vs AOP YTD" fmt='0.00'/>
<Column id= "Variance vs LY YTD" fmt='0.00'/>
</DataTable>

```sql date_filter
    WITH FiscalYears AS (
        SELECT DISTINCT 
            'Apr-' || RIGHT(column_name, 2) || ' to Mar-' || LPAD(CAST(RIGHT(column_name, 2)::INTEGER + 1 AS TEXT), 2, '0') AS date_filter
        FROM information_schema.columns
        WHERE table_name = 'GGCL_Actual'  
        AND column_name LIKE 'apr-%'  
        AND RIGHT(column_name, 2)::INTEGER < 24  -- Ensures only valid fiscal years
    )
    SELECT date_filter FROM FiscalYears ORDER BY date_filter;
```

```sql ytd
SELECT 
  'FY 2019-20' AS fiscal_year,
  A.particulars AS metric_name,
  COALESCE(A."apr-19", 0) + COALESCE(A."may-19", 0) + COALESCE(A."jun-19", 0) + 
  COALESCE(A."jul-19", 0) + COALESCE(A."aug-19", 0) + COALESCE(A."sep-19", 0) + 
  COALESCE(A."oct-19", 0) + COALESCE(A."nov-19", 0) + COALESCE(A."dec-19", 0) + 
  COALESCE(A."jan-20", 0) + COALESCE(A."feb-20", 0) + COALESCE(A."mar-20", 0) AS "CY Actual",
  COALESCE(B."apr-19", 0) + COALESCE(B."may-19", 0) + COALESCE(B."jun-19", 0) + 
  COALESCE(B."jul-19", 0) + COALESCE(B."aug-19", 0) + COALESCE(B."sep-19", 0) + 
  COALESCE(B."oct-19", 0) + COALESCE(B."nov-19", 0) + COALESCE(B."dec-19", 0) + 
  COALESCE(B."jan-20", 0) + COALESCE(B."feb-20", 0) + COALESCE(B."mar-20", 0) AS "YTD AOP",
  0 AS "LY Actual YTD",  -- First year has no LY Actual YTD
  COALESCE(A."apr-19", 0) + COALESCE(A."may-19", 0) + COALESCE(A."jun-19", 0) + 
  COALESCE(A."jul-19", 0) + COALESCE(A."aug-19", 0) + COALESCE(A."sep-19", 0) + 
  COALESCE(A."oct-19", 0) + COALESCE(A."nov-19", 0) + COALESCE(A."dec-19", 0) + 
  COALESCE(A."jan-20", 0) + COALESCE(A."feb-20", 0) + COALESCE(A."mar-20", 0) -
  (COALESCE(B."apr-19", 0) + COALESCE(B."may-19", 0) + COALESCE(B."jun-19", 0) + 
   COALESCE(B."jul-19", 0) + COALESCE(B."aug-19", 0) + COALESCE(B."sep-19", 0) + 
   COALESCE(B."oct-19", 0) + COALESCE(B."nov-19", 0) + COALESCE(B."dec-19", 0) + 
   COALESCE(B."jan-20", 0) + COALESCE(B."feb-20", 0) + COALESCE(B."mar-20", 0)) AS "Variance vs AOP YTD",
   0 AS "Variance vs LY YTD"  -- First year has no previous LY Actual YTD
FROM '${inputs.matric}_Actual' A
JOIN '${inputs.matric}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter.value}' = 'Apr-19 to Mar-20'


UNION ALL

SELECT 
  'FY 2020-21' AS fiscal_year,
  A.particulars AS metric_name,
  
  -- CY Actual YTD
  COALESCE(A."apr-20", 0) + COALESCE(A."may-20", 0) + COALESCE(A."jun-20", 0) + 
  COALESCE(A."jul-20", 0) + COALESCE(A."aug-20", 0) + COALESCE(A."sep-20", 0) + 
  COALESCE(A."oct-20", 0) + COALESCE(A."nov-20", 0) + COALESCE(A."dec-20", 0) + 
  COALESCE(A."jan-21", 0) + COALESCE(A."feb-21", 0) + COALESCE(A."mar-21", 0) AS "CY Actual",

  -- YTD AOP
  COALESCE(B."apr-20", 0) + COALESCE(B."may-20", 0) + COALESCE(B."jun-20", 0) + 
  COALESCE(B."jul-20", 0) + COALESCE(B."aug-20", 0) + COALESCE(B."sep-20", 0) + 
  COALESCE(B."oct-20", 0) + COALESCE(B."nov-20", 0) + COALESCE(B."dec-20", 0) + 
  COALESCE(B."jan-21", 0) + COALESCE(B."feb-21", 0) + COALESCE(B."mar-21", 0) AS "YTD AOP",

  -- LY Actual YTD (Previous Year CY Actual)
  (SELECT 
     COALESCE(A_prev."apr-19", 0) + COALESCE(A_prev."may-19", 0) + COALESCE(A_prev."jun-19", 0) + 
     COALESCE(A_prev."jul-19", 0) + COALESCE(A_prev."aug-19", 0) + COALESCE(A_prev."sep-19", 0) + 
     COALESCE(A_prev."oct-19", 0) + COALESCE(A_prev."nov-19", 0) + COALESCE(A_prev."dec-19", 0) + 
     COALESCE(A_prev."jan-20", 0) + COALESCE(A_prev."feb-20", 0) + COALESCE(A_prev."mar-20", 0)
   FROM '${inputs.matric}_Actual' A_prev
   WHERE A_prev.particulars = A.particulars) AS "LY Actual YTD",

  -- Variance vs AOP YTD (CY Actual - YTD AOP)
  (COALESCE(A."apr-20", 0) + COALESCE(A."may-20", 0) + COALESCE(A."jun-20", 0) + 
   COALESCE(A."jul-20", 0) + COALESCE(A."aug-20", 0) + COALESCE(A."sep-20", 0) + 
   COALESCE(A."oct-20", 0) + COALESCE(A."nov-20", 0) + COALESCE(A."dec-20", 0) + 
   COALESCE(A."jan-21", 0) + COALESCE(A."feb-21", 0) + COALESCE(A."mar-21", 0)) -
  (COALESCE(B."apr-20", 0) + COALESCE(B."may-20", 0) + COALESCE(B."jun-20", 0) + 
   COALESCE(B."jul-20", 0) + COALESCE(B."aug-20", 0) + COALESCE(B."sep-20", 0) + 
   COALESCE(B."oct-20", 0) + COALESCE(B."nov-20", 0) + COALESCE(B."dec-20", 0) + 
   COALESCE(B."jan-21", 0) + COALESCE(B."feb-21", 0) + COALESCE(B."mar-21", 0)) AS "Variance vs AOP YTD",

  -- Variance vs LY YTD (CY Actual - LY Actual YTD)
  (COALESCE(A."apr-20", 0) + COALESCE(A."may-20", 0) + COALESCE(A."jun-20", 0) + 
   COALESCE(A."jul-20", 0) + COALESCE(A."aug-20", 0) + COALESCE(A."sep-20", 0) + 
   COALESCE(A."oct-20", 0) + COALESCE(A."nov-20", 0) + COALESCE(A."dec-20", 0) + 
   COALESCE(A."jan-21", 0) + COALESCE(A."feb-21", 0) + COALESCE(A."mar-21", 0)) -
  (SELECT 
     COALESCE(A_prev."apr-19", 0) + COALESCE(A_prev."may-19", 0) + COALESCE(A_prev."jun-19", 0) + 
     COALESCE(A_prev."jul-19", 0) + COALESCE(A_prev."aug-19", 0) + COALESCE(A_prev."sep-19", 0) + 
     COALESCE(A_prev."oct-19", 0) + COALESCE(A_prev."nov-19", 0) + COALESCE(A_prev."dec-19", 0) + 
     COALESCE(A_prev."jan-20", 0) + COALESCE(A_prev."feb-20", 0) + COALESCE(A_prev."mar-20", 0)
   FROM '${inputs.matric}_Actual' A_prev
   WHERE A_prev.particulars = A.particulars) AS "Variance vs LY YTD"

FROM '${inputs.matric}_Actual' A
JOIN '${inputs.matric}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter.value}' = 'Apr-20 to Mar-21'



UNION ALL

SELECT 
  'FY 2021-22' AS fiscal_year,
  A.particulars AS metric_name,
  
  -- CY Actual Calculation
  COALESCE(A."apr-21", 0) + COALESCE(A."may-21", 0) + COALESCE(A."jun-21", 0) + 
  COALESCE(A."jul-21", 0) + COALESCE(A."aug-21", 0) + COALESCE(A."sep-21", 0) + 
  COALESCE(A."oct-21", 0) + COALESCE(A."nov-21", 0) + COALESCE(A."dec-21", 0) + 
  COALESCE(A."jan-22", 0) + COALESCE(A."feb-22", 0) + COALESCE(A."mar-22", 0) AS "CY Actual",
  
  -- YTD AOP Calculation
  COALESCE(B."apr-21", 0) + COALESCE(B."may-21", 0) + COALESCE(B."jun-21", 0) + 
  COALESCE(B."jul-21", 0) + COALESCE(B."aug-21", 0) + COALESCE(B."sep-21", 0) + 
  COALESCE(B."oct-21", 0) + COALESCE(B."nov-21", 0) + COALESCE(B."dec-21", 0) + 
  COALESCE(B."jan-22", 0) + COALESCE(B."feb-22", 0) + COALESCE(B."mar-22", 0) AS "YTD AOP",
  
  -- LY Actual YTD (Previous Year)
  (SELECT 
     COALESCE(A_prev."apr-20", 0) + COALESCE(A_prev."may-20", 0) + COALESCE(A_prev."jun-20", 0) + 
     COALESCE(A_prev."jul-20", 0) + COALESCE(A_prev."aug-20", 0) + COALESCE(A_prev."sep-20", 0) + 
     COALESCE(A_prev."oct-20", 0) + COALESCE(A_prev."nov-20", 0) + COALESCE(A_prev."dec-20", 0) + 
     COALESCE(A_prev."jan-21", 0) + COALESCE(A_prev."feb-21", 0) + COALESCE(A_prev."mar-21", 0)
   FROM GGCL_Actual A_prev
   WHERE A_prev.particulars = A.particulars) AS "LY Actual YTD",
  
  -- Variance vs AOP YTD (CY Actual - YTD AOP)
  (COALESCE(A."apr-21", 0) + COALESCE(A."may-21", 0) + COALESCE(A."jun-21", 0) + 
  COALESCE(A."jul-21", 0) + COALESCE(A."aug-21", 0) + COALESCE(A."sep-21", 0) + 
  COALESCE(A."oct-21", 0) + COALESCE(A."nov-21", 0) + COALESCE(A."dec-21", 0) + 
  COALESCE(A."jan-22", 0) + COALESCE(A."feb-22", 0) + COALESCE(A."mar-22", 0)) - 
  (COALESCE(B."apr-21", 0) + COALESCE(B."may-21", 0) + COALESCE(B."jun-21", 0) + 
  COALESCE(B."jul-21", 0) + COALESCE(B."aug-21", 0) + COALESCE(B."sep-21", 0) + 
  COALESCE(B."oct-21", 0) + COALESCE(B."nov-21", 0) + COALESCE(B."dec-21", 0) + 
  COALESCE(B."jan-22", 0) + COALESCE(B."feb-22", 0) + COALESCE(B."mar-22", 0)) AS "Variance vs AOP YTD",
  
  -- Variance vs LY YTD (CY Actual - LY Actual YTD)
  (COALESCE(A."apr-21", 0) + COALESCE(A."may-21", 0) + COALESCE(A."jun-21", 0) + 
  COALESCE(A."jul-21", 0) + COALESCE(A."aug-21", 0) + COALESCE(A."sep-21", 0) + 
  COALESCE(A."oct-21", 0) + COALESCE(A."nov-21", 0) + COALESCE(A."dec-21", 0) + 
  COALESCE(A."jan-22", 0) + COALESCE(A."feb-22", 0) + COALESCE(A."mar-22", 0)) - 
  (SELECT 
     COALESCE(A_prev."apr-20", 0) + COALESCE(A_prev."may-20", 0) + COALESCE(A_prev."jun-20", 0) + 
     COALESCE(A_prev."jul-20", 0) + COALESCE(A_prev."aug-20", 0) + COALESCE(A_prev."sep-20", 0) + 
     COALESCE(A_prev."oct-20", 0) + COALESCE(A_prev."nov-20", 0) + COALESCE(A_prev."dec-20", 0) + 
     COALESCE(A_prev."jan-21", 0) + COALESCE(A_prev."feb-21", 0) + COALESCE(A_prev."mar-21", 0)
   FROM '${inputs.matric}_Actual' A_prev
   WHERE A_prev.particulars = A.particulars) AS "Variance vs LY YTD"

FROM '${inputs.matric}_Actual' A
JOIN '${inputs.matric}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter.value}' = 'Apr-21 to Mar-22'



UNION ALL

SELECT 
  'FY 2022-23' AS fiscal_year,
  A.particulars AS metric_name,
  -- CY Actual YTD Calculation
  COALESCE(A."apr-22", 0) + COALESCE(A."may-22", 0) + COALESCE(A."jun-22", 0) + 
  COALESCE(A."jul-22", 0) + COALESCE(A."aug-22", 0) + COALESCE(A."sep-22", 0) + 
  COALESCE(A."oct-22", 0) + COALESCE(A."nov-22", 0) + COALESCE(A."dec-22", 0) + 
  COALESCE(A."jan-23", 0) + COALESCE(A."feb-23", 0) + COALESCE(A."mar-23", 0) AS "CY Actual",

  -- YTD AOP Calculation
  COALESCE(B."apr-22", 0) + COALESCE(B."may-22", 0) + COALESCE(B."jun-22", 0) + 
  COALESCE(B."jul-22", 0) + COALESCE(B."aug-22", 0) + COALESCE(B."sep-22", 0) + 
  COALESCE(B."oct-22", 0) + COALESCE(B."nov-22", 0) + COALESCE(B."dec-22", 0) + 
  COALESCE(B."jan-23", 0) + COALESCE(B."feb-23", 0) + COALESCE(B."mar-23", 0) AS "YTD AOP",

  -- LY Actual YTD Calculation
  (SELECT 
     COALESCE(A_prev."apr-21", 0) + COALESCE(A_prev."may-21", 0) + COALESCE(A_prev."jun-21", 0) + 
     COALESCE(A_prev."jul-21", 0) + COALESCE(A_prev."aug-21", 0) + COALESCE(A_prev."sep-21", 0) + 
     COALESCE(A_prev."oct-21", 0) + COALESCE(A_prev."nov-21", 0) + COALESCE(A_prev."dec-21", 0) + 
     COALESCE(A_prev."jan-22", 0) + COALESCE(A_prev."feb-22", 0) + COALESCE(A_prev."mar-22", 0)
   FROM '${inputs.matric}_Actual' A_prev
   WHERE A_prev.particulars = A.particulars) AS "LY Actual YTD",

  -- Variance vs AOP YTD Calculation
  (COALESCE(A."apr-22", 0) + COALESCE(A."may-22", 0) + COALESCE(A."jun-22", 0) + 
   COALESCE(A."jul-22", 0) + COALESCE(A."aug-22", 0) + COALESCE(A."sep-22", 0) + 
   COALESCE(A."oct-22", 0) + COALESCE(A."nov-22", 0) + COALESCE(A."dec-22", 0) + 
   COALESCE(A."jan-23", 0) + COALESCE(A."feb-23", 0) + COALESCE(A."mar-23", 0))
   - 
   (COALESCE(B."apr-22", 0) + COALESCE(B."may-22", 0) + COALESCE(B."jun-22", 0) + 
    COALESCE(B."jul-22", 0) + COALESCE(B."aug-22", 0) + COALESCE(B."sep-22", 0) + 
    COALESCE(B."oct-22", 0) + COALESCE(B."nov-22", 0) + COALESCE(B."dec-22", 0) + 
    COALESCE(B."jan-23", 0) + COALESCE(B."feb-23", 0) + COALESCE(B."mar-23", 0))
   AS "Variance vs AOP YTD",

  -- Variance vs LY YTD Calculation
  (COALESCE(A."apr-22", 0) + COALESCE(A."may-22", 0) + COALESCE(A."jun-22", 0) + 
   COALESCE(A."jul-22", 0) + COALESCE(A."aug-22", 0) + COALESCE(A."sep-22", 0) + 
   COALESCE(A."oct-22", 0) + COALESCE(A."nov-22", 0) + COALESCE(A."dec-22", 0) + 
   COALESCE(A."jan-23", 0) + COALESCE(A."feb-23", 0) + COALESCE(A."mar-23", 0))
   - 
  (SELECT 
     COALESCE(A_prev."apr-21", 0) + COALESCE(A_prev."may-21", 0) + COALESCE(A_prev."jun-21", 0) + 
     COALESCE(A_prev."jul-21", 0) + COALESCE(A_prev."aug-21", 0) + COALESCE(A_prev."sep-21", 0) + 
     COALESCE(A_prev."oct-21", 0) + COALESCE(A_prev."nov-21", 0) + COALESCE(A_prev."dec-21", 0) + 
     COALESCE(A_prev."jan-22", 0) + COALESCE(A_prev."feb-22", 0) + COALESCE(A_prev."mar-22", 0)
   FROM '${inputs.matric}_Actual' A_prev
   WHERE A_prev.particulars = A.particulars) AS "Variance vs LY YTD"

FROM '${inputs.matric}_Actual' A
JOIN '${inputs.matric}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter.value}' = 'Apr-22 to Mar-23'




UNION ALL

SELECT 
  'FY 2023-24' AS fiscal_year,
  A.particulars AS metric_name,
  -- CY Actual
  COALESCE(A."apr-23", 0) + COALESCE(A."may-23", 0) + COALESCE(A."jun-23", 0) + 
  COALESCE(A."jul-23", 0) + COALESCE(A."aug-23", 0) + COALESCE(A."sep-23", 0) + 
  COALESCE(A."oct-23", 0) + COALESCE(A."nov-23", 0) + COALESCE(A."dec-23", 0) + 
  COALESCE(A."jan-24", 0) + COALESCE(A."feb-24", 0) + COALESCE(A."mar-24", 0) AS "CY Actual",

  -- YTD AOP
  COALESCE(B."apr-23", 0) + COALESCE(B."may-23", 0) + COALESCE(B."jun-23", 0) + 
  COALESCE(B."jul-23", 0) + COALESCE(B."aug-23", 0) + COALESCE(B."sep-23", 0) + 
  COALESCE(B."oct-23", 0) + COALESCE(B."nov-23", 0) + COALESCE(B."dec-23", 0) + 
  COALESCE(B."jan-24", 0) + COALESCE(B."feb-24", 0) + COALESCE(B."mar-24", 0) AS "YTD AOP",

  -- LY Actual YTD (Previous Year Data)
  (SELECT 
     COALESCE(A_prev."apr-22", 0) + COALESCE(A_prev."may-22", 0) + COALESCE(A_prev."jun-22", 0) + 
     COALESCE(A_prev."jul-22", 0) + COALESCE(A_prev."aug-22", 0) + COALESCE(A_prev."sep-22", 0) + 
     COALESCE(A_prev."oct-22", 0) + COALESCE(A_prev."nov-22", 0) + COALESCE(A_prev."dec-22", 0) + 
     COALESCE(A_prev."jan-23", 0) + COALESCE(A_prev."feb-23", 0) + COALESCE(A_prev."mar-23", 0)
   FROM GGCL_Actual A_prev
   WHERE A_prev.particulars = A.particulars) AS "LY Actual YTD",

  -- Variance vs AOP YTD (CY Actual - YTD AOP)
  (COALESCE(A."apr-23", 0) + COALESCE(A."may-23", 0) + COALESCE(A."jun-23", 0) + 
   COALESCE(A."jul-23", 0) + COALESCE(A."aug-23", 0) + COALESCE(A."sep-23", 0) + 
   COALESCE(A."oct-23", 0) + COALESCE(A."nov-23", 0) + COALESCE(A."dec-23", 0) + 
   COALESCE(A."jan-24", 0) + COALESCE(A."feb-24", 0) + COALESCE(A."mar-24", 0)) -
  (COALESCE(B."apr-23", 0) + COALESCE(B."may-23", 0) + COALESCE(B."jun-23", 0) + 
   COALESCE(B."jul-23", 0) + COALESCE(B."aug-23", 0) + COALESCE(B."sep-23", 0) + 
   COALESCE(B."oct-23", 0) + COALESCE(B."nov-23", 0) + COALESCE(B."dec-23", 0) + 
   COALESCE(B."jan-24", 0) + COALESCE(B."feb-24", 0) + COALESCE(B."mar-24", 0)) AS "Variance vs AOP YTD",

  -- Variance vs LY YTD (CY Actual - LY Actual YTD)
  (COALESCE(A."apr-23", 0) + COALESCE(A."may-23", 0) + COALESCE(A."jun-23", 0) + 
   COALESCE(A."jul-23", 0) + COALESCE(A."aug-23", 0) + COALESCE(A."sep-23", 0) + 
   COALESCE(A."oct-23", 0) + COALESCE(A."nov-23", 0) + COALESCE(A."dec-23", 0) + 
   COALESCE(A."jan-24", 0) + COALESCE(A."feb-24", 0) + COALESCE(A."mar-24", 0)) -
  (SELECT 
     COALESCE(A_prev."apr-22", 0) + COALESCE(A_prev."may-22", 0) + COALESCE(A_prev."jun-22", 0) + 
     COALESCE(A_prev."jul-22", 0) + COALESCE(A_prev."aug-22", 0) + COALESCE(A_prev."sep-22", 0) + 
     COALESCE(A_prev."oct-22", 0) + COALESCE(A_prev."nov-22", 0) + COALESCE(A_prev."dec-22", 0) + 
     COALESCE(A_prev."jan-23", 0) + COALESCE(A_prev."feb-23", 0) + COALESCE(A_prev."mar-23", 0)
   FROM '${inputs.matric}_Actual' A_prev
   WHERE A_prev.particulars = A.particulars) AS "Variance vs LY YTD"

FROM '${inputs.matric}_Actual' A
JOIN '${inputs.matric}_Aop' B USING ("Particulars")
WHERE '${inputs.date_filter.value}' = 'Apr-23 to Mar-24'

```

```sql max_date
SELECT STRFTIME(MAX(STRPTIME(date_filter, '%b-%y')), '%b-%y') AS max_date
FROM (
    SELECT 
        UPPER(LEFT(column_name, 1)) || SUBSTRING(column_name FROM 2) AS date_filter
    FROM information_schema.columns
    WHERE table_name = 'GGCL_Actual'
    AND column_name NOT IN ('category', 'sno', 'no', 'subcategory', 'particulars') -- Exclude non-date columns
) AS subquery;
```

```sql max_date
WITH FiscalYears AS (
    SELECT DISTINCT 
        'Apr-' || RIGHT(column_name, 2) || ' to Mar-' || 
        LPAD(CAST(RIGHT(column_name, 2)::INTEGER + 1 AS TEXT), 2, '0') AS date_filter,
        RIGHT(column_name, 2)::INTEGER AS fiscal_year  -- Extract the numeric year for sorting
    FROM information_schema.columns
    WHERE table_name = 'GGCL_Actual'  
    AND column_name LIKE 'apr-%'  
    AND RIGHT(column_name, 2)::INTEGER < 24  -- Ensures only valid fiscal years
)
SELECT 'Mar-' || LPAD(CAST(MAX(fiscal_year + 1) AS TEXT), 2, '0') AS max_date
FROM FiscalYears;

```