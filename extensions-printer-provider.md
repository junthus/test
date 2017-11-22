# whale.printerProvider

| Description: | The `whale.printerProvider` API exposes events used by print manager to query printers controlled by extensions, to query their capabilities and to submit print jobs to these printers. |
|---|---|
| Availability: | Since Chrome 44. |
| Permissions: | <span class="code">"printerProvider"</span> |

<section id="toc">

## Summary

| Types |
|---|
| [PrinterInfo](#type-PrinterInfo) |
| Events |
| [onGetPrintersRequested](#event-onGetPrintersRequested) |
| [onGetUsbPrinterInfoRequested](#event-onGetUsbPrinterInfoRequested) |
| [onGetCapabilityRequested](#event-onGetCapabilityRequested) |
| [onPrintRequested](#event-onPrintRequested) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### PrinterInfo

| properties |
|---|
| string | id | 

Unique printer ID.
 |
| string | name | 

Printer's human readable name.
 |
| string | <span class="optional">(optional)</span> description | 

Printer's human readable description.
 |

</div>

## Events

<div>

### onGetPrintersRequested

<div class="description">

Event fired when print manager requests printers provided by extensions.

<div>

#### addListener

<div class="summary">`whale.printerProvider.onGetPrintersRequested.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(function resultCallback) <span class="subdued">{...}</span>;`

| function | resultCallback | 
|---|---|

Callback to return printer list. Every listener must call callback exactly once.

The _resultCallback_ parameter should be a function that looks like this:

`function(array of [PrinterInfo](/extensions/printerProvider#type-PrinterInfo) printerInfo) <span class="subdued">{...}</span>;`

| array of [PrinterInfo](/extensions/printerProvider#type-PrinterInfo) | printerInfo |  |
|---|---|---|
 |
 |

</div>

</div>

</div>

</div>

<div>

### onGetUsbPrinterInfoRequested

<div class="description">

Since Chrome 45.

Event fired when print manager requests information about a USB device that may be a printer.

_Note:_ An application should not rely on this event being fired more than once per device. If a connected device is supported it should be returned in the [onGetPrintersRequested](/extensions/printerProvider#event-onGetPrintersRequested) event.

<div>

#### addListener

<div class="summary">`whale.printerProvider.onGetUsbPrinterInfoRequested.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [usb.Device](/extensions/#type-Device) device, function resultCallback) <span class="subdued">{...}</span>;`

| [usb.Device](/extensions/#type-Device) | device | 
|---|---|

The USB device.
 |
| function | resultCallback | 

Callback to return printer info. The receiving listener must call callback exactly once. If the parameter to this callback is undefined that indicates that the application has determined that the device is not supported.

The _resultCallback_ parameter should be a function that looks like this:

`function( [PrinterInfo](/extensions/printerProvider#type-PrinterInfo) printerInfo) <span class="subdued">{...}</span>;`

| [PrinterInfo](/extensions/printerProvider#type-PrinterInfo) | <span class="optional">(optional)</span> printerInfo |  |
|---|---|---|
 |
 |

</div>

</div>

</div>

</div>

<div>

### onGetCapabilityRequested

<div class="description">

Event fired when print manager requests printer capabilities.

<div>

#### addListener

<div class="summary">`whale.printerProvider.onGetCapabilityRequested.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string printerId, function resultCallback) <span class="subdued">{...}</span>;`

| string | printerId | 
|---|---|

Unique ID of the printer whose capabilities are requested.
 |
| function | resultCallback | 

Callback to return device capabilities in [CDD format](https://developers.google.com/cloud-print/docs/cdd#cdd). The receiving listener must call callback exectly once.

The _resultCallback_ parameter should be a function that looks like this:

`function(object capabilities) <span class="subdued">{...}</span>;`

| object | capabilities | 
|---|---|

Device capabilities in [CDD format](https://developers.google.com/cloud-print/docs/cdd#cdd).
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onPrintRequested

<div class="description">

Event fired when print manager requests printing.

<div>

#### addListener

<div class="summary">`whale.printerProvider.onPrintRequested.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object printJob, function resultCallback) <span class="subdued">{...}</span>;`

| object | printJob | 
|---|---|

The printing request parameters.

| string | printerId | 
|---|---|

ID of the printer which should handle the job.
 |
| string | title | 

The print job title.
 |
| object | ticket | 

Print ticket in [CJT format](https://developers.google.com/cloud-print/docs/cdd#cjt).
 |
| string | contentType | 

The document content type. Supported formats are `"application/pdf"` and `"image/pwg-raster"`.
 |
| Blob | document | 

Blob containing the document data to print. Format must match |contentType|.
 |
 |
| function | resultCallback | 

Callback that should be called when the printing request is completed.

The _resultCallback_ parameter should be a function that looks like this:

`function(enum of `"OK"`, `"FAILED"`, `"INVALID_TICKET"`, or `"INVALID_DATA"` result) <span class="subdued">{...}</span>;`

| enum of `"OK"`, `"FAILED"`, `"INVALID_TICKET"`, or `"INVALID_DATA"` | result | 
|---|---|

<dl>

<dt>OK</dt>

<dd>Operation completed successfully.</dd>

</dl>

<dl>

<dt>FAILED</dt>

<dd>General failure.</dd>

</dl>

<dl>

<dt>INVALID_TICKET</dt>

<dd>Print ticket is invalid. For example, ticket is inconsistent with capabilities or extension is not able to handle all settings from the ticket.</dd>

</dl>

<dl>

<dt>INVALID_DATA</dt>

<dd>Document is invalid. For example, data may be corrupted or the format is incompatible with the extension.</dd>

</dl>
 |
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>