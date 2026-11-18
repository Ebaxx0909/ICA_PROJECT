**STATISTICS**
# I would report the minimum, maximum, median and standard deviation
# Minimum and maximum price can be used to detect if any outliers lie in each item.
# Standard deviation could be used to measure on average how much the price is deviating from mean
# Instead of mean, I would report median as it is a better measure of central tendency than mean.

**DIFFERENCE**
# Mean uses every value in a sum, so an outlier can disturb a mean by alot. With about 24 Staple A single outlier adds roughly (1250 − 110) / 24 ≈ 47 to the mean. The mean jumps from about 111 to about 158, which is not the actual price for that item at all.
# Median is the middle value. The extreme value only changes which position is “middle” by one step, so the median barely moves

**REFEREE'S NOTE**
# O: One Staple A record (2026-01-15, City 2) has a price entered as “1,250”, while the other Staple A prices lie between about 99.5 and 122.
# O: Because of the comma, pandas stored the price column as text, and mean failed until the commas were removed and the column was converted to numbers.
# O: With the flagged value included, the Staple A mean is [158.33] and the median is [112.5].
# O: With the flagged value excluded, the Staple A mean is [110.87] and the median is [112.5].
# O: The mean changed by [47.46], while the median changed by [0].
# E: The mean is a sum over all observations divided by their count, so a value roughly ten times the others shifts it largely.
# E: The median is the middle ranked observation, so adding one extreme value moves it by at most one step.
# C: The flagged value is the cause of the large gap between the mean with and without it.
# C: The comma in “1,250” caused the text-type column and the TypeError.
# E: This reading is supported by the value being about ten times the item’s other prices, but nothing in the dataset itself confirms it.
# C: Reporting the mean with the flagged value included would overstate Staple A’s typical price.
# E: The recommendation is to verify the record against the source, then correct or exclude it and report the mean and median together with a note on the treatment.