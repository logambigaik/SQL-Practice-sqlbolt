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
