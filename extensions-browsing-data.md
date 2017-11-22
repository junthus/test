# whale.browsingData

| Description: | Use the `whale.browsingData` API to remove browsing data from a user's local profile. |
|---|---|
| Availability: | Since Chrome 19. |
| Permissions: | <span class="code">"browsingData"</span> |

<section>

## Manifest

You must declare the "browsingData" permission in the [extension manifest](manifest) to use this API.

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"permissions": [
          "browsingData",
        ]**,
        ...
      }
      </pre>

## Usage

The simplest use-case for this API is a a time-based mechanism for clearing a user's browsing data. Your code should provide a timestamp which indicates the historical date after which the user's browsing data should be removed. This timestamp is formatted as the number of milliseconds since the Unix epoch (which can be retrieved from a JavaScript `Date` object via the `getTime` method).

For example, to clear all of a user's browsing data from the last week, you might write code as follows:

<pre>var callback = function () {
        // Do something clever here once data has been removed.
      };

      var millisecondsPerWeek = 1000 * 60 * 60 * 24 * 7;
      var oneWeekAgo = (new Date()).getTime() - millisecondsPerWeek;
      whale.browsingData.remove({
        "since": oneWeekAgo
      }, {
        "appcache": true,
        "cache": true,
        "cookies": true,
        "downloads": true,
        "fileSystems": true,
        "formData": true,
        "history": true,
        "indexedDB": true,
        "localStorage": true,
        "pluginData": true,
        "passwords": true,
        "webSQL": true
      }, callback);</pre>

The `whale.browsingData.remove` method allows you to remove various types of browsing data with a single call, and will be much faster than calling multiple more specific methods. If, however, you only want to clear one specific type of browsing data (cookies, for example), the more granular methods offer a readable alternative to a call filled with JSON.

<pre>var callback = function () {
        // Do something clever here once data has been removed.
      };

      var millisecondsPerWeek = 1000 * 60 * 60 * 24 * 7;
      var oneWeekAgo = (new Date()).getTime() - millisecondsPerWeek;
      whale.browsingData.removeCookies({
        "since": oneWeekAgo
      }, callback);</pre>

**Important**: Removing browsing data involves a good deal of heavy lifting in the background, and can take _tens of seconds_ to complete, depending on a user's profile. You should use the callback mechanism to keep your users up to date on the removal's status.

## Origin Types

Adding an `originTypes` property to the API's options object allows you to specify which types of origins ought to be effected. Currently, origins are divided into three categories:

*   `unprotectedWeb` covers the general case of websites that users visit without taking any special action. If you don't specify an `originTypes`, the API defaults to removing data from unprotected web origins.
*   `protectedWeb` covers those web origins that have been installed as hosted applications. Installing [Angry Birds](https://whale.google.com/webstore/detail/aknpkdffaafgjchaibgeefbgmgeghloj), for example, protects the origin `http://whale.angrybirds.com`, and removes it from the `unprotectedWeb` category. Please do be careful when triggering deletion of data for these origins: make sure your users know what they're getting, as this will irrevocably remove their game data. No one wants to knock tiny pig houses over more often than necessary.
*   `extension` covers origins under the `chrome-extensions:` scheme. Removing extension data is, again, something you should be very careful about.

We could adjust the previous example to remove only data from protected websites as follows:

<pre>var callback = function () {
        // Do something clever here once data has been removed.
      };

      var millisecondsPerWeek = 1000 * 60 * 60 * 24 * 7;
      var oneWeekAgo = (new Date()).getTime() - millisecondsPerWeek;
      whale.browsingData.remove({
        "since": oneWeekAgo,
        **"originTypes": {
          "protectedWeb": true
        }**
      }, {
        "appcache": true,
        "cache": true,
        "cookies": true,
        "downloads": true,
        "fileSystems": true,
        "formData": true,
        "history": true,
        "indexedDB": true,
        "localStorage": true,
        "serverBoundCertificates": true,
        "pluginData": true,
        "passwords": true,
        "webSQL": true
      }, callback);</pre>

**Seriously**: Be careful with `protectedWeb` and `extension`. These are destructive operations that your users will write angry email about if they're not well-informed about what to expect when your extension removes data on their behalf.

## Examples

Samples for the `browsingData` API are available [on the samples page](samples#search:browsingData).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [RemovalOptions](#type-RemovalOptions) |
| [DataTypeSet](#type-DataTypeSet) |
| Methods |
| [settings](#method-settings) − `whale.browsingData.settings(<span>function callback</span>)` |
| [remove](#method-remove) − `whale.browsingData.remove( <span>RemovalOptions options</span>, <span>DataTypeSet dataToRemove</span>, <span class="optional">function callback</span>)` |
| [removeAppcache](#method-removeAppcache) − `whale.browsingData.removeAppcache( <span>RemovalOptions options</span>, <span class="optional">function callback</span>)` |
| [removeCache](#method-removeCache) − `whale.browsingData.removeCache( <span>RemovalOptions options</span>, <span class="optional">function callback</span>)` |
| [removeCookies](#method-removeCookies) − `whale.browsingData.removeCookies( <span>RemovalOptions options</span>, <span class="optional">function callback</span>)` |
| [removeDownloads](#method-removeDownloads) − `whale.browsingData.removeDownloads( <span>RemovalOptions options</span>, <span class="optional">function callback</span>)` |
| [removeFileSystems](#method-removeFileSystems) − `whale.browsingData.removeFileSystems( <span>RemovalOptions options</span>, <span class="optional">function callback</span>)` |
| [removeFormData](#method-removeFormData) − `whale.browsingData.removeFormData( <span>RemovalOptions options</span>, <span class="optional">function callback</span>)` |
| [removeHistory](#method-removeHistory) − `whale.browsingData.removeHistory( <span>RemovalOptions options</span>, <span class="optional">function callback</span>)` |
| [removeIndexedDB](#method-removeIndexedDB) − `whale.browsingData.removeIndexedDB( <span>RemovalOptions options</span>, <span class="optional">function callback</span>)` |
| [removeLocalStorage](#method-removeLocalStorage) − `whale.browsingData.removeLocalStorage( <span>RemovalOptions options</span>, <span class="optional">function callback</span>)` |
| [removePluginData](#method-removePluginData) − `whale.browsingData.removePluginData( <span>RemovalOptions options</span>, <span class="optional">function callback</span>)` |
| [removePasswords](#method-removePasswords) − `whale.browsingData.removePasswords( <span>RemovalOptions options</span>, <span class="optional">function callback</span>)` |
| [removeWebSQL](#method-removeWebSQL) − `whale.browsingData.removeWebSQL( <span>RemovalOptions options</span>, <span class="optional">function callback</span>)` |

</section>

<section>

<div class="api-reference">

## Types

<div>

### RemovalOptions

<dd>Options that determine exactly what data will be removed.</dd>

| properties |
|---|
| double | <span class="optional">(optional)</span> since | 

Remove data accumulated on or after this date, represented in milliseconds since the epoch (accessible via the `getTime` method of the JavaScript `Date` object). If absent, defaults to 0 (which would remove all browsing data).
 |
| object | <span class="optional">(optional)</span> originTypes | 

Since Chrome 21.

An object whose properties specify which origin types ought to be cleared. If this object isn't specified, it defaults to clearing only "unprotected" origins. Please ensure that you _really_ want to remove application data before adding 'protectedWeb' or 'extensions'.

| boolean | <span class="optional">(optional)</span> unprotectedWeb | 
|---|---|

Normal websites.
 |
| boolean | <span class="optional">(optional)</span> protectedWeb | 

Websites that have been installed as hosted applications (be careful!).
 |
| boolean | <span class="optional">(optional)</span> extension | 

Extensions and packaged applications a user has installed (be _really_ careful!).
 |
 |

</div>

<div>

### DataTypeSet

Since Chrome 27.

<dd>A set of data types. Missing data types are interpreted as `false`.</dd>

| properties |
|---|
| boolean | <span class="optional">(optional)</span> appcache | 

Websites' appcaches.
 |
| boolean | <span class="optional">(optional)</span> cache | 

The browser's cache. Note: when removing data, this clears the _entire_ cache: it is not limited to the range you specify.
 |
| boolean | <span class="optional">(optional)</span> cookies | 

The browser's cookies.
 |
| boolean | <span class="optional">(optional)</span> downloads | 

The browser's download list.
 |
| boolean | <span class="optional">(optional)</span> fileSystems | 

Websites' file systems.
 |
| boolean | <span class="optional">(optional)</span> formData | 

The browser's stored form data.
 |
| boolean | <span class="optional">(optional)</span> history | 

The browser's history.
 |
| boolean | <span class="optional">(optional)</span> indexedDB | 

Websites' IndexedDB data.
 |
| boolean | <span class="optional">(optional)</span> localStorage | 

Websites' local storage data.
 |
| boolean | <span class="optional">(optional)</span> serverBoundCertificates | 

Server-bound certificates.
 |
| boolean | <span class="optional">(optional)</span> passwords | 

Stored passwords.
 |
| boolean | <span class="optional">(optional)</span> pluginData | 

Plugins' data.
 |
| boolean | <span class="optional">(optional)</span> serviceWorkers | 

Since Chrome 39.

Service Workers.
 |
| boolean | <span class="optional">(optional)</span> webSQL | 

Websites' WebSQL data.
 |

</div>

## Methods

<div>

### settings

<div class="summary">`whale.browsingData.settings(<span>function callback</span>)`</div>

<div class="description">

Since Chrome 26.

Reports which types of data are currently selected in the 'Clear browsing data' settings UI. Note: some of the data types included in this API are not available in the settings UI, and some UI settings control more than one data type listed here.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object result) <span class="subdued">{...}</span>;`

| object | result | 
|---|---|

| [RemovalOptions](/extensions/browsingData#type-RemovalOptions) | options |  |
|---|---|---|
| [DataTypeSet](/extensions/browsingData#type-DataTypeSet) | dataToRemove | 

All of the types will be present in the result, with values of `true` if they are both selected to be removed and permitted to be removed, otherwise `false`.
 |
| [DataTypeSet](/extensions/browsingData#type-DataTypeSet) | dataRemovalPermitted | 

All of the types will be present in the result, with values of `true` if they are permitted to be removed (e.g., by enterprise policy) and `false` if not.
 |
 |
 |

</div>

</div>

<div>

### remove

<div class="summary">`whale.browsingData.remove( <span>[RemovalOptions](/extensions/browsingData#type-RemovalOptions) options</span>, <span>[DataTypeSet](/extensions/browsingData#type-DataTypeSet) dataToRemove</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears various types of browsing data stored in a user's profile.

| Parameters |
|---|
| [RemovalOptions](/extensions/browsingData#type-RemovalOptions) | options |  |
| [DataTypeSet](/extensions/browsingData#type-DataTypeSet) | dataToRemove | 

The set of data types to remove.
 |
| function | <span class="optional">(optional)</span> callback | 

Called when deletion has completed.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### removeAppcache

<div class="summary">`whale.browsingData.removeAppcache( <span>[RemovalOptions](/extensions/browsingData#type-RemovalOptions) options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears websites' appcache data.

| Parameters |
|---|
| [RemovalOptions](/extensions/browsingData#type-RemovalOptions) | options |  |
| function | <span class="optional">(optional)</span> callback | 

Called when websites' appcache data has been cleared.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### removeCache

<div class="summary">`whale.browsingData.removeCache( <span>[RemovalOptions](/extensions/browsingData#type-RemovalOptions) options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears the browser's cache.

| Parameters |
|---|
| [RemovalOptions](/extensions/browsingData#type-RemovalOptions) | options |  |
| function | <span class="optional">(optional)</span> callback | 

Called when the browser's cache has been cleared.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### removeCookies

<div class="summary">`whale.browsingData.removeCookies( <span>[RemovalOptions](/extensions/browsingData#type-RemovalOptions) options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears the browser's cookies and server-bound certificates modified within a particular timeframe.

| Parameters |
|---|
| [RemovalOptions](/extensions/browsingData#type-RemovalOptions) | options |  |
| function | <span class="optional">(optional)</span> callback | 

Called when the browser's cookies and server-bound certificates have been cleared.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### removeDownloads

<div class="summary">`whale.browsingData.removeDownloads( <span>[RemovalOptions](/extensions/browsingData#type-RemovalOptions) options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears the browser's list of downloaded files (_not_ the downloaded files themselves).

| Parameters |
|---|
| [RemovalOptions](/extensions/browsingData#type-RemovalOptions) | options |  |
| function | <span class="optional">(optional)</span> callback | 

Called when the browser's list of downloaded files has been cleared.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### removeFileSystems

<div class="summary">`whale.browsingData.removeFileSystems( <span>[RemovalOptions](/extensions/browsingData#type-RemovalOptions) options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears websites' file system data.

| Parameters |
|---|
| [RemovalOptions](/extensions/browsingData#type-RemovalOptions) | options |  |
| function | <span class="optional">(optional)</span> callback | 

Called when websites' file systems have been cleared.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### removeFormData

<div class="summary">`whale.browsingData.removeFormData( <span>[RemovalOptions](/extensions/browsingData#type-RemovalOptions) options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears the browser's stored form data (autofill).

| Parameters |
|---|
| [RemovalOptions](/extensions/browsingData#type-RemovalOptions) | options |  |
| function | <span class="optional">(optional)</span> callback | 

Called when the browser's form data has been cleared.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### removeHistory

<div class="summary">`whale.browsingData.removeHistory( <span>[RemovalOptions](/extensions/browsingData#type-RemovalOptions) options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears the browser's history.

| Parameters |
|---|
| [RemovalOptions](/extensions/browsingData#type-RemovalOptions) | options |  |
| function | <span class="optional">(optional)</span> callback | 

Called when the browser's history has cleared.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### removeIndexedDB

<div class="summary">`whale.browsingData.removeIndexedDB( <span>[RemovalOptions](/extensions/browsingData#type-RemovalOptions) options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears websites' IndexedDB data.

| Parameters |
|---|
| [RemovalOptions](/extensions/browsingData#type-RemovalOptions) | options |  |
| function | <span class="optional">(optional)</span> callback | 

Called when websites' IndexedDB data has been cleared.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### removeLocalStorage

<div class="summary">`whale.browsingData.removeLocalStorage( <span>[RemovalOptions](/extensions/browsingData#type-RemovalOptions) options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears websites' local storage data.

| Parameters |
|---|
| [RemovalOptions](/extensions/browsingData#type-RemovalOptions) | options |  |
| function | <span class="optional">(optional)</span> callback | 

Called when websites' local storage has been cleared.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### removePluginData

<div class="summary">`whale.browsingData.removePluginData( <span>[RemovalOptions](/extensions/browsingData#type-RemovalOptions) options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears plugins' data.

| Parameters |
|---|
| [RemovalOptions](/extensions/browsingData#type-RemovalOptions) | options |  |
| function | <span class="optional">(optional)</span> callback | 

Called when plugins' data has been cleared.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### removePasswords

<div class="summary">`whale.browsingData.removePasswords( <span>[RemovalOptions](/extensions/browsingData#type-RemovalOptions) options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears the browser's stored passwords.

| Parameters |
|---|
| [RemovalOptions](/extensions/browsingData#type-RemovalOptions) | options |  |
| function | <span class="optional">(optional)</span> callback | 

Called when the browser's passwords have been cleared.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### removeWebSQL

<div class="summary">`whale.browsingData.removeWebSQL( <span>[RemovalOptions](/extensions/browsingData#type-RemovalOptions) options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears websites' WebSQL data.

| Parameters |
|---|
| [RemovalOptions](/extensions/browsingData#type-RemovalOptions) | options |  |
| function | <span class="optional">(optional)</span> callback | 

Called when websites' WebSQL databases have been cleared.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

</div>

</section>