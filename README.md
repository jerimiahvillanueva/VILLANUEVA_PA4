# ECE2112_PA4

## Experiment 4: Data Wrangling and Data Visualization

### Project Objectives

1. Load an Excel dataset into a Pandas DataFrame.
2. Calculate new columns using row-wise mathematical operations.
3. Filter records using multiple Boolean conditions (AND logic).
4. Group data by categories to calculate summary statistics.
5. Create multiple bar charts within a single Matplotlib figure.

---

### Data Preparation

This script creates a copy of the source data to preserve it, then computes a new column for the overall average.

* `df.copy()`: Creates an independent copy of the original dataset.
* `df[[...]].mean(axis=1)`: Computes the row-by-row mean for the specified subject columns.

```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_excel('board2.xlsx')
df = df.copy()
df['Average'] = df[['Math', 'Electronics', 'GEAS', 'Communication']].mean(axis=1)
```

---

### A. Visayas Communication DataFrame

This script filters the dataset for students in the Communication track from the Visayas, selects specific columns, and displays the number of rows in the resulting DataFrame.

* `df.loc[(...) & (...), [...]]`: Combines multiple Boolean conditions using the bitwise AND (`&`) operator and selects the specified columns.
* `len()`: Returns the total number of rows in the resulting DataFrame.

```python
VisComm = df.loc[
    (df['Hometown'] == 'Visayas') & (df['Track'] == 'Communication'),
    ['Name', 'Gender', 'Math', 'Electronics', 'Average']
]

display(VisComm)
print("Number of rows:", len(VisComm))

```

---

### B. Visayas Female DataFrame

This script extracts female students from the Visayas and applies a secondary filter to identify those with an average of at least 60.

* `df.loc[]`: Filters the original DataFrame using the Visayas and Female conditions.
* `VisFemale.loc[VisFemale['Average'] >= 60]`: Applies a secondary condition to display students whose average is at least 60 without changing the original `VisFemale` DataFrame.

```python
VisFemale = df.loc[
    (df['Hometown'] == 'Visayas') & (df['Gender'] == 'Female'),
    ['Name', 'Track', 'GEAS', 'Electronics', 'Average']
]

display(VisFemale)

display(VisFemale.loc[VisFemale['Average'] >= 60])
```

---

### C. Category-Average Visualization

This script groups the dataset by different categories, calculates the mean average for each category, and displays the results using three side-by-side bar charts.

* `.groupby()[['Average']].mean()`: Groups the data by categorical columns and calculates the mean of the `Average` column.
* `plt.subplot(1, 3, x)`: Divides a single figure into three side-by-side plotting areas.
* `plt.bar()`: Creates bar charts using the category labels and their corresponding mean average values.

```python
mean_track = df.groupby('Track')[['Average']].mean()
mean_gender = df.groupby('Gender')[['Average']].mean()
mean_hometown = df.groupby('Hometown')[['Average']].mean()

plt.figure(figsize=(15, 4))

plt.subplot(1, 3, 1)
plt.bar(mean_track.index, mean_track['Average'])
plt.title('Mean Average by Track')
plt.xlabel('Track')
plt.ylabel('Mean Average')

plt.subplot(1, 3, 2)
plt.bar(mean_gender.index, mean_gender['Average'])
plt.title('Mean Average by Gender')
plt.xlabel('Gender')
plt.ylabel('Mean Average')

plt.subplot(1, 3, 3)
plt.bar(mean_hometown.index, mean_hometown['Average'])
plt.title('Mean Average by Hometown')
plt.xlabel('Hometown')
plt.ylabel('Mean Average')

plt.tight_layout()
plt.show()
```

---

### Interpretation

Based on the observations from this specific dataset:

* The track category with the highest sample mean is **Communication**.
* The gender category with the highest sample mean is **Male**.
* The hometown category with the highest sample mean is **Luzon**.

---

### README History

* September 17, 2026 - README file uploaded.
