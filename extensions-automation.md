# whale.automation

| Description: | The `whale.automation` API allows developers to access the automation (accessibility) tree for the browser. The tree resembles the DOM tree, but only exposes the _semantic_ structure of a page. It can be used to programmatically interact with a page by examining names, roles, and states, listening for events, and performing actions on nodes. |
|---|---|
| Availability: | **Dev** channel only. [Learn more](api_index#dev_apis). |
| Manifest: | <span class="code">"automation": {...}</span> |

<section id="toc">

## Summary

| Types |
|---|
| [EventType](#type-EventType) |
| [RoleType](#type-RoleType) |
| [StateType](#type-StateType) |
| [Restriction](#type-Restriction) |
| [Rect](#type-Rect) |
| [FindParams](#type-FindParams) |
| [SetDocumentSelectionParams](#type-SetDocumentSelectionParams) |
| [AutomationEvent](#type-AutomationEvent) |
| [TreeChange](#type-TreeChange) |
| [AutomationNode](#type-AutomationNode) |
| Methods |
| [getTree](#method-getTree) − `whale.automation.getTree(<span class="optional">integer tabId</span>, <span class="optional">function callback</span>)` |
| [getDesktop](#method-getDesktop) − `whale.automation.getDesktop(<span>function callback</span>)` |
| [getFocus](#method-getFocus) − `whale.automation.getFocus(<span>function callback</span>)` |
| [addTreeChangeObserver](#method-addTreeChangeObserver) − `whale.automation.addTreeChangeObserver(<span>enum of `"noTreeChanges"`, `"liveRegionTreeChanges"`, `"textMarkerChanges"`, or `"allTreeChanges"` filter</span>, <span>function observer</span>)` |
| [removeTreeChangeObserver](#method-removeTreeChangeObserver) − `whale.automation.removeTreeChangeObserver(<span>function observer</span>)` |
| [setDocumentSelection](#method-setDocumentSelection) − `whale.automation.setDocumentSelection( <span>SetDocumentSelectionParams params</span>)` |

</section>

<section>

<div class="api-reference">

## Types

<div>

### EventType

<dd>Possible events fired on an [automation.AutomationNode](/extensions/automation#type-AutomationNode).</dd>

| Enum |
|---|
| `"activedescendantchanged"`, `"alert"`, `"ariaAttributeChanged"`, `"autocorrectionOccured"`, `"blur"`, `"checkedStateChanged"`, `"childrenChanged"`, `"clicked"`, `"documentSelectionChanged"`, `"expandedChanged"`, `"focus"`, `"imageFrameUpdated"`, `"hide"`, `"hover"`, `"invalidStatusChanged"`, `"layoutComplete"`, `"liveRegionCreated"`, `"liveRegionChanged"`, `"loadComplete"`, `"locationChanged"`, `"mediaStartedPlaying"`, `"mediaStoppedPlaying"`, `"menuEnd"`, `"menuListItemSelected"`, `"menuListValueChanged"`, `"menuPopupEnd"`, `"menuPopupStart"`, `"menuStart"`, `"mouseCanceled"`, `"mouseDragged"`, `"mouseMoved"`, `"mousePressed"`, `"mouseReleased"`, `"rowCollapsed"`, `"rowCountChanged"`, `"rowExpanded"`, `"scrollPositionChanged"`, `"scrolledToAnchor"`, `"selectedChildrenChanged"`, `"selection"`, `"selectionAdd"`, `"selectionRemove"`, `"show"`, `"textChanged"`, `"textSelectionChanged"`, `"treeChanged"`, or `"valueChanged"` |

</div>

<div>

### RoleType

<dd>Describes the purpose of an [automation.AutomationNode](/extensions/automation#type-AutomationNode).</dd>

| Enum |
|---|
| `"abbr"`, `"alertDialog"`, `"alert"`, `"anchor"`, `"annotation"`, `"application"`, `"article"`, `"audio"`, `"banner"`, `"blockquote"`, `"button"`, `"buttonDropDown"`, `"canvas"`, `"caption"`, `"caret"`, `"cell"`, `"checkBox"`, `"client"`, `"colorWell"`, `"columnHeader"`, `"column"`, `"comboBox"`, `"complementary"`, `"contentInfo"`, `"date"`, `"dateTime"`, `"definition"`, `"descriptionListDetail"`, `"descriptionList"`, `"descriptionListTerm"`, `"desktop"`, `"details"`, `"dialog"`, `"directory"`, `"disclosureTriangle"`, `"document"`, `"embeddedObject"`, `"feed"`, `"figcaption"`, `"figure"`, `"footer"`, `"form"`, `"genericContainer"`, `"grid"`, `"group"`, `"heading"`, `"iframe"`, `"iframePresentational"`, `"ignored"`, `"imageMap"`, `"image"`, `"inlineTextBox"`, `"inputTime"`, `"labelText"`, `"legend"`, `"lineBreak"`, `"link"`, `"listBoxOption"`, `"listBox"`, `"listItem"`, `"listMarker"`, `"list"`, `"locationBar"`, `"log"`, `"main"`, `"mark"`, `"marquee"`, `"math"`, `"menuBar"`, `"menuButton"`, `"menuItem"`, `"menuItemCheckBox"`, `"menuItemRadio"`, `"menuListOption"`, `"menuListPopup"`, `"menu"`, `"meter"`, `"navigation"`, `"note"`, `"pane"`, `"paragraph"`, `"popUpButton"`, `"pre"`, `"presentational"`, `"progressIndicator"`, `"radioButton"`, `"radioGroup"`, `"region"`, `"rootWebArea"`, `"rowHeader"`, `"row"`, `"ruby"`, `"svgRoot"`, `"scrollBar"`, `"search"`, `"searchBox"`, `"slider"`, `"sliderThumb"`, `"spinButtonPart"`, `"spinButton"`, `"splitter"`, `"staticText"`, `"status"`, `"switch"`, `"tabList"`, `"tabPanel"`, `"tab"`, `"tableHeaderContainer"`, `"table"`, `"term"`, `"textField"`, `"time"`, `"timer"`, `"titleBar"`, `"toggleButton"`, `"toolbar"`, `"treeGrid"`, `"treeItem"`, `"tree"`, `"unknown"`, `"tooltip"`, `"video"`, `"webArea"`, `"webView"`, or `"window"` |

</div>

<div>

### StateType

<dd>Describes characteristics of an [automation.AutomationNode](/extensions/automation#type-AutomationNode).</dd>

| Enum |
|---|
| `"collapsed"`, `"default"`, `"editable"`, `"expanded"`, `"focusable"`, `"focused"`, `"haspopup"`, `"horizontal"`, `"hovered"`, `"invisible"`, `"linked"`, `"multiline"`, `"multiselectable"`, `"offscreen"`, `"protected"`, `"required"`, `"richlyEditable"`, `"selectable"`, `"selected"`, `"vertical"`, or `"visited"` |

</div>

<div>

### Restriction

<dd>The input restriction for a object -- even non-controls can be disabled.</dd>

| Enum |
|---|
| `"disabled"`, or `"readOnly"` |

</div>

<div>

### Rect

| properties |
|---|
| integer | left |  |
| integer | top |  |
| integer | width |  |
| integer | height |  |

</div>

<div>

### FindParams

| properties |
|---|
| [RoleType](/extensions/automation#type-RoleType) | <span class="optional">(optional)</span> role |  |
| object | <span class="optional">(optional)</span> state | 

A map of [automation.StateType](/extensions/automation#type-StateType) to boolean, indicating for each state whether it should be set or not. For example: `{ StateType.disabled: false }` would only match if `StateType.disabled` was _not_ present in the node's `state` object.
 |
| object | <span class="optional">(optional)</span> attributes | 

A map of attribute name to expected value, for example `{ name: 'Root directory', checkbox_mixed: true }`. String attribute values may be specified as a regex, for example `{ name: /stralia$/` }. Unless specifying a regex, the expected value must be an exact match in type and value for the actual value. Thus, the type of expected value must be one of:

*   string
*   integer
*   float
*   boolean
 |

</div>

<div>

### SetDocumentSelectionParams

| properties |
|---|
| AutomationNode | anchorObject | 

The node where the selection begins.
 |
| integer | anchorOffset | 

The offset in the anchor node where the selection begins.
 |
| AutomationNode | focusObject | 

The node where the selection ends.
 |
| integer | focusOffset | 

The offset within the focus node where the selection ends.
 |

</div>

<div>

### AutomationEvent

| properties |
|---|
| [AutomationNode](/extensions/automation#type-AutomationNode) | target | 

The [automation.AutomationNode](/extensions/automation#type-AutomationNode) to which the event was targeted.
 |
| [EventType](/extensions/automation#type-EventType) | type | 

The type of the event.
 |
| string | eventFrom | 

The source of this event.
 |
| integer | mouseX |  |
| integer | mouseY |  |
| function | stopPropagation | 

Stops this event from further processing except for any remaining listeners on [AutomationEvent.target](/extensions/automation#property-AutomationEvent-target).
 |

</div>

<div>

### TreeChange

| properties |
|---|
| [AutomationNode](/extensions/automation#type-AutomationNode) | target | 

The [automation.AutomationNode](/extensions/automation#type-AutomationNode) that changed.
 |
| enum of `"nodeCreated"`, `"subtreeCreated"`, `"nodeChanged"`, `"textChanged"`, or `"nodeRemoved"` | type | 

The type of change.

<dl>

<dt>nodeCreated</dt>

<dd>* This node was added to the tree and its parent is new as well, so it's just one node in a new subtree that was added.</dd>

</dl>

<dl>

<dt>subtreeCreated</dt>

<dd>* This node was added to the tree but its parent was already in the tree, so it's possibly the root of a new subtree - it does not mean that it necessarily has children.</dd>

</dl>

<dl>

<dt>nodeChanged</dt>

<dd>* This node changed.</dd>

</dl>

<dl>

<dt>textChanged</dt>

<dd>* This node's text (name) changed.</dd>

</dl>

<dl>

<dt>nodeRemoved</dt>

<dd>* This node was removed.</dd>

</dl>
 |

</div>

<div>

### AutomationNode

| properties |
|---|
| [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> root | 

The root node of the tree containing this AutomationNode.
 |
| boolean | isRootNode | 

Whether this AutomationNode is a root node.
 |
| [RoleType](/extensions/automation#type-RoleType) | <span class="optional">(optional)</span> role | 

The role of this node.
 |
| object | <span class="optional">(optional)</span> state | 

The [automation.StateType](/extensions/automation#type-StateType)s describing this node.
 |
| [Rect](/extensions/automation#type-Rect) | <span class="optional">(optional)</span> location | 

The rendered location (as a bounding box) of this node in global screen coordinates.
 |
| function | boundsForRange | 

Computes the bounding box of a subrange of this node in global screen coordinates. Returns the same as |location| if range information is not available. The start and end indices are zero-based offsets into the node's "name" string attribute.

Returns <span class="property">[Rect](/extensions/automation#type-Rect).</span>

| Parameters |
|---|
| integer | startIndex |  |
| integer | endIndex |  |
 |
| string | <span class="optional">(optional)</span> description | 

The purpose of the node, other than the role, if any.
 |
| string | <span class="optional">(optional)</span> placeholder | 

The placeholder for this text field, if any.
 |
| string | <span class="optional">(optional)</span> roleDescription | 

The role description for this node.
 |
| string | <span class="optional">(optional)</span> name | 

The accessible name for this node, via the [Accessible Name Calculation](http://www.w3.org/TR/wai-aria/roles#namecalculation) process.
 |
| enum of `"uninitialized"`, `"attribute"`, `"attributeExplicitlyEmpty"`, `"contents"`, `"placeholder"`, `"relatedElement"`, or `"value"` | <span class="optional">(optional)</span> nameFrom | 

The source of the name.
 |
| string | <span class="optional">(optional)</span> value | 

The value for this node: for example the `value` attribute of an `<input> element.`
 |
| string | <span class="optional">(optional)</span> htmlTag | 

The HTML tag for this element, if this node is an HTML element.
 |
| integer | <span class="optional">(optional)</span> hierarchicalLevel | 

The level of a heading or tree item.
 |
| array of integer | <span class="optional">(optional)</span> wordStarts | 

The start and end index of each word in an inline text box.
 |
| array of integer | <span class="optional">(optional)</span> wordEnds |  |
| array of [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> controls | 

The nodes, if any, which this node is specified to control via [`aria-controls`](http://www.w3.org/TR/wai-aria/states_and_properties#aria-controls).
 |
| array of [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> describedBy | 

The nodes, if any, which form a description for this node.
 |
| array of [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> flowTo | 

The nodes, if any, which may optionally be navigated to after this one. See [`aria-flowto`](http://www.w3.org/TR/wai-aria/states_and_properties#aria-flowto).
 |
| array of [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> labelledBy | 

The nodes, if any, which form a label for this element. Generally, the text from these elements will also be exposed as the element's accessible name, via the [automation.AutomationNode.name](/extensions/automation#property-AutomationNode-name) attribute.
 |
| [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> activeDescendant | 

The node referred to by `aria-activedescendant`, where applicable
 |
| [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> inPageLinkTarget | 

The target of an in-page link.
 |
| array of object | <span class="optional">(optional)</span> customActions | 

An array of custom actions.

#### Properties of each object

<dl>

<div>

| integer | id |  |
|---|---|---|
| string | description |  |

</div>

</dl>
 |
| string | <span class="optional">(optional)</span> url | 

The URL that this link will navigate to.
 |
| string | <span class="optional">(optional)</span> docUrl | 

The URL of this document.
 |
| string | <span class="optional">(optional)</span> docTitle | 

The title of this document.
 |
| boolean | <span class="optional">(optional)</span> docLoaded | 

Whether this document has finished loading.
 |
| double | <span class="optional">(optional)</span> docLoadingProgress | 

The proportion (out of 1.0) that this doc has completed loading.
 |
| integer | <span class="optional">(optional)</span> scrollX | 

Scrollable container attributes.
 |
| integer | <span class="optional">(optional)</span> scrollXMin |  |
| integer | <span class="optional">(optional)</span> scrollXMax |  |
| integer | <span class="optional">(optional)</span> scrollY |  |
| integer | <span class="optional">(optional)</span> scrollYMin |  |
| integer | <span class="optional">(optional)</span> scrollYMax |  |
| integer | <span class="optional">(optional)</span> textSelStart | 

The character index of the start of the selection within this editable text element; -1 if no selection.
 |
| integer | <span class="optional">(optional)</span> textSelEnd | 

The character index of the end of the selection within this editable text element; -1 if no selection.
 |
| string | <span class="optional">(optional)</span> textInputType | 

The input type, like email or number.
 |
| array of integer | lineBreaks | 

An array of indexes of the break between lines in editable text.
 |
| array of integer | markerStarts | 

An array of indexes of the start position of each text marker.
 |
| array of integer | markerEnds | 

An array of indexes of the end position of each text marker.
 |
| array of integer | markerTypes | 

An array of numerical types indicating the type of each text marker, such as a spelling error.
 |
| [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> anchorObject | 

The anchor node of the tree selection, if any.
 |
| integer | <span class="optional">(optional)</span> anchorOffset | 

The anchor offset of the tree selection, if any.
 |
| string | <span class="optional">(optional)</span> anchorAffinity | 

The affinity of the tree selection anchor, if any.
 |
| [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> focusObject | 

The focus node of the tree selection, if any.
 |
| integer | <span class="optional">(optional)</span> focusOffset | 

The focus offset of the tree selection, if any.
 |
| string | <span class="optional">(optional)</span> focusAffinity | 

The affinity of the tree selection focus, if any.
 |
| double | <span class="optional">(optional)</span> valueForRange | 

The current value for this range.
 |
| double | <span class="optional">(optional)</span> minValueForRange | 

The minimum possible value for this range.
 |
| double | <span class="optional">(optional)</span> maxValueForRange | 

The maximum possible value for this range.
 |
| integer | <span class="optional">(optional)</span> posInSet | 

The 1-based index of an item in a set.
 |
| integer | <span class="optional">(optional)</span> setSize | 

The number of items in a set;
 |
| integer | <span class="optional">(optional)</span> tableRowCount | 

The number of rows in this table as specified in the DOM.
 |
| integer | <span class="optional">(optional)</span> ariaRowCount | 

The number of rows in this table as specified by the page author.
 |
| integer | <span class="optional">(optional)</span> tableColumnCount | 

The number of columns in this table as specified in the DOM.
 |
| integer | <span class="optional">(optional)</span> ariaColumnCount | 

The number of columns in this table as specified by the page author.
 |
| integer | <span class="optional">(optional)</span> tableCellColumnIndex | 

The zero-based index of the column that this cell is in as specified in the DOM.
 |
| integer | <span class="optional">(optional)</span> ariaCellColumnIndex | 

The ARIA column index as specified by the page author.
 |
| integer | <span class="optional">(optional)</span> tableCellColumnSpan | 

The number of columns that this cell spans (default is 1).
 |
| integer | <span class="optional">(optional)</span> tableCellRowIndex | 

The zero-based index of the row that this cell is in as specified in the DOM.
 |
| integer | <span class="optional">(optional)</span> ariaCellRowIndex | 

The ARIA row index as specified by the page author.
 |
| integer | <span class="optional">(optional)</span> tableCellRowSpan | 

The number of rows that this cell spans (default is 1).
 |
| [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> tableColumnHeader | 

The corresponding column header for this cell.
 |
| [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> tableRowHeader | 

The corresponding row header for this cell.
 |
| string | <span class="optional">(optional)</span> liveStatus | 

The type of region if this is the root of a live region. Possible values are 'polite' and 'assertive'.
 |
| string | <span class="optional">(optional)</span> liveRelevant | 

The value of aria-relevant for a live region.
 |
| boolean | <span class="optional">(optional)</span> liveAtomic | 

The value of aria-atomic for a live region.
 |
| boolean | <span class="optional">(optional)</span> busy | 

The value of aria-busy for a live region or any other element.
 |
| string | <span class="optional">(optional)</span> containerLiveStatus | 

The type of live region if this node is inside a live region.
 |
| string | <span class="optional">(optional)</span> containerLiveRelevant | 

The value of aria-relevant if this node is inside a live region.
 |
| boolean | <span class="optional">(optional)</span> containerLiveAtomic | 

The value of aria-atomic if this node is inside a live region.
 |
| boolean | <span class="optional">(optional)</span> containerLiveBusy | 

The value of aria-busy if this node is inside a live region.
 |
| object | <span class="optional">(optional)</span> htmlAttributes | 

A map containing all HTML attributes and their values
 |
| string | <span class="optional">(optional)</span> inputType | 

The input type of a text field, such as "text" or "email".
 |
| string | <span class="optional">(optional)</span> accessKey | 

The key that activates this widget.
 |
| string | <span class="optional">(optional)</span> ariaInvalidValue | 

The value of the aria-invalid attribute, indicating the error type.
 |
| string | <span class="optional">(optional)</span> display | 

The CSS display attribute for this node, if applicable.
 |
| string | <span class="optional">(optional)</span> imageDataUrl | 

A data url with the contents of this object's image or thumbnail.
 |
| string | <span class="optional">(optional)</span> language | 

The language code for this subtree.
 |
| string | <span class="optional">(optional)</span> restriction | 

Input restriction, if any, such as readonly or disabled: undefined - enabled control or other object that is not disabled Restriction.DISABLED - disallows input in itself + any descendants Restriction.READONLY - allow focus/selection but not input
 |
| string | <span class="optional">(optional)</span> checked | 

Tri-state describing checkbox or radio button: 'false' | 'true' | 'mixed'
 |
| integer | <span class="optional">(optional)</span> color | 

The RGBA foreground color of this subtree, as an integer.
 |
| integer | <span class="optional">(optional)</span> backgroundColor | 

The RGBA background color of this subtree, as an integer.
 |
| integer | <span class="optional">(optional)</span> colorValue | 

The RGBA color of an input element whose value is a color.
 |
| boolean | bold | 

Indicates node text is bold.
 |
| boolean | italic | 

Indicates node text is italic.
 |
| boolean | underline | 

Indicates node text is underline.
 |
| boolean | lineThrough | 

Indicates node text is line through.
 |
| array of [AutomationNode](/extensions/automation#type-AutomationNode) | children | 

Walking the tree.
 |
| [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> parent |  |
| [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> firstChild |  |
| [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> lastChild |  |
| [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> previousSibling |  |
| [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> nextSibling |  |
| [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> nextOnLine |  |
| [AutomationNode](/extensions/automation#type-AutomationNode) | <span class="optional">(optional)</span> previousOnLine |  |
| integer | <span class="optional">(optional)</span> indexInParent | 

The index of this node in its parent node's list of children. If this is the root node, this will be undefined.
 |
| function | doDefault | 

Does the default action based on this node's role. This is generally the same action that would result from clicking the node such as expanding a treeitem, toggling a checkbox, selecting a radiobutton, or activating a button.
 |
| function | focus | 

Places focus on this node.
 |
| function | getImageData | 

Request a data url for the contents of an image, optionally resized. Pass zero for maxWidth and/or maxHeight for the original size.

| Parameters |
|---|
| integer | maxWidth |  |
| integer | maxHeight |  |
 |
| function | hitTest | 

Does a hit test of the given global screen coordinates, and fires eventToFire on the resulting object.

| Parameters |
|---|
| integer | x |  |
| integer | y |  |
| [EventType](/extensions/automation#type-EventType) | eventToFire |  |
 |
| function | makeVisible | 

Scrolls this node to make it visible.
 |
| function | performCustomAction | 

Performs custom action.

| Parameters |
|---|
| integer | customActionId |  |
 |
| function | setSelection | 

Sets selection within a text field.

| Parameters |
|---|
| integer | startIndex |  |
| integer | endIndex |  |
 |
| function | setSequentialFocusNavigationStartingPoint | 

Clears focus and sets this node as the starting point for the next time the user presses Tab or Shift+Tab.
 |
| function | showContextMenu | 

Show the context menu for this element, as if the user right-clicked.
 |
| function | resumeMedia | 

Resume playing any media within this tree.
 |
| function | startDuckingMedia | 

Start ducking any media within this tree.
 |
| function | stopDuckingMedia | 

Stop ducking any media within this tree.
 |
| function | suspendMedia | 

Suspend any media playing within this tree.
 |
| function | scrollBackward | 

Scrolls this scrollable container backward.
 |
| function | scrollForward | 

Scrolls this scrollable container forward.
 |
| function | scrollUp | 

Scrolls this scrollable container up.
 |
| function | scrollDown | 

Scrolls this scrollable container down.
 |
| function | scrollLeft | 

Scrolls this scrollable container left.
 |
| function | scrollRight | 

Scrolls this scrollable container right.
 |
| function | addEventListener | 

Adds a listener for the given event type and event phase.

| Parameters |
|---|
| [EventType](/extensions/automation#type-EventType) | eventType |  |
| function | listener | 

A listener for events on an `AutomationNode`.

| Parameters |
|---|
| [AutomationEvent](/extensions/automation#type-AutomationEvent) | event |  |
 |
| boolean | capture |  |
 |
| function | removeEventListener | 

Removes a listener for the given event type and event phase.

| Parameters |
|---|
| [EventType](/extensions/automation#type-EventType) | eventType |  |
| function | listener | 

A listener for events on an `AutomationNode`.

| Parameters |
|---|
| [AutomationEvent](/extensions/automation#type-AutomationEvent) | event |  |
 |
| boolean | capture |  |
 |
| function | domQuerySelector | 

Gets the first node in this node's subtree which matches the given CSS selector and is within the same DOM context.

If this node doesn't correspond directly with an HTML node in the DOM, querySelector will be run on this node's nearest HTML node ancestor. Note that this may result in the query returning a node which is not a descendant of this node.

If the selector matches a node which doesn't directly correspond to an automation node (for example an element within an ARIA widget, where the ARIA widget forms one node of the automation tree, or an element which is hidden from accessibility via hiding it using CSS or using aria-hidden), this will return the nearest ancestor which does correspond to an automation node.

| Parameters |
|---|
| string | selector |  |
 |
| function | find | 

Finds the first AutomationNode in this node's subtree which matches the given search parameters.

Returns <span class="property">[AutomationNode](/extensions/automation#type-AutomationNode).</span>

| Parameters |
|---|
| [FindParams](/extensions/automation#type-FindParams) | params |  |
 |
| function | findAll | 

Finds all the AutomationNodes in this node's subtree which matches the given search parameters.

Returns <span class="property">array of [AutomationNode](/extensions/automation#type-AutomationNode).</span>

| Parameters |
|---|
| [FindParams](/extensions/automation#type-FindParams) | params |  |
 |
| function | matches | 

Returns whether this node matches the given [automation.FindParams](/extensions/automation#type-FindParams).

Returns <span class="property">boolean.</span>

| Parameters |
|---|
| [FindParams](/extensions/automation#type-FindParams) | params |  |
 |

</div>

## Methods

<div>

### getTree

<div class="summary">`whale.automation.getTree(<span class="optional">integer tabId</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Get the automation tree for the tab with the given tabId, or the current tab if no tabID is given, enabling automation if necessary. Returns a tree with a placeholder root node; listen for the "loadComplete" event to get a notification that the tree has fully loaded (the previous root node reference will stop working at or before this point).

| Parameters |
|---|
| integer | <span class="optional">(optional)</span> tabId |  |
| function | <span class="optional">(optional)</span> callback | 

Called when the `AutomationNode` for the page is available.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [AutomationNode](/extensions/automation#type-AutomationNode) rootNode) <span class="subdued">{...}</span>;`

| [AutomationNode](/extensions/automation#type-AutomationNode) | rootNode |  |
|---|---|---|
 |

</div>

</div>

<div>

### getDesktop

<div class="summary">`whale.automation.getDesktop(<span>function callback</span>)`</div>

<div class="description">

Get the automation tree for the whole desktop which consists of all on screen views. Note this API is currently only supported on Chrome OS.

| Parameters |
|---|
| function | callback | 

Called when the `AutomationNode` for the page is available.

The _callback_ parameter should be a function that looks like this:

`function( [AutomationNode](/extensions/automation#type-AutomationNode) rootNode) <span class="subdued">{...}</span>;`

| [AutomationNode](/extensions/automation#type-AutomationNode) | rootNode |  |
|---|---|---|
 |

</div>

</div>

<div>

### getFocus

<div class="summary">`whale.automation.getFocus(<span>function callback</span>)`</div>

<div class="description">

Get the automation node that currently has focus, globally. Will return null if none of the nodes in any loaded trees have focus.

| Parameters |
|---|
| function | callback | 

Called with the `AutomationNode` that currently has focus.

The _callback_ parameter should be a function that looks like this:

`function( [AutomationNode](/extensions/automation#type-AutomationNode) focusedNode) <span class="subdued">{...}</span>;`

| [AutomationNode](/extensions/automation#type-AutomationNode) | focusedNode |  |
|---|---|---|
 |

</div>

</div>

<div>

### addTreeChangeObserver

<div class="summary">`whale.automation.addTreeChangeObserver(<span>enum of `"noTreeChanges"`, `"liveRegionTreeChanges"`, `"textMarkerChanges"`, or `"allTreeChanges"` filter</span>, <span>function observer</span>)`</div>

<div class="description">

Add a tree change observer. Tree change observers are static/global, they listen to changes across all trees. Pass a filter to determine what specific tree changes to listen to, and note that listnening to all tree changes can be expensive.

| Parameters |
|---|
| enum of `"noTreeChanges"`, `"liveRegionTreeChanges"`, `"textMarkerChanges"`, or `"allTreeChanges"` | filter |  |
| function | observer | 

A listener for changes on the `AutomationNode` tree.

The _observer_ parameter should be a function that looks like this:

`function( [TreeChange](/extensions/automation#type-TreeChange) treeChange) <span class="subdued">{...}</span>;`

| [TreeChange](/extensions/automation#type-TreeChange) | treeChange |  |
|---|---|---|
 |

</div>

</div>

<div>

### removeTreeChangeObserver

<div class="summary">`whale.automation.removeTreeChangeObserver(<span>function observer</span>)`</div>

<div class="description">

Remove a tree change observer.

| Parameters |
|---|
| function | observer | 

A listener for changes on the `AutomationNode` tree.

The _observer_ parameter should be a function that looks like this:

`function( [TreeChange](/extensions/automation#type-TreeChange) treeChange) <span class="subdued">{...}</span>;`

| [TreeChange](/extensions/automation#type-TreeChange) | treeChange |  |
|---|---|---|
 |

</div>

</div>

<div>

### setDocumentSelection

<div class="summary">`whale.automation.setDocumentSelection( <span>[SetDocumentSelectionParams](/extensions/automation#type-SetDocumentSelectionParams) params</span>)`</div>

<div class="description">

Sets the selection in a tree. This creates a selection in a single tree (anchorObject and focusObject must have the same root). Everything in the tree between the two node/offset pairs gets included in the selection. The anchor is where the user started the selection, while the focus is the point at which the selection gets extended e.g. when dragging with a mouse or using the keyboard. For nodes with the role staticText, the offset gives the character offset within the value where the selection starts or ends, respectively.

| Parameters |
|---|
| [SetDocumentSelectionParams](/extensions/automation#type-SetDocumentSelectionParams) | params |  |

</div>

</div>

</div>

</section>