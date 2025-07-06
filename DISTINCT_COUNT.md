#### Survey Result
We could use SQL to calculate the percent change between each question, but it’s just as easy to analyze these manually with a calculator or in a spreadsheet program like Microsoft Excel or Google Sheets.

<img src="https://github.com/user-attachments/assets/da97a487-b893-4bfa-bcd0-31c874a579da" width=220 />

* Count the number of distinct user_id‘s for each value of modal_text. 
  
```sql
SELECT modal_text, 
   COUNT(DISTINCT user_id)
FROM onboarding_modals
GROUP BY modal_text
ORDER BY user_id;
```
<img src="https://github.com/user-attachments/assets/0d1694a4-8e1c-4f78-875d-dccebd6a3252" width=220 />

• We can use a CASE statement within our COUNT() aggregate so that we only count user_ids whose ab_group is equal to ‘control’:

```sql
SELECT modal_text,
  COUNT(DISTINCT CASE
    WHEN ab_group = 'control' THEN user_id
    END) AS 'control_clicks',
    COUNT(DISTINCT CASE
    WHEN ab_group= 'variant' THEN user_id
    END) AS 'variant_clicks'
FROM onboarding_modals
GROUP BY 1
ORDER BY 1;
```
<img src='https://github.com/user-attachments/assets/c058bbac-64d0-480a-b694-bafbb33a06bb' width=220>

 
