# whale.extensionTypes

| Description: | The `whale.extensionTypes` API contains type declarations for Chrome extensions. |
|---|---|
| Availability: | Since Chrome 39. |

<section id="toc">

## Summary

| Types |
|---|
| [ImageFormat](#type-ImageFormat) |
| [ImageDetails](#type-ImageDetails) |
| [RunAt](#type-RunAt) |
| [InjectDetails](#type-InjectDetails) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### ImageFormat

<dd>The format of an image.</dd>

| Enum |
|---|
| `"jpeg"`, or `"png"` |

</div>

<div>

### ImageDetails

<dd>Details about the format and quality of an image.</dd>

| properties |
|---|
| enum of `"jpeg"`, or `"png"` | <span class="optional">(optional)</span> format | 

The format of the resulting image. Default is `"jpeg"`.
 |
| integer | <span class="optional">(optional)</span> quality | 

When format is `"jpeg"`, controls the quality of the resulting image. This value is ignored for PNG images. As quality is decreased, the resulting image will have more visual artifacts, and the number of bytes needed to store it will decrease.
 |

</div>

<div>

### RunAt

<dd>The soonest that the JavaScript or CSS will be injected into the tab.</dd>

| Enum |
|---|
| `"document_start"`, `"document_end"`, or `"document_idle"` |

</div>

<div>

### InjectDetails

<dd>Details of the script or CSS to inject. Either the code or the file property must be set, but both may not be set at the same time.</dd>

| properties |
|---|
| string | <span class="optional">(optional)</span> code | 

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

Since Chrome 50.

The [frame](webNavigation#frame_ids) where the script or CSS should be injected. Defaults to 0 (the top-level frame).
 |
| boolean | <span class="optional">(optional)</span> matchAboutBlank | 

If matchAboutBlank is true, then the code is also injected in about:blank and about:srcdoc frames if your extension has access to its parent document. Code cannot be inserted in top-level about:-frames. By default it is `false`.
 |
| enum of `"document_start"`, `"document_end"`, or `"document_idle"` | <span class="optional">(optional)</span> runAt | 

The soonest that the JavaScript or CSS will be injected into the tab. Defaults to "document_idle".
 |

</div>

</div>

</section>