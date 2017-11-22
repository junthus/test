# whale.gcm

| Description: | Use `whale.gcm` to enable apps and extensions to send and receive messages through the [Google Cloud Messaging Service](http://developer.android.com/google/gcm/). |
|---|---|
| Availability: | Since Chrome 35. |
| Permissions: | <span class="code">"gcm"</span> |
| Learn More: | [Implementing GCM Client on Chrome](https://developers.google.com/cloud-messaging/chrome/client) |

<section id="toc">

## Summary

| Properties |
|---|
| [MAX_MESSAGE_SIZE](#property-MAX_MESSAGE_SIZE) |
| Methods |
| [register](#method-register) − `whale.gcm.register(<span>array of string senderIds</span>, <span>function callback</span>)` |
| [unregister](#method-unregister) − `whale.gcm.unregister(<span>function callback</span>)` |
| [send](#method-send) − `whale.gcm.send(<span>object message</span>, <span>function callback</span>)` |
| Events |
| [onMessage](#event-onMessage) |
| [onMessagesDeleted](#event-onMessagesDeleted) |
| [onSendError](#event-onSendError) |

</section>

<section>

<div class="api-reference">

## Properties

| <span class="type_name">`4,096`</span> | `whale.gcm.MAX_MESSAGE_SIZE` | The maximum size (in bytes) of all key/value pairs in a message. |
|---|---|---|

## Methods

<div>

### register

<div class="summary">`whale.gcm.register(<span>array of string senderIds</span>, <span>function callback</span>)`</div>

<div class="description">

Registers the application with GCM. The registration ID will be returned by the `callback`. If `register` is called again with the same list of `senderIds`, the same registration ID will be returned.

| Parameters |
|---|
| array of string | senderIds | 

A list of server IDs that are allowed to send messages to the application. It should contain at least one and no more than 100 sender IDs.
 |
| function | callback | 

Function called when registration completes. It should check [runtime.lastError](/extensions/runtime#property-lastError) for error when `registrationId` is empty.

The _callback_ parameter should be a function that looks like this:

`function(string registrationId) <span class="subdued">{...}</span>;`

| string | registrationId | 
|---|---|

A registration ID assigned to the application by the GCM.
 |
 |

</div>

</div>

<div>

### unregister

<div class="summary">`whale.gcm.unregister(<span>function callback</span>)`</div>

<div class="description">

Unregisters the application from GCM.

| Parameters |
|---|
| function | callback | 

A function called after the unregistration completes. Unregistration was successful if [runtime.lastError](/extensions/runtime#property-lastError) is not set.

The _callback_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### send

<div class="summary">`whale.gcm.send(<span>object message</span>, <span>function callback</span>)`</div>

<div class="description">

Sends a message according to its contents.

| Parameters |
|---|
| object | message | 

A message to send to the other party via GCM.

| string | destinationId | 
|---|---|

The ID of the server to send the message to as assigned by [Google API Console](https://code.google.com/apis/console).
 |
| string | messageId | 

The ID of the message. It must be unique for each message in scope of the applications. See the [Cloud Messaging documentation](cloudMessaging#send_messages) for advice for picking and handling an ID.
 |
| integer | <span class="optional">(optional)</span> timeToLive | 

Time-to-live of the message in seconds. If it is not possible to send the message within that time, an onSendError event will be raised. A time-to-live of 0 indicates that the message should be sent immediately or fail if it's not possible. The maximum and a default value of time-to-live is 86400 seconds (1 day).
 |
| object | data | 

Message data to send to the server. Case-insensitive `goog.` and `google`, as well as case-sensitive `collapse_key` are disallowed as key prefixes. Sum of all key/value pairs should not exceed [gcm.MAX_MESSAGE_SIZE](/extensions/gcm#property-MAX_MESSAGE_SIZE).
 |
 |
| function | callback | 

A function called after the message is successfully queued for sending. [runtime.lastError](/extensions/runtime#property-lastError) should be checked, to ensure a message was sent without problems.

The _callback_ parameter should be a function that looks like this:

`function(string messageId) <span class="subdued">{...}</span>;`

| string | messageId | 
|---|---|

The ID of the message that the callback was issued for.
 |
 |

</div>

</div>

## Events

<div>

### onMessage

<div class="description">

Fired when a message is received through GCM.

<div>

#### addListener

<div class="summary">`whale.gcm.onMessage.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object message) <span class="subdued">{...}</span>;`

| object | message | 
|---|---|

A message received from another party via GCM.

| object | data | 
|---|---|

The message data.
 |
| string | <span class="optional">(optional)</span> from | 

Since Chrome 41.

The sender who issued the message.
 |
| string | <span class="optional">(optional)</span> collapseKey | 

The collapse key of a message. See [Collapsible Messages](cloudMessaging#collapsible_messages) section of Cloud Messaging documentation for details.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onMessagesDeleted

<div class="description">

Fired when a GCM server had to delete messages sent by an app server to the application. See [Messages deleted event](cloudMessaging#messages_deleted_event) section of Cloud Messaging documentation for details on handling this event.

<div>

#### addListener

<div class="summary">`whale.gcm.onMessagesDeleted.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

</div>

</div>

<div>

### onSendError

<div class="description">

Fired when it was not possible to send a message to the GCM server.

<div>

#### addListener

<div class="summary">`whale.gcm.onSendError.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object error) <span class="subdued">{...}</span>;`

| object | error | 
|---|---|

An error that occured while trying to send the message either in Chrome or on the GCM server. Application can retry sending the message with a reasonable backoff and possibly longer time-to-live.

| string | errorMessage | 
|---|---|

The error message describing the problem.
 |
| string | <span class="optional">(optional)</span> messageId | 

The ID of the message with this error, if error is related to a specific message.
 |
| object | details | 

Additional details related to the error, when available.
 |
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>