# whale.tabCapture

| Description: | Use the `whale.tabCapture` API to interact with tab media streams. |
|---|---|
| Availability: | Since Chrome 31. |
| Permissions: | <span class="code">"tabCapture"</span> |

<section id="toc">

## Summary

| Types |
|---|
| [CaptureInfo](#type-CaptureInfo) |
| [MediaStreamConstraint](#type-MediaStreamConstraint) |
| [CaptureOptions](#type-CaptureOptions) |
| Methods |
| [capture](#method-capture) − `whale.tabCapture.capture( <span>CaptureOptions options</span>, <span>function callback</span>)` |
| [getCapturedTabs](#method-getCapturedTabs) − `whale.tabCapture.getCapturedTabs(<span>function callback</span>)` |
| [captureOffscreenTab](#method-captureOffscreenTab) − `whale.tabCapture.captureOffscreenTab(<span>string startUrl</span>, <span>CaptureOptions options</span>, <span>function callback</span>)` |
| Events |
| [onStatusChanged](#event-onStatusChanged) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### CaptureInfo

| properties |
|---|
| integer | tabId | 

The id of the tab whose status changed.
 |
| enum of `"pending"`, `"active"`, `"stopped"`, or `"error"` | status | 

The new capture status of the tab.
 |
| boolean | fullscreen | 

Whether an element in the tab being captured is in fullscreen mode.
 |

</div>

<div>

### MediaStreamConstraint

| properties |
|---|
| object | mandatory |  |
| object | <span class="optional">(optional)</span> optional | 

Since Chrome 32.
 |

</div>

<div>

### CaptureOptions

| properties |
|---|
| boolean | <span class="optional">(optional)</span> audio |  |
| boolean | <span class="optional">(optional)</span> video |  |
| [MediaStreamConstraint](/extensions/tabCapture#type-MediaStreamConstraint) | <span class="optional">(optional)</span> audioConstraints |  |
| [MediaStreamConstraint](/extensions/tabCapture#type-MediaStreamConstraint) | <span class="optional">(optional)</span> videoConstraints |  |

</div>

## Methods

<div>

### capture

<div class="summary">`whale.tabCapture.capture( <span>[CaptureOptions](/extensions/tabCapture#type-CaptureOptions) options</span>, <span>function callback</span>)`</div>

<div class="description">

Captures the visible area of the currently active tab. Capture can only be started on the currently active tab after the extension has been _invoked_, similar to the way that [activeTab](activeTab#invoking-activeTab) works. Capture is maintained across page navigations within the tab, and stops when the tab is closed, or the media stream is closed by the extension.

| Parameters |
|---|
| [CaptureOptions](/extensions/tabCapture#type-CaptureOptions) | options | 

Configures the returned media stream.
 |
| function | callback | 

Callback with either the tab capture MediaStream or `null`. `null` indicates an error has occurred and the client may query whale.runtime.lastError to access the error details.

The _callback_ parameter should be a function that looks like this:

`function(LocalMediaStream stream) <span class="subdued">{...}</span>;`

| LocalMediaStream | stream |  |
|---|---|---|
 |

</div>

</div>

<div>

### getCapturedTabs

<div class="summary">`whale.tabCapture.getCapturedTabs(<span>function callback</span>)`</div>

<div class="description">

Returns a list of tabs that have requested capture or are being captured, i.e. status != stopped and status != error. This allows extensions to inform the user that there is an existing tab capture that would prevent a new tab capture from succeeding (or to prevent redundant requests for the same tab).

| Parameters |
|---|
| function | callback | 

Callback invoked with CaptureInfo[] for captured tabs.

The _callback_ parameter should be a function that looks like this:

`function(array of [CaptureInfo](/extensions/tabCapture#type-CaptureInfo) result) <span class="subdued">{...}</span>;`

| array of [CaptureInfo](/extensions/tabCapture#type-CaptureInfo) | result |  |
|---|---|---|
 |

</div>

</div>

<div>

### captureOffscreenTab

<div class="summary">`whale.tabCapture.captureOffscreenTab(<span>string startUrl</span>, <span>[CaptureOptions](/extensions/tabCapture#type-CaptureOptions) options</span>, <span>function callback</span>)`</div>

<div class="description">

Since Chrome 47.

Creates an off-screen tab and navigates it to the given |startUrl|. Then, capture is started and a MediaStream is returned via |callback|.

Off-screen tabs are isolated from the user's normal browser experience. They do not show up in the browser window or tab strip, nor are they visible to extensions (e.g., via the whale.tabs.* APIs).

An off-screen tab remains alive until one of three events occurs: 1\. All MediaStreams providing its captured content are closed; 2\. the page self-closes (e.g., via window.close()); 3\. the extension that called captureOffscreenTab() is unloaded.

Sandboxing: The off-screen tab does not have any access whatsoever to the local user profile (including cookies, HTTP auth, etc.). Instead, it is provided its own sandboxed profile. Also, it cannot access any interactive resources such as keyboard/mouse input, media recording devices (e.g., web cams), copy/paste to/from the system clipboard, etc.

Note: This is a new API, currently only available in Canary/Dev channel, and may change without notice.

| Parameters |
|---|
| string | startUrl |  |
| [CaptureOptions](/extensions/tabCapture#type-CaptureOptions) | options | 

Constraints for the capture and returned MediaStream.
 |
| function | callback | 

Callback with either the tab capture MediaStream or `null`. `null` indicates an error has occurred and the client may query whale.runtime.lastError to access the error details.

The _callback_ parameter should be a function that looks like this:

`function(LocalMediaStream stream) <span class="subdued">{...}</span>;`

| LocalMediaStream | stream |  |
|---|---|---|
 |

</div>

</div>

## Events

<div>

### onStatusChanged

<div class="description">

Event fired when the capture status of a tab changes. This allows extension authors to keep track of the capture status of tabs to keep UI elements like page actions in sync.

<div>

#### addListener

<div class="summary">`whale.tabCapture.onStatusChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [CaptureInfo](/extensions/tabCapture#type-CaptureInfo) info) <span class="subdued">{...}</span>;`

| [CaptureInfo](/extensions/tabCapture#type-CaptureInfo) | info | 
|---|---|

CaptureInfo with new capture status for the tab.
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>