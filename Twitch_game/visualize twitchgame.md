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
```

<img src="https://github.com/user-attachments/assets/3efbbeff-6f1d-4454-a1e1-4db7896916a3" width=500>
