# API


## Push


### send_push

This API allows to send a push to a registered application (see [login](common.md#login) section)

|Field|Type|Mandatory|Description
|---|---|---|---
|member_uid|string|Y|Target of push
|title|string|Y|
|body|spec|Y|

Push will be sent to all registered devices for this user
