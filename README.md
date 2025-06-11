# Eda-
# Install necessary libraries (if not already installed)
# !pip install pandas

import pandas as pd

# 1. Load the dataset
df = pd.read_csv("student_info.csv")
print("🔹 Raw Dataset Loaded. Shape:", df.shape)
print(df.head(), "\n")

# 2. Identify and handle missing values
print("🔹 Missing Values:")
print(df.isnull().sum(), "\n")

# 3. Remove duplicate rows
initial_shape = df.shape
df = df.drop_duplicates()
print(f"🔹 Removed {initial_shape[0] - df.shape[0]} duplicate rows.\n")

# 4. Standardize column headers (lowercase, no spaces)
print("🔹 Column Names BEFORE:")
print(list(initial_shape))
df.columns = df.columns.str.strip().str.lower().str.replace(" ", "_")
print("🔹 Column Names AFTER:")
print(list(df.columns), "\n")

# 5. Standardize text values (e.g., gender)
if 'gender' in df.columns:
    df['gender'] = df['gender'].astype(str).str.lower().str.strip()
    print("🔹 Unique Genders:", df['gender'].unique())

# 6. Standardize any 'neighbourhood' or similar columns if applicable
if 'neighbourhood' in df.columns:
    df['neighbourhood'] = df['neighbourhood'].astype(str).str.title().str.strip()
    print("🔹 Sample Neighbourhoods:", df['neighbourhood'].unique()[:5], "\n")

# 7. Convert date columns (if any) to dd-mm-yyyy format
date_cols = [col for col in df.columns if 'date' in col or 'day' in col]
for col in date_cols:
    df[col] = pd.to_datetime(df[col], errors='coerce')
    df[col] = df[col].dt.strftime('%d-%m-%Y')
if date_cols:
    print("🔹 Date Columns Formatted:\n", df[date_cols].head(), "\n")

# 8. Check and fix data types
# Age should be numeric
if 'age' in df.columns:
    df['age'] = pd.to_numeric(df['age'], errors='coerce')
    df['age'] = df['age'].fillna(0).astype(int)
    print("🔹 Age Data Type:", df['age'].dtype)
    print(df['age'].describe(), "\n")

# 9. Final overview
print("🔹 Final Cleaned Dataset Info:")
print(df.info())
print("\n🔹 First 5 Rows of Cleaned Data:")
print(df.head())
