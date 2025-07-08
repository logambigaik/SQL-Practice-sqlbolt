#### First- and Last-Touch Attribution

Web developers, marketers, and data analysts use attribution data to improve their sources (also called **channels** or **touchpoints**) online. If an ad campaign drives a lot of visits to a site, it's considered effective — those visits are **attributed** to the campaign.

### 🧭 How Is Attribution Captured?

Websites capture this information using **UTM parameters**. These special tags are added to URLs in ads, blog posts, emails, and other traffic sources. When a user clicks one of these links:

- The UTM data is recorded.
- A row is added to a **database** describing the visit.

This helps site owners understand **when** and **how** users found their site.

---

### 📊 Page Visits Table Schema

A typical schema for tracking visits might look like this:

| Column         | Description                                                                 |
|----------------|-----------------------------------------------------------------------------|
| `user_id`      | A unique identifier for each visitor to a page                              |
| `timestamp`    | The exact time the visitor landed on the page                               |
| `page_name`    | The title or section of the page that was visited                           |
| `utm_source`   | Identifies the **touchpoint** (e.g., `google`, `email`, `facebook`)         |
| `utm_medium`   | Identifies the **type of link** (e.g., `cpc`, `email`)                      |
| `utm_campaign` | Identifies the **specific campaign** (e.g., `retargeting-ad`, `newsletter`) |

---

### First Touch Example

Imagine June. She wants to buy a new t-shirt for her mother, who is visiting from out of town. She reads about CoolTShirts.com in a Buzzfeed article, and clicks a link to their landing page. June finds a fabulous Ninja Turtle t-shirt and adds it to her cart. Before she can advance to the checkout page her mom calls, asking for directions. June navigates away from CoolTShirts.com to look up directions.

June’s initial visit is logged in the page_visits table as follows:

## 🧾 June's Initial Visit - Attribution Example

June’s first interaction with **CoolTShirts.com** is recorded in the `page_visits` table as follows:

| user_id | timestamp           | page_name          | utm_source |
|---------|---------------------|---------------------|------------|
| 10069   | 2018-01-02 23:14:01 | 1 - landing_page    | buzzfeed   |
| 10069   | 2018-01-02 23:55:01 | 2 - shopping_cart   | buzzfeed   |

### 🧩 Attribution Breakdown

- **First Touch Source:** `buzzfeed`
- **User ID:** `10069`
- **Initial Page Visit:**  
  - **Page:** Landing Page  
  - **Time:** `2018-01-02 23:14:01`
- **Follow-Up Page Visit:**  
  - **Page:** Shopping Cart  
  - **Time:** `2018-01-02 23:55:01`

June’s **first touch** — her first exposure to the brand — is attributed to **Buzzfeed**, since that was the utm_source recorded when she landed on the site.

* Find June’s rows in the table! Select all columns from the page_visits table, using a WHERE clause with:

  user_id equals 10069 and utm_source equals 'buzzfeed'.

  ```sql
   SELECT *
   FROM page_visits
   WHERE user_id=10069 AND utm_source='buzzfeed';
  ```

<img src="https://github.com/user-attachments/assets/401f9aaa-50a0-43ae-9f27-40082561771f" width=220 />


## 🧾 Last Touch Example – June's Purchase Journey

Two days after her initial visit, CoolTShirts.com runs a targeted **Facebook ad** that appears on June’s timeline. Remembering the Ninja Turtles t-shirt she liked, June clicks the ad and returns to the site.

Her complete `page_visits` now looks like this:

| user_id | timestamp           | page_name        | utm_source |
|---------|---------------------|------------------|------------|
| 10069   | 2018-01-02 23:14:01 | 1 - landing_page | buzzfeed   |
| 10069   | 2018-01-02 23:55:01 | 2 - shopping_cart| buzzfeed   |
| 10069   | 2018-01-04 08:12:01 | 3 - checkout     | facebook   |
| 10069   | 2018-01-04 08:13:01 | 4 - purchase     | facebook   |

### 🧩 Attribution Breakdown

- **Last Touch Source:** `facebook`
- **User ID:** `10069`

### 🛒 Final Interaction Before Purchase:
- **Checkout Page Visit:**  
  - **Time:** `2018-01-04 08:12:01`
- **Purchase Page Visit:**  
  - **Time:** `2018-01-04 08:13:01`

June’s **last touch** — the final source that led to her conversion — is attributed to **Facebook**, since that was the `utm_source` associated with her return and purchase.

```sql
 SELECT *
 FROM page_visits
 WHERE user_id=10069 ;
```
<img src="https://github.com/user-attachments/assets/dc3cdccc-8be9-48e4-b1de-20bd8cdeeb00" width=220>

## 🎯 First vs. Last Touch Attribution

If you're looking to **increase sales** at CoolTShirts.com, should you invest more in **Buzzfeed** (June's first visit) or boost **Facebook ads** (which led to her purchase)?

The key question is:

> Should June’s purchase be attributed to **Buzzfeed** or **Facebook**?

---

### 🧪 Two Approaches to Attribution:

#### 1️⃣ First-Touch Attribution
- **What it is:** Considers only the **first** `utm_source` for each customer.
- **Example:** For June, it's `buzzfeed`.
- **Purpose:** Helps you understand **how visitors first discover** the website.

#### 2️⃣ Last-Touch Attribution
- **What it is:** Considers only the **last** `utm_source` before a conversion or purchase.
- **Example:** For June, it's `facebook`.
- **Purpose:** Shows **what drives visitors back** to complete a purchase.

---

### 🧠 Why It Matters

The results from both models are crucial for shaping a company’s:

- Marketing strategy
- Ad spend allocation
- Customer journey understanding

📈 **Most companies analyze both** first- and last-touch attribution separately to get a well-rounded view of campaign performance.

---
## 🧮 The Attribution Query I — First Touch for All Users

We’ve learned how to attribute a user’s **first** and **last** touches individually. But what if we want to analyze **first-touch attribution for all users**?

That’s where **SQL** becomes incredibly useful.

With just one query, we can identify **every user’s first interaction** with the site. This data can be saved for later, filtered for specific user groups, or used to analyze campaign effectiveness at scale.

---

### 🛠️ First-Touch Attribution Using SQL

To determine when each user first visited the site, we can use the `MIN()` function with a `GROUP BY`. Let’s create a temporary table called `first_touch`:

```sql
SELECT 
  user_id,
  MIN(timestamp) AS 'first_touch_at'
FROM 
  page_visits
GROUP BY 
  user_id;

SELECT user_id,
  MAX(timestamp) as last_touch_at
FROM page_visits
WHERE user_id = 10069
GROUP BY user_id;
```

<img src="https://github.com/user-attachments/assets/26fe2175-de2e-4029-bf96-f13b7d8a040c" width=220 >


## 🔗 The Attribution Query II — Getting the UTM Source

To properly attribute first-touch traffic sources, we now need to retrieve the **UTM parameters** for each user's **first visit**.

We already have a table `first_touch` that contains the **earliest timestamp** (`first_touch_at`) for each `user_id`.

But that alone doesn’t give us the **source** (like `buzzfeed`, `facebook`, etc.).  
👉 For that, we need to **JOIN** the `first_touch` table with the original `page_visits` table.

---

### 🧬 The JOIN Logic

We match records using both `user_id` and `timestamp`:

```sql
ft.user_id = pv.user_id
AND ft.first_touch_at = pv.timestamp
```

• The diagram  illustrates the JOIN we need to get the UTM parameters of each first touch:

<img src="https://github.com/user-attachments/assets/3219e9db-38dc-4af6-9b1f-e9c602b8cbd9" width=550>


• On the left is page_visits (just three columns from the original table). We get the UTM parameters from there.
• On the right is first_touch (the result of the GROUP BY query). We get the first touches from there.
