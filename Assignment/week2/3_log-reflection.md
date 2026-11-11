**CODE PROVIDED BY AI**

import pandas as pd

# 1. Load the file, treating "NaN" text as missing and reading Price as text first
df = pd.read_csv("yourfile.csv", dtype={"Price": str}, na_values=["NaN", "nan", ""])

# 2. Fix the comma problem: "1,250" -> 1250
df["Price"] = df["Price"].str.replace(",", "", regex=False)
df["Price"] = pd.to_numeric(df["Price"], errors="coerce")

# 3. Remove duplicates (keeps the first occurrence, so row 96 is dropped, row 8 stays)
df = df.drop_duplicates(keep="first")

# 4. Handle the missing price in row 5
# Option A: fill with the median (safe against outliers)
df["Price"] = df["Price"].fillna(df["Price"].median())

# Option B: drop the row instead
# df = df.dropna(subset=["Price"])

# 5. Check the result
print(df.shape)
print(df.dtypes)
print(df["Price"].isna().sum())  # should be 0

df.to_csv("cleaned_file.csv", index=False)


# 6. Rows above the IQR upper fence
q1, q3 = df["Price"].quantile([0.25, 0.75])
iqr = q3 - q1
upper = q3 + 1.5 * iqr
lower = q1 - 1.5 * iqr

print(df[(df["Price"] > upper) | (df["Price"] < lower)])

# 7. df["Price"] = df["Price"].replace(9999, pd.NA)
df["Price"] = pd.to_numeric(df["Price"], errors="coerce")

# Fill all missing prices (the original NaN + the 9999s) with the median
df["Price"] = df["Price"].fillna(df["Price"].median())

# 8. print(df["Price"].describe())



**COURT VERDICTS**
# 1. Modify
# I have already written (x = pd.read_csv"File name")

# 2. Keep

# 3. keep

# 4. Reject
# This code fills the missing price with median, the real value must be obtained. It may provide an average however, the real value will be always missing, so it will provide the wrong data.

# 5.Keep

# 6. Keep

# 7. Keep

# 8. Keep

# After cleaning the rows has been reduced from 97 to 96. The price also showed "1" missing and now, has been corrected to "0" missing. The max value has also come down from 9999.00 to 1250.00 

**REFLECTION**
# My decision to remove the duplicated rows were better supported by AI. However, my decision to correct the missing price differed as I wanted to fill in the actual value but, the AI filled in median. In the unavailability of actual price might be the right decision. However, it might show inaccurate data and averages can be manipulated by outliers. Since, the max value of price is (9999.00), an outlier exists in the database. the presence of this outlier automatically affects the median. Without the help of AI, I missed the max value in the dataset.

**LOG ROW**
## AI Usage Log

# | Date | Tool | Prompt / Task | What the AI gave | What I did with it |
# |---|---|---|---|---|
# | 11 Nov 2026 | Claude (Sonnet 5.5) | I had already found the data problems myself in a 97-row, 6-column file: rows 8 and 96 were duplicates, row 5 had "NaN" as the price, and row 17 had "1,250" with a comma, so Python read it as text. I asked for pandas cleaning code. I then shared `describe()` output (max 9999, mean 316.5, median 200, std 1007) and asked for help. | Pandas code to remove duplicates (`drop_duplicates`), strip commas and convert to numeric (`str.replace` + `to_numeric`), and handle the NaN by median fill or dropping. It also flagged 9999 as a likely placeholder or outlier, using the IQR rule (upper fence 532), and suggested replacing it with missing and filling with the median. | Identified the issues myself before using AI. Ran the code on my file and re-ran `describe()` to confirm the max, mean and std moved sensibly. Checked the flagged rows before deciding whether 9999 was an error. |
