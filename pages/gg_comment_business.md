<div style="position: relative; margin-bottom: 40px;">  
    <h1 style="font-weight: bold; font-size: 30px; margin: 0;">📈 Business Comments</h1>
</div>
<Details title='Jan’24 - Nov’24'>
    {#each comm as comment}
        <p>
            {@html comment.gg_main_page
                .replace(/(GG India:)/g, "<br><br><span style='font-size: 25px; font-weight: bold; color: #008335;'>$1</span><br>")
                .replace(/(GG Europe:)/g, "<br><br><span style='font-size: 25px; font-weight: bold; color: #008335;'>$1</span><br>")
                .replace("EBITDA", "<span style='font-weight: bold; font-size: 15px; color: #FF4500; background-color: #222; padding: 2px 5px; border-radius: 3px;'>EBITDA</span>")
                .replace("EBT", "<span style='font-weight: bold; font-size: 15px; color: #FF4500; background-color: #222; padding: 2px 5px; border-radius: 3px;'>EBT</span>")
                .replace(/(GG India:.*?)((?=<br><br><span style='font-size: 15px;'|$))/gs, match =>
                    match.replace(/EBITDA/g, "<span style='font-weight: bold; font-size: 15px; color: #FF4500; background-color: #222; padding: 2px 5px; border-radius: 3px;'>EBITDA</span>")
                         .replace(/EBT/g, "<span style='font-weight: bold; font-size: 15px; color: #FF4500; background-color: #222; padding: 2px 5px; border-radius: 3px;'>EBT</span>")
                )
                .replace(/(GG Europe:.*?)((?=<br><br><span style='font-size: 25px;'|$))/gs, match =>
                    match.replace(/EBITDA/g, "<span style='font-weight: bold; font-size: 15px; color: #FF4500; background-color: #222; padding: 2px 5px; border-radius: 3px;'>EBITDA</span>")
                         .replace(/EBT/g, "<span style='font-weight: bold; font-size: 15px; color: #FF4500; background-color: #222; padding: 2px 5px; border-radius: 3px;'>EBT</span>")
                )
            }
        </p>
    {/each}
</Details>



```sql comm
SELECT DISTINCT gg_main_page
FROM Supabase.comment_business;
```