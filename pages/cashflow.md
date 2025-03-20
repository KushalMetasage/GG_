<div class="relative">  
    <h1 class="text-lg m-0 font-bold">💰 Cash Flow </h1>
</div>


<!-- Toggle Between Global Green India & Europe -->
<ButtonGroup name="matric" display="tabs">
  <ButtonGroupItem valueLabel="Global Green India" value="Global Green India" default />
  <ButtonGroupItem valueLabel="Global Green Europe" value="Global Green Europe" />
</ButtonGroup>

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
        <DataTable data={cashflow} rowshadowing={true} headerFontColor=Bold headerColor=#FFD700 title="Values are in Million USD ($)">
            <Column id="particular" title="Metric"/>
            <Column id="Dec 2023" align="center" title="Dec 2023" fmt="0.00"/>
            <Column id="Dec 2024" align="center" title="Dec 2024" fmt="0.00"/>
            <Column id="Net Change" align="center" title="Net Change" contentType="delta" fmt="0.00"/>
        </DataTable>
    </div>



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






