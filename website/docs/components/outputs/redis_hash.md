---
title: redis_hash
type: output
---

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the contents of:
     lib/output/redis_hash.go
-->


Sets Redis hash objects using the HMSET command.

```yaml
# Config fields, showing default values
output:
  redis_hash:
    url: tcp://localhost:6379
    key: ""
    walk_metadata: false
    walk_json_object: false
    fields: {}
    max_in_flight: 1
```

The field `key` supports
[interpolation functions](/docs/configuration/interpolation#functions), allowing
you to create a unique key for each message.

The field `fields` allows you to specify an explicit map of field
names to interpolated values, also evaluated per message of a batch:

```yaml
redis_hash:
  url: tcp://localhost:6379
  key: ${!json_field:id}
  fields:
    topic: ${!metadata:kafka_topic}
    partition: ${!metadata:kafka_partition}
    content: ${!json_field:document.text}
```

If the field `walk_metadata` is set to `true` then Benthos
will walk all metadata fields of messages and add them to the list of hash
fields to set.

If the field `walk_json_object` is set to `true` then
Benthos will walk each message as a JSON object, extracting keys and the string
representation of their value and adds them to the list of hash fields to set.

The order of hash field extraction is as follows:

1. Metadata (if enabled)
2. JSON object (if enabled)
3. Explicit fields

Where latter stages will overwrite matching field names of a former stage.

## Performance

This output benefits from sending multiple messages in flight in parallel for
improved performance. You can tune the max number of in flight messages with the
field `max_in_flight`.

## Fields

### `url`

The URL of a Redis server to connect to.


Type: `string`  
Default: `"tcp://localhost:6379"`  

```yaml
# Examples

url: tcp://localhost:6379
```

### `key`

The key for each message, function interpolations should be used to create a unique key per message.
This field supports [interpolation functions](/docs/configuration/interpolation#functions).


Type: `string`  
Default: `""`  

```yaml
# Examples

key: ${!metadata:kafka_key}

key: ${!json_field:doc.id}

key: ${!count:msgs}
```

### `walk_metadata`

Whether all metadata fields of messages should be walked and added to the list of hash fields to set.


Type: `bool`  
Default: `false`  

### `walk_json_object`

Whether to walk each message as a JSON object and add each key/value pair to the list of hash fields to set.


Type: `bool`  
Default: `false`  

### `fields`

A map of key/value pairs to set as hash fields.
This field supports [interpolation functions](/docs/configuration/interpolation#functions).


Type: `object`  
Default: `{}`  

### `max_in_flight`

The maximum number of messages to have in flight at a given time. Increase this to improve throughput.


Type: `number`  
Default: `1`  

