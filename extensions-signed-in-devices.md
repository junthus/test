# whale.signedInDevices

| Description: | Use the `whale.signedInDevices` API to get a list of devices signed into chrome with the same account as the current profile. |
|---|---|
| Availability: | **Dev** channel only. [Learn more](api_index#dev_apis). |
| Permissions: | <span class="code">"signedInDevices"</span> |

<section id="toc">

## Summary

| Types |
|---|
| [DeviceInfo](#type-DeviceInfo) |
| Methods |
| [get](#method-get) − `whale.signedInDevices.get(<span class="optional">boolean isLocal</span>, <span>function callback</span>)` |
| Events |
| [onDeviceInfoChange](#event-onDeviceInfoChange) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### DeviceInfo

| properties |
|---|
| string | name | 

Name of the device. This name is usually set by the user when setting up a device.
 |
| string | id | 

Unique Id for this device. Note: The id is meaningful only in the current device. This id cannot be used to refer to the same device from another device or extension.
 |
| enum of `"win"`, `"mac"`, `"linux"`, `"chrome_os"`, `"android"`, `"ios"`, or `"unknown"` | os | 

The OS of the device.
 |
| enum of `"desktop_or_laptop"`, `"phone"`, `"tablet"`, or `"unknown"` | type | 

Device Type.
 |
| string | chromeVersion | 

Version of chrome running in this device.
 |

</div>

## Methods

<div>

### get

<div class="summary">`whale.signedInDevices.get(<span class="optional">boolean isLocal</span>, <span>function callback</span>)`</div>

<div class="description">

Gets the array of signed in devices, signed into the same account as the current profile.

| Parameters |
|---|
| boolean | <span class="optional">(optional)</span> isLocal | 

If true only return the information for the local device. If false or omitted return the list of all devices including the local device.
 |
| function | callback | 

The callback to be invoked with the array of DeviceInfo objects.

The _callback_ parameter should be a function that looks like this:

`function(array of [DeviceInfo](/extensions/signedInDevices#type-DeviceInfo) devices) <span class="subdued">{...}</span>;`

| array of [DeviceInfo](/extensions/signedInDevices#type-DeviceInfo) | devices |  |
|---|---|---|
 |

</div>

</div>

## Events

<div>

### onDeviceInfoChange

<div class="description">

Fired when the DeviceInfo object of any of the signed in devices changes, or when a device is added or removed.

<div>

#### addListener

<div class="summary">`whale.signedInDevices.onDeviceInfoChange.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [DeviceInfo](/extensions/signedInDevices#type-DeviceInfo) devices) <span class="subdued">{...}</span>;`

| array of [DeviceInfo](/extensions/signedInDevices#type-DeviceInfo) | devices | 
|---|---|

The array of all signed in devices.
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>