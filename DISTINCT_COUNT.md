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

• We can use a CASE statement within our COUNT() aggregate so that we only count user_ids whose ab_group is equal to ‘control’ AND VARIANT:

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
<img src='https://github.com/user-attachments/assets/752fe327-0606-4c58-a27e-7989e9f5a232' width=220>

#### LEFT JOIN

```sql
 SELECT * 
 from browse b 
 LEFT JOIN checkout c 
   ON b.user_id = c.user_id
 LEFT JOIN purchase p
   ON p.user_id = c.user_id
 LIMIT 50;
```
<img src="https://github.com/user-attachments/assets/73609144-a1a6-4bc1-a770-c6bfcb378f59" width=220 />
 
* Instead of selecting all columns using *, let’s select these four:
    - DISTINCT b.browse_date
    - b.user_id
    - c.user_id IS NOT NULL AS 'is_checkout'
    - p.user_id IS NOT NULL AS 'is_purchase' 

```SQL
SELECT DISTINCT b.browse_date,
	b.user_id,
  c.user_id IS NOT NULL as is_checkout,
  p.user_id IS NOT NULL as is_purchase
FROM browse b
LEFT JOIN checkout c
	ON c.user_id = b.user_id
LEFT JOIN purchase p
	ON p.user_id = c.user_id
LIMIT 50;
```
<img src="https://github.com/user-attachments/assets/8f6e2469-f304-447f-a8dd-e9de49458813" width=220 />


```sql
WITH funnels AS (
  SELECT DISTINCT b.browse_date,
     b.user_id,
     c.user_id IS NOT NULL AS 'is_checkout',
     p.user_id IS NOT NULL AS 'is_purchase'
  FROM browse AS 'b'
  LEFT JOIN checkout AS 'c'
    ON c.user_id = b.user_id
  LEFT JOIN purchase AS 'p'
    ON p.user_id = c.user_id)
SELECT COUNT(*) AS 'num_browse',
       SUM(is_checkout) AS 'num_checkout',
       SUM(is_purchase) AS 'num_purchase'
FROM funnels;
```

<img src="https://github.com/user-attachments/assets/f95bbe08-134a-42be-af3f-c0a1b0d35e9d" width=220>




