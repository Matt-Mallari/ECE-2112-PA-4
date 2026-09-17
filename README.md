# **ECE-2112-PA-4**
**Dean Matthew M. Calibut | 2ECE-D**

This repository details the implementation of Experiment 4 (PA4), focusing on data wrangling, conditional logical indexing, feature engineering, and data visualization using the Pandas and Matplotlib libraries. The code fulfills the intended learning outcomes of loading tabular Excel datasets, engineering new average metrics, extracting specific DataFrame subsets safely, and generating subplots without modifying original source values.

# **Initial Setup & Library Imports**
Before tackling the specific problems, the necessary Python libraries and Excel file must be imported to establish the working environment.

## **The Following Modules were imported:**

```python
import pandas as pd
import matplotlib.pyplot as plt

board_exam = pd.read_excel('board2.xlsx')
```
* `import pandas as pd`: This is the foundational library required for data manipulation and tabular data analysis in this experiment. Importing it under the standard alias pd allows for concise calls to read CSV files and utilize structure-handling tools like DataFrame, .iloc, and .loc.
* `import matplotlib.pyplot as plt`: This module provides a MATLAB-like plotting framework necessary for constructing the 1x3 subplot bar chart matrices required in the visualization phase.
* `pd.read_excel('board2.xlsx')`: Loads the raw board exam specification Excel file into board_exam, which serves as the primary data source for creating task-specific DataFrames.

# **1. Visayas Communication Dataframe**
#### **Objective:** This problem requires creating a new distinct DataFrame from the original file, filtering the dataset for students whose Hometown is Visayas and Track is Communication, engineering a new column for their overall 4-subject average, and extracting only the Name, Gender, Math, Electronics, and Average columns.

The Following Methods/Functions were used:

```python
# Create a new DataFrame specifically for Problem 1
board_df = pd.DataFrame(board_exam)

# Feature Engineering: 4-Subject Mean
board_df['Average'] = board_df[['Math', 'Electronics', 'GEAS', 'Communication']].mean(axis=1)

# Conditional Filtering using .loc[] and .copy()
VisComm = board_df.loc[
    (board_df['Hometown'] == 'Visayas') & (board_df['Track'] == 'Communication'), 
    ['Name', 'Gender', 'Math', 'Electronics', 'Average']
].copy()

print("VisComm DataFrame:\n", VisComm)
print("Number of rows:", len(VisComm))
```
The statement `board_df = pd.DataFrame(board_exam)` creates a fresh, isolated DataFrame for Problem 1 directly from the loaded Excel dataset. Next, the `.mean(axis=1)` function calculates the row-wise average across the four subject columns to produce the `Average` feature. To extract the required subset while preventing Pandas' `SettingWithCopyWarning`, the `.loc[row_indexer, column_indexer]` accessor is utilized alongside `.copy()`. The evaluation `(board_df['Hometown'] == 'Visayas')` & `(board_df['Track'] == 'Communication'` generates a dynamic Boolean mask array, isolating matching student records in a single, memory-safe operation to identify 5 matching student rows.

# **2. Visayas Female Dataframe**
#### **Objective:** This problem requires instantiating a separate DataFrame from the Excel data, extracting female students from Visayas, and further isolating high performers who achieved an overall average score of >= 60, displaying only the Name, Track, GEAS, Electronics, and Average columns.

The Following Methods/Functions were used:

```python
# Create a new DataFrame specifically for Problem 2
visfemdf = pd.DataFrame(board_exam)

# Feature Engineering: 4-Subject Mean
visfemdf['Average'] = visfemdf[['Math', 'Electronics', 'GEAS', 'Communication']].mean(axis=1)

# Extracting female students from Visayas safely
visfemale = visfemdf.loc[
    (visfemdf['Hometown'] == 'Visayas') & (visfemdf['Gender'] == 'Female'),
    ['Name', 'Track', 'GEAS', 'Electronics', 'Average']
].copy()

# Subset with Average >= 60
visfem_avg60 = visfemale.loc[visfemale['Average'] >= 60].copy()
```
As instructed, `visfemdf = pd.DataFrame(board_exam)` instantiates an independent DataFrame for Problem 2 to preserve task separation. After calculating the overall 4-subject mean, the first `.loc[]` block evaluates `Hometown` and `Gender` conditions simultaneously, filtering the rows and selecting the five target columns into `visfemale` with `.copy()`. The second block applies a new Boolean mask `(visfemale['Average'] >= 60)` strictly to this subset, isolating the high-performing female students from Visayas without relying on hardcoded row numbers.

# **3. Category-Average Visualization**
#### **Objective:** This problem requires creating a new DataFrame from the original dataset, aggregating average scores grouped by Track, Gender, and Hometown, then constructing a 1x3 subplot bar chart matrix to visually compare the mean performances across these demographics.

The Following Methods/Functions were used:

```python
# Create a new DataFrame specifically for Problem 3
cav_df = pd.DataFrame(board_exam)

# Feature Engineering: 4-Subject Mean
cav_df['Average'] = cav_df[['Math', 'Electronics', 'GEAS', 'Communication']].mean(axis=1)

# Grouping by Category
track_avg = cav_df.groupby('Track', as_index=False)['Average'].mean()
gender_avg = cav_df.groupby('Gender', as_index=False)['Average'].mean()
hometown_avg = cav_df.groupby('Hometown', as_index=False)['Average'].mean()

print("Average Scores by Track:\n")
display(track_avg)
print("\nAverage Scores by Gender:\n")
display(gender_avg)
print("\nAverage Scores by Hometown:\n")
display(hometown_avg)
print("\n")

# Visualization
fig, axes = plt.subplots(1, 3, figsize=(15, 5))

track_avg.plot(x='Track', y='Average', kind='bar', ax=axes[0], legend=False)
axes[0].set_title('Mean Average by Track')
axes[0].set_xlabel('Track', labelpad=15)
axes[0].set_ylabel('Mean Average')

gender_avg.plot(x='Gender', y='Average', kind='bar', ax=axes[1], legend=False)
axes[1].set_title('Mean Average by Gender')
axes[1].set_xlabel('Gender', labelpad=60)
axes[1].set_ylabel('Mean Average')

hometown_avg.plot(x='Hometown', y='Average', kind='bar', ax=axes[2], legend=False)
axes[2].set_title('Mean Average by Hometown')
axes[2].set_xlabel('Hometown', labelpad=50)
axes[2].set_ylabel('Mean Average')

plt.tight_layout()
plt.show()
```
The execution begins by instantiating `cav_df = pd.DataFrame(board_exam)` to maintain a distinct DataFrame environment for Problem 3. The `.groupby()` method splits `cav_df` across distinct categorical variables `(Track, Gender, Hometown)`. Applying `.mean()` to the `['Average']` column produces the aggregated statistical summary dataframes, which are formatted and printed using `display()` and `print()`.

For the visualization step, `plt.subplots(1, 3, figsize=(15, 5))` sets up a 1x3 subplot grid. Each grouped DataFrame is plotted to its assigned axis `(ax=axes[0], ax=axes[1], ax=axes[2])`. Axis titles and labels are customized using `.set_title()`, `.set_xlabel()`, and `.set_ylabel()`. Specifically, the labelpad parameters `(labelpad=15, labelpad=60, labelpad=50)` adjust the spacing between axis titles and tick labels to prevent visual collisions caused by label orientation. Finally, `plt.tight_layout()` cleans up structural padding before displaying the plot.

To see the main Python program for Experiment 4, click this link https://github.com/Matt-Mallari/ECE-2112-PA-4/blob/main/ECE_2112_PA4.ipynb, download the .ipynb file, open it in Jupyter Notebook, and run all cells.

Moreover, the board2.xlsx file utilized for data frame creation can be found here:


### **README File Version History**
* 2026, September 17: Repository Created
