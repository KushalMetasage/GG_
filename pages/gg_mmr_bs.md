
<div style="position: relative; margin-bottom: 40px;">  
    <h1 style="font-weight: bold; font-size: 30px; margin: 0;">📊 Balance Sheet</h1>
</div>

<center>
<Dropdown data={date_filter} name=date_filter value=date_filter title="Date" defaultValue="Apr-21">
    <DropdownOption value="Apr-21" valueLabel="Apr-21" />
</Dropdown>

<Dropdown data={date_filter_1} name=date_filter_1 value=date_filter_1 title="Date" defaultValue="Apr-21">
    <DropdownOption value="Apr-21" valueLabel="Apr-21" />
</Dropdown>


</center>

<ButtonGroup name="matric" display="tabs">
    <ButtonGroupItem valueLabel="Global Green India" value="GGCL" default />
    <ButtonGroupItem valueLabel="Global Green Europe" value="GGE" />
</ButtonGroup>

<div class="bg-gray-800 text-white p-6 shadow-lg rounded-lg mb-10">
    <!-- Display Comments Dynamically -->
    <Details title='Balance Sheet Commentary' open = true>
        {#each BS_comm as comment}
            {#if inputs.matric === "GGE"}  <!-- Match with ButtonGroupItem value -->
                <p class="text-gray-300 text-lg">{comment.global_green_europe}</p>
            {:else}
                <p class="text-gray-300 text-lg">{comment.global_green_india}</p>
            {/if}
        {/each}
    </Details>
</div>

<DataTable 
    data={subcategories} 
    groupBy="subcategory" 
    subtotals=true 
    totalRow=true
    groupsOpen=false
    totalLabel="Total"
    rowshadowing={true}
    headerFontColor=Bold
    headerColor=#FFD700
    totalRowColor=#FFCBA4
    title = "Values are in Million USD">

   <Column id="subcategory" 
        name="Sub-Category"  
        totalFmt="Total" 
        totalAgg="Total"
        subtotalFmt='@value' />
         

    
    <Column id="particulars" 
            name="Particulars" 
            totalFmt='0 "Line Items"'/>

    
     <Column 
        id="Selected_Date_1"
        name="${inputs.date_filter.value}"
        fmt="$0.00" 
        totalAgg="sum" 
        subtotalAgg="sum"
        title = '{inputs.date_filter.value}'
    />
            
   <Column 
        id="Selected_Date_2"
        name="${inputs.date_filter_1.value}" 
        fmt="$0.00" 
        totalAgg="sum" 
        subtotalAgg="sum"
        title = '{inputs.date_filter_1.value}'
    />

     <Column 
        id="Variance" 
        name={"Variance (${inputs.date_filter_1.value} vs ${inputs.date_filter.value})"} 
        fmt="$0.00" 
        totalAgg="sum" 
        subtotalAgg="sum"
        title="Variance"
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
WHERE table_name = 'GGE_BS'  
AND column_name NOT IN ('sno', 'category', 'subcategory', 'no', 'particulars')
ORDER BY column_name;
```

```sql date_filter_1
SELECT 
    UPPER(LEFT(column_name, 1)) || LOWER(SUBSTRING(column_name FROM 2)) AS date_filter_1
FROM information_schema.columns
WHERE table_name = 'GGE_BS'  
AND column_name NOT IN ('sno', 'category', 'subcategory', 'no', 'particulars')
ORDER BY column_name;
```


```sql BS_comm
SELECT DISTINCT global_green_india, global_green_europe 
FROM Supabase.GG_BS_comment 
```


