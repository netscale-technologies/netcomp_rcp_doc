# List labels

This command allows to find existing device hubs labels and ids.

A POST request must be send to `/rcp/list-labels`, including the following JSON:

|Field|Type|Mandatory|Description
|---|---|---|---
|class|string|Y|Class of labels
|batch|string|Y|Current batch
|start|number|N|Serial to start at (default 0)
|size|number|N|Number of labels to return (default 10)

Returns:


|Field|Type|Description
|---|---|---
|devices|\[device]|List of devices

Device is:

|Field|Type|Mandatory|Description
|---|---|---|---
|label|string|Y|Label of device
|id|string|Y|Id of device

