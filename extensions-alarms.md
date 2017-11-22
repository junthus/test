# whale.alarms

| Description: | Use the `whale.alarms` API to schedule code to run periodically or at a specified time in the future. |
|---|---|
| Availability: | Since Chrome 22. |
| Permissions: | <span class="code">"alarms"</span> |
| Learn More: | [Event Pages](event_pages) |

<section id="toc">

## Summary

| Types |
|---|
| [Alarm](#type-Alarm) |
| Methods |
| [create](#method-create) − `whale.alarms.create(<span class="optional">string name</span>, <span>object alarmInfo</span>)` |
| [get](#method-get) − `whale.alarms.get(<span class="optional">string name</span>, <span>function callback</span>)` |
| [getAll](#method-getAll) − `whale.alarms.getAll(<span>function callback</span>)` |
| [clear](#method-clear) − `whale.alarms.clear(<span class="optional">string name</span>, <span class="optional">function callback</span>)` |
| [clearAll](#method-clearAll) − `whale.alarms.clearAll(<span class="optional">function callback</span>)` |
| Events |
| [onAlarm](#event-onAlarm) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### Alarm

| properties |
|---|
| string | name | 

Name of this alarm.
 |
| double | scheduledTime | 

Time at which this alarm was scheduled to fire, in milliseconds past the epoch (e.g. `Date.now() + n`). For performance reasons, the alarm may have been delayed an arbitrary amount beyond this.
 |
| double | <span class="optional">(optional)</span> periodInMinutes | 

If not null, the alarm is a repeating alarm and will fire again in <var>periodInMinutes</var> minutes.
 |

</div>

## Methods

<div>

### create

<div class="summary">`whale.alarms.create(<span class="optional">string name</span>, <span>object alarmInfo</span>)`</div>

<div class="description">

Creates an alarm. Near the time(s) specified by <var>alarmInfo</var>, the `onAlarm` event is fired. If there is another alarm with the same name (or no name if none is specified), it will be cancelled and replaced by this alarm.

In order to reduce the load on the user's machine, Chrome limits alarms to at most once every 1 minute but may delay them an arbitrary amount more. That is, setting `delayInMinutes` or `periodInMinutes` to less than `1` will not be honored and will cause a warning. `when` can be set to less than 1 minute after "now" without warning but won't actually cause the alarm to fire for at least 1 minute.

To help you debug your app or extension, when you've loaded it unpacked, there's no limit to how often the alarm can fire.

| Parameters |
|---|
| string | <span class="optional">(optional)</span> name | 

Optional name to identify this alarm. Defaults to the empty string.
 |
| object | alarmInfo | 

Describes when the alarm should fire. The initial time must be specified by either <var>when</var> or <var>delayInMinutes</var> (but not both). If <var>periodInMinutes</var> is set, the alarm will repeat every <var>periodInMinutes</var> minutes after the initial event. If neither <var>when</var> or <var>delayInMinutes</var> is set for a repeating alarm, <var>periodInMinutes</var> is used as the default for <var>delayInMinutes</var>.

| double | <span class="optional">(optional)</span> when | 
|---|---|

Time at which the alarm should fire, in milliseconds past the epoch (e.g. `Date.now() + n`).
 |
| double | <span class="optional">(optional)</span> delayInMinutes | 

Length of time in minutes after which the `onAlarm` event should fire.
 |
| double | <span class="optional">(optional)</span> periodInMinutes | 

If set, the onAlarm event should fire every <var>periodInMinutes</var> minutes after the initial event specified by <var>when</var> or <var>delayInMinutes</var>. If not set, the alarm will only fire once.
 |
 |

</div>

</div>

<div>

### get

<div class="summary">`whale.alarms.get(<span class="optional">string name</span>, <span>function callback</span>)`</div>

<div class="description">

Retrieves details about the specified alarm.

| Parameters |
|---|
| string | <span class="optional">(optional)</span> name | 

The name of the alarm to get. Defaults to the empty string.
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Alarm](/extensions/alarms#type-Alarm) alarm) <span class="subdued">{...}</span>;`

| [Alarm](/extensions/alarms#type-Alarm) | <span class="optional">(optional)</span> alarm |  |
|---|---|---|
 |

</div>

</div>

<div>

### getAll

<div class="summary">`whale.alarms.getAll(<span>function callback</span>)`</div>

<div class="description">

Gets an array of all the alarms.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [Alarm](/extensions/alarms#type-Alarm) alarms) <span class="subdued">{...}</span>;`

| array of [Alarm](/extensions/alarms#type-Alarm) | alarms |  |
|---|---|---|
 |

</div>

</div>

<div>

### clear

<div class="summary">`whale.alarms.clear(<span class="optional">string name</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears the alarm with the given name.

| Parameters |
|---|
| string | <span class="optional">(optional)</span> name | 

The name of the alarm to clear. Defaults to the empty string.
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(boolean wasCleared) <span class="subdued">{...}</span>;`

| boolean | wasCleared |  |
|---|---|---|
 |

</div>

</div>

<div>

### clearAll

<div class="summary">`whale.alarms.clearAll(<span class="optional">function callback</span>)`</div>

<div class="description">

Clears all alarms.

| Parameters |
|---|
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(boolean wasCleared) <span class="subdued">{...}</span>;`

| boolean | wasCleared |  |
|---|---|---|
 |

</div>

</div>

## Events

<div>

### onAlarm

<div class="description">

Fired when an alarm has elapsed. Useful for event pages.

<div>

#### addListener

<div class="summary">`whale.alarms.onAlarm.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Alarm](/extensions/alarms#type-Alarm) alarm) <span class="subdued">{...}</span>;`

| [Alarm](/extensions/alarms#type-Alarm) | alarm | 
|---|---|

The alarm that has elapsed.
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>