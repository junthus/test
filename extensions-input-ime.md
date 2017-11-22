# whale.input.ime

| Description: | Use the `whale.input.ime` API to implement a custom IME for Chrome OS. This allows your extension to handle keystrokes, set the composition, and manage the candidate window. |
|---|---|
| Availability: | Since Chrome 21. |
| Permissions: | <span class="code">"input"</span> |

<section>

## Manifest

You must declare the "input" permission in the [extension manifest](manifest) to use the input.ime API. For example:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"permissions": [
          "input"
        ]**,
        ...
      }</pre>

## Examples

The following code creates an IME that converts typed letters to upper case.

<pre>      var context_id = -1;

      whale.input.ime.onFocus.addListener(function(context) {
        context_id = context.contextID;
      });

      whale.input.ime.onKeyEvent.addListener(
        function(engineID, keyData) {
          if (keyData.type == "keydown" && keyData.key.match(/^[a-z]$/)) {
            whale.input.ime.commitText({"contextID": context_id,
                                         "text": keyData.key.toUpperCase()});
            return true;
          } else {
            return false;
          }
        });
      </pre>

For an example of using this API, see the [basic input.ime sample](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/docs/examples/api/input.ime/basic/). For other examples and for help in viewing the source code, see [Samples](samples).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [KeyboardEventType](#type-KeyboardEventType) |
| [KeyboardEvent](#type-KeyboardEvent) |
| [InputContextType](#type-InputContextType) |
| [InputContext](#type-InputContext) |
| [MenuItemStyle](#type-MenuItemStyle) |
| [MenuItem](#type-MenuItem) |
| [UnderlineStyle](#type-UnderlineStyle) |
| [WindowPosition](#type-WindowPosition) |
| [ScreenType](#type-ScreenType) |
| [CallbackStyle](#type-CallbackStyle) |
| [MouseButton](#type-MouseButton) |
| [WindowType](#type-WindowType) |
| [Bounds](#type-Bounds) |
| [CreateWindowOptions](#type-CreateWindowOptions) |
| Methods |
| [setComposition](#method-setComposition) − `whale.input.ime.setComposition(<span>object parameters</span>, <span class="optional">function callback</span>)` |
| [clearComposition](#method-clearComposition) − `whale.input.ime.clearComposition(<span>object parameters</span>, <span class="optional">function callback</span>)` |
| [commitText](#method-commitText) − `whale.input.ime.commitText(<span>object parameters</span>, <span class="optional">function callback</span>)` |
| [sendKeyEvents](#method-sendKeyEvents) − `whale.input.ime.sendKeyEvents(<span>object parameters</span>, <span class="optional">function callback</span>)` |
| [hideInputView](#method-hideInputView) − `whale.input.ime.hideInputView()` |
| [setCandidateWindowProperties](#method-setCandidateWindowProperties) − `whale.input.ime.setCandidateWindowProperties(<span>object parameters</span>, <span class="optional">function callback</span>)` |
| [setCandidates](#method-setCandidates) − `whale.input.ime.setCandidates(<span>object parameters</span>, <span class="optional">function callback</span>)` |
| [setCursorPosition](#method-setCursorPosition) − `whale.input.ime.setCursorPosition(<span>object parameters</span>, <span class="optional">function callback</span>)` |
| [setMenuItems](#method-setMenuItems) − `whale.input.ime.setMenuItems(<span>object parameters</span>, <span class="optional">function callback</span>)` |
| [updateMenuItems](#method-updateMenuItems) − `whale.input.ime.updateMenuItems(<span>object parameters</span>, <span class="optional">function callback</span>)` |
| [deleteSurroundingText](#method-deleteSurroundingText) − `whale.input.ime.deleteSurroundingText(<span>object parameters</span>, <span class="optional">function callback</span>)` |
| [keyEventHandled](#method-keyEventHandled) − `whale.input.ime.keyEventHandled(<span>string requestId</span>, <span>boolean response</span>)` |
| [createWindow](#method-createWindow) − `whale.input.ime.createWindow( <span>CreateWindowOptions options</span>, <span>function callback</span>)` |
| [showWindow](#method-showWindow) − `whale.input.ime.showWindow(<span>integer windowId</span>, <span class="optional">function callback</span>)` |
| [hideWindow](#method-hideWindow) − `whale.input.ime.hideWindow(<span>integer windowId</span>, <span class="optional">function callback</span>)` |
| [activate](#method-activate) − `whale.input.ime.activate(<span class="optional">function callback</span>)` |
| [deactivate](#method-deactivate) − `whale.input.ime.deactivate(<span class="optional">function callback</span>)` |
| Events |
| [onActivate](#event-onActivate) |
| [onDeactivated](#event-onDeactivated) |
| [onFocus](#event-onFocus) |
| [onBlur](#event-onBlur) |
| [onInputContextUpdate](#event-onInputContextUpdate) |
| [onKeyEvent](#event-onKeyEvent) |
| [onCandidateClicked](#event-onCandidateClicked) |
| [onMenuItemActivated](#event-onMenuItemActivated) |
| [onSurroundingTextChanged](#event-onSurroundingTextChanged) |
| [onReset](#event-onReset) |
| [onCompositionBoundsChanged](#event-onCompositionBoundsChanged) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### KeyboardEventType

| Enum |
|---|
| `"keyup"`, or `"keydown"` |

</div>

<div>

### KeyboardEvent

<dd>See http://www.w3.org/TR/DOM-Level-3-Events/#events-KeyboardEvent</dd>

| properties |
|---|
| [KeyboardEventType](/extensions/input.ime#type-KeyboardEventType) | type | 

One of keyup or keydown.
 |
| string | requestId | 

The ID of the request.
 |
| string | <span class="optional">(optional)</span> extensionId | 

Since Chrome 34.

The extension ID of the sender of this keyevent.
 |
| string | key | 

Value of the key being pressed
 |
| string | code | 

Since Chrome 26.

Value of the physical key being pressed. The value is not affected by current keyboard layout or modifier state.
 |
| integer | <span class="optional">(optional)</span> keyCode | 

Since Chrome 37.

The deprecated HTML keyCode, which is system- and implementation-dependent numerical code signifying the unmodified identifier associated with the key pressed.
 |
| boolean | <span class="optional">(optional)</span> altKey | 

Whether or not the ALT key is pressed.
 |
| boolean | <span class="optional">(optional)</span> ctrlKey | 

Whether or not the CTRL key is pressed.
 |
| boolean | <span class="optional">(optional)</span> shiftKey | 

Whether or not the SHIFT key is pressed.
 |
| boolean | <span class="optional">(optional)</span> capsLock | 

Since Chrome 29.

Whether or not the CAPS_LOCK is enabled.
 |

</div>

<div>

### InputContextType

<dd>Type of value this text field edits, (Text, Number, URL, etc)</dd>

| Enum |
|---|
| `"text"`, `"search"`, `"tel"`, `"url"`, `"email"`, `"number"`, or `"password"` |

</div>

<div>

### InputContext

<dd>Describes an input Context</dd>

| properties |
|---|
| integer | contextID | 

This is used to specify targets of text field operations. This ID becomes invalid as soon as onBlur is called.
 |
| [InputContextType](/extensions/input.ime#type-InputContextType) | type | 

Type of value this text field edits, (Text, Number, URL, etc)
 |
| boolean | autoCorrect | 

Since Chrome 40.

Whether the text field wants auto-correct.
 |
| boolean | autoComplete | 

Since Chrome 40.

Whether the text field wants auto-complete.
 |
| boolean | spellCheck | 

Since Chrome 40.

Whether the text field wants spell-check.
 |

</div>

<div>

### MenuItemStyle

<dd>The type of menu item. Radio buttons between separators are considered grouped.</dd>

| Enum |
|---|
| `"check"`, `"radio"`, or `"separator"` |

</div>

<div>

### MenuItem

Since Chrome 30.

<dd>A menu item used by an input method to interact with the user from the language menu.</dd>

| properties |
|---|
| string | id | 

String that will be passed to callbacks referencing this MenuItem.
 |
| string | <span class="optional">(optional)</span> label | 

Text displayed in the menu for this item.
 |
| [MenuItemStyle](/extensions/input.ime#type-MenuItemStyle) | <span class="optional">(optional)</span> style | 

The type of menu item.
 |
| boolean | <span class="optional">(optional)</span> visible | 

Indicates this item is visible.
 |
| boolean | <span class="optional">(optional)</span> checked | 

Indicates this item should be drawn with a check.
 |
| boolean | <span class="optional">(optional)</span> enabled | 

Indicates this item is enabled.
 |

</div>

<div>

### UnderlineStyle

<dd>The type of the underline to modify this segment.</dd>

| Enum |
|---|
| `"underline"`, `"doubleUnderline"`, or `"noUnderline"` |

</div>

<div>

### WindowPosition

<dd>Where to display the candidate window. If set to 'cursor', the window follows the cursor. If set to 'composition', the window is locked to the beginning of the composition.</dd>

| Enum |
|---|
| `"cursor"`, or `"composition"` |

</div>

<div>

### ScreenType

<dd>The screen type under which the IME is activated.</dd>

| Enum |
|---|
| `"normal"`, `"login"`, `"lock"`, or `"secondary-login"` |

</div>

<div>

### CallbackStyle

| Enum |
|---|
| `"async"` |

</div>

<div>

### MouseButton

<dd>Which mouse buttons was clicked.</dd>

| Enum |
|---|
| `"left"`, `"middle"`, or `"right"` |

</div>

<div>

### WindowType

<dd>The IME window types.</dd>

| Enum |
|---|
| `"normal"`, or `"followCursor"` |

</div>

<div>

### Bounds

Since Chrome 50.

<dd>Describes the screen coordinates of a rect.</dd>

| properties |
|---|
| integer | left | 

The left of the bounds.
 |
| integer | top | 

The top of the bounds.
 |
| integer | width | 

The width of the bounds.
 |
| integer | height | 

The height of the bounds .
 |

</div>

<div>

### CreateWindowOptions

Since Chrome 50.

<dd>The options to create an IME window</dd>

| properties |
|---|
| [WindowType](/extensions/input.ime#type-WindowType) | windowType |  |
| string | <span class="optional">(optional)</span> url |  |
| [Bounds](/extensions/input.ime#type-Bounds) | <span class="optional">(optional)</span> bounds |  |

</div>

## Methods

<div>

### setComposition

<div class="summary">`whale.input.ime.setComposition(<span>object parameters</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Set the current composition. If this extension does not own the active IME, this fails.

| Parameters |
|---|
| object | parameters | 

| integer | contextID | 
|---|---|

ID of the context where the composition text will be set
 |
| string | text | 

Text to set
 |
| integer | <span class="optional">(optional)</span> selectionStart | 

Position in the text that the selection starts at.
 |
| integer | <span class="optional">(optional)</span> selectionEnd | 

Position in the text that the selection ends at.
 |
| integer | cursor | 

Position in the text of the cursor.
 |
| array of object | <span class="optional">(optional)</span> segments | 

List of segments and their associated types.

#### Properties of each object

<dl>

<div>

| integer | start | 
|---|---|

Index of the character to start this segment at
 |
| integer | end | 

Index of the character to end this segment after.
 |
| [UnderlineStyle](/extensions/input.ime#type-UnderlineStyle) | style | 

The type of the underline to modify this segment.
 |

</div>

</dl>
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the operation completes with a boolean indicating if the text was accepted or not. On failure, whale.runtime.lastError is set.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(boolean success) <span class="subdued">{...}</span>;`

| boolean | success |  |
|---|---|---|
 |

</div>

</div>

<div>

### clearComposition

<div class="summary">`whale.input.ime.clearComposition(<span>object parameters</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clear the current composition. If this extension does not own the active IME, this fails.

| Parameters |
|---|
| object | parameters | 

| integer | contextID | 
|---|---|

ID of the context where the composition will be cleared
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the operation completes with a boolean indicating if the text was accepted or not. On failure, whale.runtime.lastError is set.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(boolean success) <span class="subdued">{...}</span>;`

| boolean | success |  |
|---|---|---|
 |

</div>

</div>

<div>

### commitText

<div class="summary">`whale.input.ime.commitText(<span>object parameters</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Commits the provided text to the current input.

| Parameters |
|---|
| object | parameters | 

| integer | contextID | 
|---|---|

ID of the context where the text will be committed
 |
| string | text | 

The text to commit
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the operation completes with a boolean indicating if the text was accepted or not. On failure, whale.runtime.lastError is set.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(boolean success) <span class="subdued">{...}</span>;`

| boolean | success |  |
|---|---|---|
 |

</div>

</div>

<div>

### sendKeyEvents

<div class="summary">`whale.input.ime.sendKeyEvents(<span>object parameters</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 33.

Sends the key events. This function is expected to be used by virtual keyboards. When key(s) on a virtual keyboard is pressed by a user, this function is used to propagate that event to the system.

| Parameters |
|---|
| object | parameters | 

| integer | contextID | 
|---|---|

ID of the context where the key events will be sent, or zero to send key events to non-input field.
 |
| array of [KeyboardEvent](/extensions/input.ime#type-KeyboardEvent) | keyData | 

Data on the key event.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the operation completes.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### hideInputView

<div class="summary">`whale.input.ime.hideInputView()`</div>

<div class="description">

Since Chrome 34.

Hides the input view window, which is popped up automatically by system. If the input view window is already hidden, this function will do nothing.

</div>

</div>

<div>

### setCandidateWindowProperties

<div class="summary">`whale.input.ime.setCandidateWindowProperties(<span>object parameters</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Sets the properties of the candidate window. This fails if the extension doesn't own the active IME

| Parameters |
|---|
| object | parameters | 

| string | engineID | 
|---|---|

ID of the engine to set properties on.
 |
| object | properties | 

| boolean | <span class="optional">(optional)</span> visible | 
|---|---|

True to show the Candidate window, false to hide it.
 |
| boolean | <span class="optional">(optional)</span> cursorVisible | 

True to show the cursor, false to hide it.
 |
| boolean | <span class="optional">(optional)</span> vertical | 

True if the candidate window should be rendered vertical, false to make it horizontal.
 |
| integer | <span class="optional">(optional)</span> pageSize | 

The number of candidates to display per page.
 |
| string | <span class="optional">(optional)</span> auxiliaryText | 

Text that is shown at the bottom of the candidate window.
 |
| boolean | <span class="optional">(optional)</span> auxiliaryTextVisible | 

True to display the auxiliary text, false to hide it.
 |
| [WindowPosition](/extensions/input.ime#type-WindowPosition) | <span class="optional">(optional)</span> windowPosition | 

Since Chrome 28.

Where to display the candidate window.
 |
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the operation completes.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(boolean success) <span class="subdued">{...}</span>;`

| boolean | success |  |
|---|---|---|
 |

</div>

</div>

<div>

### setCandidates

<div class="summary">`whale.input.ime.setCandidates(<span>object parameters</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Sets the current candidate list. This fails if this extension doesn't own the active IME

| Parameters |
|---|
| object | parameters | 

| integer | contextID | 
|---|---|

ID of the context that owns the candidate window.
 |
| array of object | candidates | 

List of candidates to show in the candidate window

#### Properties of each object

<dl>

<div>

| string | candidate | 
|---|---|

The candidate
 |
| integer | id | 

The candidate's id
 |
| integer | <span class="optional">(optional)</span> parentId | 

The id to add these candidates under
 |
| string | <span class="optional">(optional)</span> label | 

Short string displayed to next to the candidate, often the shortcut key or index
 |
| string | <span class="optional">(optional)</span> annotation | 

Additional text describing the candidate
 |
| object | <span class="optional">(optional)</span> usage | 

The usage or detail description of word.

| string | title | 
|---|---|

The title string of details description.
 |
| string | body | 

The body string of detail description.
 |
 |

</div>

</dl>
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the operation completes.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(boolean success) <span class="subdued">{...}</span>;`

| boolean | success |  |
|---|---|---|
 |

</div>

</div>

<div>

### setCursorPosition

<div class="summary">`whale.input.ime.setCursorPosition(<span>object parameters</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Set the position of the cursor in the candidate window. This is a no-op if this extension does not own the active IME.

| Parameters |
|---|
| object | parameters | 

| integer | contextID | 
|---|---|

ID of the context that owns the candidate window.
 |
| integer | candidateID | 

ID of the candidate to select.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the operation completes

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(boolean success) <span class="subdued">{...}</span>;`

| boolean | success |  |
|---|---|---|
 |

</div>

</div>

<div>

### setMenuItems

<div class="summary">`whale.input.ime.setMenuItems(<span>object parameters</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Adds the provided menu items to the language menu when this IME is active.

| Parameters |
|---|
| object | parameters | 

| string | engineID | 
|---|---|

ID of the engine to use
 |
| array of [MenuItem](/extensions/input.ime#type-MenuItem) | items | 

MenuItems to add. They will be added in the order they exist in the array.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### updateMenuItems

<div class="summary">`whale.input.ime.updateMenuItems(<span>object parameters</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Updates the state of the MenuItems specified

| Parameters |
|---|
| object | parameters | 

| string | engineID | 
|---|---|

ID of the engine to use
 |
| array of [MenuItem](/extensions/input.ime#type-MenuItem) | items | 

Array of MenuItems to update
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the operation completes

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### deleteSurroundingText

<div class="summary">`whale.input.ime.deleteSurroundingText(<span>object parameters</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 27.

Deletes the text around the caret.

| Parameters |
|---|
| object | parameters | 

| string | engineID | 
|---|---|

ID of the engine receiving the event.
 |
| integer | contextID | 

ID of the context where the surrounding text will be deleted.
 |
| integer | offset | 

The offset from the caret position where deletion will start. This value can be negative.
 |
| integer | length | 

The number of characters to be deleted
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the operation completes.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### keyEventHandled

<div class="summary">`whale.input.ime.keyEventHandled(<span>string requestId</span>, <span>boolean response</span>)`</div>

<div class="description">

Since Chrome 25.

Indicates that the key event received by onKeyEvent is handled. This should only be called if the onKeyEvent listener is asynchronous.

| Parameters |
|---|
| string | requestId | 

Request id of the event that was handled. This should come from keyEvent.requestId
 |
| boolean | response | 

True if the keystroke was handled, false if not
 |

</div>

</div>

<div>

### createWindow

<div class="summary">`whale.input.ime.createWindow( <span>[CreateWindowOptions](/extensions/input.ime#type-CreateWindowOptions) options</span>, <span>function callback</span>)`</div>

<div class="description">

Since Chrome 50.

Creates IME window.

| Parameters |
|---|
| [CreateWindowOptions](/extensions/input.ime#type-CreateWindowOptions) | options | 

The options of the newly created IME window.
 |
| function | callback | 

Called when the operation completes.

The _callback_ parameter should be a function that looks like this:

`function(Window windowObject) <span class="subdued">{...}</span>;`

| Window | windowObject | 
|---|---|

The JavaScript 'window' object of the newly created IME window. It contains the additional 'id' property for the parameters of the other functions like showWindow/hideWindow.
 |
 |

</div>

</div>

<div>

### showWindow

<div class="summary">`whale.input.ime.showWindow(<span>integer windowId</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 51.

Shows the IME window. This makes the hidden window visible.

| Parameters |
|---|
| integer | windowId | 

The ID of the IME window.
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the operation completes.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### hideWindow

<div class="summary">`whale.input.ime.hideWindow(<span>integer windowId</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 51.

Hides the IME window. This doesn't close the window. Instead, it makes the window invisible. The extension can cache the window and show/hide it for better performance.

| Parameters |
|---|
| integer | windowId | 

The ID of the IME window.
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the operation completes.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### activate

<div class="summary">`whale.input.ime.activate(<span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 50.

Activates the IME extension so that it can receive events.

| Parameters |
|---|
| function | <span class="optional">(optional)</span> callback | 

Called when the operation completes.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### deactivate

<div class="summary">`whale.input.ime.deactivate(<span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 50.

Deactivates the IME extension so that it cannot receive events.

| Parameters |
|---|
| function | <span class="optional">(optional)</span> callback | 

Called when the operation completes.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

## Events

<div>

### onActivate

<div class="description">

This event is sent when an IME is activated. It signals that the IME will be receiving onKeyPress events.

<div>

#### addListener

<div class="summary">`whale.input.ime.onActivate.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string engineID, [ScreenType](/extensions/input.ime#type-ScreenType) screen) <span class="subdued">{...}</span>;`

| string | engineID | 
|---|---|

ID of the engine receiving the event
 |
| [ScreenType](/extensions/input.ime#type-ScreenType) | screen | 

Since Chrome 38.

The screen type under which the IME is activated.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onDeactivated

<div class="description">

This event is sent when an IME is deactivated. It signals that the IME will no longer be receiving onKeyPress events.

<div>

#### addListener

<div class="summary">`whale.input.ime.onDeactivated.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string engineID) <span class="subdued">{...}</span>;`

| string | engineID | 
|---|---|

ID of the engine receiving the event
 |
 |

</div>

</div>

</div>

</div>

<div>

### onFocus

<div class="description">

This event is sent when focus enters a text box. It is sent to all extensions that are listening to this event, and enabled by the user.

<div>

#### addListener

<div class="summary">`whale.input.ime.onFocus.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [InputContext](/extensions/input.ime#type-InputContext) context) <span class="subdued">{...}</span>;`

| [InputContext](/extensions/input.ime#type-InputContext) | context | 
|---|---|

Describes the text field that has acquired focus.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onBlur

<div class="description">

This event is sent when focus leaves a text box. It is sent to all extensions that are listening to this event, and enabled by the user.

<div>

#### addListener

<div class="summary">`whale.input.ime.onBlur.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(integer contextID) <span class="subdued">{...}</span>;`

| integer | contextID | 
|---|---|

The ID of the text field that has lost focus. The ID is invalid after this call
 |
 |

</div>

</div>

</div>

</div>

<div>

### onInputContextUpdate

<div class="description">

This event is sent when the properties of the current InputContext change, such as the the type. It is sent to all extensions that are listening to this event, and enabled by the user.

<div>

#### addListener

<div class="summary">`whale.input.ime.onInputContextUpdate.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [InputContext](/extensions/input.ime#type-InputContext) context) <span class="subdued">{...}</span>;`

| [InputContext](/extensions/input.ime#type-InputContext) | context | 
|---|---|

An InputContext object describing the text field that has changed.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onKeyEvent

<div class="description">

Fired when a key event is sent from the operating system. The event will be sent to the extension if this extension owns the active IME.

<div>

#### addListener

<div class="summary">`whale.input.ime.onKeyEvent.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string engineID, [KeyboardEvent](/extensions/input.ime#type-KeyboardEvent) keyData) <span class="subdued">{...}</span>;`

| string | engineID | 
|---|---|

ID of the engine receiving the event
 |
| [KeyboardEvent](/extensions/input.ime#type-KeyboardEvent) | keyData | 

Data on the key event
 |
 |

</div>

</div>

</div>

</div>

<div>

### onCandidateClicked

<div class="description">

This event is sent if this extension owns the active IME.

<div>

#### addListener

<div class="summary">`whale.input.ime.onCandidateClicked.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string engineID, integer candidateID, [MouseButton](/extensions/input.ime#type-MouseButton) button) <span class="subdued">{...}</span>;`

| string | engineID | 
|---|---|

ID of the engine receiving the event
 |
| integer | candidateID | 

ID of the candidate that was clicked.
 |
| [MouseButton](/extensions/input.ime#type-MouseButton) | button | 

Which mouse buttons was clicked.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onMenuItemActivated

<div class="description">

Called when the user selects a menu item

<div>

#### addListener

<div class="summary">`whale.input.ime.onMenuItemActivated.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string engineID, string name) <span class="subdued">{...}</span>;`

| string | engineID | 
|---|---|

ID of the engine receiving the event
 |
| string | name | 

Name of the MenuItem which was activated
 |
 |

</div>

</div>

</div>

</div>

<div>

### onSurroundingTextChanged

<div class="description">

Since Chrome 27.

Called when the editable string around caret is changed or when the caret position is moved. The text length is limited to 100 characters for each back and forth direction.

<div>

#### addListener

<div class="summary">`whale.input.ime.onSurroundingTextChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string engineID, object surroundingInfo) <span class="subdued">{...}</span>;`

| string | engineID | 
|---|---|

ID of the engine receiving the event
 |
| object | surroundingInfo | 

The surrounding information.

| string | text | 
|---|---|

The text around the cursor. This is only a subset of all text in the input field.
 |
| integer | focus | 

The ending position of the selection. This value indicates caret position if there is no selection.
 |
| integer | anchor | 

The beginning position of the selection. This value indicates caret position if there is no selection.
 |
| integer | offset | 

Since Chrome 46.

The offset position of `text`. Since `text` only includes a subset of text around the cursor, offset indicates the absolute position of the first character of `text`.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onReset

<div class="description">

Since Chrome 29.

This event is sent when chrome terminates ongoing text input session.

<div>

#### addListener

<div class="summary">`whale.input.ime.onReset.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string engineID) <span class="subdued">{...}</span>;`

| string | engineID | 
|---|---|

ID of the engine receiving the event
 |
 |

</div>

</div>

</div>

</div>

<div>

### onCompositionBoundsChanged

<div class="description">

Since Chrome 50.

Triggered when the bounds of the IME composition text or cursor are changed. The IME composition text is the instance of text produced in the input method editor.

<div>

#### addListener

<div class="summary">`whale.input.ime.onCompositionBoundsChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [Bounds](/extensions/input.ime#type-Bounds) boundsList) <span class="subdued">{...}</span>;`

| array of [Bounds](/extensions/input.ime#type-Bounds) | boundsList | 
|---|---|

List of bounds information for each character on IME composition text. If there's no composition text in the editor, this array contains the bound information of the cursor.
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>