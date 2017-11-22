# whale.permissions

| Description: | Use the `whale.permissions` API to request [declared optional permissions](permissions#manifest) at run time rather than install time, so users understand why the permissions are needed and grant only those that are necessary. |
|---|---|
| Availability: | Since Chrome 20. |
| Learn More: | [Declaring permissions](declare_permissions) |

<section>

## Implementing optional permissions

### Step 1: Decide which permissions are required and which are optional

An extension can declare both required and optional permissions. In general, you should:

*   Use required permissions when they are needed for your extension’s basic functionality.
*   Use optional permissions when they are needed for optional features in your extension.

Advantages of _required_ permissions:

*   **Fewer prompts:** An extension can prompt the user once to accept all permissions.
*   **Simpler development:** Required permissions are guaranteed to be present.

Advantages of _optional_ permissions:

*   **Better security:** Extensions run with fewer permissions since users only enable permissions that are needed.
*   **Better information for users:** An extension can explain why it needs a particular permission when the user enables the relevant feature.
*   **Easier upgrades:** When you upgrade your extension, Chrome will not disable it for your users if the upgrade adds optional rather than required permissions.

### Step 2: Declare optional permissions in the manifest

Declare optional permissions in your [extension manifest](manifest) with the `optional_permissions` key, using the same format as the [permissions](declare_permissions#manifest) field:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"optional_permissions": [ "tabs", "http://www.google.com/" ],**
        ...
      }
      </pre>

You can specify any of the following as optional [permissions](declare_permissions):

*   _host permissions_
*   background
*   bookmarks
*   clipboardRead
*   clipboardWrite
*   contentSettings
*   contextMenus
*   cookies
*   debugger
*   history
*   idle
*   management
*   notifications
*   pageCapture
*   tabs
*   topSites
*   webNavigation
*   webRequest
*   webRequestBlocking

If you want to request hosts that you only discover at runtime, include `"http://*/"` and/or `"https://*/"` as an `optional_permission`. This lets you specify any origin in [Permissions.origins](/extensions/permissions#property-Permissions-origins) as long as it has a matching scheme.

### Step 3: Request optional permissions

Request the permissions from within a user gesture using `permissions.request()`:

<pre>      document.querySelector('#my-button').addEventListener('click', function(event) {
        // Permissions must be requested from inside a user gesture, like a button's
        // click handler.
        whale.permissions.request({
          permissions: ['tabs'],
          origins: ['http://www.google.com/']
        }, function(granted) {
          // The callback argument will be true if the user granted the permissions.
          if (granted) {
            doSomething();
          } else {
            doSomethingElse();
          }
        });
      });
      </pre>

Chrome prompts the user if adding the permissions results in different [warning messages](permission_warnings) than the user has already seen and accepted. For example, the previous code might result in a prompt like this:

![example permission confirmation prompt](/static/images/perms-optional.png)

### Step 4: Check the extension's current permissions

To check whether your extension has a specific permission or set of permissions, use `permission.contains()`:

<pre>      whale.permissions.contains({
        permissions: ['tabs'],
        origins: ['http://www.google.com/']
      }, function(result) {
        if (result) {
          // The extension has the permissions.
        } else {
          // The extension doesn't have the permissions.
        }
      });
      </pre>

### Step 5: Remove the permissions

You should remove permissions when you no longer need them. After a permission has been removed, calling `permissions.request()` usually adds the permission back without prompting the user.

<pre>      whale.permissions.remove({
        permissions: ['tabs'],
        origins: ['http://www.google.com/']
      }, function(removed) {
        if (removed) {
          // The permissions have been removed.
        } else {
          // The permissions have not been removed (e.g., you tried to remove
          // required permissions).
        }
      });
      </pre>

</section>

<section id="toc">

## Summary

| Types |
|---|
| [Permissions](#type-Permissions) |
| Methods |
| [getAll](#method-getAll) − `whale.permissions.getAll(<span>function callback</span>)` |
| [contains](#method-contains) − `whale.permissions.contains( <span>Permissions permissions</span>, <span>function callback</span>)` |
| [request](#method-request) − `whale.permissions.request( <span>Permissions permissions</span>, <span class="optional">function callback</span>)` |
| [remove](#method-remove) − `whale.permissions.remove( <span>Permissions permissions</span>, <span class="optional">function callback</span>)` |
| Events |
| [onAdded](#event-onAdded) |
| [onRemoved](#event-onRemoved) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### Permissions

| properties |
|---|
| array of string | <span class="optional">(optional)</span> permissions | 

List of named permissions (does not include hosts or origins). Anything listed here must appear in the `optional_permissions` list in the manifest.
 |
| array of string | <span class="optional">(optional)</span> origins | 

List of origin permissions. Anything listed here must be a subset of a host that appears in the `optional_permissions` list in the manifest. For example, if `http://*.example.com/` or `http://*/` appears in `optional_permissions`, you can request an origin of `http://help.example.com/`. Any path is ignored.
 |

</div>

## Methods

<div>

### getAll

<div class="summary">`whale.permissions.getAll(<span>function callback</span>)`</div>

<div class="description">

Gets the extension's current set of permissions.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Permissions](/extensions/permissions#type-Permissions) permissions) <span class="subdued">{...}</span>;`

| [Permissions](/extensions/permissions#type-Permissions) | permissions | 
|---|---|

The extension's active permissions.
 |
 |

</div>

</div>

<div>

### contains

<div class="summary">`whale.permissions.contains( <span>[Permissions](/extensions/permissions#type-Permissions) permissions</span>, <span>function callback</span>)`</div>

<div class="description">

Checks if the extension has the specified permissions.

| Parameters |
|---|
| [Permissions](/extensions/permissions#type-Permissions) | permissions |  |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(boolean result) <span class="subdued">{...}</span>;`

| boolean | result | 
|---|---|

True if the extension has the specified permissions.
 |
 |

</div>

</div>

<div>

### request

<div class="summary">`whale.permissions.request( <span>[Permissions](/extensions/permissions#type-Permissions) permissions</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Requests access to the specified permissions. These permissions must be defined in the optional_permissions field of the manifest. If there are any problems requesting the permissions, [runtime.lastError](/extensions/runtime#property-lastError) will be set.

| Parameters |
|---|
| [Permissions](/extensions/permissions#type-Permissions) | permissions |  |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(boolean granted) <span class="subdued">{...}</span>;`

| boolean | granted | 
|---|---|

True if the user granted the specified permissions.
 |
 |

</div>

</div>

<div>

### remove

<div class="summary">`whale.permissions.remove( <span>[Permissions](/extensions/permissions#type-Permissions) permissions</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Removes access to the specified permissions. If there are any problems removing the permissions, [runtime.lastError](/extensions/runtime#property-lastError) will be set.

| Parameters |
|---|
| [Permissions](/extensions/permissions#type-Permissions) | permissions |  |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(boolean removed) <span class="subdued">{...}</span>;`

| boolean | removed | 
|---|---|

True if the permissions were removed.
 |
 |

</div>

</div>

## Events

<div>

### onAdded

<div class="description">

Fired when the extension acquires new permissions.

<div>

#### addListener

<div class="summary">`whale.permissions.onAdded.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Permissions](/extensions/permissions#type-Permissions) permissions) <span class="subdued">{...}</span>;`

| [Permissions](/extensions/permissions#type-Permissions) | permissions | 
|---|---|

The newly acquired permissions.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onRemoved

<div class="description">

Fired when access to permissions has been removed from the extension.

<div>

#### addListener

<div class="summary">`whale.permissions.onRemoved.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Permissions](/extensions/permissions#type-Permissions) permissions) <span class="subdued">{...}</span>;`

| [Permissions](/extensions/permissions#type-Permissions) | permissions | 
|---|---|

The permissions that have been removed.
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>