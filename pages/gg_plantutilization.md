<div style="position: relative; margin-bottom: 40px;">  
    <h1 style="font-weight: bold; font-size: 30px; margin: 0;">🏭 PlantUtilization</h1>
</div>

<center>
<Dropdown data={date_filter} name=date_filter value=date_filter title="Date" defaultValue="2023-01-01">
</Dropdown>
</center>

<ButtonGroup name="matric" display="tabs">
    <ButtonGroupItem valueLabel="Global Green India" value="GGCL" default />
    <ButtonGroupItem valueLabel="Global Green Europe" value="GGE" />
</ButtonGroup>

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
    (
        ('${inputs.matric}' = 'GGE' AND month = '${inputs.date_filter.value}') 
        OR ('${inputs.matric}' = 'GGCL' 
            AND CAST(SUBSTR(month, 7, 4) || '-' || SUBSTR(month, 4, 2) || '-' || SUBSTR(month, 1, 2) AS DATE) 
            = CAST('${inputs.date_filter.value}' AS DATE))
    )
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
    (
        ('${inputs.matric}' = 'GGE' AND month = '${inputs.date_filter.value}') 
        OR ('${inputs.matric}' = 'GGCL' 
            AND CAST(SUBSTR(month, 7, 4) || '-' || SUBSTR(month, 4, 2) || '-' || SUBSTR(month, 1, 2) AS DATE) 
            = CAST('${inputs.date_filter.value}' AS DATE))
    )
AND (
    ('${inputs.matric}' = 'GGE' AND plant = 'Duna') 
    OR ('${inputs.matric}' = 'GGCL' AND plant = 'VKP')
);
```





