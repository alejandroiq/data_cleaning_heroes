# Data Cleaning

============================================================
Superhero Powers – Data Cleaning & Merging Summary
============================================================

This project merges superhero metadata with their superpowers to enable
analysis such as computing how many powers each hero has. It focuses on
cleaning, reshaping, merging, and validating two datasets: `heroes_df`
and `powers_df`.

------------------------------------------------------------
PART 1: UNDERSTANDING THE STRUCTURE
------------------------------------------------------------

- `heroes_df` contains hero metadata (name, gender, alignment, etc.).
- `powers_df` originally had powers as rows and heroes as columns.
- Values in `powers_df` were string-based: 'True' and 'False'.

------------------------------------------------------------
PART 2: CLEANING THE POWERS DATA
------------------------------------------------------------

- Transposed the DataFrame so that each row is a hero, each column a power.
- Promoted the first row to column headers:
  
  powers_df_transposed.columns = powers_df_transposed.iloc[0]  
  powers_df_transposed = powers_df_transposed.drop(powers_df_transposed.index[0])

- Converted string values to boolean True/False:

  powers_df_transposed = powers_df_transposed.applymap(
      lambda x: True if str(x).strip().lower() == 'true' else False
  )

------------------------------------------------------------
PART 3: MERGING DATASETS
------------------------------------------------------------

- Merged `heroes_df` with `powers_df_transposed` using:

  heroes_and_powers_df = heroes_df.merge(
      powers_df_transposed, left_on='name', right_index=True
  )

- Ensured each hero now has both metadata and power information.

------------------------------------------------------------
PART 4: PREPARING FOR POWER COUNT ANALYSIS
------------------------------------------------------------

- Transposed the cleaned powers data again to get powers as index:

  powers_df = powers_df_transposed.T

- This format is required for the evaluation cell to iterate over powers.

------------------------------------------------------------
PART 5: ADDING THE POWER COUNT COLUMN
------------------------------------------------------------

- Calculated how many powers each hero has using:

  heroes_and_powers_df["Power Count"] = sum(
      [heroes_and_powers_df[power_name] for power_name in powers_df.index]
  )

- This uses the fact that booleans act like integers (`True = 1`, `False = 0`).

------------------------------------------------------------
PART 6: DEBUGGING & VALIDATION TOOLS USED
------------------------------------------------------------

- Verified data types: powers_df_transposed.dtypes.value_counts()
- Checked for missing powers: 
  [col for col in powers_df.index if col not in heroes_and_powers_df.columns]
- Confirmed powers_df.index contains power names:
  print(powers_df.index[:5])

------------------------------------------------------------

Final outputs include:
- `powers_df_transposed`: cleaned, boolean power matrix
- `powers_df`: powers as index, for evaluation loop
- `heroes_and_powers_df`: merged dataset with "Power Count" per hero

This structured workflow ensures clean, analyzable data for superhero power analytics.
