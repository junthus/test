# whale.management

| Description: | The `whale.management` API provides ways to manage the list of extensions/apps that are installed and running. It is particularly useful for extensions that [override](override) the built-in New Tab page. |
|---|---|
| Availability: | Since Chrome 20. |
| Permissions: | <span class="code">"management"</span> |

<section>

## Manifest

You must declare the "management" permission in the [extension manifest](manifest) to use the management API. For example:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"permissions": [
          "management"
        ]**,
        ...
      }</pre>

[management.getPermissionWarningsByManifest](/extensions/management#method-getPermissionWarningsByManifest), [management.uninstallSelf](/extensions/management#method-uninstallSelf), and [management.getSelf](/extensions/management#method-getSelf) do not require the management permission.

</section>

<section id="toc">

## Summary

| Types |
|---|
| [IconInfo](#type-IconInfo) |
| [LaunchType](#type-LaunchType) |
| [ExtensionDisabledReason](#type-ExtensionDisabledReason) |
| [ExtensionType](#type-ExtensionType) |
| [ExtensionInstallType](#type-ExtensionInstallType) |
| [ExtensionInfo](#type-ExtensionInfo) |
| Methods |
| [getAll](#method-getAll) − `whale.management.getAll(<span class="optional">function callback</span>)` |
| [get](#method-get) − `whale.management.get(<span>string id</span>, <span class="optional">function callback</span>)` |
| [getSelf](#method-getSelf) − `whale.management.getSelf(<span class="optional">function callback</span>)` |
| [getPermissionWarningsById](#method-getPermissionWarningsById) − `whale.management.getPermissionWarningsById(<span>string id</span>, <span class="optional">function callback</span>)` |
| [getPermissionWarningsByManifest](#method-getPermissionWarningsByManifest) − `whale.management.getPermissionWarningsByManifest(<span>string manifestStr</span>, <span class="optional">function callback</span>)` |
| [setEnabled](#method-setEnabled) − `whale.management.setEnabled(<span>string id</span>, <span>boolean enabled</span>, <span class="optional">function callback</span>)` |
| [uninstall](#method-uninstall) − `whale.management.uninstall(<span>string id</span>, <span class="optional">object options</span>, <span class="optional">function callback</span>)` |
| [uninstallSelf](#method-uninstallSelf) − `whale.management.uninstallSelf(<span class="optional">object options</span>, <span class="optional">function callback</span>)` |
| [launchApp](#method-launchApp) − `whale.management.launchApp(<span>string id</span>, <span class="optional">function callback</span>)` |
| [createAppShortcut](#method-createAppShortcut) − `whale.management.createAppShortcut(<span>string id</span>, <span class="optional">function callback</span>)` |
| [setLaunchType](#method-setLaunchType) − `whale.management.setLaunchType(<span>string id</span>, <span>LaunchType launchType</span>, <span class="optional">function callback</span>)` |
| [generateAppForLink](#method-generateAppForLink) − `whale.management.generateAppForLink(<span>string url</span>, <span>string title</span>, <span class="optional">function callback</span>)` |
| Events |
| [onInstalled](#event-onInstalled) |
| [onUninstalled](#event-onUninstalled) |
| [onEnabled](#event-onEnabled) |
| [onDisabled](#event-onDisabled) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### IconInfo

<dd>Information about an icon belonging to an extension, app, or theme.</dd>

| properties |
|---|
| integer | size | 

A number representing the width and height of the icon. Likely values include (but are not limited to) 128, 48, 24, and 16.
 |
| string | url | 

The URL for this icon image. To display a grayscale version of the icon (to indicate that an extension is disabled, for example), append `?grayscale=true` to the URL.
 |

</div>

<div>

### LaunchType

<dd>These are all possible app launch types.</dd>

| Enum |
|---|
| `"OPEN_AS_REGULAR_TAB"`, `"OPEN_AS_PINNED_TAB"`, `"OPEN_AS_WINDOW"`, or `"OPEN_FULL_SCREEN"` |

</div>

<div>

### ExtensionDisabledReason

<dd>A reason the item is disabled.</dd>

| Enum |
|---|
| `"unknown"`, or `"permissions_increase"` |

</div>

<div>

### ExtensionType

<dd>The type of this extension, app, or theme.</dd>

| Enum |
|---|
| `"extension"`, `"hosted_app"`, `"packaged_app"`, `"legacy_packaged_app"`, or `"theme"` |

</div>

<div>

### ExtensionInstallType

<dd>How the extension was installed. One of
<var>admin</var>: The extension was installed because of an administrative policy,
<var>development</var>: The extension was loaded unpacked in developer mode,
<var>normal</var>: The extension was installed normally via a .crx file,
<var>sideload</var>: The extension was installed by other software on the machine,
<var>other</var>: The extension was installed by other means.</dd>

| Enum |
|---|
| `"admin"`, `"development"`, `"normal"`, `"sideload"`, or `"other"` |

</div>

<div>

### ExtensionInfo

<dd>Information about an installed extension, app, or theme.</dd>

| properties |
|---|
| string | id | 

The extension's unique identifier.
 |
| string | name | 

The name of this extension, app, or theme.
 |
| string | shortName | 

Since Chrome 31.

A short version of the name of this extension, app, or theme.
 |
| string | description | 

The description of this extension, app, or theme.
 |
| string | version | 

The [version](manifest/version) of this extension, app, or theme.
 |
| string | <span class="optional">(optional)</span> versionName | 

Since Chrome 50.

The [version name](manifest/version#version_name) of this extension, app, or theme if the manifest specified one.
 |
| boolean | mayDisable | 

Whether this extension can be disabled or uninstalled by the user.
 |
| boolean | <span class="optional">(optional)</span> mayEnable | 

Since Chrome 62.

Whether this extension can be enabled by the user. This is only returned for extensions which are not enabled.
 |
| boolean | enabled | 

Whether it is currently enabled or disabled.
 |
| [ExtensionDisabledReason](/extensions/management#type-ExtensionDisabledReason) | <span class="optional">(optional)</span> disabledReason | 

A reason the item is disabled.
 |
| boolean | isApp | 

**Deprecated** since Chrome 33. Please use [management.ExtensionInfo.type](/extensions/management#property-ExtensionInfo-type).

True if this is an app.
 |
| [ExtensionType](/extensions/management#type-ExtensionType) | type | 

Since Chrome 23.

The type of this extension, app, or theme.
 |
| string | <span class="optional">(optional)</span> appLaunchUrl | 

The launch url (only present for apps).
 |
| string | <span class="optional">(optional)</span> homepageUrl | 

The URL of the homepage of this extension, app, or theme.
 |
| string | <span class="optional">(optional)</span> updateUrl | 

The update URL of this extension, app, or theme.
 |
| boolean | offlineEnabled | 

Whether the extension, app, or theme declares that it supports offline.
 |
| string | optionsUrl | 

The url for the item's options page, if it has one.
 |
| array of [IconInfo](/extensions/management#type-IconInfo) | <span class="optional">(optional)</span> icons | 

A list of icon information. Note that this just reflects what was declared in the manifest, and the actual image at that url may be larger or smaller than what was declared, so you might consider using explicit width and height attributes on img tags referencing these images. See the [manifest documentation on icons](manifest/icons) for more details.
 |
| array of string | permissions | 

Returns a list of API based permissions.
 |
| array of string | hostPermissions | 

Returns a list of host based permissions.
 |
| [ExtensionInstallType](/extensions/management#type-ExtensionInstallType) | installType | 

Since Chrome 22.

How the extension was installed.
 |
| [LaunchType](/extensions/management#type-LaunchType) | <span class="optional">(optional)</span> launchType | 

Since Chrome 37.

The app launch type (only present for apps).
 |
| array of [LaunchType](/extensions/management#type-LaunchType) | <span class="optional">(optional)</span> availableLaunchTypes | 

Since Chrome 37.

The currently available launch types (only present for apps).
 |

</div>

## Methods

<div>

### getAll

<div class="summary">`whale.management.getAll(<span class="optional">function callback</span>)`</div>

<div class="description">

Returns a list of information about installed extensions and apps.

| Parameters |
|---|
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(array of [ExtensionInfo](/extensions/management#type-ExtensionInfo) result) <span class="subdued">{...}</span>;`

| array of [ExtensionInfo](/extensions/management#type-ExtensionInfo) | result |  |
|---|---|---|
 |

</div>

</div>

<div>

### get

<div class="summary">`whale.management.get(<span>string id</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Returns information about the installed extension, app, or theme that has the given ID.

| Parameters |
|---|
| string | id | 

The ID from an item of [management.ExtensionInfo](/extensions/management#type-ExtensionInfo).
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [ExtensionInfo](/extensions/management#type-ExtensionInfo) result) <span class="subdued">{...}</span>;`

| [ExtensionInfo](/extensions/management#type-ExtensionInfo) | result |  |
|---|---|---|
 |

</div>

</div>

<div>

### getSelf

<div class="summary">`whale.management.getSelf(<span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 39.

Returns information about the calling extension, app, or theme. Note: This function can be used without requesting the 'management' permission in the manifest.

| Parameters |
|---|
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [ExtensionInfo](/extensions/management#type-ExtensionInfo) result) <span class="subdued">{...}</span>;`

| [ExtensionInfo](/extensions/management#type-ExtensionInfo) | result |  |
|---|---|---|
 |

</div>

</div>

<div>

### getPermissionWarningsById

<div class="summary">`whale.management.getPermissionWarningsById(<span>string id</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Returns a list of [permission warnings](permission_warnings) for the given extension id.

| Parameters |
|---|
| string | id | 

The ID of an already installed extension.
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(array of string permissionWarnings) <span class="subdued">{...}</span>;`

| array of string | permissionWarnings |  |
|---|---|---|
 |

</div>

</div>

<div>

### getPermissionWarningsByManifest

<div class="summary">`whale.management.getPermissionWarningsByManifest(<span>string manifestStr</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Returns a list of [permission warnings](permission_warnings) for the given extension manifest string. Note: This function can be used without requesting the 'management' permission in the manifest.

| Parameters |
|---|
| string | manifestStr | 

Extension manifest JSON string.
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(array of string permissionWarnings) <span class="subdued">{...}</span>;`

| array of string | permissionWarnings |  |
|---|---|---|
 |

</div>

</div>

<div>

### setEnabled

<div class="summary">`whale.management.setEnabled(<span>string id</span>, <span>boolean enabled</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Enables or disables an app or extension. In most cases this function must be called in the context of a user gesture (e.g. an onclick handler for a button), and may present the user with a native confirmation UI as a way of preventing abuse.

| Parameters |
|---|
| string | id | 

This should be the id from an item of [management.ExtensionInfo](/extensions/management#type-ExtensionInfo).
 |
| boolean | enabled | 

Whether this item should be enabled or disabled.
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### uninstall

<div class="summary">`whale.management.uninstall(<span>string id</span>, <span class="optional">object options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Uninstalls a currently installed app or extension.

| Parameters |
|---|
| string | id | 

This should be the id from an item of [management.ExtensionInfo](/extensions/management#type-ExtensionInfo).
 |
| object | <span class="optional">(optional)</span> options | 

Since Chrome 21.

| boolean | <span class="optional">(optional)</span> showConfirmDialog | 
|---|---|

Whether or not a confirm-uninstall dialog should prompt the user. Defaults to false for self uninstalls. If an extension uninstalls another extension, this parameter is ignored and the dialog is always shown.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### uninstallSelf

<div class="summary">`whale.management.uninstallSelf(<span class="optional">object options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 26.

Uninstalls the calling extension. Note: This function can be used without requesting the 'management' permission in the manifest.

| Parameters |
|---|
| object | <span class="optional">(optional)</span> options | 

| boolean | <span class="optional">(optional)</span> showConfirmDialog | 
|---|---|

Whether or not a confirm-uninstall dialog should prompt the user. Defaults to false.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### launchApp

<div class="summary">`whale.management.launchApp(<span>string id</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Launches an application.

| Parameters |
|---|
| string | id | 

The extension id of the application.
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### createAppShortcut

<div class="summary">`whale.management.createAppShortcut(<span>string id</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 37.

Display options to create shortcuts for an app. On Mac, only packaged app shortcuts can be created.

| Parameters |
|---|
| string | id | 

Since Chrome 36.

This should be the id from an app item of [management.ExtensionInfo](/extensions/management#type-ExtensionInfo).
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### setLaunchType

<div class="summary">`whale.management.setLaunchType(<span>string id</span>, <span>[LaunchType](/extensions/management#type-LaunchType) launchType</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 37.

Set the launch type of an app.

| Parameters |
|---|
| string | id | 

This should be the id from an app item of [management.ExtensionInfo](/extensions/management#type-ExtensionInfo).
 |
| [LaunchType](/extensions/management#type-LaunchType) | launchType | 

The target launch type. Always check and make sure this launch type is in [ExtensionInfo.availableLaunchTypes](/extensions/management#property-ExtensionInfo-availableLaunchTypes), because the available launch types vary on different platforms and configurations.
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### generateAppForLink

<div class="summary">`whale.management.generateAppForLink(<span>string url</span>, <span>string title</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 37.

Generate an app for a URL. Returns the generated bookmark app.

| Parameters |
|---|
| string | url | 

The URL of a web page. The scheme of the URL can only be "http" or "https".
 |
| string | title | 

The title of the generated app.
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [ExtensionInfo](/extensions/management#type-ExtensionInfo) result) <span class="subdued">{...}</span>;`

| [ExtensionInfo](/extensions/management#type-ExtensionInfo) | result |  |
|---|---|---|
 |

</div>

</div>

## Events

<div>

### onInstalled

<div class="description">

Fired when an app or extension has been installed.

<div>

#### addListener

<div class="summary">`whale.management.onInstalled.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [ExtensionInfo](/extensions/management#type-ExtensionInfo) info) <span class="subdued">{...}</span>;`

| [ExtensionInfo](/extensions/management#type-ExtensionInfo) | info |  |
|---|---|---|
 |

</div>

</div>

</div>

</div>

<div>

### onUninstalled

<div class="description">

Fired when an app or extension has been uninstalled.

<div>

#### addListener

<div class="summary">`whale.management.onUninstalled.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string id) <span class="subdued">{...}</span>;`

| string | id | 
|---|---|

The id of the extension, app, or theme that was uninstalled.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onEnabled

<div class="description">

Fired when an app or extension has been enabled.

<div>

#### addListener

<div class="summary">`whale.management.onEnabled.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [ExtensionInfo](/extensions/management#type-ExtensionInfo) info) <span class="subdued">{...}</span>;`

| [ExtensionInfo](/extensions/management#type-ExtensionInfo) | info |  |
|---|---|---|
 |

</div>

</div>

</div>

</div>

<div>

### onDisabled

<div class="description">

Fired when an app or extension has been disabled.

<div>

#### addListener

<div class="summary">`whale.management.onDisabled.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [ExtensionInfo](/extensions/management#type-ExtensionInfo) info) <span class="subdued">{...}</span>;`

| [ExtensionInfo](/extensions/management#type-ExtensionInfo) | info |  |
|---|---|---|
 |

</div>

</div>

</div>

</div>

</div>

</section>