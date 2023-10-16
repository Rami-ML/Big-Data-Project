# Big-Data-Project

This script is designed to process a set of CSV files containing timestamped data. Below are the key steps and explanations for each section:


## General Setup

- A counter (`processed_files`) is initialized to keep track of the processed files.

- The base path for the dataset files is set as `base_path`.
  

## File Processing Loop

The script iterates through a list of CSV files (`file_list`). For each file:

1. The CSV file is read into a DataFrame (`df`). The delimiter is set to `';'`, and decimal points are specified as `','`.

2. The 'Zeitstempel' column is converted to datetime format.

3. It checks if the time intervals are 15 minutes apart.

4. If any time interval is greater than 1 day, a `ValueError` is raised.
   

## Mapping Dataset to Timeseries

- `timeseries_mapping` is a dictionary that maps dataset identifiers to timeseries names.

- For each file, it finds the corresponding timeseries name based on the mapping.

 

## Data Preparation

### Step 1: Insert Original Values and Status

- If the columns 'Originalwert' and 'Originalstatus' exist, it replaces 'Wert' with 'Originalwert' where 'Originalwert' is not NaN.

- It sets 'Status' at index 0 to 0 and all 'W's in 'Status' to 1.

- Columns 'Originalwert' and 'Originalstatus' are dropped.

 
### Step 2: Split Dataset into Training and Test Sets

- Data is split based on predefined time ranges (`train_1_start`, `train_1_end`, etc.).

- Four subsets (`train_1`, `test_1`, `train_2`, `test_2`) are created.

 
### Step 3: Standardize Training Set

- If `train_1` is not empty, statistics are computed, and data is scaled.

- If `train_1` is empty, `test_1` is cleared, and statistics are computed for `train_2`.

 
### Step 4: Apply Standardization to Test Set

- Test sets (`test_1`, `test_2`) are standardized using the statistics from training sets.

 
### Step 5: Store Train and Test Sets

- Relevant columns are selected (`Zeitstempel`, `Wert`, `Status`) for each subset.

- An empty DataFrame (`*_empty_df`) is merged with the subset to get the complete dataset.

 
### Step 6: Save Datasets

- DataFrames are saved to CSV files based on the dataset type (`train_1`, `test_1`, etc.).

 
### Step 7: Get Statistics from the Whole Dataset

- Statistics are generated for each dataset.

- The resulting DataFrames are concatenated.

 
## Saving Statistics

- The final statistics DataFrame is saved to a CSV file.

 
## Incrementing Processed Files Counter

- The counter `processed_files` is incremented.
