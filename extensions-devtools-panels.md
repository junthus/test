# whale.devtools.panels

| Description: | Use the `whale.devtools.panels` API to integrate your extension into Developer Tools window UI: create your own panels, access existing panels, and add sidebars. |
|---|---|
| Availability: | Since Chrome 21. |

<section>

See [DevTools APIs summary](devtools) for general introduction to using Developer Tools APIs.

## Overview

Each extension panel and sidebar is displayed as a separate HTML page. All extension pages displayed in the Developer Tools window have access to all modules in `whale.devtools` API, as well as to [whale.extension](extension) API. Other extension APIs are not available to the pages within Developer Tools window, but you may invoke them by sending a request to the background page of your extension, similarly to how it's done in the [content scripts](overview#contentScripts).

You can use the `[devtools.panels.setOpenResourceHandler](/extensions/devtools.panels#method-setOpenResourceHandler)` method to install a callback function that handles user requests to open a resource (typically, a click on a resource link in the Developer Tools window). At most one of the installed handlers gets called; users can specify (using the Developer Tools Settings dialog) either the default behavior or an extension to handle resource open requests. If an extension calls `setOpenResourceHandler()` multiple times, only the last handler is retained.

## Examples

The following code adds a panel contained in `Panel.html`, represented by `FontPicker.png` on the Developer Tools toolbar and labeled as _Font Picker_:

<pre>      whale.devtools.panels.create("Font Picker",
                                    "FontPicker.png",
                                    "Panel.html"
                                    function(panel) { ... });
      </pre>

The following code adds a sidebar pane contained in `Sidebar.html` and titled _Font Properties_ to the Elements panel, then sets its height to `8ex`:

<pre>      whale.devtools.panels.elements.createSidebarPane("Font Properties",
          function(sidebar) {
            sidebar.setPage("Sidebar.html");
            sidebar.setHeight("8ex");
          });
      </pre>

This screenshot demonstrates the effect the above examples would have on Developer Tools window: ![Extension icon panel on DevTools toolbar](/static/images/devtools-panels.png)

You can find examples that use this API in [Samples](samples#chrome-query).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [ElementsPanel](#type-ElementsPanel) |
| [SourcesPanel](#type-SourcesPanel) |
| [ExtensionPanel](#type-ExtensionPanel) |
| [ExtensionSidebarPane](#type-ExtensionSidebarPane) |
| [Button](#type-Button) |
| Properties |
| [elements](#property-elements) |
| [sources](#property-sources) |
| [themeName](#property-themeName) |
| Methods |
| [create](#method-create) − `whale.devtools.panels.create(<span>string title</span>, <span>string iconPath</span>, <span>string pagePath</span>, <span class="optional">function callback</span>)` |
| [setOpenResourceHandler](#method-setOpenResourceHandler) − `whale.devtools.panels.setOpenResourceHandler(<span class="optional">function callback</span>)` |
| [openResource](#method-openResource) − `whale.devtools.panels.openResource(<span>string url</span>, <span>integer lineNumber</span>, <span class="optional">function callback</span>)` |

</section>

<section>

<div class="api-reference">

## Types

<div>

### ElementsPanel

<dd>Represents the Elements panel.</dd>

<div>

### onSelectionChanged

<div class="description">

Fired when an object is selected in the panel.

</div>

</div>

| methods |
|---|
| 

<div>

#### createSidebarPane

<div class="summary">`ElementsPanel.createSidebarPane(<span>string title</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Creates a pane within panel's sidebar.

| Parameters |
|---|
| string | title | 

Text that is displayed in sidebar caption.
 |
| function | <span class="optional">(optional)</span> callback | 

A callback invoked when the sidebar is created.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [ExtensionSidebarPane](/extensions/devtools.panels#type-ExtensionSidebarPane) result) <span class="subdued">{...}</span>;`

| [ExtensionSidebarPane](/extensions/devtools.panels#type-ExtensionSidebarPane) | result | 
|---|---|

An ExtensionSidebarPane object for created sidebar pane.
 |
 |

</div>

</div>
 |
| events |
| 

<div>

#### addListener

<div class="summary">`onSelectionChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |

</div>

<div>

### SourcesPanel

Since Chrome 41.

<dd>Represents the Sources panel.</dd>

<div>

### onSelectionChanged

<div class="description">

Fired when an object is selected in the panel.

</div>

</div>

| methods |
|---|
| 

<div>

#### createSidebarPane

<div class="summary">`SourcesPanel.createSidebarPane(<span>string title</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Creates a pane within panel's sidebar.

| Parameters |
|---|
| string | title | 

Text that is displayed in sidebar caption.
 |
| function | <span class="optional">(optional)</span> callback | 

A callback invoked when the sidebar is created.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [ExtensionSidebarPane](/extensions/devtools.panels#type-ExtensionSidebarPane) result) <span class="subdued">{...}</span>;`

| [ExtensionSidebarPane](/extensions/devtools.panels#type-ExtensionSidebarPane) | result | 
|---|---|

An ExtensionSidebarPane object for created sidebar pane.
 |
 |

</div>

</div>
 |
| events |
| 

<div>

#### addListener

<div class="summary">`onSelectionChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |

</div>

<div>

### ExtensionPanel

<dd>Represents a panel created by extension.</dd>

<div>

### onSearch

<div class="description">

Fired upon a search action (start of a new search, search result navigation, or search being canceled).

</div>

</div>

<div>

### onShown

<div class="description">

Fired when the user switches to the panel.

</div>

</div>

<div>

### onHidden

<div class="description">

Fired when the user switches away from the panel.

</div>

</div>

| methods |
|---|
| 

<div>

#### createStatusBarButton

<div class="summary">`[Button](/extensions/devtools.panels#type-Button) ExtensionPanel.createStatusBarButton(<span>string iconPath</span>, <span>string tooltipText</span>, <span>boolean disabled</span>)`</div>

<div class="description">

Appends a button to the status bar of the panel.

| Parameters |
|---|
| string | iconPath | 

Path to the icon of the button. The file should contain a 64x24-pixel image composed of two 32x24 icons. The left icon is used when the button is inactive; the right icon is displayed when the button is pressed.
 |
| string | tooltipText | 

Text shown as a tooltip when user hovers the mouse over the button.
 |
| boolean | disabled | 

Whether the button is disabled.
 |

</div>

</div>
 |
| events |
| 

<div>

#### addListener

<div class="summary">`onSearch.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string action, string queryString) <span class="subdued">{...}</span>;`

| string | action | 
|---|---|

Type of search action being performed.
 |
| string | <span class="optional">(optional)</span> queryString | 

Query string (only for 'performSearch').
 |
 |

</div>

</div>
 |
| 

<div>

#### addListener

<div class="summary">`onShown.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(global window) <span class="subdued">{...}</span>;`

| global | window | 
|---|---|

The JavaScript `window` object of panel's page.
 |
 |

</div>

</div>
 |
| 

<div>

#### addListener

<div class="summary">`onHidden.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |

</div>

<div>

### ExtensionSidebarPane

<dd>A sidebar created by the extension.</dd>

<div>

### onShown

<div class="description">

Fired when the sidebar pane becomes visible as a result of user switching to the panel that hosts it.

</div>

</div>

<div>

### onHidden

<div class="description">

Fired when the sidebar pane becomes hidden as a result of the user switching away from the panel that hosts the sidebar pane.

</div>

</div>

| methods |
|---|
| 

<div>

#### setHeight

<div class="summary">`ExtensionSidebarPane.setHeight(<span>string height</span>)`</div>

<div class="description">

Sets the height of the sidebar.

| Parameters |
|---|
| string | height | 

A CSS-like size specification, such as `'100px'` or `'12ex'`.
 |

</div>

</div>
 |
| 

<div>

#### setExpression

<div class="summary">`ExtensionSidebarPane.setExpression(<span>string expression</span>, <span class="optional">string rootTitle</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Sets an expression that is evaluated within the inspected page. The result is displayed in the sidebar pane.

| Parameters |
|---|
| string | expression | 

An expression to be evaluated in context of the inspected page. JavaScript objects and DOM nodes are displayed in an expandable tree similar to the console/watch.
 |
| string | <span class="optional">(optional)</span> rootTitle | 

An optional title for the root of the expression tree.
 |
| function | <span class="optional">(optional)</span> callback | 

A callback invoked after the sidebar pane is updated with the expression evaluation results.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |
| 

<div>

#### setObject

<div class="summary">`ExtensionSidebarPane.setObject(<span>string jsonObject</span>, <span class="optional">string rootTitle</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Sets a JSON-compliant object to be displayed in the sidebar pane.

| Parameters |
|---|
| string | jsonObject | 

An object to be displayed in context of the inspected page. Evaluated in the context of the caller (API client).
 |
| string | <span class="optional">(optional)</span> rootTitle | 

An optional title for the root of the expression tree.
 |
| function | <span class="optional">(optional)</span> callback | 

A callback invoked after the sidebar is updated with the object.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |
| 

<div>

#### setPage

<div class="summary">`ExtensionSidebarPane.setPage(<span>string path</span>)`</div>

<div class="description">

Sets an HTML page to be displayed in the sidebar pane.

| Parameters |
|---|
| string | path | 

Relative path of an extension page to display within the sidebar.
 |

</div>

</div>
 |
| events |
| 

<div>

#### addListener

<div class="summary">`onShown.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(global window) <span class="subdued">{...}</span>;`

| global | window | 
|---|---|

The JavaScript `window` object of the sidebar page, if one was set with the `setPage()` method.
 |
 |

</div>

</div>
 |
| 

<div>

#### addListener

<div class="summary">`onHidden.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |

</div>

<div>

### Button

<dd>A button created by the extension.</dd>

<div>

### onClicked

<div class="description">

Fired when the button is clicked.

</div>

</div>

| methods |
|---|
| 

<div>

#### update

<div class="summary">`Button.update(<span class="optional">string iconPath</span>, <span class="optional">string tooltipText</span>, <span class="optional">boolean disabled</span>)`</div>

<div class="description">

Updates the attributes of the button. If some of the arguments are omitted or `null`, the corresponding attributes are not updated.

| Parameters |
|---|
| string | <span class="optional">(optional)</span> iconPath | 

Path to the new icon of the button.
 |
| string | <span class="optional">(optional)</span> tooltipText | 

Text shown as a tooltip when user hovers the mouse over the button.
 |
| boolean | <span class="optional">(optional)</span> disabled | 

Whether the button is disabled.
 |

</div>

</div>
 |
| events |
| 

<div>

#### addListener

<div class="summary">`onClicked.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |

</div>

## Properties

| <span class="type_name">[ElementsPanel](/extensions/devtools.panels#type-ElementsPanel)</span> | `whale.devtools.panels.elements` | Elements panel. |
|---|---|---|
| <span class="type_name">[SourcesPanel](/extensions/devtools.panels#type-SourcesPanel)</span> | `whale.devtools.panels.sources` | 

Since Chrome 38.

Sources panel. |
| <span class="type_name">string</span> | `whale.devtools.panels.themeName` | 

Since Chrome 59.

The name of the color theme set in user's DevTools settings. Possible values: `default` (the default) and `dark`. |

## Methods

<div>

### create

<div class="summary">`whale.devtools.panels.create(<span>string title</span>, <span>string iconPath</span>, <span>string pagePath</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Creates an extension panel.

| Parameters |
|---|
| string | title | 

Title that is displayed next to the extension icon in the Developer Tools toolbar.
 |
| string | iconPath | 

Path of the panel's icon relative to the extension directory.
 |
| string | pagePath | 

Path of the panel's HTML page relative to the extension directory.
 |
| function | <span class="optional">(optional)</span> callback | 

A function that is called when the panel is created.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [ExtensionPanel](/extensions/devtools.panels#type-ExtensionPanel) panel) <span class="subdued">{...}</span>;`

| [ExtensionPanel](/extensions/devtools.panels#type-ExtensionPanel) | panel | 
|---|---|

An ExtensionPanel object representing the created panel.
 |
 |

</div>

</div>

<div>

### setOpenResourceHandler

<div class="summary">`whale.devtools.panels.setOpenResourceHandler(<span class="optional">function callback</span>)`</div>

<div class="description">

Specifies the function to be called when the user clicks a resource link in the Developer Tools window. To unset the handler, either call the method with no parameters or pass null as the parameter.

| Parameters |
|---|
| function | <span class="optional">(optional)</span> callback | 

A function that is called when the user clicks on a valid resource link in Developer Tools window. Note that if the user clicks an invalid URL or an XHR, this function is not called.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [devtools.inspectedWindow.Resource](/extensions/devtools.inspectedWindow#type-Resource) resource) <span class="subdued">{...}</span>;`

| [devtools.inspectedWindow.Resource](/extensions/devtools.inspectedWindow#type-Resource) | resource | 
|---|---|

A [devtools.inspectedWindow.Resource](/extensions/devtools.inspectedWindow#type-Resource) object for the resource that was clicked.
 |
 |

</div>

</div>

<div>

### openResource

<div class="summary">`whale.devtools.panels.openResource(<span>string url</span>, <span>integer lineNumber</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 38.

Requests DevTools to open a URL in a Developer Tools panel.

| Parameters |
|---|
| string | url | 

The URL of the resource to open.
 |
| integer | lineNumber | 

Specifies the line number to scroll to when the resource is loaded.
 |
| function | <span class="optional">(optional)</span> callback | 

A function that is called when the resource has been successfully loaded.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

</div>

</section>