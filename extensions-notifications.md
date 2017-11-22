# whale.notifications

| Description: | Use the `whale.notifications` API to create rich notifications using templates and show these notifications to users in the system tray. |
|---|---|
| Availability: | Since Chrome 28. |
| Permissions: | <span class="code">"notifications"</span> |
| Learn More: | [Rich Notifications](richNotifications)
[Keep Users Informed](inform_users)
[Chrome Apps Office Hours: Rich Notifications](https://developers.google.com/live/shows/83992232-1001/) |

<section id="toc">

## Summary

| Types |
|---|
| [TemplateType](#type-TemplateType) |
| [PermissionLevel](#type-PermissionLevel) |
| [NotificationOptions](#type-NotificationOptions) |
| Methods |
| [create](#method-create) − `whale.notifications.create(<span class="optional">string notificationId</span>, <span>NotificationOptions options</span>, <span class="optional">function callback</span>)` |
| [update](#method-update) − `whale.notifications.update(<span>string notificationId</span>, <span>NotificationOptions options</span>, <span class="optional">function callback</span>)` |
| [clear](#method-clear) − `whale.notifications.clear(<span>string notificationId</span>, <span class="optional">function callback</span>)` |
| [getAll](#method-getAll) − `whale.notifications.getAll(<span>function callback</span>)` |
| [getPermissionLevel](#method-getPermissionLevel) − `whale.notifications.getPermissionLevel(<span>function callback</span>)` |
| Events |
| [onClosed](#event-onClosed) |
| [onClicked](#event-onClicked) |
| [onButtonClicked](#event-onButtonClicked) |
| [onPermissionLevelChanged](#event-onPermissionLevelChanged) |
| [onShowSettings](#event-onShowSettings) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### TemplateType

| Enum |
|---|
| 

<dl>

<dt>`"basic"`</dt>

<dd>icon, title, message, expandedMessage, up to two buttons</dd>

</dl>

<dl>

<dt>`"image"`</dt>

<dd>icon, title, message, expandedMessage, image, up to two buttons</dd>

</dl>

<dl>

<dt>`"list"`</dt>

<dd>icon, title, message, items, up to two buttons. Users on Mac OS X only see the first item.</dd>

</dl>

<dl>

<dt>`"progress"`</dt>

<dd>icon, title, message, progress, up to two buttons</dd>

</dl>
 |

</div>

<div>

### PermissionLevel

| Enum |
|---|
| 

<dl>

<dt>`"granted"`</dt>

<dd>User has elected to show notifications from the app or extension. This is the default at install time.</dd>

</dl>

<dl>

<dt>`"denied"`</dt>

<dd>User has elected not to show notifications from the app or extension.</dd>

</dl>
 |

</div>

<div>

### NotificationOptions

| properties |
|---|
| [TemplateType](/extensions/notifications#type-TemplateType) | <span class="optional">(optional)</span> type | 

Which type of notification to display. _Required for [notifications.create](/extensions/notifications#method-create)_ method.
 |
| string | <span class="optional">(optional)</span> iconUrl | 

A URL to the sender's avatar, app icon, or a thumbnail for image notifications.

URLs can be a data URL, a blob URL, or a URL relative to a resource within this extension's .crx file _Required for [notifications.create](/extensions/notifications#method-create)_ method.
 |
| string | <span class="optional">(optional)</span> appIconMaskUrl | 

**Deprecated** since Chrome 59. The app icon mask is not visible for Mac OS X users.

A URL to the app icon mask. URLs have the same restrictions as [iconUrl](/extensions/notifications#property-NotificationOptions-iconUrl).

The app icon mask should be in alpha channel, as only the alpha channel of the image will be considered.
 |
| string | <span class="optional">(optional)</span> title | 

Title of the notification (e.g. sender name for email). _Required for [notifications.create](/extensions/notifications#method-create)_ method.
 |
| string | <span class="optional">(optional)</span> message | 

Main notification content. _Required for [notifications.create](/extensions/notifications#method-create)_ method.
 |
| string | <span class="optional">(optional)</span> contextMessage | 

Since Chrome 31.

Alternate notification content with a lower-weight font.
 |
| integer | <span class="optional">(optional)</span> priority | 

Priority ranges from -2 to 2\. -2 is lowest priority. 2 is highest. Zero is default. On platforms that don't support a notification center (Windows, Linux & Mac), -2 and -1 result in an error as notifications with those priorities will not be shown at all.
 |
| double | <span class="optional">(optional)</span> eventTime | 

A timestamp associated with the notification, in milliseconds past the epoch (e.g. `Date.now() + n`).
 |
| array of object | <span class="optional">(optional)</span> buttons | 

Text and icons for up to two notification action buttons.

#### Properties of each object

<dl>

<div>

| string | title |  |
|---|---|---|
| string | <span class="optional">(optional)</span> iconUrl |  |

</div>

</dl>
 |
| string | <span class="optional">(optional)</span> imageUrl | 

**Deprecated** since Chrome 59. The image is not visible for Mac OS X users.

A URL to the image thumbnail for image-type notifications. URLs have the same restrictions as [iconUrl](/extensions/notifications#property-NotificationOptions-iconUrl).
 |
| array of object | <span class="optional">(optional)</span> items | 

Items for multi-item notifications. Users on Mac OS X only see the first item.

#### Properties of each object

<dl>

<div>

| string | title | 
|---|---|

Title of one item of a list notification.
 |
| string | message | 

Additional details about this item.
 |

</div>

</dl>
 |
| integer | <span class="optional">(optional)</span> progress | 

Since Chrome 30.

Current progress ranges from 0 to 100.
 |
| boolean | <span class="optional">(optional)</span> isClickable | 

Since Chrome 32.

Whether to show UI indicating that the app will visibly respond to clicks on the body of a notification.
 |
| boolean | <span class="optional">(optional)</span> requireInteraction | 

Since Chrome 50.

Indicates that the notification should remain visible on screen until the user activates or dismisses the notification. This defaults to false.
 |

</div>

## Methods

<div>

### create

<div class="summary">`whale.notifications.create(<span class="optional">string notificationId</span>, <span>[NotificationOptions](/extensions/notifications#type-NotificationOptions) options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Creates and displays a notification.

| Parameters |
|---|
| string | <span class="optional">(optional)</span> notificationId | 

Identifier of the notification. If not set or empty, an ID will automatically be generated. If it matches an existing notification, this method first clears that notification before proceeding with the create operation. The identifier may not be longer than 500 characters.

The `notificationId` parameter is required before Chrome 42.
 |
| [NotificationOptions](/extensions/notifications#type-NotificationOptions) | options | 

Contents of the notification.
 |
| function | <span class="optional">(optional)</span> callback | 

Returns the notification id (either supplied or generated) that represents the created notification.

The callback is required before Chrome 42.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(string notificationId) <span class="subdued">{...}</span>;`

| string | notificationId |  |
|---|---|---|
 |

</div>

</div>

<div>

### update

<div class="summary">`whale.notifications.update(<span>string notificationId</span>, <span>[NotificationOptions](/extensions/notifications#type-NotificationOptions) options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Updates an existing notification.

| Parameters |
|---|
| string | notificationId | 

The id of the notification to be updated. This is returned by [notifications.create](/extensions/notifications#method-create) method.
 |
| [NotificationOptions](/extensions/notifications#type-NotificationOptions) | options | 

Contents of the notification to update to.
 |
| function | <span class="optional">(optional)</span> callback | 

Called to indicate whether a matching notification existed.

The callback is required before Chrome 42.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(boolean wasUpdated) <span class="subdued">{...}</span>;`

| boolean | wasUpdated |  |
|---|---|---|
 |

</div>

</div>

<div>

### clear

<div class="summary">`whale.notifications.clear(<span>string notificationId</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears the specified notification.

| Parameters |
|---|
| string | notificationId | 

The id of the notification to be cleared. This is returned by [notifications.create](/extensions/notifications#method-create) method.
 |
| function | <span class="optional">(optional)</span> callback | 

Called to indicate whether a matching notification existed.

The callback is required before Chrome 42.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(boolean wasCleared) <span class="subdued">{...}</span>;`

| boolean | wasCleared |  |
|---|---|---|
 |

</div>

</div>

<div>

### getAll

<div class="summary">`whale.notifications.getAll(<span>function callback</span>)`</div>

<div class="description">

Since Chrome 29.

Retrieves all the notifications.

| Parameters |
|---|
| function | callback | 

Returns the set of notification_ids currently in the system.

The _callback_ parameter should be a function that looks like this:

`function(object notifications) <span class="subdued">{...}</span>;`

| object | notifications |  |
|---|---|---|
 |

</div>

</div>

<div>

### getPermissionLevel

<div class="summary">`whale.notifications.getPermissionLevel(<span>function callback</span>)`</div>

<div class="description">

Since Chrome 32.

Retrieves whether the user has enabled notifications from this app or extension.

| Parameters |
|---|
| function | callback | 

Returns the current permission level.

The _callback_ parameter should be a function that looks like this:

`function( [PermissionLevel](/extensions/notifications#type-PermissionLevel) level) <span class="subdued">{...}</span>;`

| [PermissionLevel](/extensions/notifications#type-PermissionLevel) | level |  |
|---|---|---|
 |

</div>

</div>

## Events

<div>

### onClosed

<div class="description">

The notification closed, either by the system or by user action.

<div>

#### addListener

<div class="summary">`whale.notifications.onClosed.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string notificationId, boolean byUser) <span class="subdued">{...}</span>;`

| string | notificationId |  |
|---|---|---|
| boolean | byUser |  |
 |

</div>

</div>

</div>

</div>

<div>

### onClicked

<div class="description">

The user clicked in a non-button area of the notification.

<div>

#### addListener

<div class="summary">`whale.notifications.onClicked.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string notificationId) <span class="subdued">{...}</span>;`

| string | notificationId |  |
|---|---|---|
 |

</div>

</div>

</div>

</div>

<div>

### onButtonClicked

<div class="description">

The user pressed a button in the notification.

<div>

#### addListener

<div class="summary">`whale.notifications.onButtonClicked.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string notificationId, integer buttonIndex) <span class="subdued">{...}</span>;`

| string | notificationId |  |
|---|---|---|
| integer | buttonIndex |  |
 |

</div>

</div>

</div>

</div>

<div>

### onPermissionLevelChanged

<div class="description">

Since Chrome 32.

The user changes the permission level. As of Chrome 47, only ChromeOS has UI that dispatches this event.

<div>

#### addListener

<div class="summary">`whale.notifications.onPermissionLevelChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [PermissionLevel](/extensions/notifications#type-PermissionLevel) level) <span class="subdued">{...}</span>;`

| [PermissionLevel](/extensions/notifications#type-PermissionLevel) | level |  |
|---|---|---|
 |

</div>

</div>

</div>

</div>

<div>

### onShowSettings

<div class="description">

Since Chrome 32.

The user clicked on a link for the app's notification settings. As of Chrome 47, only ChromeOS has UI that dispatches this event.

<div>

#### addListener

<div class="summary">`whale.notifications.onShowSettings.addListener(<span>function callback</span>)`</div>

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