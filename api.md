# ssb-server

Secure-scuttlebutt API server



## get: async

Get a message by its hash-id.

```bash
get {msgid}
```

```js
get(msgid, cb)
```



## createFeedStream: source

(feed) Fetch messages ordered by their claimed timestamps.

```bash
feed [--live] [--gt index] [--gte index] [--lt index] [--lte index] [--reverse]  [--keys] [--values] [--limit n]
```

```js
createFeedStream({ live:, gt:, gte:, lt:, lte:, reverse:, keys:, values:, limit:, fillCache:, keyEncoding:, valueEncoding: })
```

Create a stream of the data in the database, ordered by the timestamp claimed by the author.
NOTE - the timestamp is not verified, and may be incorrect.
The range queries (gt, gte, lt, lte) filter against this claimed timestap.

 - `live` (boolean, default: `false`): Keep the stream open and emit new messages as they are received.
 - `gt` (greater than), `gte` (greater than or equal) define the lower bound of the range to be streamed. Only records where the key is greater than (or equal to) this option will be included in the range. When `reverse=true` the order will be reversed, but the records streamed will be the same.
 - `lt` (less than), `lte` (less than or equal) define the higher bound of the range to be streamed. Only key/value pairs where the key is less than (or equal to) this option will be included in the range. When `reverse=true` the order will be reversed, but the records streamed will be the same.
 - `reverse` (boolean, default: `false`): a boolean, set true and the stream output will be reversed. Beware that due to the way LevelDB works, a reverse seek will be slower than a forward seek.
 - `keys` (boolean, default: `true`): whether the `data` event should contain keys. If set to `true` and `values` set to `false` then `data` events will simply be keys, rather than objects with a `key` property.
 - `values` (boolean, default: `true`): whether the `data` event should contain values. If set to `true` and `keys` set to `false` then `data` events will simply be values, rather than objects with a `value` property.
 - `limit` (number, default: `-1`): limit the number of results collected by this stream. This number represents a *maximum* number of results and may not be reached if you get to the end of the data first. A value of `-1` means there is no limit. When `reverse=true` the highest keys will be returned instead of the lowest keys.
 - `fillCache` (boolean, default: `false`): wheather LevelDB's LRU-cache should be filled with data read.
 - `keyEncoding` / `valueEncoding` (string): the encoding applied to each read piece of data.



## createLogStream: source

(log) Fetch messages ordered by the time received.

```bash
log [--live] [--gt index] [--gte index] [--lt index] [--lte index] [--reverse]  [--keys] [--values] [--limit n]
```

```js
createLogStream({ live:, gt:, gte:, lt:, lte:, reverse:, keys:, values:, limit:, fillCache:, keyEncoding:, valueEncoding: })
```

Creates a stream of the messages that have been written to this instance, in the order they arrived.
The objects in this stream will be of the form:

```
{ key: Hash, value: Message, timestamp: timestamp }
```

`timestamp` is the time which the message was received.
It is generated by [monotonic-timestamp](https://github.com/dominictarr/monotonic-timestamp).
The range queries (gt, gte, lt, lte) filter against this receive timestap.


 - `live` (boolean, default: `false`): Keep the stream open and emit new messages as they are received.
 - `gt` (greater than), `gte` (greater than or equal) define the lower bound of the range to be streamed. Only records where the key is greater than (or equal to) this option will be included in the range. When `reverse=true` the order will be reversed, but the records streamed will be the same.
 - `lt` (less than), `lte` (less than or equal) define the higher bound of the range to be streamed. Only key/value pairs where the key is less than (or equal to) this option will be included in the range. When `reverse=true` the order will be reversed, but the records streamed will be the same.
 - `reverse` (boolean, default: `false`): a boolean, set true and the stream output will be reversed. Beware that due to the way LevelDB works, a reverse seek will be slower than a forward seek.
 - `keys` (boolean, default: `true`): whether the `data` event should contain keys. If set to `true` and `values` set to `false` then `data` events will simply be keys, rather than objects with a `key` property.
 - `values` (boolean, default: `false`): whether the `data` event should contain values. If set to `true` and `keys` set to `false` then `data` events will simply be values, rather than objects with a `value` property.
 - `limit` (number, default: `-1`): limit the number of results collected by this stream. This number represents a *maximum* number of results and may not be reached if you get to the end of the data first. A value of `-1` means there is no limit. When `reverse=true` the highest keys will be returned instead of the lowest keys.
 - `fillCache` (boolean, default: `false`): wheather LevelDB's LRU-cache should be filled with data read.
 - `keyEncoding` / `valueEncoding` (string): the encoding applied to each read piece of data.



## messagesByType: source

(logt) Retrieve messages with a given type, ordered by receive-time.


```bash
logt --type {type} [--live] [--gt index] [--gte index] [--lt index] [--lte index] [--reverse]  [--keys] [--values] [--limit n]
```

```js
messagesByType({ type:, live:, gt:, gte:, lt:, lte:, reverse:, keys:, values:, limit:, fillCache:, keyEncoding:, valueEncoding: })
```

All messages must have a type, so this is a good way to select messages that an application might use.
Like in createLogStream, the range queries (gt, gte, lt, lte) filter against the receive timestap.

 - `type` (string): The type of the messages to emit.
 - `live` (boolean, default: `false`): Keep the stream open and emit new messages as they are received.
 - `gt` (greater than), `gte` (greater than or equal) define the lower bound of the range to be streamed. Only records where the key is greater than (or equal to) this option will be included in the range. When `reverse=true` the order will be reversed, but the records streamed will be the same.
 - `lt` (less than), `lte` (less than or equal) define the higher bound of the range to be streamed. Only key/value pairs where the key is less than (or equal to) this option will be included in the range. When `reverse=true` the order will be reversed, but the records streamed will be the same.
 - `reverse` (boolean, default: `false`): a boolean, set true and the stream output will be reversed. Beware that due to the way LevelDB works, a reverse seek will be slower than a forward seek.
 - `keys` (boolean, default: `true`): whether the `data` event should contain keys. If set to `true` and `values` set to `false` then `data` events will simply be keys, rather than objects with a `key` property.
 - `values` (boolean, default: `true`): whether the `data` event should contain values. If set to `true` and `keys` set to `false` then `data` events will simply be values, rather than objects with a `value` property.
 - `limit` (number, default: `-1`): limit the number of results collected by this stream. This number represents a *maximum* number of results and may not be reached if you get to the end of the data first. A value of `-1` means there is no limit. When `reverse=true` the highest keys will be returned instead of the lowest keys.
 - `fillCache` (boolean, default: `false`): wheather LevelDB's LRU-cache should be filled with data read.
 - `keyEncoding` / `valueEncoding` (string): the encoding applied to each read piece of data.



## createHistoryStream: source

(hist) Fetch messages from a specific user, ordered by sequence numbers.

```bash
hist {feedid} [seq] [live]
hist --id {feedid} [--seq n] [--live] [--limit n] [--keys] [--values]
```

```js
createHistoryStream(id, seq, live)
createHistoryStream({ id:, seq:, live:, limit:, keys:, values: })
```

`createHistoryStream` and `createUserStream` serve the same purpose.
`createHistoryStream` exists as a separate call because it provides fewer range parameters, which makes it safer for RPC between untrusted peers.

 - `id` (FeedID, required): The id of the feed to fetch.
 - `seq` (number, default: `0`): If `seq > 0`, then only stream messages with sequence numbers greater than `seq`.
 - `live` (boolean, default: `false`): Keep the stream open and emit new messages as they are received.
 - `keys` (boolean, default: `true`): whether the `data` event should contain keys. If set to `true` and `values` set to `false` then `data` events will simply be keys, rather than objects with a `key` property.
 - `values` (boolean, default: `true`): whether the `data` event should contain values. If set to `true` and `keys` set to `false` then `data` events will simply be values, rather than objects with a `value` property.
 - `limit` (number, default: `-1`): limit the number of results collected by this stream. This number represents a *maximum* number of results and may not be reached if you get to the end of the data first. A value of `-1` means there is no limit. When `reverse=true` the highest keys will be returned instead of the lowest keys.


## createUserStream: source

Fetch messages from a specific user, ordered by sequence numbers.

```bash
createUserStream --id {feedid} [--live] [--gt index] [--gte index] [--lt index] [--lte index] [--reverse]  [--keys] [--values] [--limit n]
```

```js
createUserStream({ id:, live:, gt:, gte:, lt:, lte:, reverse:, keys:, values:, limit:, fillCache:, keyEncoding:, valueEncoding: })
```

`createHistoryStream` and `createUserStream` serve the same purpose.
`createHistoryStream` exists as a separate call because it provides fewer range parameters, which makes it safer for RPC between untrusted peers.

The range queries (gt, gte, lt, lte) filter against the sequence number.

 - `id` (FeedID, required): The id of the feed to fetch.
 - `live` (boolean, default: `false`): Keep the stream open and emit new messages as they are received.
 - `gt` (greater than), `gte` (greater than or equal) define the lower bound of the range to be streamed. Only records where the key is greater than (or equal to) this option will be included in the range. When `reverse=true` the order will be reversed, but the records streamed will be the same.
 - `lt` (less than), `lte` (less than or equal) define the higher bound of the range to be streamed. Only key/value pairs where the key is less than (or equal to) this option will be included in the range. When `reverse=true` the order will be reversed, but the records streamed will be the same.
 - `reverse` (boolean, default: `false`): a boolean, set true and the stream output will be reversed. Beware that due to the way LevelDB works, a reverse seek will be slower than a forward seek.
 - `keys` (boolean, default: `true`): whether the `data` event should contain keys. If set to `true` and `values` set to `false` then `data` events will simply be keys, rather than objects with a `key` property.
 - `values` (boolean, default: `true`): whether the `data` event should contain values. If set to `true` and `keys` set to `false` then `data` events will simply be values, rather than objects with a `value` property.
 - `limit` (number, default: `-1`): limit the number of results collected by this stream. This number represents a *maximum* number of results and may not be reached if you get to the end of the data first. A value of `-1` means there is no limit. When `reverse=true` the highest keys will be returned instead of the lowest keys.
 - `fillCache` (boolean, default: `false`): wheather LevelDB's LRU-cache should be filled with data read.
 - `keyEncoding` / `valueEncoding` (string): the encoding applied to each read piece of data.


## createWriteStream: sink

write a number of messages to the local store.
will error if messages are not valid, but will accept
messages that the ssb-server doesn't replicate.


## links: source

Get a stream of messages, feeds, or blobs that are linked to/from an id.

```bash
links [--source id|filter] [--dest id|filter] [--rel value] [--keys] [--values] [--live] [--reverse]
```

```js
links({ source:, dest:, rel:, keys:, values:, live:, reverse: })
```

The objects in this stream will be of the form:

```
{ source: ID, rel: String, dest: ID, key: MsgID }
```

 - `source` (string, optional): An id or filter, specifying where the link should originate from. To filter, just use the sigil of the type you want: `@` for feeds, `%` for messages, and `&` for blobs.
 - `dest` (string, optional): An id or filter, specifying where the link should point to. To filter, just use the sigil of the type you want: `@` for feeds, `%` for messages, and `&` for blobs.
 - `rel` (string, optional): Filters the links by the relation string.
 - `live` (boolean, default: `false`): Keep the stream open and emit new messages as they are received.
 - `reverse` (boolean, default: `false`): a boolean, set true and the stream output will be reversed. Beware that due to the way LevelDB works, a reverse seek will be slower than a forward seek.
 - `keys` (boolean, default: `true`): whether the `data` event should contain keys. If set to `true` and `values` set to `false` then `data` events will simply be keys, rather than objects with a `key` property.
 - `values` (boolean, default: `true`): whether the `data` event should contain values. If set to `true` and `keys` set to `false` then `data` events will simply be values, rather than objects with a `value` property.


## add: async

Add a well-formed message to the database.

```bash
cat ./message.json | add
add --author {feedid} --sequence {number} --previous {msgid} --timestamp {number} --hash sha256 --signature {sig} --content.type {type} --content.{...}
```

```js
add({ author:, sequence:, previous: timestamp:, hash: 'sha256', signature:, content: { type:, ... } }, cb)
```

 - `author` (FeedID): Public key of the author of the message.
 - `sequence` (number): Sequence number of the message. (Starts from 1.)
 - `previous` (MsgID): Hash-id of the previous message in the feed (null for seq=1).
 - `timestamp` (number): Unix timestamp for the publish time.
 - `hash` (string): The hash algorithm used in the message, should always be `sha256`.
 - `signature` (string): A signature computed using the author pubkey and the content of the message (less the `signature` attribute).
 - `content` (object): The content of the message.
   - `.type` (string): The object's type.


## publish: async

Construct a message using ssb-server's current user, and add it to the DB.

```bash
cat ./message-content.json | publish
publish --type {string} [--other-attributes...]
```

```js
publish({ type:, ... }, cb)
```

This is the recommended method for publishing new messages, as it handles the tasks of correctly setting the message's timestamp, sequence number, previous-hash, and signature.

 - `content` (object): The content of the message.
   - `.type` (string): The object's type.




## getAddress: sync

Get the address of the server. Default scope is public.

```bash
getAddress {scope}
```

```js
getAddress(scope, cb)
```



## getLatest: async

Get the latest message in the database by the given feedid.

```bash
getLatest {feedid}
```

```js
getLatest(id, cb)
```



## latest: source

Get the seq numbers of the latest messages of all users in the database.

```bash
latest
```

```js
latest()
```



## latestSequence: async

Get the sequence and local timestamp of the last received message from
a given `feedId`.

```bash
latestSequence {feedId}
```

```js
latest({feedId})
```



## whoami: sync

Get information about the current ssb-server user.

```bash
whoami
```

```js
whoami(cb)
```

Outputs information in the following form:

```
{ id: FeedID }
```



## progress: sync

returns an object reflecting the progress state of various plugins.
the return value is a `{}` with subobjects showing `{start,current,target}`
to represent progress. Currently implemented are `migration` (legacy->flume)
migration progress and `indexes` (index regeneration).


## status: sync

returns an object reflecting the status of various ssb operations,
such as db read activity, connection statuses, etc, etc. The purpose is to provide
an overview of how ssb is working.

## getVectorClock: async

## version: sync

return the current version number of the running server
