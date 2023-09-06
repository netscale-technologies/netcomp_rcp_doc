# API


## Values

### insert_value
Inserts a new 'value' reading

A 'value' reading is intended for high volume, simple readings that are not processed or generated alerts on. They are only inserted in database.
However the system is optimized to manage any number of them efficiently

|Field|Type|Mandatory|Description
|---|---|---|---
|class|string|Y|Source of entry. Only "app" allowed for now
|type|string|Y|Type of entry. Valid values are "bpm", "glucose", "heartrate", "pulseoximeter", "scale", "spirometer", "thermometer"
|timestamp|string|Y|Datetime of reading
|patient_uid|string|Y|UID of patient
|reading|map|Y|Reading itself. It should use standard format
|extra|map|N|Extra information related to reading

Returns:


|Field|Type|Description
|---|---|---
|uid|string|UID of the created value
