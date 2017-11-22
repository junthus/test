# whale.storage

| Description: | Use the `whale.storage` API to store, retrieve, and track changes to user data. |
|---|---|
| Availability: | Since Chrome 20. |
| Permissions: | <span class="code">"storage"</span> |
| Content Scripts: | Fully supported. [Learn more](content_scripts) |
| Learn More: | [Chrome Apps Office Hours: Chrome Storage APIs](https://developers.google.com/live/shows/7320022/)
[Chrome Apps Office Hours: Storage API Deep Dive](https://developers.google.com/live/shows/7320022-1/) |

<section>

## Overview

This API has been optimized to meet the specific storage needs of extensions. It provides the same storage capabilities as the [localStorage API](https://developer.mozilla.org/en/DOM/Storage#localStorage) with the following key differences:

*   User data can be automatically synced with Chrome sync (using `storage.sync`).
*   Your extension's content scripts can directly access user data without the need for a background page.
*   A user's extension settings can be persisted even when using [split incognito behavior](manifest/incognito).
*   It's asynchronous with bulk read and write operations, and therefore faster than the blocking and serial `localStorage API`.
*   User data can be stored as objects (the `localStorage API` stores data in strings).
*   Enterprise policies configured by the administrator for the extension can be read (using `storage.managed` with a [schema](manifest/storage)).

## Manifest

You must declare the "storage" permission in the [extension manifest](manifest) to use the storage API. For example:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"permissions": [
          "storage"
        ]**,
        ...
      }
      </pre>

## Usage

To store user data for your extension, you can use either `storage.sync` or `storage.local`. When using `storage.sync`, the stored data will automatically be synced to any Chrome browser that the user is logged into, provided the user has sync enabled.

When Chrome is offline, Chrome stores the data locally. The next time the browser is online, Chrome syncs the data. Even if a user disables syncing, `storage.sync` will still work. In this case, it will behave identically to `storage.local`.

Confidential user information should not be stored! The storage area isn't encrypted.

The `storage.managed` storage is read-only.

## Storage and throttling limits

`whale.storage` is not a big truck. It's a series of tubes. And if you don't understand, those tubes can be filled, and if they are filled when you put your message in, it gets in line, and it's going to be delayed by anyone that puts into that tube enormous amounts of material.

For details on the current limits of the storage API, and what happens when those limits are exceeded, please see the quota information for [sync](/extensions/storage#property-sync) and [local](/extensions/storage#property-local).

## Examples

The following example checks for CSS code saved by a user on a form, and if found, stores it.

<pre>      function saveChanges() {
        // Get a value saved in a form.
        var theValue = textarea.value;
        // Check that there's some code there.
        if (!theValue) {
          message('Error: No value specified');
          return;
        }
        // Save it using the Chrome extension storage API.
        whale.storage.sync.set({'value': theValue}, function() {
          // Notify that we saved.
          message('Settings saved');
        });
      }
      </pre>

If you're interested in tracking changes made to a data object, you can add a listener to its `onChanged` event. Whenever anything changes in storage, that event fires. Here's sample code to listen for saved changes:

<pre>      whale.storage.onChanged.addListener(function(changes, namespace) {
        for (key in changes) {
          var storageChange = changes[key];
          console.log('Storage key "%s" in namespace "%s" changed. ' +
                      'Old value was "%s", new value is "%s".',
                      key,
                      namespace,
                      storageChange.oldValue,
                      storageChange.newValue);
        }
      });
      </pre>

</section>

<section id="toc">

## Summary

| Types |
|---|
| [StorageChange](#type-StorageChange) |
| [StorageArea](#type-StorageArea) |
| Properties |
| [sync](#property-sync) |
| [local](#property-local) |
| [managed](#property-managed) |
| Events |
| [onChanged](#event-onChanged) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### StorageChange

| properties |
|---|
| any | <span class="optional">(optional)</span> oldValue | 

The old value of the item, if there was an old value.
 |
| any | <span class="optional">(optional)</span> newValue | 

The new value of the item, if there is a new value.
 |

</div>

<div>

### StorageArea

| methods |
|---|
| 

<div>

#### get

<div class="summary">`StorageArea.get(<span class="optional">string or array of string or object keys</span>, <span>function callback</span>)`</div>

<div class="description">

Gets one or more items from storage.

| Parameters |
|---|
| string or array of string or object | <span class="optional">(optional)</span> keys | 

A single key to get, list of keys to get, or a dictionary specifying default values (see description of the object). An empty list or object will return an empty result object. Pass in `null` to get the entire contents of storage.
 |
| function | callback | 

Callback with storage items, or on failure (in which case [runtime.lastError](/extensions/runtime#property-lastError) will be set).

The _callback_ parameter should be a function that looks like this:

`function(object items) <span class="subdued">{...}</span>;`

| object | items | 
|---|---|

Object with items in their key-value mappings.
 |
 |

</div>

</div>
 |
| 

<div>

#### getBytesInUse

<div class="summary">`StorageArea.getBytesInUse(<span class="optional">string or array of string keys</span>, <span>function callback</span>)`</div>

<div class="description">

Gets the amount of space (in bytes) being used by one or more items.

| Parameters |
|---|
| string or array of string | <span class="optional">(optional)</span> keys | 

A single key or list of keys to get the total usage for. An empty list will return 0\. Pass in `null` to get the total usage of all of storage.
 |
| function | callback | 

Callback with the amount of space being used by storage, or on failure (in which case [runtime.lastError](/extensions/runtime#property-lastError) will be set).

The _callback_ parameter should be a function that looks like this:

`function(integer bytesInUse) <span class="subdued">{...}</span>;`

| integer | bytesInUse | 
|---|---|

Amount of space being used in storage, in bytes.
 |
 |

</div>

</div>
 |
| 

<div>

#### set

<div class="summary">`StorageArea.set(<span>object items</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Sets multiple items.

| Parameters |
|---|
| object | items | 

An object which gives each key/value pair to update storage with. Any other key/value pairs in storage will not be affected.

Primitive values such as numbers will serialize as expected. Values with a `typeof` `"object"` and `"function"` will typically serialize to `{}`, with the exception of `Array` (serializes as expected), `Date`, and `Regex` (serialize using their `String` representation).
 |
| function | <span class="optional">(optional)</span> callback | 

Callback on success, or on failure (in which case [runtime.lastError](/extensions/runtime#property-lastError) will be set).

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |
| 

<div>

#### remove

<div class="summary">`StorageArea.remove(<span>string or array of string keys</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Removes one or more items from storage.

| Parameters |
|---|
| string or array of string | keys | 

A single key or a list of keys for items to remove.
 |
| function | <span class="optional">(optional)</span> callback | 

Callback on success, or on failure (in which case [runtime.lastError](/extensions/runtime#property-lastError) will be set).

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |
| 

<div>

#### clear

<div class="summary">`StorageArea.clear(<span class="optional">function callback</span>)`</div>

<div class="description">

Removes all items from storage.

| Parameters |
|---|
| function | <span class="optional">(optional)</span> callback | 

Callback on success, or on failure (in which case [runtime.lastError](/extensions/runtime#property-lastError) will be set).

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |

</div>

## Properties

| <span class="type_name">[StorageArea](/extensions/storage#type-StorageArea)</span> | `whale.storage.sync` | Items in the `sync` storage area are synced using Chrome Sync.

| Properties |
|---|
| --- |
| `102,400` | QUOTA_BYTES | 

The maximum total amount (in bytes) of data that can be stored in sync storage, as measured by the JSON stringification of every value plus every key's length. Updates that would cause this limit to be exceeded fail immediately and set [runtime.lastError](/extensions/runtime#property-lastError).
 |
| `8,192` | QUOTA_BYTES_PER_ITEM | 

The maximum size (in bytes) of each individual item in sync storage, as measured by the JSON stringification of its value plus its key length. Updates containing items larger than this limit will fail immediately and set [runtime.lastError](/extensions/runtime#property-lastError).
 |
| `512` | MAX_ITEMS | 

The maximum number of items that can be stored in sync storage. Updates that would cause this limit to be exceeded will fail immediately and set [runtime.lastError](/extensions/runtime#property-lastError).
 |
| `1,800` | MAX_WRITE_OPERATIONS_PER_HOUR | 

The maximum number of `set`, `remove`, or `clear` operations that can be performed each hour. This is 1 every 2 seconds, a lower ceiling than the short term higher writes-per-minute limit.

Updates that would cause this limit to be exceeded fail immediately and set [runtime.lastError](/extensions/runtime#property-lastError).
 |
| `120` | MAX_WRITE_OPERATIONS_PER_MINUTE | 

Since Chrome 40.

The maximum number of `set`, `remove`, or `clear` operations that can be performed each minute. This is 2 per second, providing higher throughput than writes-per-hour over a shorter period of time.

Updates that would cause this limit to be exceeded fail immediately and set [runtime.lastError](/extensions/runtime#property-lastError).
 |
| `1,000,000` | MAX_SUSTAINED_WRITE_OPERATIONS_PER_MINUTE | 

**Deprecated** since Chrome 40. The storage.sync API no longer has a sustained write operation quota.
 |
 |
| <span class="type_name">[StorageArea](/extensions/storage#type-StorageArea)</span> | `whale.storage.local` | Items in the `local` storage area are local to each machine.

| Properties |
|---|
| --- |
| `5,242,880` | QUOTA_BYTES | 

The maximum amount (in bytes) of data that can be stored in local storage, as measured by the JSON stringification of every value plus every key's length. This value will be ignored if the extension has the `unlimitedStorage` permission. Updates that would cause this limit to be exceeded fail immediately and set [runtime.lastError](/extensions/runtime#property-lastError).
 |
 |
| <span class="type_name">[StorageArea](/extensions/storage#type-StorageArea)</span> | `whale.storage.managed` | 

Since Chrome 33.

Items in the `managed` storage area are set by the domain administrator, and are read-only for the extension; trying to modify this namespace results in an error. |

## Events

<div>

### onChanged

<div class="description">

Fired when one or more items change.

<div>

#### addListener

<div class="summary">`whale.storage.onChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object changes, string areaName) <span class="subdued">{...}</span>;`

| object | changes | 
|---|---|

Object mapping each key that changed to its corresponding [storage.StorageChange](/extensions/storage#type-StorageChange) for that item.
 |
| string | areaName | 

Since Chrome 22.

The name of the storage area (`"sync"`, `"local"` or `"managed"`) the changes are for.
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>