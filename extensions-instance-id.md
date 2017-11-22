# whale.instanceID

| Description: | Use `whale.instanceID` to access the Instance ID service. |
|---|---|
| Availability: | Since Chrome 46. |
| Permissions: | <span class="code">"gcm"</span> |

<section id="toc">

## Summary

| Methods |
|---|
| [getID](#method-getID) − `whale.instanceID.getID(<span>function callback</span>)` |
| [getCreationTime](#method-getCreationTime) − `whale.instanceID.getCreationTime(<span>function callback</span>)` |
| [getToken](#method-getToken) − `whale.instanceID.getToken(<span>object getTokenParams</span>, <span>function callback</span>)` |
| [deleteToken](#method-deleteToken) − `whale.instanceID.deleteToken(<span>object deleteTokenParams</span>, <span>function callback</span>)` |
| [deleteID](#method-deleteID) − `whale.instanceID.deleteID(<span>function callback</span>)` |
| Events |
| [onTokenRefresh](#event-onTokenRefresh) |

</section>

<section>

<div class="api-reference">

## Methods

<div>

### getID

<div class="summary">`whale.instanceID.getID(<span>function callback</span>)`</div>

<div class="description">

Retrieves an identifier for the app instance. The instance ID will be returned by the `callback`. The same ID will be returned as long as the application identity has not been revoked or expired.

| Parameters |
|---|
| function | callback | 

Function called when the retrieval completes. It should check [runtime.lastError](/extensions/runtime#property-lastError) for error when instanceID is empty.

The _callback_ parameter should be a function that looks like this:

`function(string instanceID) <span class="subdued">{...}</span>;`

| string | instanceID | 
|---|---|

An Instance ID assigned to the app instance.
 |
 |

</div>

</div>

<div>

### getCreationTime

<div class="summary">`whale.instanceID.getCreationTime(<span>function callback</span>)`</div>

<div class="description">

Retrieves the time when the InstanceID has been generated. The creation time will be returned by the `callback`.

| Parameters |
|---|
| function | callback | 

Function called when the retrieval completes. It should check [runtime.lastError](/extensions/runtime#property-lastError) for error when creationTime is zero.

The _callback_ parameter should be a function that looks like this:

`function(double creationTime) <span class="subdued">{...}</span>;`

| double | creationTime | 
|---|---|

The time when the Instance ID has been generated, represented in milliseconds since the epoch.
 |
 |

</div>

</div>

<div>

### getToken

<div class="summary">`whale.instanceID.getToken(<span>object getTokenParams</span>, <span>function callback</span>)`</div>

<div class="description">

Return a token that allows the authorized entity to access the service defined by scope.

| Parameters |
|---|
| object | getTokenParams | 

Parameters for getToken.

| string | authorizedEntity | 
|---|---|

Identifies the entity that is authorized to access resources associated with this Instance ID. It can be a project ID from [Google developer console](https://code.google.com/apis/console).
 |
| string | scope | 

Identifies authorized actions that the authorized entity can take. E.g. for sending GCM messages, `GCM` scope should be used.
 |
| object | <span class="optional">(optional)</span> options | 

Allows including a small number of string key/value pairs that will be associated with the token and may be used in processing the request.
 |
 |
| function | callback | 

Function called when the retrieval completes. It should check [runtime.lastError](/extensions/runtime#property-lastError) for error when token is empty.

The _callback_ parameter should be a function that looks like this:

`function(string token) <span class="subdued">{...}</span>;`

| string | token | 
|---|---|

A token assigned by the requested service.
 |
 |

</div>

</div>

<div>

### deleteToken

<div class="summary">`whale.instanceID.deleteToken(<span>object deleteTokenParams</span>, <span>function callback</span>)`</div>

<div class="description">

Revokes a granted token.

| Parameters |
|---|
| object | deleteTokenParams | 

Parameters for deleteToken.

| string | authorizedEntity | 
|---|---|

The authorized entity that is used to obtain the token.
 |
| string | scope | 

The scope that is used to obtain the token.
 |
 |
| function | callback | 

Function called when the token deletion completes. The token was revoked successfully if [runtime.lastError](/extensions/runtime#property-lastError) is not set.

The _callback_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### deleteID

<div class="summary">`whale.instanceID.deleteID(<span>function callback</span>)`</div>

<div class="description">

Resets the app instance identifier and revokes all tokens associated with it.

| Parameters |
|---|
| function | callback | 

Function called when the deletion completes. The instance identifier was revoked successfully if [runtime.lastError](/extensions/runtime#property-lastError) is not set.

The _callback_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

## Events

<div>

### onTokenRefresh

<div class="description">

Fired when all the granted tokens need to be refreshed.

<div>

#### addListener

<div class="summary">`whale.instanceID.onTokenRefresh.addListener(<span>function callback</span>)`</div>

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

</div>

</section>