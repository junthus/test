# whale.power

| Description: | Use the `whale.power` API to override the system's power management features. |
|---|---|
| Availability: | Since Chrome 27. |
| Permissions: | <span class="code">"power"</span> |

<section>

## Usage

By default, operating systems dim the screen when users are inactive and eventually suspend the system. With the power API, an app or extension can keep the system awake.

Using this API, you can specify the [Level](/extensions/power#type-Level) to which power management is disabled. The `"system"` level keeps the system active, but allows the screen to be dimmed or turned off. For example, a communication app can continue to receive messages while the screen is off. The `"display"` level keeps the screen and system active. E-book and presentation apps, for example, can keep the screen and system active while users read.

When a user has more than one app or extension active, each with its own power level, the highest-precedence level takes effect; `"display"` always takes precedence over `"system"`. For example, if app A asks for `"system"` power management, and app B asks for `"display"`, `"display"` is used until app B is unloaded or releases its request. If app A is still active, `"system"` is then used.

</section>

<section id="toc">

## Summary

| Types |
|---|
| [Level](#type-Level) |
| Methods |
| [requestKeepAwake](#method-requestKeepAwake) − `whale.power.requestKeepAwake( <span>Level level</span>)` |
| [releaseKeepAwake](#method-releaseKeepAwake) − `whale.power.releaseKeepAwake()` |

</section>

<section>

<div class="api-reference">

## Types

<div>

### Level

| Enum |
|---|
| 

<dl>

<dt>`"system"`</dt>

<dd>Prevent the system from sleeping in response to user inactivity.</dd>

</dl>

<dl>

<dt>`"display"`</dt>

<dd>Prevent the display from being turned off or dimmed or the system from sleeping in response to user inactivity.</dd>

</dl>
 |

</div>

## Methods

<div>

### requestKeepAwake

<div class="summary">`whale.power.requestKeepAwake( <span>[Level](/extensions/power#type-Level) level</span>)`</div>

<div class="description">

Requests that power management be temporarily disabled. |level| describes the degree to which power management should be disabled. If a request previously made by the same app is still active, it will be replaced by the new request.

| Parameters |
|---|
| [Level](/extensions/power#type-Level) | level |  |

</div>

</div>

<div>

### releaseKeepAwake

<div class="summary">`whale.power.releaseKeepAwake()`</div>

<div class="description">

Releases a request previously made via requestKeepAwake().

</div>

</div>

</div>

</section>