# whale.tabs

| Description: | Use the `whale.tabs` API to interact with the browser's tab system. You can use this API to create, modify, and rearrange tabs in the browser. |
|---|---|
| Availability: | Since Chrome 20. |
| Permissions: | The majority of the `whale.tabs` API can be used without declaring any permission. However, the `"tabs"` permission is required in order to populate the `[url](#property-Tab-url)`, `[title](#property-Tab-title)`, and `[favIconUrl](#property-Tab-favIconUrl)` properties of `[Tab](#type-Tab)`. |

<section>

## Manifest

You can use most `whale.tabs` methods and events without declaring any permissions in the extension's [manifest](manifest) file. However, if you require access to the `[url](/extensions/tabs#property-Tab-url)`, `[title](/extensions/tabs#property-Tab-title)`, or `[favIconUrl](/extensions/tabs#property-Tab-favIconUrl)` properties of `[tabs.Tab](/extensions/tabs#type-Tab)`, you must declare the `"tabs"` permission in the manifest, as shown below:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"permissions": [
          "tabs"
        ]**,
        ...
      }
      </pre>

## Examples

![Two tabs in a window](/static/images/tabs.png)
You can find simple examples of manipulating tabs with the `whale.tabs` API in the [examples/api/tabs](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/docs/examples/api/tabs/) directory. For other examples and for help in viewing the source code, see [Samples](samples).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [MutedInfoReason](#type-MutedInfoReason) |
| [MutedInfo](#type-MutedInfo) |
| [Tab](#type-Tab) |
| [ZoomSettingsMode](#type-ZoomSettingsMode) |
| [ZoomSettingsScope](#type-ZoomSettingsScope) |
| [ZoomSettings](#type-ZoomSettings) |
| [TabStatus](#type-TabStatus) |
| [WindowType](#type-WindowType) |
| Properties |
| [TAB_ID_NONE](#property-TAB_ID_NONE) |
| Methods |
| [get](#method-get) − `whale.tabs.get(<span>integer tabId</span>, <span>function callback</span>)` |
| [getCurrent](#method-getCurrent) − `whale.tabs.getCurrent(<span>function callback</span>)` |
| [connect](#method-connect) − `runtime.Port whale.tabs.connect(<span>integer tabId</span>, <span class="optional">object connectInfo</span>)` |
| [sendRequest](#method-sendRequest) − `whale.tabs.sendRequest(<span>integer tabId</span>, <span>any request</span>, <span class="optional">function responseCallback</span>)` |
| [sendMessage](#method-sendMessage) − `whale.tabs.sendMessage(<span>integer tabId</span>, <span>any message</span>, <span class="optional">object options</span>, <span class="optional">function responseCallback</span>)` |
| [getSelected](#method-getSelected) − `whale.tabs.getSelected(<span class="optional">integer windowId</span>, <span>function callback</span>)` |
| [getAllInWindow](#method-getAllInWindow) − `whale.tabs.getAllInWindow(<span class="optional">integer windowId</span>, <span>function callback</span>)` |
| [create](#method-create) − `whale.tabs.create(<span>object createProperties</span>, <span class="optional">function callback</span>)` |
| [duplicate](#method-duplicate) − `whale.tabs.duplicate(<span>integer tabId</span>, <span class="optional">function callback</span>)` |
| [query](#method-query) − `whale.tabs.query(<span>object queryInfo</span>, <span>function callback</span>)` |
| [highlight](#method-highlight) − `whale.tabs.highlight(<span>object highlightInfo</span>, <span class="optional">function callback</span>)` |
| [update](#method-update) − `whale.tabs.update(<span class="optional">integer tabId</span>, <span>object updateProperties</span>, <span class="optional">function callback</span>)` |
| [move](#method-move) − `whale.tabs.move(<span>integer or array of integer tabIds</span>, <span>object moveProperties</span>, <span class="optional">function callback</span>)` |
| [reload](#method-reload) − `whale.tabs.reload(<span class="optional">integer tabId</span>, <span class="optional">object reloadProperties</span>, <span class="optional">function callback</span>)` |
| [remove](#method-remove) − `whale.tabs.remove(<span>integer or array of integer tabIds</span>, <span class="optional">function callback</span>)` |
| [detectLanguage](#method-detectLanguage) − `whale.tabs.detectLanguage(<span class="optional">integer tabId</span>, <span>function callback</span>)` |
| [captureVisibleTab](#method-captureVisibleTab) − `whale.tabs.captureVisibleTab(<span class="optional">integer windowId</span>, <span class="optional">object options</span>, <span>function callback</span>)` |
| [executeScript](#method-executeScript) − `whale.tabs.executeScript(<span class="optional">integer tabId</span>, <span>object details</span>, <span class="optional">function callback</span>)` |
| [insertCSS](#method-insertCSS) − `whale.tabs.insertCSS(<span class="optional">integer tabId</span>, <span>object details</span>, <span class="optional">function callback</span>)` |
| [setZoom](#method-setZoom) − `whale.tabs.setZoom(<span class="optional">integer tabId</span>, <span>double zoomFactor</span>, <span class="optional">function callback</span>)` |
| [getZoom](#method-getZoom) − `whale.tabs.getZoom(<span class="optional">integer tabId</span>, <span>function callback</span>)` |
| [setZoomSettings](#method-setZoomSettings) − `whale.tabs.setZoomSettings(<span class="optional">integer tabId</span>, <span>ZoomSettings zoomSettings</span>, <span class="optional">function callback</span>)` |
| [getZoomSettings](#method-getZoomSettings) − `whale.tabs.getZoomSettings(<span class="optional">integer tabId</span>, <span>function callback</span>)` |
| [discard](#method-discard) − `whale.tabs.discard(<span class="optional">integer tabId</span>, <span class="optional">function callback</span>)` |
| Events |
| [onCreated](#event-onCreated) |
| [onUpdated](#event-onUpdated) |
| [onMoved](#event-onMoved) |
| [onSelectionChanged](#event-onSelectionChanged) |
| [onActiveChanged](#event-onActiveChanged) |
| [onActivated](#event-onActivated) |
| [onHighlightChanged](#event-onHighlightChanged) |
| [onHighlighted](#event-onHighlighted) |
| [onDetached](#event-onDetached) |
| [onAttached](#event-onAttached) |
| [onRemoved](#event-onRemoved) |
| [onReplaced](#event-onReplaced) |
| [onZoomChange](#event-onZoomChange) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### MutedInfoReason

<dd>An event that caused a muted state change.</dd>

| Enum |
|---|
| 

<dl>

<dt>`"user"`</dt>

<dd>A user input action has set/overridden the muted state.</dd>

</dl>

<dl>

<dt>`"capture"`</dt>

<dd>Tab capture started, forcing a muted state change.</dd>

</dl>

<dl>

<dt>`"extension"`</dt>

<dd>An extension, identified by the extensionId field, set the muted state.</dd>

</dl>
 |

</div>

<div>

### MutedInfo

Since Chrome 46.

<dd>Tab muted state and the reason for the last state change.</dd>

| properties |
|---|
| boolean | muted | 

Whether the tab is prevented from playing sound (but hasn't necessarily recently produced sound). Equivalent to whether the muted audio indicator is showing.
 |
| [MutedInfoReason](/extensions/tabs#type-MutedInfoReason) | <span class="optional">(optional)</span> reason | 

The reason the tab was muted or unmuted. Not set if the tab's mute state has never been changed.
 |
| string | <span class="optional">(optional)</span> extensionId | 

The ID of the extension that changed the muted state. Not set if an extension was not the reason the muted state last changed.
 |

</div>

<div>

### Tab

| properties |
|---|
| integer | <span class="optional">(optional)</span> id | 

The ID of the tab. Tab IDs are unique within a browser session. Under some circumstances a Tab may not be assigned an ID, for example when querying foreign tabs using the [sessions](/extensions/sessions) API, in which case a session ID may be present. Tab ID can also be set to whale.tabs.TAB_ID_NONE for apps and devtools windows.
 |
| integer | index | 

The zero-based index of the tab within its window.
 |
| integer | windowId | 

The ID of the window the tab is contained within.
 |
| integer | <span class="optional">(optional)</span> openerTabId | 

The ID of the tab that opened this tab, if any. This property is only present if the opener tab still exists.
 |
| boolean | selected | 

**Deprecated** since Chrome 33. Please use [tabs.Tab.highlighted](/extensions/tabs#property-Tab-highlighted).

Whether the tab is selected.
 |
| boolean | highlighted | 

Whether the tab is highlighted.
 |
| boolean | active | 

Whether the tab is active in its window. (Does not necessarily mean the window is focused.)
 |
| boolean | pinned | 

Whether the tab is pinned.
 |
| boolean | <span class="optional">(optional)</span> audible | 

Since Chrome 45.

Whether the tab has produced sound over the past couple of seconds (but it might not be heard if also muted). Equivalent to whether the speaker audio indicator is showing.
 |
| boolean | discarded | 

Since Chrome 54.

Whether the tab is discarded. A discarded tab is one whose content has been unloaded from memory, but is still visible in the tab strip. Its content gets reloaded the next time it's activated.
 |
| boolean | autoDiscardable | 

Since Chrome 54.

Whether the tab can be discarded automatically by the browser when resources are low.
 |
| [MutedInfo](/extensions/tabs#type-MutedInfo) | <span class="optional">(optional)</span> mutedInfo | 

Since Chrome 46.

Current tab muted state and the reason for the last state change.
 |
| string | <span class="optional">(optional)</span> url | 

The URL the tab is displaying. This property is only present if the extension's manifest includes the `"tabs"` permission.
 |
| string | <span class="optional">(optional)</span> title | 

The title of the tab. This property is only present if the extension's manifest includes the `"tabs"` permission.
 |
| string | <span class="optional">(optional)</span> favIconUrl | 

The URL of the tab's favicon. This property is only present if the extension's manifest includes the `"tabs"` permission. It may also be an empty string if the tab is loading.
 |
| string | <span class="optional">(optional)</span> status | 

Either _loading_ or _complete_.
 |
| boolean | incognito | 

Whether the tab is in an incognito window.
 |
| integer | <span class="optional">(optional)</span> width | 

Since Chrome 31.

The width of the tab in pixels.
 |
| integer | <span class="optional">(optional)</span> height | 

Since Chrome 31.

The height of the tab in pixels.
 |
| string | <span class="optional">(optional)</span> sessionId | 

Since Chrome 31.

The session ID used to uniquely identify a Tab obtained from the [sessions](/extensions/sessions) API.
 |

</div>

<div>

### ZoomSettingsMode

<dd>Defines how zoom changes are handled, i.e. which entity is responsible for the actual scaling of the page; defaults to `automatic`.</dd>

| Enum |
|---|
| 

<dl>

<dt>`"automatic"`</dt>

<dd>Zoom changes are handled automatically by the browser.</dd>

</dl>

<dl>

<dt>`"manual"`</dt>

<dd>Overrides the automatic handling of zoom changes. The `onZoomChange` event will still be dispatched, and it is the responsibility of the extension to listen for this event and manually scale the page. This mode does not support `per-origin` zooming, and will thus ignore the `scope` zoom setting and assume `per-tab`.</dd>

</dl>

<dl>

<dt>`"disabled"`</dt>

<dd>Disables all zooming in the tab. The tab will revert to the default zoom level, and all attempted zoom changes will be ignored.</dd>

</dl>
 |

</div>

<div>

### ZoomSettingsScope

<dd>Defines whether zoom changes will persist for the page's origin, or only take effect in this tab; defaults to `per-origin` when in `automatic` mode, and `per-tab` otherwise.</dd>

| Enum |
|---|
| 

<dl>

<dt>`"per-origin"`</dt>

<dd>Zoom changes will persist in the zoomed page's origin, i.e. all other tabs navigated to that same origin will be zoomed as well. Moreover, `per-origin` zoom changes are saved with the origin, meaning that when navigating to other pages in the same origin, they will all be zoomed to the same zoom factor. The `per-origin` scope is only available in the `automatic` mode.</dd>

</dl>

<dl>

<dt>`"per-tab"`</dt>

<dd>Zoom changes only take effect in this tab, and zoom changes in other tabs will not affect the zooming of this tab. Also, `per-tab` zoom changes are reset on navigation; navigating a tab will always load pages with their `per-origin` zoom factors.</dd>

</dl>
 |

</div>

<div>

### ZoomSettings

Since Chrome 38.

<dd>Defines how zoom changes in a tab are handled and at what scope.</dd>

| properties |
|---|
| [ZoomSettingsMode](/extensions/tabs#type-ZoomSettingsMode) | <span class="optional">(optional)</span> mode | 

Defines how zoom changes are handled, i.e. which entity is responsible for the actual scaling of the page; defaults to `automatic`.
 |
| [ZoomSettingsScope](/extensions/tabs#type-ZoomSettingsScope) | <span class="optional">(optional)</span> scope | 

Defines whether zoom changes will persist for the page's origin, or only take effect in this tab; defaults to `per-origin` when in `automatic` mode, and `per-tab` otherwise.
 |
| double | <span class="optional">(optional)</span> defaultZoomFactor | 

Since Chrome 43.

Used to return the default zoom level for the current tab in calls to tabs.getZoomSettings.
 |

</div>

<div>

### TabStatus

<dd>Whether the tabs have completed loading.</dd>

| Enum |
|---|
| `"loading"`, or `"complete"` |

</div>

<div>

### WindowType

<dd>The type of window.</dd>

| Enum |
|---|
| `"normal"`, `"popup"`, `"panel"`, `"app"`, or `"devtools"` |

</div>

## Properties

| <span class="type_name">`-1`</span> | `whale.tabs.TAB_ID_NONE` | 
|---|---|

Since Chrome 46.

An ID which represents the absence of a browser tab. |

## Methods

<div>

### get

<div class="summary">`whale.tabs.get(<span>integer tabId</span>, <span>function callback</span>)`</div>

<div class="description">

Retrieves details about the specified tab.

| Parameters |
|---|
| integer | tabId |  |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Tab](/extensions/tabs#type-Tab) tab) <span class="subdued">{...}</span>;`

| [Tab](/extensions/tabs#type-Tab) | tab |  |
|---|---|---|
 |

</div>

</div>

<div>

### getCurrent

<div class="summary">`whale.tabs.getCurrent(<span>function callback</span>)`</div>

<div class="description">

Gets the tab that this script call is being made from. May be undefined if called from a non-tab context (for example: a background page or popup view).

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Tab](/extensions/tabs#type-Tab) tab) <span class="subdued">{...}</span>;`

| [Tab](/extensions/tabs#type-Tab) | <span class="optional">(optional)</span> tab |  |
|---|---|---|
 |

</div>

</div>

<div>

### connect

<div class="summary">`[runtime.Port](/extensions/runtime#type-Port) whale.tabs.connect(<span>integer tabId</span>, <span class="optional">object connectInfo</span>)`</div>

<div class="description">

Connects to the content script(s) in the specified tab. The [runtime.onConnect](/extensions/runtime#event-onConnect) event is fired in each content script running in the specified tab for the current extension. For more details, see [Content Script Messaging](messaging).

| Parameters |
|---|
| integer | tabId |  |
| object | <span class="optional">(optional)</span> connectInfo | 

| string | <span class="optional">(optional)</span> name | 
|---|---|

Will be passed into onConnect for content scripts that are listening for the connection event.
 |
| integer | <span class="optional">(optional)</span> frameId | 

Since Chrome 41.

Open a port to a specific [frame](webNavigation#frame_ids) identified by `frameId` instead of all frames in the tab.
 |
 |

</div>

</div>

<div>

### sendRequest

<div class="summary">`whale.tabs.sendRequest(<span>integer tabId</span>, <span>any request</span>, <span class="optional">function responseCallback</span>)`</div>

<div class="description">

**Deprecated** since Chrome 33. Please use [runtime.sendMessage](/extensions/runtime#method-sendMessage).

Sends a single request to the content script(s) in the specified tab, with an optional callback to run when a response is sent back. The [extension.onRequest](/extensions/extension#event-onRequest) event is fired in each content script running in the specified tab for the current extension.

| Parameters |
|---|
| integer | tabId |  |
| any | request |  |
| function | <span class="optional">(optional)</span> responseCallback | 

If you specify the _responseCallback_ parameter, it should be a function that looks like this:

`function(any response) <span class="subdued">{...}</span>;`

| any | response | 
|---|---|

The JSON response object sent by the handler of the request. If an error occurs while connecting to the specified tab, the callback will be called with no arguments and [runtime.lastError](/extensions/runtime#property-lastError) will be set to the error message.
 |
 |

</div>

</div>

<div>

### sendMessage

<div class="summary">`whale.tabs.sendMessage(<span>integer tabId</span>, <span>any message</span>, <span class="optional">object options</span>, <span class="optional">function responseCallback</span>)`</div>

<div class="description">

Sends a single message to the content script(s) in the specified tab, with an optional callback to run when a response is sent back. The [runtime.onMessage](/extensions/runtime#event-onMessage) event is fired in each content script running in the specified tab for the current extension.

| Parameters |
|---|
| integer | tabId |  |
| any | message | 

The message to send. This message should be a JSON-ifiable object.
 |
| object | <span class="optional">(optional)</span> options | 

Since Chrome 41.

| integer | <span class="optional">(optional)</span> frameId | 
|---|---|

Send a message to a specific [frame](webNavigation#frame_ids) identified by `frameId` instead of all frames in the tab.
 |
 |
| function | <span class="optional">(optional)</span> responseCallback | 

If you specify the _responseCallback_ parameter, it should be a function that looks like this:

`function(any response) <span class="subdued">{...}</span>;`

| any | response | 
|---|---|

The JSON response object sent by the handler of the message. If an error occurs while connecting to the specified tab, the callback will be called with no arguments and [runtime.lastError](/extensions/runtime#property-lastError) will be set to the error message.
 |
 |

</div>

</div>

<div>

### getSelected

<div class="summary">`whale.tabs.getSelected(<span class="optional">integer windowId</span>, <span>function callback</span>)`</div>

<div class="description">

**Deprecated** since Chrome 33. Please use [tabs.query](/extensions/tabs#method-query) `{active: true}`.

Gets the tab that is selected in the specified window.

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> windowId | 

Defaults to the [current window](windows#current-window).
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Tab](/extensions/tabs#type-Tab) tab) <span class="subdued">{...}</span>;`

| [Tab](/extensions/tabs#type-Tab) | tab |  |
|---|---|---|
 |

</div>

</div>

<div>

### getAllInWindow

<div class="summary">`whale.tabs.getAllInWindow(<span class="optional">integer windowId</span>, <span>function callback</span>)`</div>

<div class="description">

**Deprecated** since Chrome 33. Please use [tabs.query](/extensions/tabs#method-query) `{windowId: windowId}`.

Gets details about all tabs in the specified window.

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> windowId | 

Defaults to the [current window](windows#current-window).
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [Tab](/extensions/tabs#type-Tab) tabs) <span class="subdued">{...}</span>;`

| array of [Tab](/extensions/tabs#type-Tab) | tabs |  |
|---|---|---|
 |

</div>

</div>

<div>

### create

<div class="summary">`whale.tabs.create(<span>object createProperties</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Creates a new tab.

| Parameters |
|---|
| object | createProperties | 

| integer | <span class="optional">(optional)</span> windowId | 
|---|---|

The window to create the new tab in. Defaults to the [current window](windows#current-window).
 |
| integer | <span class="optional">(optional)</span> index | 

The position the tab should take in the window. The provided value will be clamped to between zero and the number of tabs in the window.
 |
| string | <span class="optional">(optional)</span> url | 

The URL to navigate the tab to initially. Fully-qualified URLs must include a scheme (i.e. 'http://www.google.com', not 'www.google.com'). Relative URLs will be relative to the current page within the extension. Defaults to the New Tab Page.
 |
| boolean | <span class="optional">(optional)</span> active | 

Whether the tab should become the active tab in the window. Does not affect whether the window is focused (see [windows.update](/extensions/windows#method-update)). Defaults to <var>true</var>.
 |
| boolean | <span class="optional">(optional)</span> selected | 

**Deprecated** since Chrome 33. Please use _active_.

Whether the tab should become the selected tab in the window. Defaults to <var>true</var>
 |
| boolean | <span class="optional">(optional)</span> pinned | 

Whether the tab should be pinned. Defaults to <var>false</var>
 |
| integer | <span class="optional">(optional)</span> openerTabId | 

The ID of the tab that opened this tab. If specified, the opener tab must be in the same window as the newly created tab.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [Tab](/extensions/tabs#type-Tab) tab) <span class="subdued">{...}</span>;`

| [Tab](/extensions/tabs#type-Tab) | tab | 
|---|---|

Details about the created tab. Will contain the ID of the new tab.
 |
 |

</div>

</div>

<div>

### duplicate

<div class="summary">`whale.tabs.duplicate(<span>integer tabId</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 23.

Duplicates a tab.

| Parameters |
|---|
| integer | tabId | 

The ID of the tab which is to be duplicated.
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [Tab](/extensions/tabs#type-Tab) tab) <span class="subdued">{...}</span>;`

| [Tab](/extensions/tabs#type-Tab) | <span class="optional">(optional)</span> tab | 
|---|---|

Details about the duplicated tab. The [tabs.Tab](/extensions/tabs#type-Tab) object doesn't contain `url`, `title` and `favIconUrl` if the `"tabs"` permission has not been requested.
 |
 |

</div>

</div>

<div>

### query

<div class="summary">`whale.tabs.query(<span>object queryInfo</span>, <span>function callback</span>)`</div>

<div class="description">

Gets all tabs that have the specified properties, or all tabs if no properties are specified.

| Parameters |
|---|
| object | queryInfo | 

| boolean | <span class="optional">(optional)</span> active | 
|---|---|

Whether the tabs are active in their windows.
 |
| boolean | <span class="optional">(optional)</span> pinned | 

Whether the tabs are pinned.
 |
| boolean | <span class="optional">(optional)</span> audible | 

Since Chrome 45.

Whether the tabs are audible.
 |
| boolean | <span class="optional">(optional)</span> muted | 

Since Chrome 45.

Whether the tabs are muted.
 |
| boolean | <span class="optional">(optional)</span> highlighted | 

Whether the tabs are highlighted.
 |
| boolean | <span class="optional">(optional)</span> discarded | 

Since Chrome 54.

Whether the tabs are discarded. A discarded tab is one whose content has been unloaded from memory, but is still visible in the tab strip. Its content gets reloaded the next time it's activated.
 |
| boolean | <span class="optional">(optional)</span> autoDiscardable | 

Since Chrome 54.

Whether the tabs can be discarded automatically by the browser when resources are low.
 |
| boolean | <span class="optional">(optional)</span> currentWindow | 

Whether the tabs are in the [current window](windows#current-window).
 |
| boolean | <span class="optional">(optional)</span> lastFocusedWindow | 

Whether the tabs are in the last focused window.
 |
| [TabStatus](/extensions/tabs#type-TabStatus) | <span class="optional">(optional)</span> status | 

Whether the tabs have completed loading.
 |
| string | <span class="optional">(optional)</span> title | 

Match page titles against a pattern. Note that this property is ignored if the extension doesn't have the `"tabs"` permission.
 |
| string or array of string | <span class="optional">(optional)</span> url | 

Match tabs against one or more [URL patterns](match_patterns). Note that fragment identifiers are not matched. Note that this property is ignored if the extension doesn't have the `"tabs"` permission.
 |
| integer | <span class="optional">(optional)</span> windowId | 

The ID of the parent window, or [windows.WINDOW_ID_CURRENT](/extensions/windows#property-WINDOW_ID_CURRENT) for the [current window](windows#current-window).
 |
| [WindowType](/extensions/tabs#type-WindowType) | <span class="optional">(optional)</span> windowType | 

The type of window the tabs are in.
 |
| integer | <span class="optional">(optional)</span> index | 

The position of the tabs within their windows.
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [Tab](/extensions/tabs#type-Tab) result) <span class="subdued">{...}</span>;`

| array of [Tab](/extensions/tabs#type-Tab) | result |  |
|---|---|---|
 |

</div>

</div>

<div>

### highlight

<div class="summary">`whale.tabs.highlight(<span>object highlightInfo</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Highlights the given tabs.

| Parameters |
|---|
| object | highlightInfo | 

| integer | <span class="optional">(optional)</span> windowId | 
|---|---|

The window that contains the tabs.
 |
| array of integer or integer | tabs | 

One or more tab indices to highlight.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [windows.Window](/extensions/windows#type-Window) window) <span class="subdued">{...}</span>;`

| [windows.Window](/extensions/windows#type-Window) | window | 
|---|---|

Contains details about the window whose tabs were highlighted.
 |
 |

</div>

</div>

<div>

### update

<div class="summary">`whale.tabs.update(<span class="optional">integer tabId</span>, <span>object updateProperties</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Modifies the properties of a tab. Properties that are not specified in <var>updateProperties</var> are not modified.

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> tabId | 

Defaults to the selected tab of the [current window](windows#current-window).
 |
| object | updateProperties | 

| string | <span class="optional">(optional)</span> url | 
|---|---|

A URL to navigate the tab to.
 |
| boolean | <span class="optional">(optional)</span> active | 

Whether the tab should be active. Does not affect whether the window is focused (see [windows.update](/extensions/windows#method-update)).
 |
| boolean | <span class="optional">(optional)</span> highlighted | 

Adds or removes the tab from the current selection.
 |
| boolean | <span class="optional">(optional)</span> selected | 

**Deprecated** since Chrome 33. Please use _highlighted_.

Whether the tab should be selected.
 |
| boolean | <span class="optional">(optional)</span> pinned | 

Whether the tab should be pinned.
 |
| boolean | <span class="optional">(optional)</span> muted | 

Since Chrome 45.

Whether the tab should be muted.
 |
| integer | <span class="optional">(optional)</span> openerTabId | 

The ID of the tab that opened this tab. If specified, the opener tab must be in the same window as this tab.
 |
| boolean | <span class="optional">(optional)</span> autoDiscardable | 

Since Chrome 54.

Whether the tab should be discarded automatically by the browser when resources are low.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [Tab](/extensions/tabs#type-Tab) tab) <span class="subdued">{...}</span>;`

| [Tab](/extensions/tabs#type-Tab) | <span class="optional">(optional)</span> tab | 
|---|---|

Details about the updated tab. The [tabs.Tab](/extensions/tabs#type-Tab) object doesn't contain `url`, `title` and `favIconUrl` if the `"tabs"` permission has not been requested.
 |
 |

</div>

</div>

<div>

### move

<div class="summary">`whale.tabs.move(<span>integer or array of integer tabIds</span>, <span>object moveProperties</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Moves one or more tabs to a new position within its window, or to a new window. Note that tabs can only be moved to and from normal (window.type === "normal") windows.

| Parameters |
|---|
| integer or array of integer | tabIds | 

The tab or list of tabs to move.
 |
| object | moveProperties | 

| integer | <span class="optional">(optional)</span> windowId | 
|---|---|

Defaults to the window the tab is currently in.
 |
| integer | index | 

The position to move the window to. -1 will place the tab at the end of the window.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [Tab](/extensions/tabs#type-Tab) or array of [Tab](/extensions/tabs#type-Tab) tabs) <span class="subdued">{...}</span>;`

| [Tab](/extensions/tabs#type-Tab) or array of [Tab](/extensions/tabs#type-Tab) | tabs | 
|---|---|

Details about the moved tabs.
 |
 |

</div>

</div>

<div>

### reload

<div class="summary">`whale.tabs.reload(<span class="optional">integer tabId</span>, <span class="optional">object reloadProperties</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Reload a tab.

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> tabId | 

The ID of the tab to reload; defaults to the selected tab of the current window.
 |
| object | <span class="optional">(optional)</span> reloadProperties | 

| boolean | <span class="optional">(optional)</span> bypassCache | 
|---|---|

Whether using any local cache. Default is false.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### remove

<div class="summary">`whale.tabs.remove(<span>integer or array of integer tabIds</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Closes one or more tabs.

| Parameters |
|---|
| integer or array of integer | tabIds | 

The tab or list of tabs to close.
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### detectLanguage

<div class="summary">`whale.tabs.detectLanguage(<span class="optional">integer tabId</span>, <span>function callback</span>)`</div>

<div class="description">

Detects the primary language of the content in a tab.

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> tabId | 

Defaults to the active tab of the [current window](windows#current-window).
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string language) <span class="subdued">{...}</span>;`

| string | language | 
|---|---|

An ISO language code such as `en` or `fr`. For a complete list of languages supported by this method, see [kLanguageInfoTable](http://src.chromium.org/viewvc/chrome/trunk/src/third_party/cld/languages/internal/languages.cc). The 2nd to 4th columns will be checked and the first non-NULL value will be returned except for Simplified Chinese for which zh-CN will be returned. For an unknown language, `und` will be returned.
 |
 |

</div>

</div>

<div>

### captureVisibleTab

<div class="summary">`whale.tabs.captureVisibleTab(<span class="optional">integer windowId</span>, <span class="optional">object options</span>, <span>function callback</span>)`</div>

<div class="description">

Captures the visible area of the currently active tab in the specified window. You must have [<all_urls>](declare_permissions) permission to use this method.

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> windowId | 

The target window. Defaults to the [current window](windows#current-window).
 |
| object | <span class="optional">(optional)</span> options | 

Details about the format and quality of an image.

| enum of `"jpeg"`, or `"png"` | <span class="optional">(optional)</span> format | 
|---|---|

The format of the resulting image. Default is `"jpeg"`.
 |
| integer | <span class="optional">(optional)</span> quality | 

When format is `"jpeg"`, controls the quality of the resulting image. This value is ignored for PNG images. As quality is decreased, the resulting image will have more visual artifacts, and the number of bytes needed to store it will decrease.
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string dataUrl) <span class="subdued">{...}</span>;`

| string | dataUrl | 
|---|---|

A data URL which encodes an image of the visible area of the captured tab. May be assigned to the 'src' property of an HTML Image element for display.
 |
 |

</div>

</div>

<div>

### executeScript

<div class="summary">`whale.tabs.executeScript(<span class="optional">integer tabId</span>, <span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Injects JavaScript code into a page. For details, see the [programmatic injection](content_scripts#pi) section of the content scripts doc.

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> tabId | 

The ID of the tab in which to run the script; defaults to the active tab of the current window.
 |
| object | details | 

Details of the script to run. Either the code or the file property must be set, but both may not be set at the same time.

| string | <span class="optional">(optional)</span> code | 
|---|---|

JavaScript or CSS code to inject.

**Warning:**
Be careful using the `code` parameter. Incorrect use of it may open your extension to [cross site scripting](https://en.wikipedia.org/wiki/Cross-site_scripting) attacks.
 |
| string | <span class="optional">(optional)</span> file | 

JavaScript or CSS file to inject.
 |
| boolean | <span class="optional">(optional)</span> allFrames | 

If allFrames is `true`, implies that the JavaScript or CSS should be injected into all frames of current page. By default, it's `false` and is only injected into the top frame. If `true` and `frameId` is set, then the code is inserted in the selected frame and all of its child frames.
 |
| integer | <span class="optional">(optional)</span> frameId | 

Since Chrome 39.

The [frame](webNavigation#frame_ids) where the script or CSS should be injected. Defaults to 0 (the top-level frame).
 |
| boolean | <span class="optional">(optional)</span> matchAboutBlank | 

Since Chrome 39.

If matchAboutBlank is true, then the code is also injected in about:blank and about:srcdoc frames if your extension has access to its parent document. Code cannot be inserted in top-level about:-frames. By default it is `false`.
 |
| enum of `"document_start"`, `"document_end"`, or `"document_idle"` | <span class="optional">(optional)</span> runAt | 

The soonest that the JavaScript or CSS will be injected into the tab. Defaults to "document_idle".
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called after all the JavaScript has been executed.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(array of any result) <span class="subdued">{...}</span>;`

| array of any | <span class="optional">(optional)</span> result | 
|---|---|

The result of the script in every injected frame.
 |
 |

</div>

</div>

<div>

### insertCSS

<div class="summary">`whale.tabs.insertCSS(<span class="optional">integer tabId</span>, <span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Injects CSS into a page. For details, see the [programmatic injection](content_scripts#pi) section of the content scripts doc.

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> tabId | 

The ID of the tab in which to insert the CSS; defaults to the active tab of the current window.
 |
| object | details | 

Details of the CSS text to insert. Either the code or the file property must be set, but both may not be set at the same time.

| string | <span class="optional">(optional)</span> code | 
|---|---|

JavaScript or CSS code to inject.

**Warning:**
Be careful using the `code` parameter. Incorrect use of it may open your extension to [cross site scripting](https://en.wikipedia.org/wiki/Cross-site_scripting) attacks.
 |
| string | <span class="optional">(optional)</span> file | 

JavaScript or CSS file to inject.
 |
| boolean | <span class="optional">(optional)</span> allFrames | 

If allFrames is `true`, implies that the JavaScript or CSS should be injected into all frames of current page. By default, it's `false` and is only injected into the top frame. If `true` and `frameId` is set, then the code is inserted in the selected frame and all of its child frames.
 |
| integer | <span class="optional">(optional)</span> frameId | 

Since Chrome 39.

The [frame](webNavigation#frame_ids) where the script or CSS should be injected. Defaults to 0 (the top-level frame).
 |
| boolean | <span class="optional">(optional)</span> matchAboutBlank | 

Since Chrome 39.

If matchAboutBlank is true, then the code is also injected in about:blank and about:srcdoc frames if your extension has access to its parent document. Code cannot be inserted in top-level about:-frames. By default it is `false`.
 |
| enum of `"document_start"`, `"document_end"`, or `"document_idle"` | <span class="optional">(optional)</span> runAt | 

The soonest that the JavaScript or CSS will be injected into the tab. Defaults to "document_idle".
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called when all the CSS has been inserted.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### setZoom

<div class="summary">`whale.tabs.setZoom(<span class="optional">integer tabId</span>, <span>double zoomFactor</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 42.

Zooms a specified tab.

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> tabId | 

Since Chrome 38.

The ID of the tab to zoom; defaults to the active tab of the current window.
 |
| double | zoomFactor | 

Since Chrome 38.

The new zoom factor. Use a value of 0 here to set the tab to its current default zoom factor. Values greater than zero specify a (possibly non-default) zoom factor for the tab.
 |
| function | <span class="optional">(optional)</span> callback | 

Called after the zoom factor has been changed.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### getZoom

<div class="summary">`whale.tabs.getZoom(<span class="optional">integer tabId</span>, <span>function callback</span>)`</div>

<div class="description">

Since Chrome 42.

Gets the current zoom factor of a specified tab.

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> tabId | 

Since Chrome 38.

The ID of the tab to get the current zoom factor from; defaults to the active tab of the current window.
 |
| function | callback | 

Called with the tab's current zoom factor after it has been fetched.

The _callback_ parameter should be a function that looks like this:

`function(double zoomFactor) <span class="subdued">{...}</span>;`

| double | zoomFactor | 
|---|---|

The tab's current zoom factor.
 |
 |

</div>

</div>

<div>

### setZoomSettings

<div class="summary">`whale.tabs.setZoomSettings(<span class="optional">integer tabId</span>, <span>[ZoomSettings](/extensions/tabs#type-ZoomSettings) zoomSettings</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 42.

Sets the zoom settings for a specified tab, which define how zoom changes are handled. These settings are reset to defaults upon navigating the tab.

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> tabId | 

Since Chrome 38.

The ID of the tab to change the zoom settings for; defaults to the active tab of the current window.
 |
| [ZoomSettings](/extensions/tabs#type-ZoomSettings) | zoomSettings | 

Since Chrome 38.

Defines how zoom changes are handled and at what scope.
 |
| function | <span class="optional">(optional)</span> callback | 

Called after the zoom settings have been changed.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### getZoomSettings

<div class="summary">`whale.tabs.getZoomSettings(<span class="optional">integer tabId</span>, <span>function callback</span>)`</div>

<div class="description">

Since Chrome 42.

Gets the current zoom settings of a specified tab.

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> tabId | 

Since Chrome 38.

The ID of the tab to get the current zoom settings from; defaults to the active tab of the current window.
 |
| function | callback | 

Called with the tab's current zoom settings.

The _callback_ parameter should be a function that looks like this:

`function( [ZoomSettings](/extensions/tabs#type-ZoomSettings) zoomSettings) <span class="subdued">{...}</span>;`

| [ZoomSettings](/extensions/tabs#type-ZoomSettings) | zoomSettings | 
|---|---|

The tab's current zoom settings.
 |
 |

</div>

</div>

<div>

### discard

<div class="summary">`whale.tabs.discard(<span class="optional">integer tabId</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 54.

Discards a tab from memory. Discarded tabs are still visible on the tab strip and are reloaded when activated.

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> tabId | 

The ID of the tab to be discarded. If specified, the tab will be discarded unless it's active or already discarded. If omitted, the browser will discard the least important tab. This can fail if no discardable tabs exist.
 |
| function | <span class="optional">(optional)</span> callback | 

Called after the operation is completed.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [Tab](/extensions/tabs#type-Tab) tab) <span class="subdued">{...}</span>;`

| [Tab](/extensions/tabs#type-Tab) | <span class="optional">(optional)</span> tab | 
|---|---|

Discarded tab if it was successfully discarded. Undefined otherwise.
 |
 |

</div>

</div>

## Events

<div>

### onCreated

<div class="description">

Fired when a tab is created. Note that the tab's URL may not be set at the time this event fired, but you can listen to onUpdated events to be notified when a URL is set.

<div>

#### addListener

<div class="summary">`whale.tabs.onCreated.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Tab](/extensions/tabs#type-Tab) tab) <span class="subdued">{...}</span>;`

| [Tab](/extensions/tabs#type-Tab) | tab | 
|---|---|

Details of the tab that was created.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onUpdated

<div class="description">

Fired when a tab is updated.

<div>

#### addListener

<div class="summary">`whale.tabs.onUpdated.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(integer tabId, object changeInfo, [Tab](/extensions/tabs#type-Tab) tab) <span class="subdued">{...}</span>;`

| integer | tabId |  |
|---|---|---|
| object | changeInfo | 

Lists the changes to the state of the tab that was updated.

| string | <span class="optional">(optional)</span> status | 
|---|---|

The status of the tab. Can be either _loading_ or _complete_.
 |
| string | <span class="optional">(optional)</span> url | 

The tab's URL if it has changed.
 |
| boolean | <span class="optional">(optional)</span> pinned | 

The tab's new pinned state.
 |
| boolean | <span class="optional">(optional)</span> audible | 

Since Chrome 45.

The tab's new audible state.
 |
| boolean | <span class="optional">(optional)</span> discarded | 

Since Chrome 54.

The tab's new discarded state.
 |
| boolean | <span class="optional">(optional)</span> autoDiscardable | 

Since Chrome 54.

The tab's new auto-discardable state.
 |
| [MutedInfo](/extensions/tabs#type-MutedInfo) | <span class="optional">(optional)</span> mutedInfo | 

Since Chrome 46.

The tab's new muted state and the reason for the change.
 |
| string | <span class="optional">(optional)</span> favIconUrl | 

Since Chrome 27.

The tab's new favicon URL.
 |
| string | <span class="optional">(optional)</span> title | 

Since Chrome 48.

The tab's new title.
 |
 |
| [Tab](/extensions/tabs#type-Tab) | tab | 

Gives the state of the tab that was updated.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onMoved

<div class="description">

Fired when a tab is moved within a window. Only one move event is fired, representing the tab the user directly moved. Move events are not fired for the other tabs that must move in response. This event is not fired when a tab is moved between windows. For that, see [tabs.onDetached](/extensions/tabs#event-onDetached).

<div>

#### addListener

<div class="summary">`whale.tabs.onMoved.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(integer tabId, object moveInfo) <span class="subdued">{...}</span>;`

| integer | tabId |  |
|---|---|---|
| object | moveInfo | 

| integer | windowId |  |
|---|---|---|
| integer | fromIndex |  |
| integer | toIndex |  |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onSelectionChanged

<div class="description">

**Deprecated** since Chrome 33. Please use [tabs.onActivated](/extensions/tabs#event-onActivated).

Fires when the selected tab in a window changes.

<div>

#### addListener

<div class="summary">`whale.tabs.onSelectionChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(integer tabId, object selectInfo) <span class="subdued">{...}</span>;`

| integer | tabId | 
|---|---|

The ID of the tab that has become active.
 |
| object | selectInfo | 

| integer | windowId | 
|---|---|

The ID of the window the selected tab changed inside of.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onActiveChanged

<div class="description">

**Deprecated** since Chrome 33. Please use [tabs.onActivated](/extensions/tabs#event-onActivated).

Fires when the selected tab in a window changes. Note that the tab's URL may not be set at the time this event fired, but you can listen to [tabs.onUpdated](/extensions/tabs#event-onUpdated) events to be notified when a URL is set.

<div>

#### addListener

<div class="summary">`whale.tabs.onActiveChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(integer tabId, object selectInfo) <span class="subdued">{...}</span>;`

| integer | tabId | 
|---|---|

The ID of the tab that has become active.
 |
| object | selectInfo | 

| integer | windowId | 
|---|---|

The ID of the window the selected tab changed inside of.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onActivated

<div class="description">

Fires when the active tab in a window changes. Note that the tab's URL may not be set at the time this event fired, but you can listen to onUpdated events to be notified when a URL is set.

<div>

#### addListener

<div class="summary">`whale.tabs.onActivated.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object activeInfo) <span class="subdued">{...}</span>;`

| object | activeInfo | 
|---|---|

| integer | tabId | 
|---|---|

The ID of the tab that has become active.
 |
| integer | windowId | 

The ID of the window the active tab changed inside of.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onHighlightChanged

<div class="description">

**Deprecated** since Chrome 33. Please use [tabs.onHighlighted](/extensions/tabs#event-onHighlighted).

Fired when the highlighted or selected tabs in a window changes.

<div>

#### addListener

<div class="summary">`whale.tabs.onHighlightChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object selectInfo) <span class="subdued">{...}</span>;`

| object | selectInfo | 
|---|---|

| integer | windowId | 
|---|---|

The window whose tabs changed.
 |
| array of integer | tabIds | 

All highlighted tabs in the window.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onHighlighted

<div class="description">

Fired when the highlighted or selected tabs in a window changes.

<div>

#### addListener

<div class="summary">`whale.tabs.onHighlighted.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object highlightInfo) <span class="subdued">{...}</span>;`

| object | highlightInfo | 
|---|---|

| integer | windowId | 
|---|---|

The window whose tabs changed.
 |
| array of integer | tabIds | 

All highlighted tabs in the window.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onDetached

<div class="description">

Fired when a tab is detached from a window, for example because it is being moved between windows.

<div>

#### addListener

<div class="summary">`whale.tabs.onDetached.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(integer tabId, object detachInfo) <span class="subdued">{...}</span>;`

| integer | tabId |  |
|---|---|---|
| object | detachInfo | 

| integer | oldWindowId |  |
|---|---|---|
| integer | oldPosition |  |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onAttached

<div class="description">

Fired when a tab is attached to a window, for example because it was moved between windows.

<div>

#### addListener

<div class="summary">`whale.tabs.onAttached.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(integer tabId, object attachInfo) <span class="subdued">{...}</span>;`

| integer | tabId |  |
|---|---|---|
| object | attachInfo | 

| integer | newWindowId |  |
|---|---|---|
| integer | newPosition |  |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onRemoved

<div class="description">

Fired when a tab is closed.

<div>

#### addListener

<div class="summary">`whale.tabs.onRemoved.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(integer tabId, object removeInfo) <span class="subdued">{...}</span>;`

| integer | tabId |  |
|---|---|---|
| object | removeInfo | 

| integer | windowId | 
|---|---|

Since Chrome 25.

The window whose tab is closed.
 |
| boolean | isWindowClosing | 

True when the tab is being closed because its window is being closed.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onReplaced

<div class="description">

Since Chrome 26.

Fired when a tab is replaced with another tab due to prerendering or instant.

<div>

#### addListener

<div class="summary">`whale.tabs.onReplaced.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(integer addedTabId, integer removedTabId) <span class="subdued">{...}</span>;`

| integer | addedTabId |  |
|---|---|---|
| integer | removedTabId |  |
 |

</div>

</div>

</div>

</div>

<div>

### onZoomChange

<div class="description">

Since Chrome 38.

Fired when a tab is zoomed.

<div>

#### addListener

<div class="summary">`whale.tabs.onZoomChange.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object ZoomChangeInfo) <span class="subdued">{...}</span>;`

| object | ZoomChangeInfo | 
|---|---|

| integer | tabId |  |
|---|---|---|
| double | oldZoomFactor |  |
| double | newZoomFactor |  |
| [ZoomSettings](/extensions/tabs#type-ZoomSettings) | zoomSettings |  |
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>