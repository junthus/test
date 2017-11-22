# whale.extension

| Description: | The `whale.extension` API has utilities that can be used by any extension page. It includes support for exchanging messages between an extension and its content scripts or between extensions, as described in detail in [Message Passing](messaging). |
|---|---|
| Availability: | Since Chrome 20. |
| Content Scripts: | [getURL](/extensions/extension#method-getURL) , [inIncognitoContext](/extensions/extension#property-inIncognitoContext) , [lastError](/extensions/extension#property-lastError) , [onRequest](/extensions/extension#event-onRequest) and [sendRequest](/extensions/extension#method-sendRequest) are supported. [Learn more](content_scripts) |

<section id="toc">

## Summary

| Types |
|---|
| [ViewType](#type-ViewType) |
| Properties |
| [lastError](#property-lastError) |
| [inIncognitoContext](#property-inIncognitoContext) |
| Methods |
| [sendRequest](#method-sendRequest) − `whale.extension.sendRequest(<span class="optional">string extensionId</span>, <span>any request</span>, <span class="optional">function responseCallback</span>)` |
| [getURL](#method-getURL) − `string whale.extension.getURL(<span>string path</span>)` |
| [getViews](#method-getViews) − `array of Window whale.extension.getViews(<span class="optional">object fetchProperties</span>)` |
| [getBackgroundPage](#method-getBackgroundPage) − `Window whale.extension.getBackgroundPage()` |
| [getExtensionTabs](#method-getExtensionTabs) − `array of Window whale.extension.getExtensionTabs(<span class="optional">integer windowId</span>)` |
| [isAllowedIncognitoAccess](#method-isAllowedIncognitoAccess) − `whale.extension.isAllowedIncognitoAccess(<span>function callback</span>)` |
| [isAllowedFileSchemeAccess](#method-isAllowedFileSchemeAccess) − `whale.extension.isAllowedFileSchemeAccess(<span>function callback</span>)` |
| [setUpdateUrlData](#method-setUpdateUrlData) − `whale.extension.setUpdateUrlData(<span>string data</span>)` |
| Events |
| [onRequest](#event-onRequest) |
| [onRequestExternal](#event-onRequestExternal) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### ViewType

<dd>The type of extension view.</dd>

| Enum |
|---|
| `"tab"`, or `"popup"` |

</div>

## Properties

| <span class="type_name">object</span> | `whale.extension.lastError` | 
|---|---|

**Deprecated** since Chrome 58. Please use [runtime.lastError](/extensions/runtime#property-lastError).

Set for the lifetime of a callback if an ansychronous extension api has resulted in an error. If no error has occured lastError will be <var>undefined</var>.

| Properties |
|---|
| --- |
| string | message | 

Description of the error that has taken place.
 |
 |
| <span class="type_name">boolean</span> | `whale.extension.inIncognitoContext` | True for content scripts running inside incognito tabs, and for extension pages running inside an incognito process. The latter only applies to extensions with 'split' incognito_behavior. |

## Methods

<div>

### sendRequest

<div class="summary">`whale.extension.sendRequest(<span class="optional">string extensionId</span>, <span>any request</span>, <span class="optional">function responseCallback</span>)`</div>

<div class="description">

**Deprecated** since Chrome 33. Please use [runtime.sendMessage](/extensions/runtime#method-sendMessage).

Sends a single request to other listeners within the extension. Similar to [runtime.connect](/extensions/runtime#method-connect), but only sends a single request with an optional response. The [extension.onRequest](/extensions/extension#event-onRequest) event is fired in each page of the extension.

| Parameters |
|---|
| string | <span class="optional">(optional)</span> extensionId | 

Since Chrome 33.

The extension ID of the extension you want to connect to. If omitted, default is your own extension.
 |
| any | request | 

Since Chrome 33.
 |
| function | <span class="optional">(optional)</span> responseCallback | 

If you specify the _responseCallback_ parameter, it should be a function that looks like this:

`function(any response) <span class="subdued">{...}</span>;`

| any | response | 
|---|---|

The JSON response object sent by the handler of the request. If an error occurs while connecting to the extension, the callback will be called with no arguments and [runtime.lastError](/extensions/runtime#property-lastError) will be set to the error message.
 |
 |

</div>

</div>

<div>

### getURL

<div class="summary">`string whale.extension.getURL(<span>string path</span>)`</div>

<div class="description">

**Deprecated** since Chrome 58. Please use [runtime.getURL](/extensions/runtime#method-getURL).

Converts a relative path within an extension install directory to a fully-qualified URL.

| Parameters |
|---|
| string | path | 

A path to a resource within an extension expressed relative to its install directory.
 |

</div>

</div>

<div>

### getViews

<div class="summary">`array of Window whale.extension.getViews(<span class="optional">object fetchProperties</span>)`</div>

<div class="description">

Returns an array of the JavaScript 'window' objects for each of the pages running inside the current extension.

| Parameters |
|---|
| object | <span class="optional">(optional)</span> fetchProperties | 

| [ViewType](/extensions/extension#type-ViewType) | <span class="optional">(optional)</span> type | 
|---|---|

The type of view to get. If omitted, returns all views (including background pages and tabs). Valid values: 'tab', 'notification', 'popup'.
 |
| integer | <span class="optional">(optional)</span> windowId | 

The window to restrict the search to. If omitted, returns all views.
 |
| integer | <span class="optional">(optional)</span> tabId | 

Since Chrome 54.

Find a view according to a tab id. If this field is omitted, returns all views.
 |
 |

</div>

</div>

<div>

### getBackgroundPage

<div class="summary">`Window whale.extension.getBackgroundPage()`</div>

<div class="description">

Returns the JavaScript 'window' object for the background page running inside the current extension. Returns null if the extension has no background page.

#### Returns

</div>

</div>

<div>

### getExtensionTabs

<div class="summary">`array of Window whale.extension.getExtensionTabs(<span class="optional">integer windowId</span>)`</div>

<div class="description">

**Deprecated** since Chrome 33. Please use [extension.getViews](/extensions/extension#method-getViews) `{type: "tab"}`.

Returns an array of the JavaScript 'window' objects for each of the tabs running inside the current extension. If `windowId` is specified, returns only the 'window' objects of tabs attached to the specified window.

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> windowId |  |

</div>

</div>

<div>

### isAllowedIncognitoAccess

<div class="summary">`whale.extension.isAllowedIncognitoAccess(<span>function callback</span>)`</div>

<div class="description">

Retrieves the state of the extension's access to Incognito-mode (as determined by the user-controlled 'Allowed in Incognito' checkbox.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(boolean isAllowedAccess) <span class="subdued">{...}</span>;`

| boolean | isAllowedAccess | 
|---|---|

True if the extension has access to Incognito mode, false otherwise.
 |
 |

</div>

</div>

<div>

### isAllowedFileSchemeAccess

<div class="summary">`whale.extension.isAllowedFileSchemeAccess(<span>function callback</span>)`</div>

<div class="description">

Retrieves the state of the extension's access to the 'file://' scheme (as determined by the user-controlled 'Allow access to File URLs' checkbox.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(boolean isAllowedAccess) <span class="subdued">{...}</span>;`

| boolean | isAllowedAccess | 
|---|---|

True if the extension can access the 'file://' scheme, false otherwise.
 |
 |

</div>

</div>

<div>

### setUpdateUrlData

<div class="summary">`whale.extension.setUpdateUrlData(<span>string data</span>)`</div>

<div class="description">

Sets the value of the ap CGI parameter used in the extension's update URL. This value is ignored for extensions that are hosted in the Chrome Extension Gallery.

| Parameters |
|---|
| string | data |  |

</div>

</div>

## Events

<div>

### onRequest

<div class="description">

**Deprecated** since Chrome 33. Please use [runtime.onMessage](/extensions/runtime#event-onMessage).

Fired when a request is sent from either an extension process or a content script.

<div>

#### addListener

<div class="summary">`whale.extension.onRequest.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(any request, [runtime.MessageSender](/extensions/runtime#type-MessageSender) sender, function sendResponse) <span class="subdued">{...}</span>;`

| any | <span class="optional">(optional)</span> request | 
|---|---|

Since Chrome 33.

The request sent by the calling script.
 |
| [runtime.MessageSender](/extensions/runtime#type-MessageSender) | sender | 

Since Chrome 33.
 |
| function | sendResponse | 

Function to call (at most once) when you have a response. The argument should be any JSON-ifiable object, or undefined if there is no response. If you have more than one `onRequest` listener in the same document, then only one may send a response.

The _sendResponse_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |
 |

</div>

</div>

</div>

</div>

<div>

### onRequestExternal

<div class="description">

**Deprecated** since Chrome 33. Please use [runtime.onMessageExternal](/extensions/runtime#event-onMessageExternal).

Fired when a request is sent from another extension.

<div>

#### addListener

<div class="summary">`whale.extension.onRequestExternal.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(any request, [runtime.MessageSender](/extensions/runtime#type-MessageSender) sender, function sendResponse) <span class="subdued">{...}</span>;`

| any | <span class="optional">(optional)</span> request | 
|---|---|

The request sent by the calling script.
 |
| [runtime.MessageSender](/extensions/runtime#type-MessageSender) | sender |  |
| function | sendResponse | 

Function to call when you have a response. The argument should be any JSON-ifiable object, or undefined if there is no response.

The _sendResponse_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |
 |

</div>

</div>

</div>

</div>

</div>

</section>