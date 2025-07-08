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

Understanding these parameters allows teams
