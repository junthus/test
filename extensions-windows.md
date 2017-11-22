# whale.windows

| Description: | Use the `whale.windows` API to interact with browser windows. You can use this API to create, modify, and rearrange windows in the browser. |
|---|---|
| Availability: | Since Chrome 20. |
| Permissions: | The `whale.windows` API can be used without declaring any permission. However, the `"tabs"` permission is required in order to populate the `[url](tabs#property-Tab-url)`, `[title](tabs#property-Tab-title)`, and `[favIconUrl](tabs#property-Tab-favIconUrl)` properties of `[Tab](tabs#type-Tab)` objects. |

<section>

## Manifest

When requested, a `[windows.Window](/extensions/windows#type-Window)` will contain an array of `[tabs.Tab](/extensions/tabs#type-Tab)` objects. You must declare the `"tabs"` permission in your [manifest](manifest) if you require access to the `[url](/extensions/tabs#property-Tab-url)`, `[title](/extensions/tabs#property-Tab-title)`, or `[favIconUrl](/extensions/tabs#property-Tab-favIconUrl)` properties of `[tabs.Tab](/extensions/tabs#type-Tab)`. For example:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"permissions": ["tabs"]**,
        ...
      }
      </pre>

## The current window

Many functions in the extension system take an optional <var>windowId</var> parameter, which defaults to the current window.

The _current window_ is the window that contains the code that is currently executing. It's important to realize that this can be different from the topmost or focused window.

For example, say an extension creates a few tabs or windows from a single HTML file, and that the HTML file contains a call to [tabs.query](/extensions/tabs#method-query). The current window is the window that contains the page that made the call, no matter what the topmost window is.

In the case of the [event page](event_pages), the value of the current window falls back to the last active window. Under some circumstances, there may be no current window for background pages.

## Examples

![Two windows, each with one tab](/static/images/windows.png)
You can find simple examples of using the windows module in the [examples/api/windows](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/docs/examples/api/windows/) directory. Another example is in the [tabs_api.html](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/docs/examples/api/tabs/inspector/tabs_api.html) file of the [inspector](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/docs/examples/api/tabs/inspector/) example. For other examples and for help in viewing the source code, see [Samples](samples).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [WindowType](#type-WindowType) |
| [WindowState](#type-WindowState) |
| [Window](#type-Window) |
| [CreateType](#type-CreateType) |
| Properties |
| [WINDOW_ID_NONE](#property-WINDOW_ID_NONE) |
| [WINDOW_ID_CURRENT](#property-WINDOW_ID_CURRENT) |
| Methods |
| [get](#method-get) − `whale.windows.get(<span>integer windowId</span>, <span class="optional">object getInfo</span>, <span>function callback</span>)` |
| [getCurrent](#method-getCurrent) − `whale.windows.getCurrent(<span class="optional">object getInfo</span>, <span>function callback</span>)` |
| [getLastFocused](#method-getLastFocused) − `whale.windows.getLastFocused(<span class="optional">object getInfo</span>, <span>function callback</span>)` |
| [getAll](#method-getAll) − `whale.windows.getAll(<span class="optional">object getInfo</span>, <span>function callback</span>)` |
| [create](#method-create) − `whale.windows.create(<span class="optional">object createData</span>, <span class="optional">function callback</span>)` |
| [update](#method-update) − `whale.windows.update(<span>integer windowId</span>, <span>object updateInfo</span>, <span class="optional">function callback</span>)` |
| [remove](#method-remove) − `whale.windows.remove(<span>integer windowId</span>, <span class="optional">function callback</span>)` |
| Events |
| [onCreated](#event-onCreated) |
| [onRemoved](#event-onRemoved) |
| [onFocusChanged](#event-onFocusChanged) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### WindowType

<dd>The type of browser window this is. Under some circumstances a Window may not be assigned type property, for example when querying closed windows from the [sessions](/extensions/sessions) API.</dd>

| Enum |
|---|
| `"normal"`, `"popup"`, `"panel"`, `"app"`, or `"devtools"` |

</div>

<div>

### WindowState

<dd>The state of this browser window. Under some circumstances a Window may not be assigned state property, for example when querying closed windows from the [sessions](/extensions/sessions) API.</dd>

| Enum |
|---|
| 

<dl>

<dt>`"normal"`</dt>

<dd>Normal window state (i.e. not minimized, maximized, or fullscreen).</dd>

</dl>

<dl>

<dt>`"minimized"`</dt>

<dd>Minimized window state.</dd>

</dl>

<dl>

<dt>`"maximized"`</dt>

<dd>Maximized window state.</dd>

</dl>

<dl>

<dt>`"fullscreen"`</dt>

<dd>Fullscreen window state.</dd>

</dl>

<dl>

<dt>`"docked"`</dt>

<dd>_Deprecated since Chrome M59._ Docked windows are no longer supported. This state will be converted to "normal".</dd>

</dl>
 |

</div>

<div>

### Window

| properties |
|---|
| integer | <span class="optional">(optional)</span> id | 

The ID of the window. Window IDs are unique within a browser session. Under some circumstances a Window may not be assigned an ID, for example when querying windows using the [sessions](/extensions/sessions) API, in which case a session ID may be present.
 |
| boolean | focused | 

Whether the window is currently the focused window.
 |
| integer | <span class="optional">(optional)</span> top | 

The offset of the window from the top edge of the screen in pixels. Under some circumstances a Window may not be assigned top property, for example when querying closed windows from the [sessions](/extensions/sessions) API.
 |
| integer | <span class="optional">(optional)</span> left | 

The offset of the window from the left edge of the screen in pixels. Under some circumstances a Window may not be assigned left property, for example when querying closed windows from the [sessions](/extensions/sessions) API.
 |
| integer | <span class="optional">(optional)</span> width | 

The width of the window, including the frame, in pixels. Under some circumstances a Window may not be assigned width property, for example when querying closed windows from the [sessions](/extensions/sessions) API.
 |
| integer | <span class="optional">(optional)</span> height | 

The height of the window, including the frame, in pixels. Under some circumstances a Window may not be assigned height property, for example when querying closed windows from the [sessions](/extensions/sessions) API.
 |
| array of [tabs.Tab](/extensions/tabs#type-Tab) | <span class="optional">(optional)</span> tabs | 

Array of [tabs.Tab](/extensions/tabs#type-Tab) objects representing the current tabs in the window.
 |
| boolean | incognito | 

Whether the window is incognito.
 |
| [WindowType](/extensions/windows#type-WindowType) | <span class="optional">(optional)</span> type | 

The type of browser window this is.
 |
| [WindowState](/extensions/windows#type-WindowState) | <span class="optional">(optional)</span> state | 

The state of this browser window.
 |
| boolean | alwaysOnTop | 

Whether the window is set to be always on top.
 |
| string | <span class="optional">(optional)</span> sessionId | 

Since Chrome 31.

The session ID used to uniquely identify a Window obtained from the [sessions](/extensions/sessions) API.
 |

</div>

<div>

### CreateType

<dd>Specifies what type of browser window to create. 'panel' is deprecated and only available to existing whitelisted extensions on Chrome OS.</dd>

| Enum |
|---|
| `"normal"`, `"popup"`, or `"panel"` |

</div>

## Properties

| <span class="type_name">`-1`</span> | `whale.windows.WINDOW_ID_NONE` | The windowId value that represents the absence of a chrome browser window. |
|---|---|---|
| <span class="type_name">`-2`</span> | `whale.windows.WINDOW_ID_CURRENT` | The windowId value that represents the [current window](windows#current-window). |

## Methods

<div>

### get

<div class="summary">`whale.windows.get(<span>integer windowId</span>, <span class="optional">object getInfo</span>, <span>function callback</span>)`</div>

<div class="description">

Gets details about a window.

| Parameters |
|---|
| integer | windowId |  |
| object | <span class="optional">(optional)</span> getInfo | 

| boolean | <span class="optional">(optional)</span> populate | 
|---|---|

If true, the [windows.Window](/extensions/windows#type-Window) object will have a <var>tabs</var> property that contains a list of the [tabs.Tab](/extensions/tabs#type-Tab) objects. The `Tab` objects only contain the `url`, `title` and `favIconUrl` properties if the extension's manifest file includes the `"tabs"` permission.
 |
| array of [WindowType](/extensions/windows#type-WindowType) | <span class="optional">(optional)</span> windowTypes | 

Since Chrome 46.

If set, the [windows.Window](/extensions/windows#type-Window) returned will be filtered based on its type. If unset the default filter is set to `['app', 'normal', 'panel', 'popup']`, with `'app'` and `'panel'` window types limited to the extension's own windows.
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Window](/extensions/windows#type-Window) window) <span class="subdued">{...}</span>;`

| [Window](/extensions/windows#type-Window) | window |  |
|---|---|---|
 |

</div>

</div>

<div>

### getCurrent

<div class="summary">`whale.windows.getCurrent(<span class="optional">object getInfo</span>, <span>function callback</span>)`</div>

<div class="description">

Gets the [current window](#current-window).

| Parameters |
|---|
| object | <span class="optional">(optional)</span> getInfo | 

| boolean | <span class="optional">(optional)</span> populate | 
|---|---|

If true, the [windows.Window](/extensions/windows#type-Window) object will have a <var>tabs</var> property that contains a list of the [tabs.Tab](/extensions/tabs#type-Tab) objects. The `Tab` objects only contain the `url`, `title` and `favIconUrl` properties if the extension's manifest file includes the `"tabs"` permission.
 |
| array of [WindowType](/extensions/windows#type-WindowType) | <span class="optional">(optional)</span> windowTypes | 

Since Chrome 46.

If set, the [windows.Window](/extensions/windows#type-Window) returned will be filtered based on its type. If unset the default filter is set to `['app', 'normal', 'panel', 'popup']`, with `'app'` and `'panel'` window types limited to the extension's own windows.
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Window](/extensions/windows#type-Window) window) <span class="subdued">{...}</span>;`

| [Window](/extensions/windows#type-Window) | window |  |
|---|---|---|
 |

</div>

</div>

<div>

### getLastFocused

<div class="summary">`whale.windows.getLastFocused(<span class="optional">object getInfo</span>, <span>function callback</span>)`</div>

<div class="description">

Gets the window that was most recently focused — typically the window 'on top'.

| Parameters |
|---|
| object | <span class="optional">(optional)</span> getInfo | 

| boolean | <span class="optional">(optional)</span> populate | 
|---|---|

If true, the [windows.Window](/extensions/windows#type-Window) object will have a <var>tabs</var> property that contains a list of the [tabs.Tab](/extensions/tabs#type-Tab) objects. The `Tab` objects only contain the `url`, `title` and `favIconUrl` properties if the extension's manifest file includes the `"tabs"` permission.
 |
| array of [WindowType](/extensions/windows#type-WindowType) | <span class="optional">(optional)</span> windowTypes | 

Since Chrome 46.

If set, the [windows.Window](/extensions/windows#type-Window) returned will be filtered based on its type. If unset the default filter is set to `['app', 'normal', 'panel', 'popup']`, with `'app'` and `'panel'` window types limited to the extension's own windows.
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Window](/extensions/windows#type-Window) window) <span class="subdued">{...}</span>;`

| [Window](/extensions/windows#type-Window) | window |  |
|---|---|---|
 |

</div>

</div>

<div>

### getAll

<div class="summary">`whale.windows.getAll(<span class="optional">object getInfo</span>, <span>function callback</span>)`</div>

<div class="description">

Gets all windows.

| Parameters |
|---|
| object | <span class="optional">(optional)</span> getInfo | 

| boolean | <span class="optional">(optional)</span> populate | 
|---|---|

If true, each [windows.Window](/extensions/windows#type-Window) object will have a <var>tabs</var> property that contains a list of the [tabs.Tab](/extensions/tabs#type-Tab) objects for that window. The `Tab` objects only contain the `url`, `title` and `favIconUrl` properties if the extension's manifest file includes the `"tabs"` permission.
 |
| array of [WindowType](/extensions/windows#type-WindowType) | <span class="optional">(optional)</span> windowTypes | 

Since Chrome 46.

If set, the [windows.Window](/extensions/windows#type-Window) returned will be filtered based on its type. If unset the default filter is set to `['app', 'normal', 'panel', 'popup']`, with `'app'` and `'panel'` window types limited to the extension's own windows.
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [Window](/extensions/windows#type-Window) windows) <span class="subdued">{...}</span>;`

| array of [Window](/extensions/windows#type-Window) | windows |  |
|---|---|---|
 |

</div>

</div>

<div>

### create

<div class="summary">`whale.windows.create(<span class="optional">object createData</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Creates (opens) a new browser with any optional sizing, position or default URL provided.

| Parameters |
|---|
| object | <span class="optional">(optional)</span> createData | 

| string or array of string | <span class="optional">(optional)</span> url | 
|---|---|

A URL or array of URLs to open as tabs in the window. Fully-qualified URLs must include a scheme (i.e. 'http://www.google.com', not 'www.google.com'). Relative URLs will be relative to the current page within the extension. Defaults to the New Tab Page.
 |
| integer | <span class="optional">(optional)</span> tabId | 

The id of the tab for which you want to adopt to the new window.
 |
| integer | <span class="optional">(optional)</span> left | 

The number of pixels to position the new window from the left edge of the screen. If not specified, the new window is offset naturally from the last focused window. This value is ignored for panels.
 |
| integer | <span class="optional">(optional)</span> top | 

The number of pixels to position the new window from the top edge of the screen. If not specified, the new window is offset naturally from the last focused window. This value is ignored for panels.
 |
| integer | <span class="optional">(optional)</span> width | 

The width in pixels of the new window, including the frame. If not specified defaults to a natural width.
 |
| integer | <span class="optional">(optional)</span> height | 

The height in pixels of the new window, including the frame. If not specified defaults to a natural height.
 |
| boolean | <span class="optional">(optional)</span> focused | 

If true, opens an active window. If false, opens an inactive window.
 |
| boolean | <span class="optional">(optional)</span> incognito | 

Whether the new window should be an incognito window.
 |
| [CreateType](/extensions/windows#type-CreateType) | <span class="optional">(optional)</span> type | 

Specifies what type of browser window to create.
 |
| [WindowState](/extensions/windows#type-WindowState) | <span class="optional">(optional)</span> state | 

Since Chrome 44.

The initial state of the window. The 'minimized', 'maximized' and 'fullscreen' states cannot be combined with 'left', 'top', 'width' or 'height'.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [Window](/extensions/windows#type-Window) window) <span class="subdued">{...}</span>;`

| [Window](/extensions/windows#type-Window) | <span class="optional">(optional)</span> window | 
|---|---|

Contains details about the created window.
 |
 |

</div>

</div>

<div>

### update

<div class="summary">`whale.windows.update(<span>integer windowId</span>, <span>object updateInfo</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Updates the properties of a window. Specify only the properties that you want to change; unspecified properties will be left unchanged.

| Parameters |
|---|
| integer | windowId |  |
| object | updateInfo | 

| integer | <span class="optional">(optional)</span> left | 
|---|---|

The offset from the left edge of the screen to move the window to in pixels. This value is ignored for panels.
 |
| integer | <span class="optional">(optional)</span> top | 

The offset from the top edge of the screen to move the window to in pixels. This value is ignored for panels.
 |
| integer | <span class="optional">(optional)</span> width | 

The width to resize the window to in pixels. This value is ignored for panels.
 |
| integer | <span class="optional">(optional)</span> height | 

The height to resize the window to in pixels. This value is ignored for panels.
 |
| boolean | <span class="optional">(optional)</span> focused | 

If true, brings the window to the front. If false, brings the next window in the z-order to the front.
 |
| boolean | <span class="optional">(optional)</span> drawAttention | 

If true, causes the window to be displayed in a manner that draws the user's attention to the window, without changing the focused window. The effect lasts until the user changes focus to the window. This option has no effect if the window already has focus. Set to false to cancel a previous draw attention request.
 |
| [WindowState](/extensions/windows#type-WindowState) | <span class="optional">(optional)</span> state | 

The new state of the window. The 'minimized', 'maximized' and 'fullscreen' states cannot be combined with 'left', 'top', 'width' or 'height'.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [Window](/extensions/windows#type-Window) window) <span class="subdued">{...}</span>;`

| [Window](/extensions/windows#type-Window) | window |  |
|---|---|---|
 |

</div>

</div>

<div>

### remove

<div class="summary">`whale.windows.remove(<span>integer windowId</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Removes (closes) a window, and all the tabs inside it.

| Parameters |
|---|
| integer | windowId |  |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

## Events

<div>

### onCreated

<div class="description">

Fired when a window is created.

#### Filters

<dl>array of [WindowType](/extensions/windows#type-WindowType) windowTypes

Conditions that the window's type being created must satisfy. By default it will satisfy `['app', 'normal', 'panel', 'popup']`, with `'app'` and `'panel'` window types limited to the extension's own windows.

</dl>

<div>

#### addListener

<div class="summary">`whale.windows.onCreated.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Window](/extensions/windows#type-Window) window) <span class="subdued">{...}</span>;`

| [Window](/extensions/windows#type-Window) | window | 
|---|---|

Details of the window that was created.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onRemoved

<div class="description">

Fired when a window is removed (closed).

#### Filters

<dl>array of [WindowType](/extensions/windows#type-WindowType) windowTypes

Conditions that the window's type being removed must satisfy. By default it will satisfy `['app', 'normal', 'panel', 'popup']`, with `'app'` and `'panel'` window types limited to the extension's own windows.

</dl>

<div>

#### addListener

<div class="summary">`whale.windows.onRemoved.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(integer windowId) <span class="subdued">{...}</span>;`

| integer | windowId | 
|---|---|

ID of the removed window.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onFocusChanged

<div class="description">

Fired when the currently focused window changes. Will be whale.windows.WINDOW_ID_NONE if all chrome windows have lost focus. Note: On some Linux window managers, WINDOW_ID_NONE will always be sent immediately preceding a switch from one chrome window to another.

#### Filters

<dl>array of [WindowType](/extensions/windows#type-WindowType) windowTypes

Conditions that the window's type being removed must satisfy. By default it will satisfy `['app', 'normal', 'panel', 'popup']`, with `'app'` and `'panel'` window types limited to the extension's own windows.

</dl>

<div>

#### addListener

<div class="summary">`whale.windows.onFocusChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(integer windowId) <span class="subdued">{...}</span>;`

| integer | windowId | 
|---|---|

ID of the newly focused window.
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>