# whale.pageCapture

| Description: | Use the `whale.pageCapture` API to save a tab as MHTML. |
|---|---|
| Availability: | Since Chrome 19. |
| Permissions: | <span class="code">"pageCapture"</span> |

<section>

MHTML is a [standard format](http://tools.ietf.org/html/rfc2557) supported by most browsers. It encapsulates in a single file a page and all its resources (CSS files, images..).

Note that for security reasons a MHTML file can only be loaded from the file system and that it can only be loaded in the main frame.

## Manifest

You must declare the "pageCapture" permission in the [extension manifest](manifest) to use the pageCapture API. For example:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"permissions": [
          "pageCapture"
        ]**,
        ...
      }
      </pre>

</section>

<section id="toc">

## Summary

| Methods |
|---|
| [saveAsMHTML](#method-saveAsMHTML) − `whale.pageCapture.saveAsMHTML(<span>object details</span>, <span>function callback</span>)` |

</section>

<section>

<div class="api-reference">

## Methods

<div>

### saveAsMHTML

<div class="summary">`whale.pageCapture.saveAsMHTML(<span>object details</span>, <span>function callback</span>)`</div>

<div class="description">

Saves the content of the tab with given id as MHTML.

| Parameters |
|---|
| object | details | 

| integer | tabId | 
|---|---|

The id of the tab to save as MHTML.
 |
 |
| function | callback | 

Called when the MHTML has been generated.

The _callback_ parameter should be a function that looks like this:

`function(binary mhtmlData) <span class="subdued">{...}</span>;`

| binary | <span class="optional">(optional)</span> mhtmlData | 
|---|---|

The MHTML data as a Blob.
 |
 |

</div>

</div>

</div>

</section>