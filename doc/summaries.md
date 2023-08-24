# API


## Summaries

### insert_summary
Inserts a new 'summary' reading

A 'summary' reading is intended for data meant to be stored only once per day. A date must be provided, and any previous reading with same class, type and date will be over written.

|Field|Type|Mandatory|Description
|---|---|---|---
|class|string|Y|Source of entry. Only "app" allowed for now
|type|string|Y|Type of entry. Valid values are "activity_sum", "heart_sum", "sleep_sum", "breath_sum", "heart_var_sum", "spo2_sum", "temp_sum"
|timestamp|string|Y|Datetime of reading
|date|string|Y|Date of reading (f.e. 2022-01-01)
|patient_uid|string|Y|UID of patient
|reading|map|Y|Reading itself. It should use standard format
|extra|map|N|Extra information related to reading

Returns:


|Field|Type|Description
|---|---|---
|uid|string|UID of the created value
