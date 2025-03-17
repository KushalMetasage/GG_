<Grid cols = 2>

<div style="position: relative; margin-bottom: 40px;">  
    <h1 style="font-weight: bold; font-size: 30px; margin: 0;">🏭 Plant Utilization</h1>
</div>


<div>
<Dropdown data={date_filter} name=date_filter value=date_filter title="Date" defaultValue="Jan-25">
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

<div class="mt-10">
<DataTable data={plant_1} rows=20 groupBy=plant groupType=section rowshadowing={true} headerFontColor=Bold headerColor=#FFD700>
    <!-- Define Dynamic colGroup -->
    <Column id="category" totalAgg=sum fmt='0' totalFmt='0' colGroup="{inputs.matric == 'GGE' ? 'Duna' : 'OBL'}"/>
    <Column id="CY Actual" totalAgg=sum fmt='0.00' totalFmt='0' colGroup="{inputs.matric == 'GGE' ? 'Duna' : 'OBL'}"/>
    <Column id="CY Aop" totalAgg=sum fmt='0.00' totalFmt='0' colGroup="{inputs.matric == 'GGE' ? 'Duna' : 'OBL'}"/>
    <Column id="LY Actual" totalAgg=sum fmt='0.00' totalFmt='0' colGroup="{inputs.matric == 'GGE' ? 'Duna' : 'OBL'}"/>
    <Column id="ACT vs AOP %" totalAgg="weightedMean" fmt='0.00"%"' contentType=delta colGroup="{inputs.matric == 'GGE' ? 'Duna' : 'OBL'}"/>
    <Column id="Growth vs LY %" totalAgg="weightedMean" fmt='0.00"%"' contentType=delta colGroup="{inputs.matric == 'GGE' ? 'Duna' : 'OBL'}"/>
</DataTable>
</div>

<div class="mt-25">
<DataTable data={plant_2} rows=20 rowshadowing={true} headerFontColor=Bold headerColor=#FFD700>
    <Column id="category" totalAgg=sum fmt='0' totalFmt='0' colGroup="{inputs.matric == 'GGE' ? 'Puszta' : 'VKP'}"/>
    <Column id="CY Actual" totalAgg=sum fmt='0.00' totalFmt='0' colGroup="{inputs.matric == 'GGE' ? 'Puszta' : 'VKP'}"/>
    <Column id="CY Aop" totalAgg=sum fmt='0.00' totalFmt='0' colGroup="{inputs.matric == 'GGE' ? 'Puszta' : 'VKP'}"/>
    <Column id="LY Actual" totalAgg=sum fmt='0.00' totalFmt='0' colGroup="{inputs.matric == 'GGE' ? 'Puszta' : 'VKP'}"/>
    <Column id="ACT vs AOP %" totalAgg="weightedMean" fmt='0.00"%"' contentType=delta colGroup="{inputs.matric == 'GGE' ? 'Puszta' : 'VKP'}"/>
    <Column id="Growth vs LY %" totalAgg="weightedMean" fmt='0.00"%"' contentType=delta colGroup="{inputs.matric == 'GGE' ? 'Puszta' : 'VKP'}"/>
</DataTable>
</div>



```sql date_filter
SELECT DISTINCT
    month AS date_filter
FROM GGE_plantutilization;
```

```sql plant_1
SELECT 
    plant,
    category,
    CAST(NULLIF(TRIM(cyactual), '') AS DOUBLE) AS "CY Actual",
    CAST(NULLIF(TRIM(cyaop), '') AS DOUBLE) AS "CY Aop",
    CAST(NULLIF(TRIM(lyactual), '') AS DOUBLE) AS "LY Actual",
    ROUND((CAST(NULLIF(TRIM(cyactual), '') AS DOUBLE) / NULLIF(CAST(NULLIF(TRIM(cyaop), '') AS DOUBLE), 0)) * 100, 2) AS "ACT vs AOP %",
    ROUND(((CAST(NULLIF(TRIM(cyactual), '') AS DOUBLE) - CAST(NULLIF(TRIM(lyactual), '') AS DOUBLE)) / NULLIF(CAST(NULLIF(TRIM(lyactual), '') AS DOUBLE), 0)) * 100, 2) AS "Growth vs LY %"
FROM "${inputs.matric}_plantutilization"
WHERE 
    month = '${inputs.date_filter.value}'
AND (
    ('${inputs.matric}' = 'GGE' AND plant = 'Puszta') 
    OR ('${inputs.matric}' = 'GGCL' AND plant = 'OBL')
);


```

```sql plant_2
SELECT 
    plant,
    category,
    CAST(NULLIF(TRIM(cyactual), '') AS DOUBLE) AS "CY Actual",
    CAST(NULLIF(TRIM(cyaop), '') AS DOUBLE) AS "CY Aop",
    CAST(NULLIF(TRIM(lyactual), '') AS DOUBLE) AS "LY Actual",
    ROUND((CAST(NULLIF(TRIM(cyactual), '') AS DOUBLE) / NULLIF(CAST(NULLIF(TRIM(cyaop), '') AS DOUBLE), 0)) * 100, 2) AS "ACT vs AOP %",
    ROUND(((CAST(NULLIF(TRIM(cyactual), '') AS DOUBLE) - CAST(NULLIF(TRIM(lyactual), '') AS DOUBLE)) / NULLIF(CAST(NULLIF(TRIM(lyactual), '') AS DOUBLE), 0)) * 100, 2) AS "Growth vs LY %"
FROM "${inputs.matric}_plantutilization"
WHERE  
    month = '${inputs.date_filter.value}'
AND (
    ('${inputs.matric}' = 'GGE' AND plant = 'Duna') 
    OR ('${inputs.matric}' = 'GGCL' AND plant = 'VKP')
);
``` 

```sql max_date
SELECT STRFTIME(MAX(STRPTIME(month, '%b-%y')), '%b-%y') AS max_date
FROM GGCL_plantutilization;
```




