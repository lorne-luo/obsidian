# Heatmap

```
import seaborn as sns

# git reflog --date=iso | grep commit >git_reflog.txt
df=pd.read_csv("git_reflog_total.csv")

# Extract weekday and hour
df['weekday'] = pd.to_datetime(df['datetime']).dt.weekday
df['hour'] = pd.to_datetime(df['datetime']).dt.hour

# Create pivot table
heatmap_data = df.pivot_table(index='hour', columns='weekday', aggfunc='size', fill_value=0)

# Plot heatmap
plt.figure(figsize=(10, 6))
sns.heatmap(heatmap_data, cmap='YlGnBu', annot=True, fmt='d')
plt.xlabel('Weekday (0=Mon)')
plt.ylabel('Hour')
plt.title('Heatmap of Git Commits by Weekday and Hour')
plt.show()
```

![image.png](https://cdn.jsdelivr.net/gh/lorne-luo/note-gen-image-sync@main/1f056b45-e973-424d-9c87-aeea7e39c4b8.png)


## Hist
