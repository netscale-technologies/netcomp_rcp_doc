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


### summary_set_reviewed

Marks an existing summary as 'reviewed'

|Field|Type|Mandatory|Description
|---|---|---|---
|summary_uid|string|Y|UID of summary to update
|reviewer_uid|string|Y|UID of reviewer

If summary is not already reviewed, a new object "review" will be added to 'extra', containing "reviewer_uid" and "time",
and `{"result": true}` will be returned.

It is already reviewed, nothing is changed and  `{"result": false}` is returned

If later on, you call insert_summary again for the same class, type and date (because of new data or because you want to add a notification_uid), **field 'extra' will be overwritten**, so marking the summary as 'not reviewed' again

