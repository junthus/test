# whale.webstore

| Description: | Use the `whale.webstore` API to initiate app and extension installations "inline" from your site. |
|---|---|
| Availability: | Since Chrome 20. |
| Learn More: | [Using Inline Installation](https://developers.google.com/chrome/web-store/docs/inline_installation) |

<section id="toc">

## Summary

| Types |
|---|
| [InstallStage](#type-InstallStage) |
| [ErrorCode](#type-ErrorCode) |
| Methods |
| [install](#method-install) − `whale.webstore.install(<span class="optional">string url</span>, <span class="optional">function successCallback</span>, <span class="optional">function failureCallback</span>)` |
| Events |
| [onInstallStageChanged](#event-onInstallStageChanged) |
| [onDownloadProgress](#event-onDownloadProgress) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### InstallStage

<dd>Enum used to indicate the stage of the installation process. 'downloading' indicates that the necessary files are being downloaded, and 'installing' indicates that the files are downloaded and are being actively installed.</dd>

| Enum |
|---|
| `"installing"`, or `"downloading"` |

</div>

<div>

### ErrorCode

<dd>Enum of the possible install results, including error codes sent back in the event that an inline installation has failed.</dd>

| Enum |
|---|
| 

<dl>

<dt>`"otherError"`</dt>

<dd>An uncommon, unrecognized, or unexpected error. In some cases, the readable error string can provide more information.</dd>

</dl>

<dl>

<dt>`"aborted"`</dt>

<dd>The operation was aborted as the requestor is no longer alive.</dd>

</dl>

<dl>

<dt>`"installInProgress"`</dt>

<dd>An installation of the same extension is in progress.</dd>

</dl>

<dl>

<dt>`"notPermitted"`</dt>

<dd>The installation is not permitted.</dd>

</dl>

<dl>

<dt>`"invalidId"`</dt>

<dd>Invalid Chrome Web Store item ID.</dd>

</dl>

<dl>

<dt>`"webstoreRequestError"`</dt>

<dd>Failed to retrieve extension metadata from the Web Store.</dd>

</dl>

<dl>

<dt>`"invalidWebstoreResponse"`</dt>

<dd>The extension metadata retrieved from the Web Store was invalid.</dd>

</dl>

<dl>

<dt>`"invalidManifest"`</dt>

<dd>An error occurred while parsing the extension manifest retrieved from the Web Store.</dd>

</dl>

<dl>

<dt>`"iconError"`</dt>

<dd>Failed to retrieve the extension's icon from the Web Store, or the icon was invalid.</dd>

</dl>

<dl>

<dt>`"userCanceled"`</dt>

<dd>The user canceled the operation.</dd>

</dl>

<dl>

<dt>`"blacklisted"`</dt>

<dd>The extension is blacklisted.</dd>

</dl>

<dl>

<dt>`"missingDependencies"`</dt>

<dd>Unsatisfied dependencies, such as shared modules.</dd>

</dl>

<dl>

<dt>`"requirementViolations"`</dt>

<dd>Unsatisfied requirements, such as webgl.</dd>

</dl>

<dl>

<dt>`"blockedByPolicy"`</dt>

<dd>The extension is blocked by management policies.</dd>

</dl>

<dl>

<dt>`"launchFeatureDisabled"`</dt>

<dd>The launch feature is not available.</dd>

</dl>

<dl>

<dt>`"launchUnsupportedExtensionType"`</dt>

<dd>The launch feature is not supported for the extension type.</dd>

</dl>

<dl>

<dt>`"launchInProgress"`</dt>

<dd>A launch of the same extension is in progress.</dd>

</dl>
 |

</div>

## Methods

<div>

### install

<div class="summary">`whale.webstore.install(<span class="optional">string url</span>, <span class="optional">function successCallback</span>, <span class="optional">function failureCallback</span>)`</div>

<div class="description">

| Parameters |
|---|
| string | <span class="optional">(optional)</span> url | 

If you have more than one `<link>` tag on your page with the `chrome-webstore-item` relation, you can choose which item you'd like to install by passing in its URL here. If it is omitted, then the first (or only) link will be used. An exception will be thrown if the passed in URL does not exist on the page.
 |
| function | <span class="optional">(optional)</span> successCallback | 

This function is invoked when inline installation successfully completes (after the dialog is shown and the user agrees to add the item to Chrome). You may wish to use this to hide the user interface element that prompted the user to install the app or extension.
 |
| function | <span class="optional">(optional)</span> failureCallback | 

This function is invoked when inline installation does not successfully complete. Possible reasons for this include the user canceling the dialog, the linked item not being found in the store, or the install being initiated from a non-verified site.

If you specify the _failureCallback_ parameter, it should be a function that looks like this:

`function(string error, [ErrorCode](/extensions/webstore#type-ErrorCode) errorCode) <span class="subdued">{...}</span>;`

| string | error | 
|---|---|

The failure detail. You may wish to inspect or log this for debugging purposes, but you should not rely on specific strings being passed back.
 |
| [ErrorCode](/extensions/webstore#type-ErrorCode) | <span class="optional">(optional)</span> errorCode | 

The error code from the stable set of possible errors.
 |
 |

</div>

</div>

## Events

<div>

### onInstallStageChanged

<div class="description">

Since Chrome 35.

Fired when an inline installation enters a new InstallStage. In order to receive notifications about this event, listeners must be registered before the inline installation begins.

<div>

#### addListener

<div class="summary">`whale.webstore.onInstallStageChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [InstallStage](/extensions/webstore#type-InstallStage) stage) <span class="subdued">{...}</span>;`

| [InstallStage](/extensions/webstore#type-InstallStage) | stage | 
|---|---|

The InstallStage that just began.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onDownloadProgress

<div class="description">

Since Chrome 35.

Fired periodically with the download progress of an inline install. In order to receive notifications about this event, listeners must be registered before the inline installation begins.

<div>

#### addListener

<div class="summary">`whale.webstore.onDownloadProgress.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(double percentDownloaded) <span class="subdued">{...}</span>;`

| double | percentDownloaded | 
|---|---|

The progress of the download, between 0 and 1\. 0 indicates no progress; 1.0 indicates complete.
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>