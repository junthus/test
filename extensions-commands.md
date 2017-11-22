# whale.commands

| Description: | Use the commands API to add keyboard shortcuts that trigger actions in your extension, for example, an action to open the browser action or send a command to the extension. |
|---|---|
| Availability: | Since Chrome 25. |
| Manifest: | <span class="code">"commands": {...}</span> |

<section>

## Manifest

You must have a `"manifest_version"` of at least `2` to use this API.

## Usage

The commands API allows you to define specific commands, and bind them to a default key combination. Each command your extension accepts must be listed in the manifest as an attribute of the 'commands' manifest key. An extension can have many commands but only 4 suggested keys can be specified. The user can manually add more shortcuts from the chrome://extensions/configureCommands dialog.

Supported keys: A-Z, 0-9, Comma, Period, Home, End, PageUp, PageDown, Space, Insert, Delete, Arrow keys (Up, Down, Left, Right) and the Media Keys (MediaNextTrack, MediaPlayPause, MediaPrevTrack, MediaStop).

Note: All key combinations must include either Ctrl* or Alt. Combinations that involve Ctrl+Alt are not permitted in order to avoid conflicts with the AltGr key. Shift can be used in addition to Alt or Ctrl, but is not required. Modifiers (such as Ctrl) can not be used in combination with the Media Keys. Tab key was removed from list of supported keys in Chrome version 33 and above for accessibility reasons.

* Also note that on Mac 'Ctrl' is automatically converted to 'Command'. If you want 'Ctrl' instead, please specify 'MacCtrl'.

* Additionally, on Chrome OS, you can specify 'Search' as a modifier.

Certain Chrome shortcuts (e.g. window management) always take priority over Extension Command shortcuts and can not be overwritten.

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
      **  "commands": {
          "toggle-feature-foo": {
            "suggested_key": {
              "default": "Ctrl+Shift+Y",
              "mac": "Command+Shift+Y"
            },
            "description": "Toggle feature foo"
          },
          "_execute_browser_action": {
            "suggested_key": {
              "windows": "Ctrl+Shift+Y",
              "mac": "Command+Shift+Y",
              "chromeos": "Ctrl+Shift+U",
              "linux": "Ctrl+Shift+J"
            }
          },
          "_execute_page_action": {
            "suggested_key": {
              "default": "Ctrl+Shift+E",
              "windows": "Alt+Shift+P",
              "mac": "Alt+Shift+P"
            }
          }
        }**,
        ...
      }</pre>

In your background page, you can bind a handler to each of the commands defined in the manifest (except for '_execute_browser_action' and '_execute_page_action') via onCommand.addListener. For example:

<pre>      whale.commands.onCommand.addListener(function(command) {
        console.log('Command:', command);
      });
      </pre>

The '_execute_browser_action' and '_execute_page_action' commands are reserved for the action of opening your extension's popups. They won't normally generate events that you can handle. If you need to take action based on your popup opening, consider listening for an 'onDomReady' event inside your popup's code.

## Scope

By default, Commands are scoped to the Chrome browser, which means that while the browser does not have focus, the shortcut will be inactive. On desktop Chrome, Commands can instead have global scope, as of version 35, and will then also work while Chrome does *not* have focus. NOTE: The exception here is Chrome OS, where global commands are not allowed at the moment.

The user is free to designate any shortcut as global using the UI in chrome://extensions \ Keyboard Shortcuts, but the extension developer is limited to specifying only Ctrl+Shift+[0..9] as global shortcuts. This is to minimize the risk of overriding shortcuts in other applications since if, for example, Alt+P were to be allowed as global, the printing shortcut might not work in other applications.

Example:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        "commands": {
          "toggle-feature-foo": {
            "suggested_key": {
              "default": "Ctrl+Shift+5"
            },
            "description": "Toggle feature foo",
            **"global": true**
          }
        },
        ...
      }</pre>

</section>

<section id="toc">

## Summary

| Types |
|---|
| [Command](#type-Command) |
| Methods |
| [getAll](#method-getAll) − `whale.commands.getAll(<span class="optional">function callback</span>)` |
| Events |
| [onCommand](#event-onCommand) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### Command

| properties |
|---|
| string | <span class="optional">(optional)</span> name | 

The name of the Extension Command
 |
| string | <span class="optional">(optional)</span> description | 

The Extension Command description
 |
| string | <span class="optional">(optional)</span> shortcut | 

The shortcut active for this command, or blank if not active.
 |

</div>

## Methods

<div>

### getAll

<div class="summary">`whale.commands.getAll(<span class="optional">function callback</span>)`</div>

<div class="description">

Returns all the registered extension commands for this extension and their shortcut (if active).

| Parameters |
|---|
| function | <span class="optional">(optional)</span> callback | 

Called to return the registered commands.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(array of [Command](/extensions/commands#type-Command) commands) <span class="subdued">{...}</span>;`

| array of [Command](/extensions/commands#type-Command) | commands |  |
|---|---|---|
 |

</div>

</div>

## Events

<div>

### onCommand

<div class="description">

Fired when a registered command is activated using a keyboard shortcut.

<div>

#### addListener

<div class="summary">`whale.commands.onCommand.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string command) <span class="subdued">{...}</span>;`

| string | command |  |
|---|---|---|
 |

</div>

</div>

</div>

</div>

</div>

</section>