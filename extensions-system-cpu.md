# whale.system.cpu

| Description: | Use the `system.cpu` API to query CPU metadata. |
|---|---|
| Availability: | Since Chrome 32. |
| Permissions: | <span class="code">"system.cpu"</span> |

## Summary

| Methods |
|---|
| [getInfo](#method-getInfo) − `whale.system.cpu.getInfo(function callback)` |

## Methods

### getInfo

<div class="summary">`whale.system.cpu.getInfo(<span>function callback</span>)`</div>

<div class="description">

Queries basic CPU information of the system.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object info) <span class="subdued">{...}</span>;`

| object | info | 
|---|---|

| integer | numOfProcessors | 
|---|---|

The number of logical processors.
 |
| string | archName | 

The architecture name of the processors.
 |
| string | modelName | 

The model name of the processors.
 |
| array of string | features | 

A set of feature codes indicating some of the processor's capabilities. The currently supported codes are "mmx", "sse", "sse2", "sse3", "ssse3", "sse4_1", "sse4_2", and "avx".
 |
| array of object | processors | 

Information about each logical processor.

#### Properties of each object

<dl>

<div>

| object | usage | 
|---|---|

Cumulative usage info for this logical processor.

| double | user | 
|---|---|

The cumulative time used by userspace programs on this processor.
 |
| double | kernel | 

The cumulative time used by kernel programs on this processor.
 |
| double | idle | 

The cumulative time spent idle by this processor.
 |
| double | total | 

The total cumulative time for this processor. This value is equal to user + kernel + idle.
 |
 |

</div>

</dl>
 |
| array of double | temperatures | 

List of CPU temperature readings from each thermal zone of the CPU. Temperatures are in degrees Celsius.

**Currently supported on Chrome OS only.**
 |
 |
 |

</div>

</div>

</div>

</section>
