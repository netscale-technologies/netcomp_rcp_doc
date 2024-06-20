# API

## External Events

These APIs allow to create external-created events that will be stored in the core and sent to Kafa



### event_create

|Field|Type|Mandatory|Description
|---|---|---|---
class|string|Y|"watch-event"
type|string|Y|Subtype of the event
target|string|N|Target of the event, if available (like patient uid, or watch uid)
payload|object|Y|Body of event

Events's uid will be returned, and kafka message will be sent

Kafka generated event is documented [here](emulated_events.md#external-event)
