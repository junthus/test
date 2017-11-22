# whale.browserAction

| Description: | Use browser actions to put icons in the main Google Chrome toolbar, to the right of the address bar. In addition to its [icon](browserAction#icon), a browser action can also have a [tooltip](browserAction#tooltip), a [badge](browserAction#badge), and a [popup](browserAction#popups). |
|---|---|
| Availability: | Since Chrome 19. |
| Manifest: | <span class="code">"browser_action": {...}</span> |

<section>

In the following figure, the multicolored square to the right of the address bar is the icon for a browser action. A popup is below the icon.

![](/static/images/browser-action.png)

If you want to create an icon that isn't always visible, use a [page action](pageAction) instead of a browser action.

## Manifest

Register your browser action in the [extension manifest](manifest) like this:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"browser_action": {
          "default_icon": {                    _// optional_
            "16": "images/icon16.png",           _// optional_
            "24": "images/icon24.png",           _// optional_
            "32": "images/icon32.png"            _// optional_
          },
          "default_title": "Google Mail",      _// optional; shown in tooltip_
          "default_popup": "popup.html"        _// optional_
        }**,
        ...
      }
      </pre>

You can provide any size icon to be used in Chrome, and Chrome will select the closest one and scale it to the appropriate size to fill the 16-dip space. However, if the exact size isn't provided, this scaling can cause the icon to lose detail or look fuzzy.

Since devices with less-common scale factors like 1.5x or 1.2x are becoming more common, you are encouraged to provide multiple sizes for your icons. This also ensures that if the icon display size is ever changed, you don't need to do any more work to provide different icons!

The old syntax for registering the default icon is still supported:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"browser_action": {
          ...
          "default_icon": "images/icon32.png"  _// optional_
          _// equivalent to "default_icon": { "32": "images/icon32.png" }_
        }**,
        ...
      }
      </pre>

## Parts of the UI

A browser action can have an [icon](#icon), a [tooltip](#tooltip), a [badge](#badge), and a [popup](#popups).

### Icon

The browser action icons in Chrome are 16 dips (device-independent pixels) wide and high. Larger icons are resized to fit, but for best results, use a 16-dip square icon.

You can set the icon in two ways: using a static image or using the HTML5 [canvas element](http://www.whatwg.org/specs/web-apps/current-work/multipage/the-canvas-element.html). Using static images is easier for simple applications, but you can create more dynamic UIs — such as smooth animation — using the canvas element.

Static images can be in any format WebKit can display, including BMP, GIF, ICO, JPEG, or PNG. For unpacked extensions, images must be in the PNG format.

To set the icon, use the **default_icon** field of **browser_action** in the [manifest](#manifest), or call the [browserAction.setIcon](/extensions/browserAction#method-setIcon) method.

To properly display icon when screen pixel density (ratio `size_in_pixel / size_in_dip`) is different than 1, the icon can be defined as set of images with different sizes. The actual image to display will be selected from the set to best fit the pixel size of 16 dip. The icon set can contain any size icon specification, and Chrome will select the most appropriate one.

### Tooltip

To set the tooltip, use the **default_title** field of **browser_action** in the [manifest](#manifest), or call the [browserAction.setTitle](/extensions/browserAction#method-setTitle) method. You can specify locale-specific strings for the **default_title** field; see [Internationalization](i18n) for details.

### Badge

Browser actions can optionally display a _badge_ — a bit of text that is layered over the icon. Badges make it easy to update the browser action to display a small amount of information about the state of the extension.

Because the badge has limited space, it should have 4 characters or less.

Set the text and color of the badge using [browserAction.setBadgeText](/extensions/browserAction#method-setBadgeText) and [browserAction.setBadgeBackgroundColor](/extensions/browserAction#method-setBadgeBackgroundColor), respectively.

### Popup

If a browser action has a popup, the popup appears when the user clicks the icon. The popup can contain any HTML contents that you like, and it's automatically sized to fit its contents.

To add a popup to your browser action, create an HTML file with the popup's contents. Specify the HTML file in the **default_popup** field of **browser_action** in the [manifest](#manifest), or call the [browserAction.setPopup](/extensions/browserAction#method-setPopup) method.

## Tips

For the best visual impact, follow these guidelines:

*   **Do** use browser actions for features that make sense on most pages.
*   **Don't** use browser actions for features that make sense for only a few pages. Use [page actions](pageAction) instead.
*   **Do** use big, colorful icons that make the most of the 16x16-dip space. Browser action icons should seem a little bigger and heavier than page action icons.
*   **Don't** attempt to mimic Google Chrome's monochrome menu icon. That doesn't work well with themes, and anyway, extensions should stand out a little.
*   **Do** use alpha transparency to add soft edges to your icon. Because many people use themes, your icon should look nice on a variety of background colors.
*   **Don't** constantly animate your icon. That's just annoying.

## Examples

You can find simple examples of using browser actions in the [examples/api/browserAction](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/docs/examples/api/browserAction/) directory. For other examples and for help in viewing the source code, see [Samples](samples).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [ColorArray](#type-ColorArray) |
| [ImageDataType](#type-ImageDataType) |
| Methods |
| [setTitle](#method-setTitle) − `whale.browserAction.setTitle(<span>object details</span>)` |
| [getTitle](#method-getTitle) − `whale.browserAction.getTitle(<span>object details</span>, <span>function callback</span>)` |
| [setIcon](#method-setIcon) − `whale.browserAction.setIcon(<span>object details</span>, <span class="optional">function callback</span>)` |
| [setPopup](#method-setPopup) − `whale.browserAction.setPopup(<span>object details</span>)` |
| [getPopup](#method-getPopup) − `whale.browserAction.getPopup(<span>object details</span>, <span>function callback</span>)` |
| [setBadgeText](#method-setBadgeText) − `whale.browserAction.setBadgeText(<span>object details</span>)` |
| [getBadgeText](#method-getBadgeText) − `whale.browserAction.getBadgeText(<span>object details</span>, <span>function callback</span>)` |
| [setBadgeBackgroundColor](#method-setBadgeBackgroundColor) − `whale.browserAction.setBadgeBackgroundColor(<span>object details</span>)` |
| [getBadgeBackgroundColor](#method-getBadgeBackgroundColor) − `whale.browserAction.getBadgeBackgroundColor(<span>object details</span>, <span>function callback</span>)` |
| [enable](#method-enable) − `whale.browserAction.enable(<span class="optional">integer tabId</span>)` |
| [disable](#method-disable) − `whale.browserAction.disable(<span class="optional">integer tabId</span>)` |
| Events |
| [onClicked](#event-onClicked) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### ColorArray

<dd>array of integer</dd>

</div>

<div>

### ImageDataType

Since Chrome 23.

<dd>Pixel data for an image. Must be an ImageData object (for example, from a `canvas` element).</dd>

</div>

## Methods

<div>

### setTitle

<div class="summary">`whale.browserAction.setTitle(<span>object details</span>)`</div>

<div class="description">

Sets the title of the browser action. This shows up in the tooltip.

| Parameters |
|---|
| object | details | 

| string | title | 
|---|---|

The string the browser action should display when moused over.
 |
| integer | <span class="optional">(optional)</span> tabId | 

Limits the change to when a particular tab is selected. Automatically resets when the tab is closed.
 |
 |

</div>

</div>

<div>

### getTitle

<div class="summary">`whale.browserAction.getTitle(<span>object details</span>, <span>function callback</span>)`</div>

<div class="description">

Gets the title of the browser action.

| Parameters |
|---|
| object | details | 

| integer | <span class="optional">(optional)</span> tabId | 
|---|---|

Specify the tab to get the title from. If no tab is specified, the non-tab-specific title is returned.
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

<div class="summary">`whale.browserAction.setIcon(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Sets the icon for the browser action. The icon can be specified either as the path to an image file or as the pixel data from a canvas element, or as dictionary of either one of those. Either the **path** or the **imageData** property must be specified.

| Parameters |
|---|
| object | details | 

| [ImageDataType](/extensions/browserAction#type-ImageDataType) or object | <span class="optional">(optional)</span> imageData | 
|---|---|

Either an ImageData object or a dictionary {size -> ImageData} representing icon to be set. If the icon is specified as a dictionary, the actual image to be used is chosen depending on screen's pixel density. If the number of image pixels that fit into one screen space unit equals `scale`, then image with size `scale` * n will be selected, where n is the size of the icon in the UI. At least one image must be specified. Note that 'details.imageData = foo' is equivalent to 'details.imageData = {'16': foo}'
 |
| string or object | <span class="optional">(optional)</span> path | 

Either a relative image path or a dictionary {size -> relative image path} pointing to icon to be set. If the icon is specified as a dictionary, the actual image to be used is chosen depending on screen's pixel density. If the number of image pixels that fit into one screen space unit equals `scale`, then image with size `scale` * n will be selected, where n is the size of the icon in the UI. At least one image must be specified. Note that 'details.path = foo' is equivalent to 'details.path = {'16': foo}'
 |
| integer | <span class="optional">(optional)</span> tabId | 

Limits the change to when a particular tab is selected. Automatically resets when the tab is closed.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### setPopup

<div class="summary">`whale.browserAction.setPopup(<span>object details</span>)`</div>

<div class="description">

Sets the html document to be opened as a popup when the user clicks on the browser action's icon.

| Parameters |
|---|
| object | details | 

| integer | <span class="optional">(optional)</span> tabId | 
|---|---|

Limits the change to when a particular tab is selected. Automatically resets when the tab is closed.
 |
| string | popup | 

The html file to show in a popup. If set to the empty string (''), no popup is shown.
 |
 |

</div>

</div>

<div>

### getPopup

<div class="summary">`whale.browserAction.getPopup(<span>object details</span>, <span>function callback</span>)`</div>

<div class="description">

Gets the html document set as the popup for this browser action.

| Parameters |
|---|
| object | details | 

| integer | <span class="optional">(optional)</span> tabId | 
|---|---|

Specify the tab to get the popup from. If no tab is specified, the non-tab-specific popup is returned.
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

### setBadgeText

<div class="summary">`whale.browserAction.setBadgeText(<span>object details</span>)`</div>

<div class="description">

Sets the badge text for the browser action. The badge is displayed on top of the icon.

| Parameters |
|---|
| object | details | 

| string | text | 
|---|---|

Any number of characters can be passed, but only about four can fit in the space.
 |
| integer | <span class="optional">(optional)</span> tabId | 

Limits the change to when a particular tab is selected. Automatically resets when the tab is closed.
 |
 |

</div>

</div>

<div>

### getBadgeText

<div class="summary">`whale.browserAction.getBadgeText(<span>object details</span>, <span>function callback</span>)`</div>

<div class="description">

Gets the badge text of the browser action. If no tab is specified, the non-tab-specific badge text is returned.

| Parameters |
|---|
| object | details | 

| integer | <span class="optional">(optional)</span> tabId | 
|---|---|

Specify the tab to get the badge text from. If no tab is specified, the non-tab-specific badge text is returned.
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

### setBadgeBackgroundColor

<div class="summary">`whale.browserAction.setBadgeBackgroundColor(<span>object details</span>)`</div>

<div class="description">

Sets the background color for the badge.

| Parameters |
|---|
| object | details | 

| string or [ColorArray](/extensions/browserAction#type-ColorArray) | color | 
|---|---|

An array of four integers in the range [0,255] that make up the RGBA color of the badge. For example, opaque red is `[255, 0, 0, 255]`. Can also be a string with a CSS value, with opaque red being `#FF0000` or `#F00`.
 |
| integer | <span class="optional">(optional)</span> tabId | 

Limits the change to when a particular tab is selected. Automatically resets when the tab is closed.
 |
 |

</div>

</div>

<div>

### getBadgeBackgroundColor

<div class="summary">`whale.browserAction.getBadgeBackgroundColor(<span>object details</span>, <span>function callback</span>)`</div>

<div class="description">

Gets the background color of the browser action.

| Parameters |
|---|
| object | details | 

| integer | <span class="optional">(optional)</span> tabId | 
|---|---|

Specify the tab to get the badge background color from. If no tab is specified, the non-tab-specific badge background color is returned.
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [ColorArray](/extensions/browserAction#type-ColorArray) result) <span class="subdued">{...}</span>;`

| [ColorArray](/extensions/browserAction#type-ColorArray) | result |  |
|---|---|---|
 |

</div>

</div>

<div>

### enable

<div class="summary">`whale.browserAction.enable(<span class="optional">integer tabId</span>)`</div>

<div class="description">

Since Chrome 22.

Enables the browser action for a tab. By default, browser actions are enabled.

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> tabId | 

The id of the tab for which you want to modify the browser action.
 |

</div>

</div>

<div>

### disable

<div class="summary">`whale.browserAction.disable(<span class="optional">integer tabId</span>)`</div>

<div class="description">

Since Chrome 22.

Disables the browser action for a tab.

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> tabId | 

The id of the tab for which you want to modify the browser action.
 |

</div>

</div>

## Events

<div>

### onClicked

<div class="description">

Fired when a browser action icon is clicked. This event will not fire if the browser action has a popup.

<div>

#### addListener

<div class="summary">`whale.browserAction.onClicked.addListener(<span>function callback</span>)`</div>

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