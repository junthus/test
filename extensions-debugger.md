# whale.debugger

| Description: | The `whale.debugger` API serves as an alternate transport for Chrome's [remote debugging protocol](https://developer.whale.com/devtools/docs/debugger-protocol). Use `whale.debugger` to attach to one or more tabs to instrument network interaction, debug JavaScript, mutate the DOM and CSS, etc. Use the Debuggee `tabId` to target tabs with sendCommand and route events by `tabId` from onEvent callbacks. |
|---|---|
| Availability: | Since Chrome 20. |
| Permissions: | <span class="code">"debugger"</span> |

<section>

## Notes

As of today, attaching to the tab by means of the debugger API and using embedded Chrome DevTools with that tab are mutually exclusive. If user invokes Chrome DevTools while extension is attached to the tab, debugging session is terminated. Extension can re-establish it later.

## Manifest

You must declare the "debugger" permission in your extension's manifest to use this API.

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
      **  "permissions": [
          "debugger",
        ]**,
        ...
      }
      </pre>

## Examples

You can find samples of this API in [Samples](samples#search:debugger).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [Debuggee](#type-Debuggee) |
| [TargetInfoType](#type-TargetInfoType) |
| [DetachReason](#type-DetachReason) |
| [TargetInfo](#type-TargetInfo) |
| Methods |
| [attach](#method-attach) − `whale.debugger.attach( <span>Debuggee target</span>, <span>string requiredVersion</span>, <span class="optional">function callback</span>)` |
| [detach](#method-detach) − `whale.debugger.detach( <span>Debuggee target</span>, <span class="optional">function callback</span>)` |
| [sendCommand](#method-sendCommand) − `whale.debugger.sendCommand( <span>Debuggee target</span>, <span>string method</span>, <span class="optional">object commandParams</span>, <span class="optional">function callback</span>)` |
| [getTargets](#method-getTargets) − `whale.debugger.getTargets(<span>function callback</span>)` |
| Events |
| [onEvent](#event-onEvent) |
| [onDetach](#event-onDetach) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### Debuggee

<dd>Debuggee identifier. Either tabId or extensionId must be specified</dd>

| properties |
|---|
| integer | <span class="optional">(optional)</span> tabId | 

The id of the tab which you intend to debug.
 |
| string | <span class="optional">(optional)</span> extensionId | 

Since Chrome 27.

The id of the extension which you intend to debug. Attaching to an extension background page is only possible when 'silent-debugger-extension-api' flag is enabled on the target browser.
 |
| string | <span class="optional">(optional)</span> targetId | 

Since Chrome 28.

The opaque id of the debug target.
 |

</div>

<div>

### TargetInfoType

<dd>Target type.</dd>

| Enum |
|---|
| `"page"`, `"background_page"`, `"worker"`, or `"other"` |

</div>

<div>

### DetachReason

<dd>Connection termination reason.</dd>

| Enum |
|---|
| `"target_closed"`, `"canceled_by_user"`, or `"replaced_with_devtools"` |

</div>

<div>

### TargetInfo

Since Chrome 28.

<dd>Debug target information</dd>

| properties |
|---|
| [TargetInfoType](/extensions/debugger#type-TargetInfoType) | type | 

Target type.
 |
| string | id | 

Target id.
 |
| integer | <span class="optional">(optional)</span> tabId | 

Since Chrome 30.

The tab id, defined if type == 'page'.
 |
| string | <span class="optional">(optional)</span> extensionId | 

Since Chrome 30.

The extension id, defined if type = 'background_page'.
 |
| boolean | attached | 

True if debugger is already attached.
 |
| string | title | 

Target page title.
 |
| string | url | 

Target URL.
 |
| string | <span class="optional">(optional)</span> faviconUrl | 

Target favicon URL.
 |

</div>

## Methods

<div>

### attach

<div class="summary">`whale.debugger.attach( <span>[Debuggee](/extensions/debugger#type-Debuggee) target</span>, <span>string requiredVersion</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Attaches debugger to the given target.

| Parameters |
|---|
| [Debuggee](/extensions/debugger#type-Debuggee) | target | 

Debugging target to which you want to attach.
 |
| string | requiredVersion | 

Required debugging protocol version ("0.1"). One can only attach to the debuggee with matching major version and greater or equal minor version. List of the protocol versions can be obtained [here](https://developer.whale.com/devtools/docs/debugger-protocol).
 |
| function | <span class="optional">(optional)</span> callback | 

Called once the attach operation succeeds or fails. Callback receives no arguments. If the attach fails, [runtime.lastError](/extensions/runtime#property-lastError) will be set to the error message.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### detach

<div class="summary">`whale.debugger.detach( <span>[Debuggee](/extensions/debugger#type-Debuggee) target</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Detaches debugger from the given target.

| Parameters |
|---|
| [Debuggee](/extensions/debugger#type-Debuggee) | target | 

Debugging target from which you want to detach.
 |
| function | <span class="optional">(optional)</span> callback | 

Called once the detach operation succeeds or fails. Callback receives no arguments. If the detach fails, [runtime.lastError](/extensions/runtime#property-lastError) will be set to the error message.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### sendCommand

<div class="summary">`whale.debugger.sendCommand( <span>[Debuggee](/extensions/debugger#type-Debuggee) target</span>, <span>string method</span>, <span class="optional">object commandParams</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Sends given command to the debugging target.

| Parameters |
|---|
| [Debuggee](/extensions/debugger#type-Debuggee) | target | 

Debugging target to which you want to send the command.
 |
| string | method | 

Method name. Should be one of the methods defined by the [remote debugging protocol](https://developer.whale.com/devtools/docs/debugger-protocol).
 |
| object | <span class="optional">(optional)</span> commandParams | 

Since Chrome 22.

JSON object with request parameters. This object must conform to the remote debugging params scheme for given method.
 |
| function | <span class="optional">(optional)</span> callback | 

Response body. If an error occurs while posting the message, the callback will be called with no arguments and [runtime.lastError](/extensions/runtime#property-lastError) will be set to the error message.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(object result) <span class="subdued">{...}</span>;`

| object | <span class="optional">(optional)</span> result | 
|---|---|

JSON object with the response. Structure of the response varies depending on the method name and is defined by the 'returns' attribute of the command description in the remote debugging protocol.
 |
 |

</div>

</div>

<div>

### getTargets

<div class="summary">`whale.debugger.getTargets(<span>function callback</span>)`</div>

<div class="description">

Since Chrome 28.

Returns the list of available debug targets.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [TargetInfo](/extensions/debugger#type-TargetInfo) result) <span class="subdued">{...}</span>;`

| array of [TargetInfo](/extensions/debugger#type-TargetInfo) | result | 
|---|---|

Array of TargetInfo objects corresponding to the available debug targets.
 |
 |

</div>

</div>

## Events

<div>

### onEvent

<div class="description">

Fired whenever debugging target issues instrumentation event.

<div>

#### addListener

<div class="summary">`whale.debugger.onEvent.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Debuggee](/extensions/debugger#type-Debuggee) source, string method, object params) <span class="subdued">{...}</span>;`

| [Debuggee](/extensions/debugger#type-Debuggee) | source | 
|---|---|

The debuggee that generated this event.
 |
| string | method | 

Method name. Should be one of the notifications defined by the [remote debugging protocol](https://developer.whale.com/devtools/docs/debugger-protocol).
 |
| object | <span class="optional">(optional)</span> params | 

JSON object with the parameters. Structure of the parameters varies depending on the method name and is defined by the 'parameters' attribute of the event description in the remote debugging protocol.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onDetach

<div class="description">

Fired when browser terminates debugging session for the tab. This happens when either the tab is being closed or Chrome DevTools is being invoked for the attached tab.

<div>

#### addListener

<div class="summary">`whale.debugger.onDetach.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Debuggee](/extensions/debugger#type-Debuggee) source, [DetachReason](/extensions/debugger#type-DetachReason) reason) <span class="subdued">{...}</span>;`

| [Debuggee](/extensions/debugger#type-Debuggee) | source | 
|---|---|

The debuggee that was detached.
 |
| [DetachReason](/extensions/debugger#type-DetachReason) | reason | 

Since Chrome 24.

Connection termination reason.
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>