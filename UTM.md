#### First- and Last-Touch Attribution
Web developers, marketers, and data analysts use that information to improve their sources (sometimes called channels or touchpoints) online. If an ad campaign drives a lot of visits to their site, then they know that source is working! We say that those visits are attributed to the ad campaign.

But how do websites capture that information? The answer is UTM parameters. These parameters capture when and how a user finds the site. Site owners use special links containing UTM parameters in their ads, blog posts, and other sources. When a user clicks one, a row is added to a 
database describing their page visit. 

You can see a common schema for a “page visits” table below and at this link.

  • user_id - A unique identifier for each visitor to a page
  • timestamp - The time at which the visitor came to the page
  • page_name - The title of the section of the page that was visited
  • utm_source - Identifies which touchpoint sent the traffic (e.g. google, email, or facebook)
  • utm_medium - Identifies what type of link was used (e.g. cost-per-click or email)
  • utm_campaign - Identifies the specific ad or email blast (e.g. retargetting-ad or weekly-newsletter)
