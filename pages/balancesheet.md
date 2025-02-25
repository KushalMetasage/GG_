<div style="position: relative; margin-bottom: 40px;">  
    <h1 style="font-weight: bold; font-size: 30px; margin: 0;">Balance Sheet</h1>
</div>

<center>
<Dropdown data={category} name=category value=category title="Category">
    <DropdownOption value="%" valueLabel="All"/>
</Dropdown>

<Dropdown data={date_filter} name=date_filter value=date_filter title="Date">
    <DropdownOption value="%" valueLabel="All"/>
</Dropdown>


</center>



```sql category
  select
      category
  from Supabase.GGCL_BS
  group by category
```

```sql date_filter
SELECT column_name AS date_filter
FROM information_schema.columns
WHERE table_name = 'GGCL_BS'  
AND column_name NOT IN ('category', 'sno', 'no', 'subcategory', 'particulars'); -- Exclude any non-date columns
```


<!-- 
```sql bs
SELECT 
    category
    "${inputs.date.value}" AS selected_date_value
FROM Supabase.GGCL_BS
WHERE 
    ('${inputs.category.value}' = 'All' OR category = '${inputs.category.value}')
    AND "${inputs.date_filter.value}" IS NOT NULL;
```
-->

```sql bs
SELECT
    category,
    date_filter
FROM Supabase.GGCL_BS
GROUP BY category;
SELECT column_name AS date_filter
FROM information_schema.columns
WHERE table_name = 'GGCL_BS'
AND column_name NOT IN ('category', 'sno', 'no', 'subcategory', 'particulars');
SELECT
    category,
    ${inputs.date.value} AS selected_date_value
FROM Supabase.GGCL_BS
WHERE
    (${inputs.category.value} = 'All' OR category = ${inputs.category.value})
    AND ${inputs.date_filter.value} IS NOT NULL;
```
