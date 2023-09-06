# API


## Push


### send_push

This API allows to send a push to a registered application (see [login](common.md#login) section)

|Field|Type|Mandatory|Description
|---|---|---|---
|member_uid|string|N|Target of push. If provided, currently connected user will be used
|type|string|Y|
|data|object|Y|

Push will be sent to all registered devices for this user
