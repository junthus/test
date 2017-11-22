# whale.system.memory

| Description: | The `whale.system.memory` API. |
|---|---|
| Availability: | Since Chrome 32. |
| Permissions: | <span class="code">"system.memory"</span> |

<section id="toc">

## Summary

| Methods |
|---|
| [getInfo](#method-getInfo) − `whale.system.memory.getInfo(<span>function callback</span>)` |

</section>

<section>

<div class="api-reference">

## Methods

<div>

### getInfo

<div class="summary">`whale.system.memory.getInfo(<span>function callback</span>)`</div>

<div class="description">

Get physical memory information.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object info) <span class="subdued">{...}</span>;`

| object | info | 
|---|---|

| double | capacity | 
|---|---|

The total amount of physical memory capacity, in bytes.
 |
| double | availableCapacity | 

The amount of available capacity, in bytes.
 |
 |
 |

</div>

</div>

</div>

</section>