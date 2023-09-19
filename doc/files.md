# API


## Files


### get_file

Retrieves an existing file object

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of file


Returns:

|Field|Type|Description
|---|---|---
|data|object|Fields parent_uid, class, type and extra
|metadata|object|Fields content_type, length, encryption, hash and uid
|payload|binary|For inlined files, the file content as Base64


### view_list_files

Retrieves a set of files

|Field|Type|Mandatory|Description
|---|---|---|---
|parent_uid|string|Y|Parent uid of file
|class|string|N|
|type|string|N|
|start_time|string|N|First creation_time to consider
|stop_time|string|N|Last creation_time to consider
|size|integer|N|

Returns:

|Field|Type|Description
|---|---|---
|items|list|List of objects with same format as 'get'

### view_count_files

Counts files

|Field|Type|Mandatory|Description
|---|---|---|---
|parent_uid|string|Y|Parent uid of file
|class|string|N|
|type|string|N|
|start_time|string|N|First creation_time to consider
|stop_time|string|N|Last creation_time to consider


Returns:

|Field|Type|Description
|---|---|---
|count|integer|


### create_file

This API allows to create a file directly inside the database.
Maximum size using this method is 16KB.
See (see [Uploading](uploading.md)) for other creation options

|Field|Type|Mandatory|Description
|---|---|---|---
|parent_uid|string|Y|Parent uid of file
|class|string|Y|
|type|string|N|
|content_type|string|Y|
|payload|string|Y|Base64 encoded of file


Returns UID of created file

