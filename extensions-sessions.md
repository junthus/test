# whale.sessions

| Description: | Use the `whale.sessions` API to query and restore tabs and windows from a browsing session. |
|---|---|
| Availability: | Since Chrome 37. |
| Permissions: | <span class="code">"sessions"</span> |

<section id="toc">

## Summary

| Types |
|---|
| [Filter](#type-Filter) |
| [Session](#type-Session) |
| [Device](#type-Device) |
| Properties |
| [MAX_SESSION_RESULTS](#property-MAX_SESSION_RESULTS) |
| Methods |
| [getRecentlyClosed](#method-getRecentlyClosed) − `whale.sessions.getRecentlyClosed( <span class="optional">Filter filter</span>, <span>function callback</span>)` |
| [getDevices](#method-getDevices) − `whale.sessions.getDevices( <span class="optional">Filter filter</span>, <span>function callback</span>)` |
| [restore](#method-restore) − `whale.sessions.restore(<span class="optional">string sessionId</span>, <span class="optional">function callback</span>)` |
| Events |
| [onChanged](#event-onChanged) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### Filter

| properties |
|---|
| integer | <span class="optional">(optional)</span> maxResults | 

The maximum number of entries to be fetched in the requested list. Omit this parameter to fetch the maximum number of entries ([sessions.MAX_SESSION_RESULTS](/extensions/sessions#property-MAX_SESSION_RESULTS)).
 |

</div>

<div>

### Session

| properties |
|---|
| integer | lastModified | 

The time when the window or tab was closed or modified, represented in milliseconds since the epoch.
 |
| [tabs.Tab](/extensions/tabs#type-Tab) | <span class="optional">(optional)</span> tab | 

The [tabs.Tab](/extensions/tabs#type-Tab), if this entry describes a tab. Either this or [sessions.Session.window](/extensions/sessions#property-Session-window) will be set.
 |
| [windows.Window](/extensions/windows#type-Window) | <span class="optional">(optional)</span> window | 

The [windows.Window](/extensions/windows#type-Window), if this entry describes a window. Either this or [sessions.Session.tab](/extensions/sessions#property-Session-tab) will be set.
 |

</div>

<div>

### Device

| properties |
|---|
| string | deviceName | 

The name of the foreign device.
 |
| array of [Session](/extensions/sessions#type-Session) | sessions | 

A list of open window sessions for the foreign device, sorted from most recently to least recently modified session.
 |

</div>

## Properties

| <span class="type_name">`25`</span> | `whale.sessions.MAX_SESSION_RESULTS` | The maximum number of [sessions.Session](/extensions/sessions#type-Session) that will be included in a requested list. |
|---|---|---|

## Methods

<div>

### getRecentlyClosed

<div class="summary">`whale.sessions.getRecentlyClosed( <span class="optional">[Filter](/extensions/sessions#type-Filter) filter</span>, <span>function callback</span>)`</div>

<div class="description">

Gets the list of recently closed tabs and/or windows.

| Parameters |
|---|
| [Filter](/extensions/sessions#type-Filter) | <span class="optional">(optional)</span> filter |  |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [Session](/extensions/sessions#type-Session) sessions) <span class="subdued">{...}</span>;`

| array of [Session](/extensions/sessions#type-Session) | sessions | 
|---|---|

The list of closed entries in reverse order that they were closed (the most recently closed tab or window will be at index `0`). The entries may contain either tabs or windows.
 |
 |

</div>

</div>

<div>

### getDevices

<div class="summary">`whale.sessions.getDevices( <span class="optional">[Filter](/extensions/sessions#type-Filter) filter</span>, <span>function callback</span>)`</div>

<div class="description">

Retrieves all devices with synced sessions.

| Parameters |
|---|
| [Filter](/extensions/sessions#type-Filter) | <span class="optional">(optional)</span> filter |  |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [Device](/extensions/sessions#type-Device) devices) <span class="subdued">{...}</span>;`

| array of [Device](/extensions/sessions#type-Device) | devices | 
|---|---|

The list of [sessions.Device](/extensions/sessions#type-Device) objects for each synced session, sorted in order from device with most recently modified session to device with least recently modified session. [tabs.Tab](/extensions/tabs#type-Tab) objects are sorted by recency in the [windows.Window](/extensions/windows#type-Window) of the [sessions.Session](/extensions/sessions#type-Session) objects.
 |
 |

</div>

</div>

<div>

### restore

<div class="summary">`whale.sessions.restore(<span class="optional">string sessionId</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Reopens a [windows.Window](/extensions/windows#type-Window) or [tabs.Tab](/extensions/tabs#type-Tab), with an optional callback to run when the entry has been restored.

| Parameters |
|---|
| string | <span class="optional">(optional)</span> sessionId | 

The [windows.Window.sessionId](/extensions/windows#property-Window-sessionId), or [tabs.Tab.sessionId](/extensions/tabs#property-Tab-sessionId) to restore. If this parameter is not specified, the most recently closed session is restored.
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [Session](/extensions/sessions#type-Session) restoredSession) <span class="subdued">{...}</span>;`

| [Session](/extensions/sessions#type-Session) | restoredSession | 
|---|---|

A [sessions.Session](/extensions/sessions#type-Session) containing the restored [windows.Window](/extensions/windows#type-Window) or [tabs.Tab](/extensions/tabs#type-Tab) object.
 |
 |

</div>

</div>

## Events

<div>

### onChanged

<div class="description">

Fired when recently closed tabs and/or windows are changed. This event does not monitor synced sessions changes.

<div>

#### addListener

<div class="summary">`whale.sessions.onChanged.addListener(<span>function callback</span>)`</div>

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