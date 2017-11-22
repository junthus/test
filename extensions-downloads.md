# whale.downloads

| Description: | Use the `whale.downloads` API to programmatically initiate, monitor, manipulate, and search for downloads. |
|---|---|
| Availability: | Since Chrome 31. |
| Permissions: | <span class="code">"downloads"</span> |

<section>

## Manifest

You must declare the `"downloads"` permission in the [extension manifest](manifest) to use this API.

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"permissions": [
          "downloads"
        ]**,
        ...
      }
      </pre>

## Examples

You can find simple examples of using the `whale.downloads` API in the [examples/api/downloads](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/docs/examples/api/downloads/) directory. For other examples and for help in viewing the source code, see [Samples](samples).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [FilenameConflictAction](#type-FilenameConflictAction) |
| [InterruptReason](#type-InterruptReason) |
| [DangerType](#type-DangerType) |
| [State](#type-State) |
| [DownloadItem](#type-DownloadItem) |
| [StringDelta](#type-StringDelta) |
| [DoubleDelta](#type-DoubleDelta) |
| [BooleanDelta](#type-BooleanDelta) |
| Methods |
| [download](#method-download) − `whale.downloads.download(<span>object options</span>, <span class="optional">function callback</span>)` |
| [search](#method-search) − `whale.downloads.search(<span>object query</span>, <span>function callback</span>)` |
| [pause](#method-pause) − `whale.downloads.pause(<span>integer downloadId</span>, <span class="optional">function callback</span>)` |
| [resume](#method-resume) − `whale.downloads.resume(<span>integer downloadId</span>, <span class="optional">function callback</span>)` |
| [cancel](#method-cancel) − `whale.downloads.cancel(<span>integer downloadId</span>, <span class="optional">function callback</span>)` |
| [getFileIcon](#method-getFileIcon) − `whale.downloads.getFileIcon(<span>integer downloadId</span>, <span class="optional">object options</span>, <span>function callback</span>)` |
| [open](#method-open) − `whale.downloads.open(<span>integer downloadId</span>)` |
| [show](#method-show) − `whale.downloads.show(<span>integer downloadId</span>)` |
| [showDefaultFolder](#method-showDefaultFolder) − `whale.downloads.showDefaultFolder()` |
| [erase](#method-erase) − `whale.downloads.erase(<span>object query</span>, <span class="optional">function callback</span>)` |
| [removeFile](#method-removeFile) − `whale.downloads.removeFile(<span>integer downloadId</span>, <span class="optional">function callback</span>)` |
| [acceptDanger](#method-acceptDanger) − `whale.downloads.acceptDanger(<span>integer downloadId</span>, <span class="optional">function callback</span>)` |
| [drag](#method-drag) − `whale.downloads.drag(<span>integer downloadId</span>)` |
| [setShelfEnabled](#method-setShelfEnabled) − `whale.downloads.setShelfEnabled(<span>boolean enabled</span>)` |
| Events |
| [onCreated](#event-onCreated) |
| [onErased](#event-onErased) |
| [onChanged](#event-onChanged) |
| [onDeterminingFilename](#event-onDeterminingFilename) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### FilenameConflictAction

<dd>

<dl>

<dt>uniquify</dt>

<dd>To avoid duplication, the `filename` is changed to include a counter before the filename extension.</dd>

<dt>overwrite</dt>

<dd>The existing file will be overwritten with the new file.</dd>

<dt>prompt</dt>

<dd>The user will be prompted with a file chooser dialog.</dd>

</dl>

</dd>

| Enum |
|---|
| `"uniquify"`, `"overwrite"`, or `"prompt"` |

</div>

<div>

### InterruptReason

| Enum |
|---|
| `"FILE_FAILED"`, `"FILE_ACCESS_DENIED"`, `"FILE_NO_SPACE"`, `"FILE_NAME_TOO_LONG"`, `"FILE_TOO_LARGE"`, `"FILE_VIRUS_INFECTED"`, `"FILE_TRANSIENT_ERROR"`, `"FILE_BLOCKED"`, `"FILE_SECURITY_CHECK_FAILED"`, `"FILE_TOO_SHORT"`, `"FILE_HASH_MISMATCH"`, `"FILE_SAME_AS_SOURCE"`, `"NETWORK_FAILED"`, `"NETWORK_TIMEOUT"`, `"NETWORK_DISCONNECTED"`, `"NETWORK_SERVER_DOWN"`, `"NETWORK_INVALID_REQUEST"`, `"SERVER_FAILED"`, `"SERVER_NO_RANGE"`, `"SERVER_BAD_CONTENT"`, `"SERVER_UNAUTHORIZED"`, `"SERVER_CERT_PROBLEM"`, `"SERVER_FORBIDDEN"`, `"SERVER_UNREACHABLE"`, `"SERVER_CONTENT_LENGTH_MISMATCH"`, `"USER_CANCELED"`, `"USER_SHUTDOWN"`, or `"CRASH"` |

</div>

<div>

### DangerType

<dd>

<dl>

<dt>file</dt>

<dd>The download's filename is suspicious.</dd>

<dt>url</dt>

<dd>The download's URL is known to be malicious.</dd>

<dt>content</dt>

<dd>The downloaded file is known to be malicious.</dd>

<dt>uncommon</dt>

<dd>The download's URL is not commonly downloaded and could be dangerous.</dd>

<dt>host</dt>

<dd>The download came from a host known to distribute malicious binaries and is likely dangerous.</dd>

<dt>unwanted</dt>

<dd>The download is potentially unwanted or unsafe. E.g. it could make changes to browser or computer settings.</dd>

<dt>safe</dt>

<dd>The download presents no known danger to the user's computer.</dd>

<dt>accepted</dt>

<dd>The user has accepted the dangerous download.</dd>

</dl>

</dd>

| Enum |
|---|
| `"file"`, `"url"`, `"content"`, `"uncommon"`, `"host"`, `"unwanted"`, `"safe"`, or `"accepted"` |

</div>

<div>

### State

<dd>

<dl>

<dt>in_progress</dt>

<dd>The download is currently receiving data from the server.</dd>

<dt>interrupted</dt>

<dd>An error broke the connection with the file host.</dd>

<dt>complete</dt>

<dd>The download completed successfully.</dd>

</dl>

</dd>

| Enum |
|---|
| `"in_progress"`, `"interrupted"`, or `"complete"` |

</div>

<div>

### DownloadItem

| properties |
|---|
| integer | id | 

An identifier that is persistent across browser sessions.
 |
| string | url | 

The absolute URL that this download initiated from, before any redirects.
 |
| string | finalUrl | 

Since Chrome 54.

The absolute URL that this download is being made from, after all redirects.
 |
| string | referrer | 

Absolute URL.
 |
| string | filename | 

Absolute local path.
 |
| boolean | incognito | 

False if this download is recorded in the history, true if it is not recorded.
 |
| [DangerType](/extensions/downloads#type-DangerType) | danger | 

Indication of whether this download is thought to be safe or known to be suspicious.
 |
| string | mime | 

The file's MIME type.
 |
| string | startTime | 

The time when the download began in ISO 8601 format. May be passed directly to the Date constructor: `whale.downloads.search({}, function(items){items.forEach(function(item){console.log(new Date(item.startTime))})})`
 |
| string | <span class="optional">(optional)</span> endTime | 

The time when the download ended in ISO 8601 format. May be passed directly to the Date constructor: `whale.downloads.search({}, function(items){items.forEach(function(item){if (item.endTime) console.log(new Date(item.endTime))})})`
 |
| string | <span class="optional">(optional)</span> estimatedEndTime | 

Estimated time when the download will complete in ISO 8601 format. May be passed directly to the Date constructor: `whale.downloads.search({}, function(items){items.forEach(function(item){if (item.estimatedEndTime) console.log(new Date(item.estimatedEndTime))})})`
 |
| [State](/extensions/downloads#type-State) | state | 

Indicates whether the download is progressing, interrupted, or complete.
 |
| boolean | paused | 

True if the download has stopped reading data from the host, but kept the connection open.
 |
| boolean | canResume | 

True if the download is in progress and paused, or else if it is interrupted and can be resumed starting from where it was interrupted.
 |
| [InterruptReason](/extensions/downloads#type-InterruptReason) | <span class="optional">(optional)</span> error | 

Why the download was interrupted. Several kinds of HTTP errors may be grouped under one of the errors beginning with `SERVER_`. Errors relating to the network begin with `NETWORK_`, errors relating to the process of writing the file to the file system begin with `FILE_`, and interruptions initiated by the user begin with `USER_`.
 |
| double | bytesReceived | 

Number of bytes received so far from the host, without considering file compression.
 |
| double | totalBytes | 

Number of bytes in the whole file, without considering file compression, or -1 if unknown.
 |
| double | fileSize | 

Number of bytes in the whole file post-decompression, or -1 if unknown.
 |
| boolean | exists | 

Whether the downloaded file still exists. This information may be out of date because Chrome does not automatically watch for file removal. Call [search](/extensions/downloads#method-search)() in order to trigger the check for file existence. When the existence check completes, if the file has been deleted, then an [onChanged](/extensions/downloads#event-onChanged) event will fire. Note that [search](/extensions/downloads#method-search)() does not wait for the existence check to finish before returning, so results from [search](/extensions/downloads#method-search)() may not accurately reflect the file system. Also, [search](/extensions/downloads#method-search)() may be called as often as necessary, but will not check for file existence any more frequently than once every 10 seconds.
 |
| string | <span class="optional">(optional)</span> byExtensionId | 

The identifier for the extension that initiated this download if this download was initiated by an extension. Does not change once it is set.
 |
| string | <span class="optional">(optional)</span> byExtensionName | 

The localized name of the extension that initiated this download if this download was initiated by an extension. May change if the extension changes its name or if the user changes their locale.
 |

</div>

<div>

### StringDelta

| properties |
|---|
| string | <span class="optional">(optional)</span> previous |  |
| string | <span class="optional">(optional)</span> current |  |

</div>

<div>

### DoubleDelta

Since Chrome 34.

| properties |
|---|
| double | <span class="optional">(optional)</span> previous |  |
| double | <span class="optional">(optional)</span> current |  |

</div>

<div>

### BooleanDelta

| properties |
|---|
| boolean | <span class="optional">(optional)</span> previous |  |
| boolean | <span class="optional">(optional)</span> current |  |

</div>

## Methods

<div>

### download

<div class="summary">`whale.downloads.download(<span>object options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Download a URL. If the URL uses the HTTP[S] protocol, then the request will include all cookies currently set for its hostname. If both `filename` and `saveAs` are specified, then the Save As dialog will be displayed, pre-populated with the specified `filename`. If the download started successfully, `callback` will be called with the new [DownloadItem](/extensions/downloads#type-DownloadItem)'s `downloadId`. If there was an error starting the download, then `callback` will be called with `downloadId=undefined` and [runtime.lastError](/extensions/runtime#property-lastError) will contain a descriptive string. The error strings are not guaranteed to remain backwards compatible between releases. Extensions must not parse it.

| Parameters |
|---|
| object | options | 

What to download and how.

| string | url | 
|---|---|

The URL to download.
 |
| string | <span class="optional">(optional)</span> filename | 

A file path relative to the Downloads directory to contain the downloaded file, possibly containing subdirectories. Absolute paths, empty paths, and paths containing back-references ".." will cause an error. [onDeterminingFilename](/extensions/downloads#event-onDeterminingFilename) allows suggesting a filename after the file's MIME type and a tentative filename have been determined.
 |
| [FilenameConflictAction](/extensions/downloads#type-FilenameConflictAction) | <span class="optional">(optional)</span> conflictAction | 

The action to take if `filename` already exists.
 |
| boolean | <span class="optional">(optional)</span> saveAs | 

Use a file-chooser to allow the user to select a filename regardless of whether `filename` is set or already exists.
 |
| enum of `"GET"`, or `"POST"` | <span class="optional">(optional)</span> method | 

The HTTP method to use if the URL uses the HTTP[S] protocol.
 |
| array of object | <span class="optional">(optional)</span> headers | 

Extra HTTP headers to send with the request if the URL uses the HTTP[s] protocol. Each header is represented as a dictionary containing the keys `name` and either `value` or `binaryValue`, restricted to those allowed by XMLHttpRequest.

#### Properties of each object

<dl>

<div>

| string | name | 
|---|---|

Name of the HTTP header.
 |
| string | value | 

Value of the HTTP header.
 |

</div>

</dl>
 |
| string | <span class="optional">(optional)</span> body | 

Post body.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called with the id of the new [DownloadItem](/extensions/downloads#type-DownloadItem).

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(integer downloadId) <span class="subdued">{...}</span>;`

| integer | downloadId |  |
|---|---|---|
 |

</div>

</div>

<div>

### search

<div class="summary">`whale.downloads.search(<span>object query</span>, <span>function callback</span>)`</div>

<div class="description">

Find [DownloadItem](/extensions/downloads#type-DownloadItem). Set `query` to the empty object to get all [DownloadItem](/extensions/downloads#type-DownloadItem). To get a specific [DownloadItem](/extensions/downloads#type-DownloadItem), set only the `id` field. To page through a large number of items, set `orderBy: ['-startTime']`, set `limit` to the number of items per page, and set `startedAfter` to the `startTime` of the last item from the last page.

| Parameters |
|---|
| object | query | 

| array of string | <span class="optional">(optional)</span> query | 
|---|---|

This array of search terms limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) whose `filename` or `url` or `finalUrl` contain all of the search terms that do not begin with a dash '-' and none of the search terms that do begin with a dash.
 |
| string | <span class="optional">(optional)</span> startedBefore | 

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) that started before the given ms since the epoch.
 |
| string | <span class="optional">(optional)</span> startedAfter | 

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) that started after the given ms since the epoch.
 |
| string | <span class="optional">(optional)</span> endedBefore | 

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) that ended before the given ms since the epoch.
 |
| string | <span class="optional">(optional)</span> endedAfter | 

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) that ended after the given ms since the epoch.
 |
| double | <span class="optional">(optional)</span> totalBytesGreater | 

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) whose `totalBytes` is greater than the given integer.
 |
| double | <span class="optional">(optional)</span> totalBytesLess | 

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) whose `totalBytes` is less than the given integer.
 |
| string | <span class="optional">(optional)</span> filenameRegex | 

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) whose `filename` matches the given regular expression.
 |
| string | <span class="optional">(optional)</span> urlRegex | 

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) whose `url` matches the given regular expression.
 |
| string | <span class="optional">(optional)</span> finalUrlRegex | 

Since Chrome 54.

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) whose `finalUrl` matches the given regular expression.
 |
| integer | <span class="optional">(optional)</span> limit | 

The maximum number of matching [DownloadItem](/extensions/downloads#type-DownloadItem) returned. Defaults to 1000\. Set to 0 in order to return all matching [DownloadItem](/extensions/downloads#type-DownloadItem). See [search](/extensions/downloads#method-search) for how to page through results.
 |
| array of string | <span class="optional">(optional)</span> orderBy | 

Set elements of this array to [DownloadItem](/extensions/downloads#type-DownloadItem) properties in order to sort search results. For example, setting `orderBy=['startTime']` sorts the [DownloadItem](/extensions/downloads#type-DownloadItem) by their start time in ascending order. To specify descending order, prefix with a hyphen: '-startTime'.
 |
| integer | <span class="optional">(optional)</span> id | 

The `id` of the [DownloadItem](/extensions/downloads#type-DownloadItem) to query.
 |
| string | <span class="optional">(optional)</span> url | 

The absolute URL that this download initiated from, before any redirects.
 |
| string | <span class="optional">(optional)</span> finalUrl | 

Since Chrome 54.

The absolute URL that this download is being made from, after all redirects.
 |
| string | <span class="optional">(optional)</span> filename | 

Absolute local path.
 |
| [DangerType](/extensions/downloads#type-DangerType) | <span class="optional">(optional)</span> danger | 

Indication of whether this download is thought to be safe or known to be suspicious.
 |
| string | <span class="optional">(optional)</span> mime | 

The file's MIME type.
 |
| string | <span class="optional">(optional)</span> startTime | 

The time when the download began in ISO 8601 format.
 |
| string | <span class="optional">(optional)</span> endTime | 

The time when the download ended in ISO 8601 format.
 |
| [State](/extensions/downloads#type-State) | <span class="optional">(optional)</span> state | 

Indicates whether the download is progressing, interrupted, or complete.
 |
| boolean | <span class="optional">(optional)</span> paused | 

True if the download has stopped reading data from the host, but kept the connection open.
 |
| [InterruptReason](/extensions/downloads#type-InterruptReason) | <span class="optional">(optional)</span> error | 

Why a download was interrupted.
 |
| double | <span class="optional">(optional)</span> bytesReceived | 

Number of bytes received so far from the host, without considering file compression.
 |
| double | <span class="optional">(optional)</span> totalBytes | 

Number of bytes in the whole file, without considering file compression, or -1 if unknown.
 |
| double | <span class="optional">(optional)</span> fileSize | 

Number of bytes in the whole file post-decompression, or -1 if unknown.
 |
| boolean | <span class="optional">(optional)</span> exists | 

Whether the downloaded file exists;
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [DownloadItem](/extensions/downloads#type-DownloadItem) results) <span class="subdued">{...}</span>;`

| array of [DownloadItem](/extensions/downloads#type-DownloadItem) | results |  |
|---|---|---|
 |

</div>

</div>

<div>

### pause

<div class="summary">`whale.downloads.pause(<span>integer downloadId</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Pause the download. If the request was successful the download is in a paused state. Otherwise [runtime.lastError](/extensions/runtime#property-lastError) contains an error message. The request will fail if the download is not active.

| Parameters |
|---|
| integer | downloadId | 

The id of the download to pause.
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the pause request is completed.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### resume

<div class="summary">`whale.downloads.resume(<span>integer downloadId</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Resume a paused download. If the request was successful the download is in progress and unpaused. Otherwise [runtime.lastError](/extensions/runtime#property-lastError) contains an error message. The request will fail if the download is not active.

| Parameters |
|---|
| integer | downloadId | 

The id of the download to resume.
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the resume request is completed.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### cancel

<div class="summary">`whale.downloads.cancel(<span>integer downloadId</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Cancel a download. When `callback` is run, the download is cancelled, completed, interrupted or doesn't exist anymore.

| Parameters |
|---|
| integer | downloadId | 

The id of the download to cancel.
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the cancel request is completed.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### getFileIcon

<div class="summary">`whale.downloads.getFileIcon(<span>integer downloadId</span>, <span class="optional">object options</span>, <span>function callback</span>)`</div>

<div class="description">

Retrieve an icon for the specified download. For new downloads, file icons are available after the [onCreated](/extensions/downloads#event-onCreated) event has been received. The image returned by this function while a download is in progress may be different from the image returned after the download is complete. Icon retrieval is done by querying the underlying operating system or toolkit depending on the platform. The icon that is returned will therefore depend on a number of factors including state of the download, platform, registered file types and visual theme. If a file icon cannot be determined, [runtime.lastError](/extensions/runtime#property-lastError) will contain an error message.

| Parameters |
|---|
| integer | downloadId | 

The identifier for the download.
 |
| object | <span class="optional">(optional)</span> options | 

| integer | <span class="optional">(optional)</span> size | 
|---|---|

The size of the returned icon. The icon will be square with dimensions size * size pixels. The default and largest size for the icon is 32x32 pixels. The only supported sizes are 16 and 32\. It is an error to specify any other size.
 |
 |
| function | callback | 

A URL to an image that represents the download.

The _callback_ parameter should be a function that looks like this:

`function(string iconURL) <span class="subdued">{...}</span>;`

| string | <span class="optional">(optional)</span> iconURL |  |
|---|---|---|
 |

</div>

</div>

<div>

### open

<div class="summary">`whale.downloads.open(<span>integer downloadId</span>)`</div>

<div class="description">

Open the downloaded file now if the [DownloadItem](/extensions/downloads#type-DownloadItem) is complete; otherwise returns an error through [runtime.lastError](/extensions/runtime#property-lastError). Requires the `"downloads.open"` permission in addition to the `"downloads"` permission. An [onChanged](/extensions/downloads#event-onChanged) event will fire when the item is opened for the first time.

| Parameters |
|---|
| integer | downloadId | 

The identifier for the downloaded file.
 |

</div>

</div>

<div>

### show

<div class="summary">`whale.downloads.show(<span>integer downloadId</span>)`</div>

<div class="description">

Show the downloaded file in its folder in a file manager.

| Parameters |
|---|
| integer | downloadId | 

The identifier for the downloaded file.
 |

</div>

</div>

<div>

### showDefaultFolder

<div class="summary">`whale.downloads.showDefaultFolder()`</div>

<div class="description">

Show the default Downloads folder in a file manager.

</div>

</div>

<div>

### erase

<div class="summary">`whale.downloads.erase(<span>object query</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Erase matching [DownloadItem](/extensions/downloads#type-DownloadItem) from history without deleting the downloaded file. An [onErased](/extensions/downloads#event-onErased) event will fire for each [DownloadItem](/extensions/downloads#type-DownloadItem) that matches `query`, then `callback` will be called.

| Parameters |
|---|
| object | query | 

| array of string | <span class="optional">(optional)</span> query | 
|---|---|

This array of search terms limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) whose `filename` or `url` or `finalUrl` contain all of the search terms that do not begin with a dash '-' and none of the search terms that do begin with a dash.
 |
| string | <span class="optional">(optional)</span> startedBefore | 

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) that started before the given ms since the epoch.
 |
| string | <span class="optional">(optional)</span> startedAfter | 

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) that started after the given ms since the epoch.
 |
| string | <span class="optional">(optional)</span> endedBefore | 

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) that ended before the given ms since the epoch.
 |
| string | <span class="optional">(optional)</span> endedAfter | 

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) that ended after the given ms since the epoch.
 |
| double | <span class="optional">(optional)</span> totalBytesGreater | 

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) whose `totalBytes` is greater than the given integer.
 |
| double | <span class="optional">(optional)</span> totalBytesLess | 

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) whose `totalBytes` is less than the given integer.
 |
| string | <span class="optional">(optional)</span> filenameRegex | 

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) whose `filename` matches the given regular expression.
 |
| string | <span class="optional">(optional)</span> urlRegex | 

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) whose `url` matches the given regular expression.
 |
| string | <span class="optional">(optional)</span> finalUrlRegex | 

Since Chrome 54.

Limits results to [DownloadItem](/extensions/downloads#type-DownloadItem) whose `finalUrl` matches the given regular expression.
 |
| integer | <span class="optional">(optional)</span> limit | 

The maximum number of matching [DownloadItem](/extensions/downloads#type-DownloadItem) returned. Defaults to 1000\. Set to 0 in order to return all matching [DownloadItem](/extensions/downloads#type-DownloadItem). See [search](/extensions/downloads#method-search) for how to page through results.
 |
| array of string | <span class="optional">(optional)</span> orderBy | 

Set elements of this array to [DownloadItem](/extensions/downloads#type-DownloadItem) properties in order to sort search results. For example, setting `orderBy=['startTime']` sorts the [DownloadItem](/extensions/downloads#type-DownloadItem) by their start time in ascending order. To specify descending order, prefix with a hyphen: '-startTime'.
 |
| integer | <span class="optional">(optional)</span> id | 

The `id` of the [DownloadItem](/extensions/downloads#type-DownloadItem) to query.
 |
| string | <span class="optional">(optional)</span> url | 

The absolute URL that this download initiated from, before any redirects.
 |
| string | <span class="optional">(optional)</span> finalUrl | 

Since Chrome 54.

The absolute URL that this download is being made from, after all redirects.
 |
| string | <span class="optional">(optional)</span> filename | 

Absolute local path.
 |
| [DangerType](/extensions/downloads#type-DangerType) | <span class="optional">(optional)</span> danger | 

Indication of whether this download is thought to be safe or known to be suspicious.
 |
| string | <span class="optional">(optional)</span> mime | 

The file's MIME type.
 |
| string | <span class="optional">(optional)</span> startTime | 

The time when the download began in ISO 8601 format.
 |
| string | <span class="optional">(optional)</span> endTime | 

The time when the download ended in ISO 8601 format.
 |
| [State](/extensions/downloads#type-State) | <span class="optional">(optional)</span> state | 

Indicates whether the download is progressing, interrupted, or complete.
 |
| boolean | <span class="optional">(optional)</span> paused | 

True if the download has stopped reading data from the host, but kept the connection open.
 |
| [InterruptReason](/extensions/downloads#type-InterruptReason) | <span class="optional">(optional)</span> error | 

Why a download was interrupted.
 |
| double | <span class="optional">(optional)</span> bytesReceived | 

Number of bytes received so far from the host, without considering file compression.
 |
| double | <span class="optional">(optional)</span> totalBytes | 

Number of bytes in the whole file, without considering file compression, or -1 if unknown.
 |
| double | <span class="optional">(optional)</span> fileSize | 

Number of bytes in the whole file post-decompression, or -1 if unknown.
 |
| boolean | <span class="optional">(optional)</span> exists | 

Whether the downloaded file exists;
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(array of integer erasedIds) <span class="subdued">{...}</span>;`

| array of integer | erasedIds |  |
|---|---|---|
 |

</div>

</div>

<div>

### removeFile

<div class="summary">`whale.downloads.removeFile(<span>integer downloadId</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Remove the downloaded file if it exists and the [DownloadItem](/extensions/downloads#type-DownloadItem) is complete; otherwise return an error through [runtime.lastError](/extensions/runtime#property-lastError).

| Parameters |
|---|
| integer | downloadId |  |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### acceptDanger

<div class="summary">`whale.downloads.acceptDanger(<span>integer downloadId</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Prompt the user to accept a dangerous download. Can only be called from a visible context (tab, window, or page/browser action popup). Does not automatically accept dangerous downloads. If the download is accepted, then an [onChanged](/extensions/downloads#event-onChanged) event will fire, otherwise nothing will happen. When all the data is fetched into a temporary file and either the download is not dangerous or the danger has been accepted, then the temporary file is renamed to the target filename, the |state| changes to 'complete', and [onChanged](/extensions/downloads#event-onChanged) fires.

| Parameters |
|---|
| integer | downloadId | 

The identifier for the [DownloadItem](/extensions/downloads#type-DownloadItem).
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the danger prompt dialog closes.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### drag

<div class="summary">`whale.downloads.drag(<span>integer downloadId</span>)`</div>

<div class="description">

Initiate dragging the downloaded file to another application. Call in a javascript `ondragstart` handler.

| Parameters |
|---|
| integer | downloadId |  |

</div>

</div>

<div>

### setShelfEnabled

<div class="summary">`whale.downloads.setShelfEnabled(<span>boolean enabled</span>)`</div>

<div class="description">

Enable or disable the gray shelf at the bottom of every window associated with the current browser profile. The shelf will be disabled as long as at least one extension has disabled it. Enabling the shelf while at least one other extension has disabled it will return an error through [runtime.lastError](/extensions/runtime#property-lastError). Requires the `"downloads.shelf"` permission in addition to the `"downloads"` permission.

| Parameters |
|---|
| boolean | enabled |  |

</div>

</div>

## Events

<div>

### onCreated

<div class="description">

This event fires with the [DownloadItem](/extensions/downloads#type-DownloadItem) object when a download begins.

<div>

#### addListener

<div class="summary">`whale.downloads.onCreated.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [DownloadItem](/extensions/downloads#type-DownloadItem) downloadItem) <span class="subdued">{...}</span>;`

| [DownloadItem](/extensions/downloads#type-DownloadItem) | downloadItem |  |
|---|---|---|
 |

</div>

</div>

</div>

</div>

<div>

### onErased

<div class="description">

Fires with the `downloadId` when a download is erased from history.

<div>

#### addListener

<div class="summary">`whale.downloads.onErased.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(integer downloadId) <span class="subdued">{...}</span>;`

| integer | downloadId | 
|---|---|

The `id` of the [DownloadItem](/extensions/downloads#type-DownloadItem) that was erased.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onChanged

<div class="description">

When any of a [DownloadItem](/extensions/downloads#type-DownloadItem)'s properties except `bytesReceived` and `estimatedEndTime` changes, this event fires with the `downloadId` and an object containing the properties that changed.

<div>

#### addListener

<div class="summary">`whale.downloads.onChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object downloadDelta) <span class="subdued">{...}</span>;`

| object | downloadDelta | 
|---|---|

| integer | id | 
|---|---|

The `id` of the [DownloadItem](/extensions/downloads#type-DownloadItem) that changed.
 |
| [StringDelta](/extensions/downloads#type-StringDelta) | <span class="optional">(optional)</span> url | 

The change in `url`, if any.
 |
| [StringDelta](/extensions/downloads#type-StringDelta) | <span class="optional">(optional)</span> finalUrl | 

Since Chrome 54.

The change in `finalUrl`, if any.
 |
| [StringDelta](/extensions/downloads#type-StringDelta) | <span class="optional">(optional)</span> filename | 

The change in `filename`, if any.
 |
| [StringDelta](/extensions/downloads#type-StringDelta) | <span class="optional">(optional)</span> danger | 

The change in `danger`, if any.
 |
| [StringDelta](/extensions/downloads#type-StringDelta) | <span class="optional">(optional)</span> mime | 

The change in `mime`, if any.
 |
| [StringDelta](/extensions/downloads#type-StringDelta) | <span class="optional">(optional)</span> startTime | 

The change in `startTime`, if any.
 |
| [StringDelta](/extensions/downloads#type-StringDelta) | <span class="optional">(optional)</span> endTime | 

The change in `endTime`, if any.
 |
| [StringDelta](/extensions/downloads#type-StringDelta) | <span class="optional">(optional)</span> state | 

The change in `state`, if any.
 |
| [BooleanDelta](/extensions/downloads#type-BooleanDelta) | <span class="optional">(optional)</span> canResume | 

The change in `canResume`, if any.
 |
| [BooleanDelta](/extensions/downloads#type-BooleanDelta) | <span class="optional">(optional)</span> paused | 

The change in `paused`, if any.
 |
| [StringDelta](/extensions/downloads#type-StringDelta) | <span class="optional">(optional)</span> error | 

The change in `error`, if any.
 |
| [DoubleDelta](/extensions/downloads#type-DoubleDelta) | <span class="optional">(optional)</span> totalBytes | 

The change in `totalBytes`, if any.
 |
| [DoubleDelta](/extensions/downloads#type-DoubleDelta) | <span class="optional">(optional)</span> fileSize | 

The change in `fileSize`, if any.
 |
| [BooleanDelta](/extensions/downloads#type-BooleanDelta) | <span class="optional">(optional)</span> exists | 

The change in `exists`, if any.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onDeterminingFilename

<div class="description">

During the filename determination process, extensions will be given the opportunity to override the target [DownloadItem.filename](/extensions/downloads#property-DownloadItem-filename). Each extension may not register more than one listener for this event. Each listener must call `suggest` exactly once, either synchronously or asynchronously. If the listener calls `suggest` asynchronously, then it must return `true`. If the listener neither calls `suggest` synchronously nor returns `true`, then `suggest` will be called automatically. The [DownloadItem](/extensions/downloads#type-DownloadItem) will not complete until all listeners have called `suggest`. Listeners may call `suggest` without any arguments in order to allow the download to use `downloadItem.filename` for its filename, or pass a `suggestion` object to `suggest` in order to override the target filename. If more than one extension overrides the filename, then the last extension installed whose listener passes a `suggestion` object to `suggest` wins. In order to avoid confusion regarding which extension will win, users should not install extensions that may conflict. If the download is initiated by [download](/extensions/downloads#method-download) and the target filename is known before the MIME type and tentative filename have been determined, pass `filename` to [download](/extensions/downloads#method-download) instead.

<div>

#### addListener

<div class="summary">`whale.downloads.onDeterminingFilename.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [DownloadItem](/extensions/downloads#type-DownloadItem) downloadItem, function suggest) <span class="subdued">{...}</span>;`

| [DownloadItem](/extensions/downloads#type-DownloadItem) | downloadItem |  |
|---|---|---|
| function | suggest | 

The _suggest_ parameter should be a function that looks like this:

`function(object suggestion) <span class="subdued">{...}</span>;`

| object | <span class="optional">(optional)</span> suggestion | 
|---|---|

| string | filename | 
|---|---|

The [DownloadItem](/extensions/downloads#type-DownloadItem)'s new target [DownloadItem.filename](/extensions/downloads#property-DownloadItem-filename), as a path relative to the user's default Downloads directory, possibly containing subdirectories. Absolute paths, empty paths, and paths containing back-references ".." will be ignored.
 |
| [FilenameConflictAction](/extensions/downloads#type-FilenameConflictAction) | <span class="optional">(optional)</span> conflictAction | 

The action to take if `filename` already exists.
 |
 |
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>