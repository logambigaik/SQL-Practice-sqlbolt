#### Survey Result
We could use SQL to calculate the percent change between each question, but it’s just as easy to analyze these manually with a calculator or in a spreadsheet program like Microsoft Excel or Google Sheets.

<img src="https://github.com/user-attachments/assets/da97a487-b893-4bfa-bcd0-31c874a579da" width=220 />

```sql
SELECT modal_text, 
   COUNT(DISTINCT user_id)
FROM onboarding_modals
GROUP BY modal_text
ORDER BY user_id;
```
<img src="https://github.com/user-attachments/assets/0d1694a4-8e1c-4f78-875d-dccebd6a3252" width=220 />
