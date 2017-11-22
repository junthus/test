# whale.system.storage

| Description: | Use the `whale.system.storage` API to query storage device information and be notified when a removable storage device is attached and detached. |
|---|---|
| Availability: | Since Chrome 30. |
| Permissions: | <span class="code">"system.storage"</span> |

<section id="toc">

## Summary

| Types |
|---|
| [StorageUnitInfo](#type-StorageUnitInfo) |
| Methods |
| [getInfo](#method-getInfo) − `whale.system.storage.getInfo(<span>function callback</span>)` |
| [ejectDevice](#method-ejectDevice) − `whale.system.storage.ejectDevice(<span>string id</span>, <span>function callback</span>)` |
| [getAvailableCapacity](#method-getAvailableCapacity) − `whale.system.storage.getAvailableCapacity(<span>string id</span>, <span>function callback</span>)` |
| Events |
| [onAttached](#event-onAttached) |
| [onDetached](#event-onDetached) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### StorageUnitInfo

| properties |
|---|
| string | id | 

The transient ID that uniquely identifies the storage device. This ID will be persistent within the same run of a single application. It will not be a persistent identifier between different runs of an application, or between different applications.
 |
| string | name | 

The name of the storage unit.
 |
| enum of `"fixed"`, `"removable"`, or `"unknown"` | type | 

The media type of the storage unit.

<dl>

<dt>fixed</dt>

<dd>The storage has fixed media, e.g. hard disk or SSD.</dd>

</dl>

<dl>

<dt>removable</dt>

<dd>The storage is removable, e.g. USB flash drive.</dd>

</dl>

<dl>

<dt>unknown</dt>

<dd>The storage type is unknown.</dd>

</dl>
 |
| double | capacity | 

The total amount of the storage space, in bytes.
 |

</div>

## Methods

<div>

### getInfo

<div class="summary">`whale.system.storage.getInfo(<span>function callback</span>)`</div>

<div class="description">

Get the storage information from the system. The argument passed to the callback is an array of StorageUnitInfo objects.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [StorageUnitInfo](/extensions/system.storage#type-StorageUnitInfo) info) <span class="subdued">{...}</span>;`

| array of [StorageUnitInfo](/extensions/system.storage#type-StorageUnitInfo) | info |  |
|---|---|---|
 |

</div>

</div>

<div>

### ejectDevice

<div class="summary">`whale.system.storage.ejectDevice(<span>string id</span>, <span>function callback</span>)`</div>

<div class="description">

Ejects a removable storage device.

| Parameters |
|---|
| string | id |  |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(enum of `"success"`, `"in_use"`, `"no_such_device"`, or `"failure"` result) <span class="subdued">{...}</span>;`

| enum of `"success"`, `"in_use"`, `"no_such_device"`, or `"failure"` | result | 
|---|---|

<dl>

<dt>success</dt>

<dd>The ejection command is successful -- the application can prompt the user to remove the device.</dd>

</dl>

<dl>

<dt>in_use</dt>

<dd>The device is in use by another application. The ejection did not succeed; the user should not remove the device until the other application is done with the device.</dd>

</dl>

<dl>

<dt>no_such_device</dt>

<dd>There is no such device known.</dd>

</dl>

<dl>

<dt>failure</dt>

<dd>The ejection command failed.</dd>

</dl>
 |
 |

</div>

</div>

<div>

### getAvailableCapacity

<div class="summary">`whale.system.storage.getAvailableCapacity(<span>string id</span>, <span>function callback</span>)`</div>

<div class="description">

**Dev** channel only. [Learn more](api_index#dev_apis).

Get the available capacity of a specified |id| storage device. The |id| is the transient device ID from StorageUnitInfo.

| Parameters |
|---|
| string | id | 

Since Chrome 32.
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object info) <span class="subdued">{...}</span>;`

| object | info | 
|---|---|

| string | id | 
|---|---|

A copied |id| of getAvailableCapacity function parameter |id|.
 |
| double | availableCapacity | 

The available capacity of the storage device, in bytes.
 |
 |
 |

</div>

</div>

## Events

<div>

### onAttached

<div class="description">

Fired when a new removable storage is attached to the system.

<div>

#### addListener

<div class="summary">`whale.system.storage.onAttached.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [StorageUnitInfo](/extensions/system.storage#type-StorageUnitInfo) info) <span class="subdued">{...}</span>;`

| [StorageUnitInfo](/extensions/system.storage#type-StorageUnitInfo) | info |  |
|---|---|---|
 |

</div>

</div>

</div>

</div>

<div>

### onDetached

<div class="description">

Fired when a removable storage is detached from the system.

<div>

#### addListener

<div class="summary">`whale.system.storage.onDetached.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string id) <span class="subdued">{...}</span>;`

| string | id |  |
|---|---|---|
 |

</div>

</div>

</div>

</div>

</div>

</section>