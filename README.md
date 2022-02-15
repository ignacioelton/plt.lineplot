# plt.lineplot
line plot with a rolling avg and a trend linear line
import pandas as pd
from matplotlib import pyplot as plt
import numpy as np

df = pd.read_csv("/Users/ielton/Documents/MSc Data Science/Data Visualisation/Data/Pages/DailyHits.csv", index_col=0)


df = df.reindex(df.sum().sort_values(ascending=False).index, axis=1)

df.index = pd.to_datetime(df.index)

dfh = df.iloc[:,:2]
dfm = df.iloc[:,2:10]
dfs = df.iloc[:,10:]

#print((df.dtypes!=int).value_counts())
#dfh.plot.line()
#plt.show()

#avg_data = dfm.resample("2W").mean()

rolling_avg = dfm.rolling(window= 14).mean()


plt.figure(figsize=(8, 8))
plt.plot(dfm, linewidth= 0.5)
plt.ylim(ymin=0)
plt.gca().set_prop_cycle(None)
for name in dfm.columns:
    x = np.arange(len(dfm[name]))
    z = np.polyfit(x, dfm[name], 1)
    trend = np.poly1d(z)
    plt.plot(dfm.index, trend(x), linestyle = "-")
plt.gca().set_prop_cycle(None)
plt.plot(rolling_avg, linewidth = 2)
plt.xlabel("date", fontsize = 18)
plt.ylabel("hits", fontsize = 18)
plt.title("Daily Hits medium", fontsize = 20)
plt.legend(dfm.columns, loc=2, markerscale= 4)
plt.show()
