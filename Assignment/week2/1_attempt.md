**DIAGNOSIS**
# 1- Row no8 and row no 96 were duplicates, must have been printed twice
# x.duplicated()
# x[x.duplicated(keep=False)]

# 2- Row no5 had price missing, "NaN" was written there instead
# Row no17 "1,250" has a comma in between numbers, which is why python is not treating it as number
# x[n.isna()][["date", "item", "city", "price"]]

**PLANNED DECISION**
# 1- Exclude
# 2- Correct 