## Configs

### create_config

Allows to create a new config record at the server.
Field 'spec' must be provided with following information:

|Field|Type|Mandatory|Description
|---|---|---|---
|parent_uid|string|Y|Parent of object, for now only patients are allowed
|class|string|Y|Must be "test" or "app"
|type|string|Y|
|data|(any)|Y|Payload of config, in any JSON format (string, object...)

Returns UID of the config object


### get_config

Get's specific config. Required field is `uid`

### update_config

Modifies an existing config. Required fields are `uid` and `spec`

### delete_config

Deletes existing config. Required field is `uid`

### list_config

Lists existing configs. Required fields are:

|Field|Type|Mandatory|Description
|---|---|---|---
|parent_uid|string|Y|Parent of object, for now only patients are allowed
|class|string|N|
|type|string|N|

Data will be sorted by creation time (desc)


