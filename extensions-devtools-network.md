# whale.devtools.network

| Description: | Use the `whale.devtools.network` API to retrieve the information about network requests displayed by the Developer Tools in the Network panel. |
|---|---|
| Availability: | Since Chrome 21. |

<section>

See [DevTools APIs summary](devtools) for general introduction to using Developer Tools APIs.

## Overview

Network requests information is represented in the HTTP Archive format (_HAR_). The description of HAR is outside of scope of this document, please refer to [HAR v1.2 Specification](http://www.softwareishard.com/blog/har-12-spec/).

In terms of HAR, the `whale.devtools.network.getHAR()` method returns entire _HAR log_, while `whale.devtools.network.onRequestFinished` event provides _HAR entry_ as an argument to the event callback.

Note that request content is not provided as part of HAR for efficieny reasons. You may call request's `getContent()` method to retrieve content.

If the Developer Tools window is opened after the page is loaded, some requests may be missing in the array of entries returned by `getHAR()`. Reload the page to get all requests. In general, the list of requests returned by `getHAR()` should match that displayed in the Network panel.

## Examples

The following code logs URLs of all images larger than 40KB as they are loaded:

<pre>      whale.devtools.network.onRequestFinished.addListener(
          function(request) {
            if (request.response.bodySize > 40*1024) {
              whale.devtools.inspectedWindow.eval(
                  'console.log("Large image: " + unescape("' +
                  escape(request.request.url) + '"))');
            }
      });
      </pre>

You can find more examples that use this API in [Samples](samples#search:devtools.network).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [Request](#type-Request) |
| Methods |
| [getHAR](#method-getHAR) − `whale.devtools.network.getHAR(<span>function callback</span>)` |
| Events |
| [onRequestFinished](#event-onRequestFinished) |
| [onNavigated](#event-onNavigated) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### Request

<dd>Represents a network request for a document resource (script, image and so on). See HAR Specification for reference.</dd>

| methods |
|---|
| 

<div>

#### getContent

<div class="summary">`Request.getContent(<span>function callback</span>)`</div>

<div class="description">

Returns content of the response body.

| Parameters |
|---|
| function | callback | 

A function that receives the response body when the request completes.

The _callback_ parameter should be a function that looks like this:

`function(string content, string encoding) <span class="subdued">{...}</span>;`

| string | content | 
|---|---|

Content of the response body (potentially encoded).
 |
| string | encoding | 

Empty if content is not encoded, encoding name otherwise. Currently, only base64 is supported.
 |
 |

</div>

</div>
 |

</div>

## Methods

<div>

### getHAR

<div class="summary">`whale.devtools.network.getHAR(<span>function callback</span>)`</div>

<div class="description">

Returns HAR log that contains all known network requests.

| Parameters |
|---|
| function | callback | 

A function that receives the HAR log when the request completes.

The _callback_ parameter should be a function that looks like this:

`function(object harLog) <span class="subdued">{...}</span>;`

| object | harLog | 
|---|---|

A HAR log. See HAR specification for details.
 |
 |

</div>

</div>

## Events

<div>

### onRequestFinished

<div class="description">

Fired when a network request is finished and all request data are available.

<div>

#### addListener

<div class="summary">`whale.devtools.network.onRequestFinished.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Request](/extensions/devtools.network#type-Request) request) <span class="subdued">{...}</span>;`

| [Request](/extensions/devtools.network#type-Request) | request | 
|---|---|

Description of a network request in the form of a HAR entry. See HAR specification for details.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onNavigated

<div class="description">

Fired when the inspected window navigates to a new page.

<div>

#### addListener

<div class="summary">`whale.devtools.network.onNavigated.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string url) <span class="subdued">{...}</span>;`

| string | url | 
|---|---|

URL of the new page.
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>