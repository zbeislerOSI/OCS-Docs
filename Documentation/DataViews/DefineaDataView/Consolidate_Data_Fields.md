---
uid: ConsolidateDataFields
---

# Consolidate data fields
Data views containing streams that refer to the same property by different names will return a null value for both. When semantically identicial fields are incorrectly designated as separate fields in the data view, consolidate data fields to remedy this undesirable condition. 

The Gen2 Weather streams in the above data view have renamed the "Temperature" property to "AmbientTemperature." Consolidate the "Temperature" and "AmbientTemperature" data fields to alleviate this condition.

Find the `Field` associated with "AmbientTemperature". To the `Field`'s `.Keys` array, add "Temperature":


