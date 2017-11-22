# whale.contextMenus

| Description: | Use the `whale.contextMenus` API to add items to Google Chrome's context menu. You can choose what types of objects your context menu additions apply to, such as images, hyperlinks, and pages. |
|---|---|
| Availability: | Since Chrome 19. |
| Permissions: | <span class="code">"contextMenus"</span> |

<section>

## Usage

Context menu items can appear in any document (or frame within a document), even those with file:// or chrome:// URLs. To control which documents your items can appear in, specify the documentUrlPatterns field when you call the create() or update() method.

You can create as many context menu items as you need, but if more than one from your extension is visible at once, Google Chrome automatically collapses them into a single parent menu.

## Manifest

You must declare the "contextMenus" permission in your extension's manifest to use the API. Also, you should specify a 16x16-pixel icon for display next to your menu item. For example:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        "permissions": [
          **"contextMenus"**
        ],
        "icons": {
          **"16": "icon-bitty.png",**
          "48": "icon-small.png",
          "128": "icon-large.png"
        },
        ...
      }
      </pre>

## Examples

You can find samples of this API on the [sample page](samples#search:contextMenus).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [ContextType](#type-ContextType) |
| [ItemType](#type-ItemType) |
| Properties |
| [ACTION_MENU_TOP_LEVEL_LIMIT](#property-ACTION_MENU_TOP_LEVEL_LIMIT) |
| Methods |
| [create](#method-create) − `integer or string whale.contextMenus.create(<span>object createProperties</span>, <span class="optional">function callback</span>)` |
| [update](#method-update) − `whale.contextMenus.update(<span>integer or string id</span>, <span>object updateProperties</span>, <span class="optional">function callback</span>)` |
| [remove](#method-remove) − `whale.contextMenus.remove(<span>integer or string menuItemId</span>, <span class="optional">function callback</span>)` |
| [removeAll](#method-removeAll) − `whale.contextMenus.removeAll(<span class="optional">function callback</span>)` |
| Events |
| [onClicked](#event-onClicked) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### ContextType

<dd>The different contexts a menu can appear in. Specifying 'all' is equivalent to the combination of all other contexts except for 'launcher'. The 'launcher' context is only supported by apps and is used to add menu items to the context menu that appears when clicking on the app icon in the launcher/taskbar/dock/etc. Different platforms might put limitations on what is actually supported in a launcher context menu.</dd>

| Enum |
|---|
| `"all"`, `"page"`, `"frame"`, `"selection"`, `"link"`, `"editable"`, `"image"`, `"video"`, `"audio"`, `"launcher"`, `"browser_action"`, or `"page_action"` |

</div>

<div>

### ItemType

<dd>The type of menu item.</dd>

| Enum |
|---|
| `"normal"`, `"checkbox"`, `"radio"`, or `"separator"` |

</div>

## Properties

| <span class="type_name">`6`</span> | `whale.contextMenus.ACTION_MENU_TOP_LEVEL_LIMIT` | 
|---|---|

Since Chrome 38.

The maximum number of top level extension items that can be added to an extension action context menu. Any items beyond this limit will be ignored. |

## Methods

<div>

### create

<div class="summary">`integer or string whale.contextMenus.create(<span>object createProperties</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Creates a new context menu item. Note that if an error occurs during creation, you may not find out until the creation callback fires (the details will be in whale.runtime.lastError).

| Parameters |
|---|
| object | createProperties | 

| [ItemType](/extensions/contextMenus#type-ItemType) | <span class="optional">(optional)</span> type | 
|---|---|

The type of menu item. Defaults to 'normal' if not specified.
 |
| string | <span class="optional">(optional)</span> id | 

Since Chrome 21.

The unique ID to assign to this item. Mandatory for event pages. Cannot be the same as another ID for this extension.
 |
| string | <span class="optional">(optional)</span> title | 

The text to be displayed in the item; this is _required_ unless `type` is 'separator'. When the context is 'selection', you can use `%s` within the string to show the selected text. For example, if this parameter's value is "Translate '%s' to Pig Latin" and the user selects the word "cool", the context menu item for the selection is "Translate 'cool' to Pig Latin".
 |
| boolean | <span class="optional">(optional)</span> checked | 

The initial state of a checkbox or radio item: true for selected and false for unselected. Only one radio item can be selected at a time in a given group of radio items.
 |
| array of [ContextType](/extensions/contextMenus#type-ContextType) | <span class="optional">(optional)</span> contexts | 

List of contexts this menu item will appear in. Defaults to ['page'] if not specified.
 |
| boolean | <span class="optional">(optional)</span> visible | 

Since Chrome 63. _Warning:_ this is the current **Dev** channel. [Learn more](api_index#dev_apis).

Whether the item is visible in the menu.
 |
| function | <span class="optional">(optional)</span> onclick | 

A function that will be called back when the menu item is clicked. Event pages cannot use this; instead, they should register a listener for whale.contextMenus.onClicked.

| Parameters |
|---|
| object | info | 

Information about the item clicked and the context where the click happened.

| integer or string | menuItemId | 
|---|---|

Since Chrome 35.

The ID of the menu item that was clicked.
 |
| integer or string | <span class="optional">(optional)</span> parentMenuItemId | 

Since Chrome 35.

The parent ID, if any, for the item clicked.
 |
| string | <span class="optional">(optional)</span> mediaType | 

Since Chrome 35.

One of 'image', 'video', or 'audio' if the context menu was activated on one of these types of elements.
 |
| string | <span class="optional">(optional)</span> linkUrl | 

Since Chrome 35.

If the element is a link, the URL it points to.
 |
| string | <span class="optional">(optional)</span> srcUrl | 

Since Chrome 35.

Will be present for elements with a 'src' URL.
 |
| string | <span class="optional">(optional)</span> pageUrl | 

Since Chrome 35.

The URL of the page where the menu item was clicked. This property is not set if the click occured in a context where there is no current page, such as in a launcher context menu.
 |
| string | <span class="optional">(optional)</span> frameUrl | 

Since Chrome 35.

The URL of the frame of the element where the context menu was clicked, if it was in a frame.
 |
| integer | <span class="optional">(optional)</span> frameId | 

Since Chrome 35.

The [ID of the frame](webNavigation#frame_ids) of the element where the context menu was clicked, if it was in a frame.
 |
| string | <span class="optional">(optional)</span> selectionText | 

Since Chrome 35.

The text for the context selection, if any.
 |
| boolean | editable | 

Since Chrome 35.

A flag indicating whether the element is editable (text input, textarea, etc.).
 |
| boolean | <span class="optional">(optional)</span> wasChecked | 

Since Chrome 35.

A flag indicating the state of a checkbox or radio item before it was clicked.
 |
| boolean | <span class="optional">(optional)</span> checked | 

Since Chrome 35.

A flag indicating the state of a checkbox or radio item after it is clicked.
 |
 |
| [tabs.Tab](/extensions/tabs#type-Tab) | tab | 

The details of the tab where the click took place. Note: this parameter only present for extensions.
 |
 |
| integer or string | <span class="optional">(optional)</span> parentId | 

The ID of a parent menu item; this makes the item a child of a previously added item.
 |
| array of string | <span class="optional">(optional)</span> documentUrlPatterns | 

Lets you restrict the item to apply only to documents whose URL matches one of the given patterns. (This applies to frames as well.) For details on the format of a pattern, see [Match Patterns](match_patterns).
 |
| array of string | <span class="optional">(optional)</span> targetUrlPatterns | 

Similar to documentUrlPatterns, but lets you filter based on the src attribute of img/audio/video tags and the href of anchor tags.
 |
| boolean | <span class="optional">(optional)</span> enabled | 

Since Chrome 20.

Whether this context menu item is enabled or disabled. Defaults to true.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the item has been created in the browser. If there were any problems creating the item, details will be available in whale.runtime.lastError.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### update

<div class="summary">`whale.contextMenus.update(<span>integer or string id</span>, <span>object updateProperties</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Updates a previously created context menu item.

| Parameters |
|---|
| integer or string | id | 

The ID of the item to update.
 |
| object | updateProperties | 

The properties to update. Accepts the same values as the create function.

| [ItemType](/extensions/contextMenus#type-ItemType) | <span class="optional">(optional)</span> type |  |
|---|---|---|
| string | <span class="optional">(optional)</span> title |  |
| boolean | <span class="optional">(optional)</span> checked |  |
| array of [ContextType](/extensions/contextMenus#type-ContextType) | <span class="optional">(optional)</span> contexts |  |
| boolean | <span class="optional">(optional)</span> visible | 

Since Chrome 63. _Warning:_ this is the current **Dev** channel. [Learn more](api_index#dev_apis).

Whether the item is visible in the menu.
 |
| function | <span class="optional">(optional)</span> onclick | 

| Parameters |
|---|
| object | info | 

Since Chrome 44.

Information sent when a context menu item is clicked.

| integer or string | menuItemId | 
|---|---|

The ID of the menu item that was clicked.
 |
| integer or string | <span class="optional">(optional)</span> parentMenuItemId | 

The parent ID, if any, for the item clicked.
 |
| string | <span class="optional">(optional)</span> mediaType | 

One of 'image', 'video', or 'audio' if the context menu was activated on one of these types of elements.
 |
| string | <span class="optional">(optional)</span> linkUrl | 

If the element is a link, the URL it points to.
 |
| string | <span class="optional">(optional)</span> srcUrl | 

Will be present for elements with a 'src' URL.
 |
| string | <span class="optional">(optional)</span> pageUrl | 

The URL of the page where the menu item was clicked. This property is not set if the click occured in a context where there is no current page, such as in a launcher context menu.
 |
| string | <span class="optional">(optional)</span> frameUrl | 

The URL of the frame of the element where the context menu was clicked, if it was in a frame.
 |
| integer | <span class="optional">(optional)</span> frameId | 

The [ID of the frame](webNavigation#frame_ids) of the element where the context menu was clicked, if it was in a frame.
 |
| string | <span class="optional">(optional)</span> selectionText | 

The text for the context selection, if any.
 |
| boolean | editable | 

A flag indicating whether the element is editable (text input, textarea, etc.).
 |
| boolean | <span class="optional">(optional)</span> wasChecked | 

A flag indicating the state of a checkbox or radio item before it was clicked.
 |
| boolean | <span class="optional">(optional)</span> checked | 

A flag indicating the state of a checkbox or radio item after it is clicked.
 |
 |
| [tabs.Tab](/extensions/tabs#type-Tab) | tab | 

Since Chrome 44.

The details of the tab where the click took place. Note: this parameter only present for extensions.
 |
 |
| integer or string | <span class="optional">(optional)</span> parentId | 

Note: You cannot change an item to be a child of one of its own descendants.
 |
| array of string | <span class="optional">(optional)</span> documentUrlPatterns |  |
| array of string | <span class="optional">(optional)</span> targetUrlPatterns |  |
| boolean | <span class="optional">(optional)</span> enabled | 

Since Chrome 20.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the context menu has been updated.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### remove

<div class="summary">`whale.contextMenus.remove(<span>integer or string menuItemId</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Removes a context menu item.

| Parameters |
|---|
| integer or string | menuItemId | 

The ID of the context menu item to remove.
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the context menu has been removed.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### removeAll

<div class="summary">`whale.contextMenus.removeAll(<span class="optional">function callback</span>)`</div>

<div class="description">

Removes all context menu items added by this extension.

| Parameters |
|---|
| function | <span class="optional">(optional)</span> callback | 

Called when removal is complete.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

## Events

<div>

### onClicked

<div class="description">

Since Chrome 21.

Fired when a context menu item is clicked.

<div>

#### addListener

<div class="summary">`whale.contextMenus.onClicked.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object info, [tabs.Tab](/extensions/tabs#type-Tab) tab) <span class="subdued">{...}</span>;`

| object | info | 
|---|---|

Information about the item clicked and the context where the click happened.

| integer or string | menuItemId | 
|---|---|

Since Chrome 35.

The ID of the menu item that was clicked.
 |
| integer or string | <span class="optional">(optional)</span> parentMenuItemId | 

Since Chrome 35.

The parent ID, if any, for the item clicked.
 |
| string | <span class="optional">(optional)</span> mediaType | 

Since Chrome 35.

One of 'image', 'video', or 'audio' if the context menu was activated on one of these types of elements.
 |
| string | <span class="optional">(optional)</span> linkUrl | 

Since Chrome 35.

If the element is a link, the URL it points to.
 |
| string | <span class="optional">(optional)</span> srcUrl | 

Since Chrome 35.

Will be present for elements with a 'src' URL.
 |
| string | <span class="optional">(optional)</span> pageUrl | 

Since Chrome 35.

The URL of the page where the menu item was clicked. This property is not set if the click occured in a context where there is no current page, such as in a launcher context menu.
 |
| string | <span class="optional">(optional)</span> frameUrl | 

Since Chrome 35.

The URL of the frame of the element where the context menu was clicked, if it was in a frame.
 |
| integer | <span class="optional">(optional)</span> frameId | 

Since Chrome 35.

The [ID of the frame](webNavigation#frame_ids) of the element where the context menu was clicked, if it was in a frame.
 |
| string | <span class="optional">(optional)</span> selectionText | 

Since Chrome 35.

The text for the context selection, if any.
 |
| boolean | editable | 

Since Chrome 35.

A flag indicating whether the element is editable (text input, textarea, etc.).
 |
| boolean | <span class="optional">(optional)</span> wasChecked | 

Since Chrome 35.

A flag indicating the state of a checkbox or radio item before it was clicked.
 |
| boolean | <span class="optional">(optional)</span> checked | 

Since Chrome 35.

A flag indicating the state of a checkbox or radio item after it is clicked.
 |
 |
| [tabs.Tab](/extensions/tabs#type-Tab) | <span class="optional">(optional)</span> tab | 

The details of the tab where the click took place. If the click did not take place in a tab, this parameter will be missing.
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>