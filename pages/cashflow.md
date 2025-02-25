<div style="position: relative; margin-bottom: 40px;">  
    <h1 style="font-weight: bold; font-size: 30px; margin: 0;">💰 Cash Flow Analysis</h1>
</div>

<DataTable data={cashflow} rowshadowing={true} rows=25 > 
    <Column id="company" title="Company Name"/> 
    <Column id="particular" wrap = true  title="Metric"/> 
    <Column id="Dec 2023 Cashflow" align = center title="Dec 2023 Cashflow" fmt="usd" /> 
    <Column id="Dec 2024 Cashflow" align = center title="Dec 2024 Cashflow" fmt="usd"/> 
    <Column id="Net Change" contentType=delta align = center title="Net Change" fmt="usd"/> 
</DataTable>



```sql cashflow
SELECT 
    company,
    particular,
    dec23 AS "Dec 2023 Cashflow",
    dec24 AS "Dec 2024 Cashflow",
    dec24 - dec23 AS "Net Change"
FROM Supabase.GG_cashflow;
```






