#### Visualize Twitch Gaming Data

In this project, we will visualize Twitch data using Python and Matplotlib, in the forms of:

• Bar Graph: Featured Games

• Pie Chart: Stream Viewers’ Locations
• Line Graph: Time Series Analysis

#### Bar Graph:

```python
from matplotlib import pyplot as plt
import numpy as np
import pandas as pd

# Bar Graph: Featured Games

games = ["LoL", "Dota 2", "CS:GO", "DayZ", "HOS", "Isaac", "Shows", "Hearth", "WoT", "Agar.io"]

viewers =  [1070, 472, 302, 239, 210, 171, 170, 90, 86, 71]


df = pd.DataFrame(zip(games, viewers), columns=['game', 'viewer'])
# print(df.head(5))
print(df.groupby('game').sum())

# If you have more than 10 bars, repeat colors
colors = plt.cm.tab10.colors 
bar_colors = colors * (len(games) // len(colors) + 1)
plt.bar(range(len(games)),viewers,color=bar_colors[0:len(games)])
plt.title("Featured games Viewers")
plt.legend(["Twitch"])
plt.xlabel("Games")
plt.ylabel("Viewers")
ax = plt.subplot()

ax.set_xticks([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
ax.set_xticklabels(games, rotation=30)
ax.set_yticklabels(viewers)
```
<img src="https://github.com/user-attachments/assets/8c61edaf-b1ed-4990-858f-ddf55c11befb" width=500>

#### Pie Chart: Stream Viewers’ Locations

```python
# Pie Chart: League of Legends Viewers' Whereabouts
plt.clf() 

# Pie Chart: League of Legends Viewers' Whereabouts
plt.clf() 

labels = ["US", "DE", "CA", "N/A", "GB", "TR", "BR", "DK", "PL", "BE", "NL", "Others"]

countries = [447, 66, 64, 49, 45, 28, 25, 20, 19, 17, 17, 279]

colors = ['lightskyblue', 'gold', 'lightcoral', 'gainsboro', 'royalblue', 'lightpink', 'darkseagreen', 'sienna', 'khaki', 'gold', 'violet', 'yellowgreen']

# Optional: explode to highlight top country (e.g., US)
explode = [0.1 if label == "US" or label =="Others" else 0 for label in labels]
# explode = (0.1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0)

# Plot
plt.figure(figsize=(8, 8))
plt.pie(
    countries,
    labels=labels,
    autopct='%1.1f%%',  # show percentages
    startangle=345,
    explode=explode,
    shadow=True,
    colors=colors,
    pctdistance=1.15
)

# Add legend
# plt.legend(title="Countries", loc="center left",bbox_to_anchor=(1.2, 0.5))
plt.legend(labels, loc="right")

# Title
plt.title("League of Legends Viewers' Whereabouts")

plt.tight_layout()
plt.show()
```

<img src="https://github.com/user-attachments/assets/3075d5b8-6ee7-468e-9860-7d440383cc74" width=450>

#### Line chart

```python
# Line Graph: Time Series Analysis
plt.clf()
# Create figure and axis with lavender background for the figure
fig, ax = plt.subplots(facecolor='yellow', figsize=(10, 6))

hour = range(24)
viewers_hour = [30, 17, 34, 29, 19, 14, 3, 2, 4, 9, 5, 48, 62, 58, 40, 51, 69, 55, 76, 81, 102, 120, 71, 63]

ax.plot(hour, viewers_hour)
ax.set_title("Time Series")    # <-- corrected here
ax.set_xlabel("Hour")          # <-- corrected here
ax.set_ylabel("Viewers")       # <-- corrected here

ax.legend(['2015-01-01'])

ax.set_xticks(hour)
ax.set_yticks([0, 20, 40, 60, 80, 100, 120])

y_upper = [i + (i * 0.15) for i in viewers_hour]
y_lower = [i - (i * 0.15) for i in viewers_hour]

ax.fill_between(hour, y_lower, y_upper, alpha=0.2)  # <-- use ax.fill_between

plt.show()

```


<img src="https://github.com/user-attachments/assets/62931dcb-3459-48b8-b0c2-b7918aaf47ec" width=450>


