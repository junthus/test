# whale.idle

| Description: | Use the `whale.idle` API to detect when the machine's idle state changes. |
|---|---|
| Availability: | Since Chrome 20. |
| Permissions: | <span class="code">"idle"</span> |

<section>

## Manifest

You must declare the "idle" permission in your extension's manifest to use the idle API. For example:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"permissions": [
          "idle"
        ]**,
        ...
      }
      </pre>

</section>

<section id="toc">

## Summary

| Types |
|---|
| [IdleState](#type-IdleState) |
| Methods |
| [queryState](#method-queryState) − `whale.idle.queryState(<span>integer detectionIntervalInSeconds</span>, <span>function callback</span>)` |
| [setDetectionInterval](#method-setDetectionInterval) − `whale.idle.setDetectionInterval(<span>integer intervalInSeconds</span>)` |
| Events |
| [onStateChanged](#event-onStateChanged) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### IdleState

| Enum |
|---|
| `"active"`, `"idle"`, or `"locked"` |

</div>

## Methods

<div>

### queryState

<div class="summary">`whale.idle.queryState(<span>integer detectionIntervalInSeconds</span>, <span>function callback</span>)`</div>

<div class="description">

Returns "locked" if the system is locked, "idle" if the user has not generated any input for a specified number of seconds, or "active" otherwise.

| Parameters |
|---|
| integer | detectionIntervalInSeconds | 

Since Chrome 25.

The system is considered idle if detectionIntervalInSeconds seconds have elapsed since the last user input detected.
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [IdleState](/extensions/idle#type-IdleState) newState) <span class="subdued">{...}</span>;`

| [IdleState](/extensions/idle#type-IdleState) | newState |  |
|---|---|---|
 |

</div>

</div>

<div>

### setDetectionInterval

<div class="summary">`whale.idle.setDetectionInterval(<span>integer intervalInSeconds</span>)`</div>

<div class="description">

Since Chrome 25.

Sets the interval, in seconds, used to determine when the system is in an idle state for onStateChanged events. The default interval is 60 seconds.

| Parameters |
|---|
| integer | intervalInSeconds | 

Threshold, in seconds, used to determine when the system is in an idle state.
 |

</div>

</div>

## Events

<div>

### onStateChanged

<div class="description">

Fired when the system changes to an active, idle or locked state. The event fires with "locked" if the screen is locked or the screensaver activates, "idle" if the system is unlocked and the user has not generated any input for a specified number of seconds, and "active" when the user generates input on an idle system.

<div>

#### addListener

<div class="summary">`whale.idle.onStateChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [IdleState](/extensions/idle#type-IdleState) newState) <span class="subdued">{...}</span>;`

| [IdleState](/extensions/idle#type-IdleState) | newState |  |
|---|---|---|
 |

</div>

</div>

</div>

</div>

</div>

</section>