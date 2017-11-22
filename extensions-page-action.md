# whale.pageAction

| Description: | Use the `whale.pageAction` API to put icons in the main Google Chrome toolbar, to the right of the address bar. Page actions represent actions that can be taken on the current page, but that aren't applicable to all pages. Page actions appear grayed out when inactive. |
|---|---|
| Availability: | Since Chrome 19. |
| Manifest: | <span class="code">"page_action": {...}</span> |

<section>

Some examples:

*   Subscribe to this page's RSS feed
*   Make a slideshow out of this page's photos

The RSS icon in the following screenshot represents a page action that lets you subscribe to the RSS feed for the current page.

![](/static/images/page_action.png)

Hidden page actions appear grayed out. For example, the RSS feed below is grayed out, as you can't subscribe to the feed for the current page:

![](/static/images/page_action_grey.png)

Please consider using a [browser action](browserAction) instead, so that users can always interact with your extension.

## Manifest

Register your page action in the [extension manifest](manifest) like this:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"page_action": {
          "default_icon": {                    _// optional_
            "16": "images/icon16.png",           _// optional_
            "24": "images/icon24.png",           _// optional_
            "32": "images/icon32.png"            _// optional_
          },
          "default_title": "Google Mail",      _// optional; shown in tooltip_
          "default_popup": "popup.html"        _// optional_
        }**,
        ...
      }</pre>

You can provide any size icon to be used in Chrome, and Chrome will select the closest one and scale it to the appropriate size to fill the 16-dip space. However, if the exact size isn't provided, this scaling can cause the icon to lose detail or look fuzzy.

Since devices with less-common scale factors like 1.5x or 1.2x are becoming more common, you are encouraged to provide multiple sizes for your icons. This also ensures that if the icon display size is ever changed, you don't need to do any more work to provide different icons!

The old syntax for registering the default icon is still supported:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"page_action": {
          ...
          "default_icon": "images/icon32.png"  _// optional_
          _// equivalent to "default_icon": { "32": "images/icon32.png" }_
        }**,
        ...
      }
      </pre>

## Parts of the UI

Like browser actions, page actions can have an icon, a tooltip, and popup; they can't have badges, however. In addition, page actions can be grayed out. You can find information about icons, tooltips, and popups by reading about the [browser action UI](browserAction#ui).

You make a page action appear and be grayed out using the [pageAction.show](/extensions/pageAction#method-show) and [pageAction.hide](/extensions/pageAction#method-hide) methods, respectively. By default, a page action appears grayed out. When you show it, you specify the tab in which the icon should appear. The icon remains visible until the tab is closed or starts displaying a different URL (because the user clicks a link, for example).

## Tips

For the best visual impact, follow these guidelines:

*   **Do** use page actions for features that make sense for only a few pages.
*   **Don't** use page actions for features that make sense for most pages. Use [browser actions](browserAction) instead.
*   **Don't** constantly animate your icon. That's just annoying.

## Examples

You can find simple examples of using page actions in the [examples/api/pageAction](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/docs/examples/api/pageAction/) directory. For other examples and for help in viewing the source code, see [Samples](samples).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [ImageDataType](#type-ImageDataType) |
| Methods |
| [show](#method-show) − `whale.pageAction.show(<span>integer tabId</span>)` |
| [hide](#method-hide) − `whale.pageAction.hide(<span>integer tabId</span>)` |
| [setTitle](#method-setTitle) − `whale.pageAction.setTitle(<span>object details</span>)` |
| [getTitle](#method-getTitle) − `whale.pageAction.getTitle(<span>object details</span>, <span>function callback</span>)` |
| [setIcon](#method-setIcon) − `whale.pageAction.setIcon(<span>object details</span>, <span class="optional">function callback</span>)` |
| [setPopup](#method-setPopup) − `whale.pageAction.setPopup(<span>object details</span>)` |
| [getPopup](#method-getPopup) − `whale.pageAction.getPopup(<span>object details</span>, <span>function callback</span>)` |
| Events |
| [onClicked](#event-onClicked) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### ImageDataType

Since Chrome 23.

<dd>Pixel data for an image. Must be an ImageData object (for example, from a `canvas` element).</dd>

</div>

## Methods

<div>

### show

<div class="summary">`whale.pageAction.show(<span>integer tabId</span>)`</div>

<div class="description">

Shows the page action. The page action is shown whenever the tab is selected.

| Parameters |
|---|
| integer | tabId | 

The id of the tab for which you want to modify the page action.
 |

</div>

</div>

<div>

### hide

<div class="summary">`whale.pageAction.hide(<span>integer tabId</span>)`</div>

<div class="description">

Hides the page action. Hidden page actions still appear in the Chrome toolbar, but are grayed out.

| Parameters |
|---|
| integer | tabId | 

The id of the tab for which you want to modify the page action.
 |

</div>

</div>

<div>

### setTitle

<div class="summary">`whale.pageAction.setTitle(<span>object details</span>)`</div>

<div class="description">

Sets the title of the page action. This is displayed in a tooltip over the page action.

| Parameters |
|---|
| object | details | 

| integer | tabId | 
|---|---|

The id of the tab for which you want to modify the page action.
 |
| string | title | 

The tooltip string.
 |
 |

</div>

</div>

<div>

### getTitle

<div class="summary">`whale.pageAction.getTitle(<span>object details</span>, <span>function callback</span>)`</div>

<div class="description">

Gets the title of the page action.

| Parameters |
|---|
| object | details | 

| integer | tabId | 
|---|---|

Specify the tab to get the title from.
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string result) <span class="subdued">{...}</span>;`

| string | result |  |
|---|---|---|
 |

</div>

</div>

<div>

### setIcon

<div class="summary">`whale.pageAction.setIcon(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Sets the icon for the page action. The icon can be specified either as the path to an image file or as the pixel data from a canvas element, or as dictionary of either one of those. Either the **path** or the **imageData** property must be specified.

| Parameters |
|---|
| object | details | 

| integer | tabId | 
|---|---|

The id of the tab for which you want to modify the page action.
 |
| [ImageDataType](/extensions/pageAction#type-ImageDataType) or object | <span class="optional">(optional)</span> imageData | 

Either an ImageData object or a dictionary {size -> ImageData} representing icon to be set. If the icon is specified as a dictionary, the actual image to be used is chosen depending on screen's pixel density. If the number of image pixels that fit into one screen space unit equals `scale`, then image with size `scale` * n will be selected, where n is the size of the icon in the UI. At least one image must be specified. Note that 'details.imageData = foo' is equivalent to 'details.imageData = {'16': foo}'
 |
| string or object | <span class="optional">(optional)</span> path | 

Either a relative image path or a dictionary {size -> relative image path} pointing to icon to be set. If the icon is specified as a dictionary, the actual image to be used is chosen depending on screen's pixel density. If the number of image pixels that fit into one screen space unit equals `scale`, then image with size `scale` * n will be selected, where n is the size of the icon in the UI. At least one image must be specified. Note that 'details.path = foo' is equivalent to 'details.path = {'16': foo}'
 |
| integer | <span class="optional">(optional)</span> iconIndex | 

**Deprecated.** This argument is ignored.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### setPopup

<div class="summary">`whale.pageAction.setPopup(<span>object details</span>)`</div>

<div class="description">

Sets the html document to be opened as a popup when the user clicks on the page action's icon.

| Parameters |
|---|
| object | details | 

| integer | tabId | 
|---|---|

The id of the tab for which you want to modify the page action.
 |
| string | popup | 

The html file to show in a popup. If set to the empty string (''), no popup is shown.
 |
 |

</div>

</div>

<div>

### getPopup

<div class="summary">`whale.pageAction.getPopup(<span>object details</span>, <span>function callback</span>)`</div>

<div class="description">

Gets the html document set as the popup for this page action.

| Parameters |
|---|
| object | details | 

| integer | tabId | 
|---|---|

Specify the tab to get the popup from.
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string result) <span class="subdued">{...}</span>;`

| string | result |  |
|---|---|---|
 |

</div>

</div>

## Events

<div>

### onClicked

<div class="description">

Fired when a page action icon is clicked. This event will not fire if the page action has a popup.

<div>

#### addListener

<div class="summary">`whale.pageAction.onClicked.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [tabs.Tab](/extensions/tabs#type-Tab) tab) <span class="subdued">{...}</span>;`

| [tabs.Tab](/extensions/tabs#type-Tab) | tab |  |
|---|---|---|
 |

</div>

</div>

</div>

</div>

</div>

</section>