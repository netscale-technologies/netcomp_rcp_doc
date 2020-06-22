# API


## Notifications


### create_notification
Creates a new Notification object

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|N|Unique (for domain) id of notification, no special characters allowed. Generated if not included.
|domain|string|N|Domain to use (like "zone1.type")
|spec|spec|Y|See bellow

Spec object:

|Field|Type|Mandatory|Description
|---|---|---|---
|group|string|Y|Must be `medical` or `staff` 
|class|string|Y|Class for the notification
|type|string|N|For a class, a specific type
|status|string|N|No yet used
|is_read|boolean|N|Default false
|topics|\[string\]|Y|List of patients, staff or medicals this notifications is linked to
|priority|number|N|0..999 (default 500)
|meta|object|N|Specific metadata


Returns:


|Field|Type|Description
|---|---|---
|uid|string|UID of the created notification
|id|string|Unique (in domain) id


### get_notification
Gets the data of an existing notification. Two formats available:

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|UID of notification


|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|ID of notification
|domain|string|N|Domain of creation


If successful, full information will be retrieved:

|Field|Type|Description
|---|---|---
|spec|spec|See above
|status|status|See bellow
|metadata|metadata|See bellow


Status description:

|Field|Type|Description
|---|---|---
|read_by|string|UID of member registering the read 
|read_time|string|Date time of read


Metadata description:

|Field|Type|Description
|---|---|---
|uid|string|Globally unique UID
|id|string|Id unique for domain
|creation_date|string|Creation date in RFC3339 format
|update_date|string|Last update date in RFC3339 format
|generation|integer|Increases at each update



### set_notification_read
Sets a notification as read

|Field|Type|Mandatory|Description
|---|---|---|---
|notification_uid|string|Y|
|member_uid|string|Y|




### update_notification
Allows to update notification's data

|Field|Type|Mandatory|Description
|---|---|---|---
|uid|string|Y|Unique UUID
|spec|spec|Y|Spec object

|Field|Type|Mandatory|Description
|---|---|---|---
|id|string|Y|Unique ID for domain
|domain|string|N|Domain to use (like "zone1.type")
|spec|spec|Y|Spec object

Must supply either UID or Id (possibly with domain). Fields UID, id and domain cannot be modified.

All fields in spec should be included, or the missing ones will be deleted from object

Returns ok or error


### delete_notification

Allows to delete an existing notification. Same parameters as get_notification.


### search_notifications
Allows for a powerful search operation on notifications. This api can however produce non optimal queries on the server, use `list_notification_by_time` and `list_notification_by_priority`


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|class|string|
|type|string|
|status|string|
|is_read|boolean|If not used, reads and unreads will be found
|priority|integer|Use operators like `gte:` or `lt:`
|with_topic|string|
|start_time|string|Starting date (no operators allowed)
|stop_time|string|Stopping date (no operators allowed)
|creation_date|string|Creation date in RFC3339 format
|update_date|string|Last update date in RFC3339 format
|from|integer|Starting point (for pagination)
|size|integer|Number or records to retrieve
|get_total|boolean|If true, calculates the grand total of records
|sort|\[string\]|List of fields to sort on

* Can filter on domain, or domain and subdomains using 'deep'
* Can paginate with from and size
* Can filter on any of the previous fields, or any number of them, working like and 'and'
  * If you set a raw string, it will find objects with that exact string: `"phone": "123"`
  * You can use a prefix to modify the behaviour: eq, ne, gt, gte, lt, lte. They work for string and numeric fields (like timezone): `"timezone": "gte:2"` 
  * You can use a prefix: `"phone": "prefix:123"`
  * You can target several values: `"phone": "123|321"`
* Can sort on one or several fields, ascending or descending: `"sort": ["asc:update_time", "timezone"]` 
* Operations on field 'name' will be performed over a 'normalized' version of the field (without uppercase, utf8, etc.)


### search_medical_notifications

Fast search of medical notifications.
Must provide either a medical_uid or an office_uid.
Full notification actors will be retrieved

|Field|Type|Description
|---|---|---
|medical_uid|string|
|office_uid|string|
|is_read|boolean|If not used, reads and unreads will be found
|start_time|string|Starting date
|stop_time|string|Stopping date
|from|integer|Starting point (for pagination)
|size|integer|Number or records to retrieve


### count_medical_notifications

Fast count of medical notifications.
Must provide either a medical_uid or an office_uid

|Field|Type|Description
|---|---|---
|medical_uid|string|
|office_uid|string|
|is_read|boolean|If not used, reads and unreads will be found
|start_time|string|Starting date
|stop_time|string|Stopping date


### search_staff_notifications

Fast search of non-medical notifications.
Full notification actors will be retrieved

|Field|Type|Description
|---|---|---
|office_uid|string|*Mandatory
|is_read|boolean|If not used, reads and unreads will be found
|start_time|string|Starting date
|stop_time|string|Stopping date
|from|integer|Starting point (for pagination)
|size|integer|Number or records to retrieve


### count_staff_notifications

Fast count of non-medical notifications.

|Field|Type|Description
|---|---|---
|office_uid|string|*Mandatory
|is_read|boolean|If not used, reads and unreads will be found
|start_time|string|Starting date
|stop_time|string|Stopping date



### list_notifications_by_time (TO BE REMOVED)


Produces a list of notifications sorted by creation date (ascending or descending). Allowed additional filters are:


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|topic|string|*Mandatory
|is_read|boolean|If not used, reads and unreads will be found
|start_time|string|Starting date
|stop_time|string|Stopping date
|order|string|"desc" (default) or "asc"
|from|integer|Starting point (for pagination)
|size|integer|Number or records to retrieve
|get_total|boolean|If true, calculates the grand total of records

No operators are allowed on fields


### list_notifications_by_priority (TO BE RMEMOVED)

Produces a list of notifications sorted by priority. Notifications with higher priority will be shown first, even if they have an older creation date.

When a notification is read, its priority falls behind any non-read notification.


|Field|Type|Description
|---|---|---
|domain|string|Domain to filter on
|deep|boolean|If true, will search in sub-domains
|topic|string|*Mandatory
|from|integer|Starting point (for pagination)
|size|integer|Number or records to retrieve
|get_total|boolean|If true, calculates the grand total of records

No operators are allowed on fields
