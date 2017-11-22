# whale.desktopCapture

| Description: | Desktop Capture API that can be used to capture content of screen, individual windows or tabs. |
|---|---|
| Availability: | Since Chrome 34. |
| Permissions: | <span class="code">"desktopCapture"</span> |

<section id="toc">

## Summary

| Types |
|---|
| [DesktopCaptureSourceType](#type-DesktopCaptureSourceType) |
| Methods |
| [chooseDesktopMedia](#method-chooseDesktopMedia) − `integer whale.desktopCapture.chooseDesktopMedia(<span>array of [DesktopCaptureSourceType](/extensions/desktopCapture#type-DesktopCaptureSourceType) sources</span>, <span class="optional">tabs.Tab targetTab</span>, <span>function callback</span>)` |
| [cancelChooseDesktopMedia](#method-cancelChooseDesktopMedia) − `whale.desktopCapture.cancelChooseDesktopMedia(<span>integer desktopMediaRequestId</span>)` |

</section>

<section>

<div class="api-reference">

## Types

<div>

### DesktopCaptureSourceType

<dd>Enum used to define set of desktop media sources used in chooseDesktopMedia().</dd>

| Enum |
|---|
| `"screen"`, `"window"`, `"tab"`, or `"audio"` |

</div>

## Methods

<div>

### chooseDesktopMedia

<div class="summary">`integer whale.desktopCapture.chooseDesktopMedia(<span>array of [DesktopCaptureSourceType](/extensions/desktopCapture#type-DesktopCaptureSourceType) sources</span>, <span class="optional">[tabs.Tab](/extensions/tabs#type-Tab) targetTab</span>, <span>function callback</span>)`</div>

<div class="description">

Shows desktop media picker UI with the specified set of sources.

| Parameters |
|---|
| array of [DesktopCaptureSourceType](/extensions/desktopCapture#type-DesktopCaptureSourceType) | sources | 

Set of sources that should be shown to the user. The sources order in the set decides the tab order in the picker.
 |
| [tabs.Tab](/extensions/tabs#type-Tab) | <span class="optional">(optional)</span> targetTab | 

Optional tab for which the stream is created. If not specified then the resulting stream can be used only by the calling extension. The stream can only be used by frames in the given tab whose security origin matches `tab.url`. The tab's origin must be a secure origin, e.g. HTTPS.
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string streamId, object options) <span class="subdued">{...}</span>;`

| string | streamId | 
|---|---|

An opaque string that can be passed to `getUserMedia()` API to generate media stream that corresponds to the source selected by the user. If user didn't select any source (i.e. canceled the prompt) then the callback is called with an empty `streamId`. The created `streamId` can be used only once and expires after a few seconds when it is not used.
 |
| object | options | 

Contains properties that describe the stream.

| boolean | canRequestAudioTrack | 
|---|---|

True if "audio" is included in parameter sources, and the end user does not uncheck the "Share audio" checkbox. Otherwise false, and in this case, one should not ask for audio stream through getUserMedia call.
 |
 |
 |

</div>

</div>

<div>

### cancelChooseDesktopMedia

<div class="summary">`whale.desktopCapture.cancelChooseDesktopMedia(<span>integer desktopMediaRequestId</span>)`</div>

<div class="description">

Hides desktop media picker dialog shown by chooseDesktopMedia().

| Parameters |
|---|
| integer | desktopMediaRequestId | 

Id returned by chooseDesktopMedia()
 |

</div>

</div>

</div>

</section>