# whale.runtime

| Description: | Use the `whale.runtime` API to retrieve the background page, return details about the manifest, and listen for and respond to events in the app or extension lifecycle. You can also use this API to convert the relative path of URLs to fully-qualified URLs. |
|---|---|
| Availability: | Since Chrome 22. |
| Content Scripts: | [connect](/extensions/runtime#method-connect) , [getManifest](/extensions/runtime#method-getManifest) , [getURL](/extensions/runtime#method-getURL) , [id](/extensions/runtime#property-id) , [onConnect](/extensions/runtime#event-onConnect) , [onMessage](/extensions/runtime#event-onMessage) and [sendMessage](/extensions/runtime#method-sendMessage) are supported. [Learn more](content_scripts) |
| Learn More: | [Event Pages](event_pages) |

<section id="toc">

## Summary

| Types |
|---|
| [Port](#type-Port) |
| [MessageSender](#type-MessageSender) |
| [PlatformOs](#type-PlatformOs) |
| [PlatformArch](#type-PlatformArch) |
| [PlatformNaclArch](#type-PlatformNaclArch) |
| [PlatformInfo](#type-PlatformInfo) |
| [RequestUpdateCheckStatus](#type-RequestUpdateCheckStatus) |
| [OnInstalledReason](#type-OnInstalledReason) |
| [OnRestartRequiredReason](#type-OnRestartRequiredReason) |
| Properties |
| [lastError](#property-lastError) |
| [id](#property-id) |
| Methods |
| [getBackgroundPage](#method-getBackgroundPage) − `whale.runtime.getBackgroundPage(<span>function callback</span>)` |
| [openOptionsPage](#method-openOptionsPage) − `whale.runtime.openOptionsPage(<span class="optional">function callback</span>)` |
| [getManifest](#method-getManifest) − `object whale.runtime.getManifest()` |
| [getURL](#method-getURL) − `string whale.runtime.getURL(<span>string path</span>)` |
| [setUninstallURL](#method-setUninstallURL) − `whale.runtime.setUninstallURL(<span>string url</span>, <span class="optional">function callback</span>)` |
| [reload](#method-reload) − `whale.runtime.reload()` |
| [requestUpdateCheck](#method-requestUpdateCheck) − `whale.runtime.requestUpdateCheck(<span>function callback</span>)` |
| [restart](#method-restart) − `whale.runtime.restart()` |
| [restartAfterDelay](#method-restartAfterDelay) − `whale.runtime.restartAfterDelay(<span>integer seconds</span>, <span class="optional">function callback</span>)` |
| [connect](#method-connect) − `Port whale.runtime.connect(<span class="optional">string extensionId</span>, <span class="optional">object connectInfo</span>)` |
| [connectNative](#method-connectNative) − `Port whale.runtime.connectNative(<span>string application</span>)` |
| [sendMessage](#method-sendMessage) − `whale.runtime.sendMessage(<span class="optional">string extensionId</span>, <span>any message</span>, <span class="optional">object options</span>, <span class="optional">function responseCallback</span>)` |
| [sendNativeMessage](#method-sendNativeMessage) − `whale.runtime.sendNativeMessage(<span>string application</span>, <span>object message</span>, <span class="optional">function responseCallback</span>)` |
| [getPlatformInfo](#method-getPlatformInfo) − `whale.runtime.getPlatformInfo(<span>function callback</span>)` |
| [getPackageDirectoryEntry](#method-getPackageDirectoryEntry) − `whale.runtime.getPackageDirectoryEntry(<span>function callback</span>)` |
| Events |
| [onStartup](#event-onStartup) |
| [onInstalled](#event-onInstalled) |
| [onSuspend](#event-onSuspend) |
| [onSuspendCanceled](#event-onSuspendCanceled) |
| [onUpdateAvailable](#event-onUpdateAvailable) |
| [onBrowserUpdateAvailable](#event-onBrowserUpdateAvailable) |
| [onConnect](#event-onConnect) |
| [onConnectExternal](#event-onConnectExternal) |
| [onMessage](#event-onMessage) |
| [onMessageExternal](#event-onMessageExternal) |
| [onRestartRequired](#event-onRestartRequired) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### Port

Since Chrome 26.

<dd>An object which allows two way communication with other pages. See [Long-lived connections](messaging#connect) for more information.</dd>

| properties |
|---|
| string | name | 

The name of the port, as specified in the call to [runtime.connect](/extensions/runtime#method-connect).
 |
| function | disconnect | 

Immediately disconnect the port. Calling `disconnect()` on an already-disconnected port has no effect. When a port is disconnected, no new events will be dispatched to this port.
 |
| object | onDisconnect | 

Fired when the port is disconnected from the other end(s). [runtime.lastError](/extensions/runtime#property-lastError) may be set if the port was disconnected by an error. If the port is closed via [disconnect](/extensions/runtime#property-Port-disconnect), then this event is _only_ fired on the other end. This event is fired at most once (see also [Port lifetime](messaging#port-lifetime)). The first and only parameter to the event handler is this disconnected port.
 |
| object | onMessage | 

This event is fired when [postMessage](/extensions/runtime#property-Port-postMessage) is called by the other end of the port. The first parameter is the message, the second parameter is the port that received the message.
 |
| function | postMessage | 

Send a message to the other end of the port. If the port is disconnected, an error is thrown.

| Parameters |
|---|
| any | message | 

Since Chrome 52.

The message to send. This object should be JSON-ifiable.
 |
 |
| [MessageSender](/extensions/runtime#type-MessageSender) | <span class="optional">(optional)</span> sender | 

This property will **only** be present on ports passed to [onConnect](/extensions/runtime#event-onConnect) / [onConnectExternal](/extensions/runtime#event-onConnectExternal) listeners.
 |

</div>

<div>

### MessageSender

Since Chrome 26.

<dd>An object containing information about the script context that sent a message or request.</dd>

| properties |
|---|
| [tabs.Tab](/extensions/tabs#type-Tab) | <span class="optional">(optional)</span> tab | 

The [tabs.Tab](/extensions/tabs#type-Tab) which opened the connection, if any. This property will **only** be present when the connection was opened from a tab (including content scripts), and **only** if the receiver is an extension, not an app.
 |
| integer | <span class="optional">(optional)</span> frameId | 

Since Chrome 41.

The [frame](webNavigation#frame_ids) that opened the connection. 0 for top-level frames, positive for child frames. This will only be set when `tab` is set.
 |
| string | <span class="optional">(optional)</span> id | 

The ID of the extension or app that opened the connection, if any.
 |
| string | <span class="optional">(optional)</span> url | 

Since Chrome 28.

The URL of the page or frame that opened the connection. If the sender is in an iframe, it will be iframe's URL not the URL of the page which hosts it.
 |
| string | <span class="optional">(optional)</span> tlsChannelId | 

Since Chrome 32.

The TLS channel ID of the page or frame that opened the connection, if requested by the extension or app, and if available.
 |

</div>

<div>

### PlatformOs

<dd>The operating system chrome is running on.</dd>

| Enum |
|---|
| `"mac"`, `"win"`, `"android"`, `"cros"`, `"linux"`, or `"openbsd"` |

</div>

<div>

### PlatformArch

<dd>The machine's processor architecture.</dd>

| Enum |
|---|
| `"arm"`, `"x86-32"`, or `"x86-64"` |

</div>

<div>

### PlatformNaclArch

<dd>The native client architecture. This may be different from arch on some platforms.</dd>

| Enum |
|---|
| `"arm"`, `"x86-32"`, or `"x86-64"` |

</div>

<div>

### PlatformInfo

Since Chrome 36.

<dd>An object containing information about the current platform.</dd>

| properties |
|---|
| [PlatformOs](/extensions/runtime#type-PlatformOs) | os | 

The operating system chrome is running on.
 |
| [PlatformArch](/extensions/runtime#type-PlatformArch) | arch | 

The machine's processor architecture.
 |
| [PlatformNaclArch](/extensions/runtime#type-PlatformNaclArch) | nacl_arch | 

The native client architecture. This may be different from arch on some platforms.
 |

</div>

<div>

### RequestUpdateCheckStatus

<dd>Result of the update check.</dd>

| Enum |
|---|
| `"throttled"`, `"no_update"`, or `"update_available"` |

</div>

<div>

### OnInstalledReason

<dd>The reason that this event is being dispatched.</dd>

| Enum |
|---|
| `"install"`, `"update"`, `"chrome_update"`, or `"shared_module_update"` |

</div>

<div>

### OnRestartRequiredReason

<dd>The reason that the event is being dispatched. 'app_update' is used when the restart is needed because the application is updated to a newer version. 'os_update' is used when the restart is needed because the browser/OS is updated to a newer version. 'periodic' is used when the system runs for more than the permitted uptime set in the enterprise policy.</dd>

| Enum |
|---|
| `"app_update"`, `"os_update"`, or `"periodic"` |

</div>

## Properties

| <span class="type_name">object</span> | `whale.runtime.lastError` | This will be defined during an API method callback if there was an error

| Properties |
|---|
| --- |
| string | <span class="optional">(optional)</span> message | 

Details about the error which occurred.
 |
 |
| <span class="type_name">string</span> | `whale.runtime.id` | The ID of the extension/app. |

## Methods

<div>

### getBackgroundPage

<div class="summary">`whale.runtime.getBackgroundPage(<span>function callback</span>)`</div>

<div class="description">

Retrieves the JavaScript 'window' object for the background page running inside the current extension/app. If the background page is an event page, the system will ensure it is loaded before calling the callback. If there is no background page, an error is set.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(Window backgroundPage) <span class="subdued">{...}</span>;`

| Window | <span class="optional">(optional)</span> backgroundPage | 
|---|---|

The JavaScript 'window' object for the background page.
 |
 |

</div>

</div>

<div>

### openOptionsPage

<div class="summary">`whale.runtime.openOptionsPage(<span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 42.

Open your Extension's options page, if possible.

The precise behavior may depend on your manifest's `[options_ui](optionsV2)` or `[options_page](options)` key, or what Chrome happens to support at the time. For example, the page may be opened in a new tab, within chrome://extensions, within an App, or it may just focus an open options page. It will never cause the caller page to reload.

If your Extension does not declare an options page, or Chrome failed to create one for some other reason, the callback will set [lastError](/extensions/runtime#property-lastError).

| Parameters |
|---|
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### getManifest

<div class="summary">`object whale.runtime.getManifest()`</div>

<div class="description">

Returns details about the app or extension from the manifest. The object returned is a serialization of the full [manifest file](manifest.html).

#### Returns

<div>

<dd>The manifest details.</dd>

</div>

</div>

</div>

<div>

### getURL

<div class="summary">`string whale.runtime.getURL(<span>string path</span>)`</div>

<div class="description">

Converts a relative path within an app/extension install directory to a fully-qualified URL.

| Parameters |
|---|
| string | path | 

A path to a resource within an app/extension expressed relative to its install directory.
 |

</div>

</div>

<div>

### setUninstallURL

<div class="summary">`whale.runtime.setUninstallURL(<span>string url</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 41.

Sets the URL to be visited upon uninstallation. This may be used to clean up server-side data, do analytics, and implement surveys. Maximum 255 characters.

| Parameters |
|---|
| string | url | 

Since Chrome 34.

URL to be opened after the extension is uninstalled. This URL must have an http: or https: scheme. Set an empty string to not open a new tab upon uninstallation.
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the uninstall URL is set. If the given URL is invalid, [runtime.lastError](/extensions/runtime#property-lastError) will be set.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### reload

<div class="summary">`whale.runtime.reload()`</div>

<div class="description">

Since Chrome 25.

Reloads the app or extension. This method is not supported in kiosk mode. For kiosk mode, use whale.runtime.restart() method.

</div>

</div>

<div>

### requestUpdateCheck

<div class="summary">`whale.runtime.requestUpdateCheck(<span>function callback</span>)`</div>

<div class="description">

Since Chrome 25.

Requests an immediate update check be done for this app/extension.

**Important**: Most extensions/apps should **not** use this method, since chrome already does automatic checks every few hours, and you can listen for the [runtime.onUpdateAvailable](/extensions/runtime#event-onUpdateAvailable) event without needing to call requestUpdateCheck.

This method is only appropriate to call in very limited circumstances, such as if your extension/app talks to a backend service, and the backend service has determined that the client extension/app version is very far out of date and you'd like to prompt a user to update. Most other uses of requestUpdateCheck, such as calling it unconditionally based on a repeating timer, probably only serve to waste client, network, and server resources.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [RequestUpdateCheckStatus](/extensions/runtime#type-RequestUpdateCheckStatus) status, object details) <span class="subdued">{...}</span>;`

| [RequestUpdateCheckStatus](/extensions/runtime#type-RequestUpdateCheckStatus) | status | 
|---|---|

Result of the update check.
 |
| object | <span class="optional">(optional)</span> details | 

If an update is available, this contains more information about the available update.

| string | version | 
|---|---|

The version of the available update.
 |
 |
 |

</div>

</div>

<div>

### restart

<div class="summary">`whale.runtime.restart()`</div>

<div class="description">

Since Chrome 32.

Restart the ChromeOS device when the app runs in kiosk mode. Otherwise, it's no-op.

</div>

</div>

<div>

### restartAfterDelay

<div class="summary">`whale.runtime.restartAfterDelay(<span>integer seconds</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 53.

Restart the ChromeOS device when the app runs in kiosk mode after the given seconds. If called again before the time ends, the reboot will be delayed. If called with a value of -1, the reboot will be cancelled. It's a no-op in non-kiosk mode. It's only allowed to be called repeatedly by the first extension to invoke this API.

| Parameters |
|---|
| integer | seconds | 

Time to wait in seconds before rebooting the device, or -1 to cancel a scheduled reboot.
 |
| function | <span class="optional">(optional)</span> callback | 

A callback to be invoked when a restart request was successfully rescheduled.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### connect

<div class="summary">`[Port](/extensions/runtime#type-Port) whale.runtime.connect(<span class="optional">string extensionId</span>, <span class="optional">object connectInfo</span>)`</div>

<div class="description">

Since Chrome 26.

Attempts to connect to connect listeners within an extension/app (such as the background page), or other extensions/apps. This is useful for content scripts connecting to their extension processes, inter-app/extension communication, and [web messaging](manifest/externally_connectable.html). Note that this does not connect to any listeners in a content script. Extensions may connect to content scripts embedded in tabs via [tabs.connect](/extensions/tabs#method-connect).

| Parameters |
|---|
| string | <span class="optional">(optional)</span> extensionId | 

The ID of the extension or app to connect to. If omitted, a connection will be attempted with your own extension. Required if sending messages from a web page for [web messaging](manifest/externally_connectable.html).
 |
| object | <span class="optional">(optional)</span> connectInfo | 

| string | <span class="optional">(optional)</span> name | 
|---|---|

Will be passed into onConnect for processes that are listening for the connection event.
 |
| boolean | <span class="optional">(optional)</span> includeTlsChannelId | 

Since Chrome 32.

Whether the TLS channel ID will be passed into onConnectExternal for processes that are listening for the connection event.
 |
 |

</div>

</div>

<div>

### connectNative

<div class="summary">`[Port](/extensions/runtime#type-Port) whale.runtime.connectNative(<span>string application</span>)`</div>

<div class="description">

Since Chrome 28.

Connects to a native application in the host machine. See [Native Messaging](nativeMessaging) for more information.

| Parameters |
|---|
| string | application | 

The name of the registered application to connect to.
 |

</div>

</div>

<div>

### sendMessage

<div class="summary">`whale.runtime.sendMessage(<span class="optional">string extensionId</span>, <span>any message</span>, <span class="optional">object options</span>, <span class="optional">function responseCallback</span>)`</div>

<div class="description">

Since Chrome 26.

Sends a single message to event listeners within your extension/app or a different extension/app. Similar to [runtime.connect](/extensions/runtime#method-connect) but only sends a single message, with an optional response. If sending to your extension, the [runtime.onMessage](/extensions/runtime#event-onMessage) event will be fired in every frame of your extension (except for the sender's frame), or [runtime.onMessageExternal](/extensions/runtime#event-onMessageExternal), if a different extension. Note that extensions cannot send messages to content scripts using this method. To send messages to content scripts, use [tabs.sendMessage](/extensions/tabs#method-sendMessage).

| Parameters |
|---|
| string | <span class="optional">(optional)</span> extensionId | 

The ID of the extension/app to send the message to. If omitted, the message will be sent to your own extension/app. Required if sending messages from a web page for [web messaging](manifest/externally_connectable.html).
 |
| any | message | 

The message to send. This message should be a JSON-ifiable object.
 |
| object | <span class="optional">(optional)</span> options | 

Since Chrome 32.

| boolean | <span class="optional">(optional)</span> includeTlsChannelId | 
|---|---|

Whether the TLS channel ID will be passed into onMessageExternal for processes that are listening for the connection event.
 |
 |
| function | <span class="optional">(optional)</span> responseCallback | 

If you specify the _responseCallback_ parameter, it should be a function that looks like this:

`function(any response) <span class="subdued">{...}</span>;`

| any | response | 
|---|---|

The JSON response object sent by the handler of the message. If an error occurs while connecting to the extension, the callback will be called with no arguments and [runtime.lastError](/extensions/runtime#property-lastError) will be set to the error message.
 |
 |

</div>

</div>

<div>

### sendNativeMessage

<div class="summary">`whale.runtime.sendNativeMessage(<span>string application</span>, <span>object message</span>, <span class="optional">function responseCallback</span>)`</div>

<div class="description">

Since Chrome 28.

Send a single message to a native application.

| Parameters |
|---|
| string | application | 

The name of the native messaging host.
 |
| object | message | 

The message that will be passed to the native messaging host.
 |
| function | <span class="optional">(optional)</span> responseCallback | 

If you specify the _responseCallback_ parameter, it should be a function that looks like this:

`function(any response) <span class="subdued">{...}</span>;`

| any | response | 
|---|---|

The response message sent by the native messaging host. If an error occurs while connecting to the native messaging host, the callback will be called with no arguments and [runtime.lastError](/extensions/runtime#property-lastError) will be set to the error message.
 |
 |

</div>

</div>

<div>

### getPlatformInfo

<div class="summary">`whale.runtime.getPlatformInfo(<span>function callback</span>)`</div>

<div class="description">

Since Chrome 29.

Returns information about the current platform.

| Parameters |
|---|
| function | callback | 

Called with results

The _callback_ parameter should be a function that looks like this:

`function( [PlatformInfo](/extensions/runtime#type-PlatformInfo) platformInfo) <span class="subdued">{...}</span>;`

| [PlatformInfo](/extensions/runtime#type-PlatformInfo) | platformInfo |  |
|---|---|---|
 |

</div>

</div>

<div>

### getPackageDirectoryEntry

<div class="summary">`whale.runtime.getPackageDirectoryEntry(<span>function callback</span>)`</div>

<div class="description">

Since Chrome 29.

Returns a DirectoryEntry for the package directory.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(DirectoryEntry directoryEntry) <span class="subdued">{...}</span>;`

| DirectoryEntry | directoryEntry |  |
|---|---|---|
 |

</div>

</div>

## Events

<div>

### onStartup

<div class="description">

Since Chrome 23.

Fired when a profile that has this extension installed first starts up. This event is not fired when an incognito profile is started, even if this extension is operating in 'split' incognito mode.

<div>

#### addListener

<div class="summary">`whale.runtime.onStartup.addListener(<span>function callback</span>)`</div>

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

### onInstalled

<div class="description">

Fired when the extension is first installed, when the extension is updated to a new version, and when Chrome is updated to a new version.

<div>

#### addListener

<div class="summary">`whale.runtime.onInstalled.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

Since Chrome 23.

| [OnInstalledReason](/extensions/runtime#type-OnInstalledReason) | reason | 
|---|---|

The reason that this event is being dispatched.
 |
| string | <span class="optional">(optional)</span> previousVersion | 

Indicates the previous version of the extension, which has just been updated. This is present only if 'reason' is 'update'.
 |
| string | <span class="optional">(optional)</span> id | 

Since Chrome 29.

Indicates the ID of the imported shared module extension which updated. This is present only if 'reason' is 'shared_module_update'.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onSuspend

<div class="description">

Sent to the event page just before it is unloaded. This gives the extension opportunity to do some clean up. Note that since the page is unloading, any asynchronous operations started while handling this event are not guaranteed to complete. If more activity for the event page occurs before it gets unloaded the onSuspendCanceled event will be sent and the page won't be unloaded.

<div>

#### addListener

<div class="summary">`whale.runtime.onSuspend.addListener(<span>function callback</span>)`</div>

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

### onSuspendCanceled

<div class="description">

Sent after onSuspend to indicate that the app won't be unloaded after all.

<div>

#### addListener

<div class="summary">`whale.runtime.onSuspendCanceled.addListener(<span>function callback</span>)`</div>

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

### onUpdateAvailable

<div class="description">

Since Chrome 25.

Fired when an update is available, but isn't installed immediately because the app is currently running. If you do nothing, the update will be installed the next time the background page gets unloaded, if you want it to be installed sooner you can explicitly call whale.runtime.reload(). If your extension is using a persistent background page, the background page of course never gets unloaded, so unless you call whale.runtime.reload() manually in response to this event the update will not get installed until the next time chrome itself restarts. If no handlers are listening for this event, and your extension has a persistent background page, it behaves as if whale.runtime.reload() is called in response to this event.

<div>

#### addListener

<div class="summary">`whale.runtime.onUpdateAvailable.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

The manifest details of the available update.

| string | version | 
|---|---|

The version number of the available update.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onBrowserUpdateAvailable

<div class="description">

**Deprecated** since Chrome 33. Please use [runtime.onRestartRequired](/extensions/runtime#event-onRestartRequired).

Fired when a Chrome update is available, but isn't installed immediately because a browser restart is required.

<div>

#### addListener

<div class="summary">`whale.runtime.onBrowserUpdateAvailable.addListener(<span>function callback</span>)`</div>

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

### onConnect

<div class="description">

Since Chrome 26.

Fired when a connection is made from either an extension process or a content script (by [runtime.connect](/extensions/runtime#method-connect)).

<div>

#### addListener

<div class="summary">`whale.runtime.onConnect.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Port](/extensions/runtime#type-Port) port) <span class="subdued">{...}</span>;`

| [Port](/extensions/runtime#type-Port) | port |  |
|---|---|---|
 |

</div>

</div>

</div>

</div>

<div>

### onConnectExternal

<div class="description">

Since Chrome 26.

Fired when a connection is made from another extension (by [runtime.connect](/extensions/runtime#method-connect)).

<div>

#### addListener

<div class="summary">`whale.runtime.onConnectExternal.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Port](/extensions/runtime#type-Port) port) <span class="subdued">{...}</span>;`

| [Port](/extensions/runtime#type-Port) | port |  |
|---|---|---|
 |

</div>

</div>

</div>

</div>

<div>

### onMessage

<div class="description">

Since Chrome 26.

Fired when a message is sent from either an extension process (by [runtime.sendMessage](/extensions/runtime#method-sendMessage)) or a content script (by [tabs.sendMessage](/extensions/tabs#method-sendMessage)).

<div>

#### addListener

<div class="summary">`whale.runtime.onMessage.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(any message, [MessageSender](/extensions/runtime#type-MessageSender) sender, function sendResponse) <span class="subdued">{...}</span>;`

| any | <span class="optional">(optional)</span> message | 
|---|---|

The message sent by the calling script.
 |
| [MessageSender](/extensions/runtime#type-MessageSender) | sender |  |
| function | sendResponse | 

Function to call (at most once) when you have a response. The argument should be any JSON-ifiable object. If you have more than one `onMessage` listener in the same document, then only one may send a response. This function becomes invalid when the event listener returns, **unless you return true** from the event listener to indicate you wish to send a response asynchronously (this will keep the message channel open to the other end until `sendResponse` is called).

The _sendResponse_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |
 |

</div>

</div>

</div>

</div>

<div>

### onMessageExternal

<div class="description">

Since Chrome 26.

Fired when a message is sent from another extension/app (by [runtime.sendMessage](/extensions/runtime#method-sendMessage)). Cannot be used in a content script.

<div>

#### addListener

<div class="summary">`whale.runtime.onMessageExternal.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(any message, [MessageSender](/extensions/runtime#type-MessageSender) sender, function sendResponse) <span class="subdued">{...}</span>;`

| any | <span class="optional">(optional)</span> message | 
|---|---|

The message sent by the calling script.
 |
| [MessageSender](/extensions/runtime#type-MessageSender) | sender |  |
| function | sendResponse | 

Function to call (at most once) when you have a response. The argument should be any JSON-ifiable object. If you have more than one `onMessage` listener in the same document, then only one may send a response. This function becomes invalid when the event listener returns, **unless you return true** from the event listener to indicate you wish to send a response asynchronously (this will keep the message channel open to the other end until `sendResponse` is called).

The _sendResponse_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |
 |

</div>

</div>

</div>

</div>

<div>

### onRestartRequired

<div class="description">

Since Chrome 29.

Fired when an app or the device that it runs on needs to be restarted. The app should close all its windows at its earliest convenient time to let the restart to happen. If the app does nothing, a restart will be enforced after a 24-hour grace period has passed. Currently, this event is only fired for Chrome OS kiosk apps.

<div>

#### addListener

<div class="summary">`whale.runtime.onRestartRequired.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [OnRestartRequiredReason](/extensions/runtime#type-OnRestartRequiredReason) reason) <span class="subdued">{...}</span>;`

| [OnRestartRequiredReason](/extensions/runtime#type-OnRestartRequiredReason) | reason | 
|---|---|

The reason that the event is being dispatched.
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>