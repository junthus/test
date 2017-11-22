# whale.contentSettings

| Description: | Use the `whale.contentSettings` API to change settings that control whether websites can use features such as cookies, JavaScript, and plugins. More generally speaking, content settings allow you to customize Chrome's behavior on a per-site basis instead of globally. |
|---|---|
| Availability: | Since Chrome 19. |
| Permissions: | <span class="code">"contentSettings"</span> |

<section>

## Manifest

You must declare the "contentSettings" permission in your extension's manifest to use the API. For example:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"permissions": [
          "contentSettings"
        ]**,
        ...
      }
      </pre>

## Content setting patterns

You can use patterns to specify the websites that each content setting affects. For example, `http://*.youtube.com/*` specifies youtube.com and all of its subdomains. The syntax for content setting patterns is the same as for [match patterns](match_patterns), with a few differences:

*   For `http`, `https`, and `ftp` URLs, the path must be a wildcard (`/*`). For `file` URLs, the path must be completely specified and **must not** contain wildcards.
*   In contrast to match patterns, content setting patterns can specify a port number. If a port number is specified, the pattern only matches websites with that port. If no port number is specified, the pattern matches all ports.

### Pattern precedence

When more than one content setting rule applies for a given site, the rule with the more specific pattern takes precedence.

For example, the following patterns are ordered by precedence:

1.  `http://www.example.com/*`
2.  `http://*.example.com/*` (matching example.com and all subdomains)
3.  `<all_urls>` (matching every URL)

Three kinds of wildcards affect how specific a pattern is:

*   Wildcards in the port (for example `http://www.example.com:*/*`)
*   Wildcards in the scheme (for example `*://www.example.com:123/*`)
*   Wildcards in the hostname (for example `http://*.example.com:123/*`)

If a pattern is more specific than another pattern in one part but less specific in another part, the different parts are checked in the following order: hostname, scheme, port. For example, the following patterns are ordered by precedence:

1.  `http://www.example.com:*/*`
    Specifies the hostname and scheme.
2.  `*:/www.example.com:123/*`
    Not as high, because although it specifies the hostname, it doesn't specify the scheme.
3.  `http://*.example.com:123/*`
    Lower because although it specifies the port and scheme, it has a wildcard in the hostname.

## Primary and secondary patterns

The URL taken into account when deciding which content setting to apply depends on the content type. For example, for [contentSettings.notifications](/extensions/contentSettings#property-notifications) settings are based on the URL shown in the omnibox. This URL is called the "primary" URL.

Some content types can take additional URLs into account. For example, whether a site is allowed to set a [contentSettings.cookies](/extensions/contentSettings#property-cookies) is decided based on the URL of the HTTP request (which is the primary URL in this case) as well as the URL shown in the omnibox (which is called the "secondary" URL).

If multiple rules have primary and secondary patterns, the rule with the more specific primary pattern takes precedence. If there multiple rules have the same primary pattern, the rule with the more specific secondary pattern takes precedence. For example, the following list of primary/secondary pattern pairs is ordered by precedence:

| Precedence | Primary pattern | Secondary pattern |
|---|---|---|
| 1 | `http://www.moose.com/*`, | `http://www.wombat.com/*` |
| 2 | `http://www.moose.com/*`, | `<all_urls>` |
| 3 | `<all_urls>`, | `http://www.wombat.com/*` |
| 4 | `<all_urls>`, | `<all_urls>` |

## Resource identifiers

Resource identifiers allow you to specify content settings for specific subtypes of a content type. Currently, the only content type that supports resource identifiers is [contentSettings.plugins](/extensions/contentSettings#property-plugins), where a resource identifier identifies a specific plugin. When applying content settings, first the settings for the specific plugin are checked. If there are no settings found for the specific plugin, the general content settings for plugins are checked.

For example, if a content setting rule has the resource identifier `adobe-flash-player` and the pattern `<all_urls>`, it takes precedence over a rule without a resource identifier and the pattern `http://www.example.com/*`, even if that pattern is more specific.

You can get a list of resource identifiers for a content type by calling the [contentSettings.ContentSetting.getResourceIdentifiers](/extensions/contentSettings#method-ContentSetting-getResourceIdentifiers) method. The returned list can change with the set of installed plugins on the user's machine, but Chrome tries to keep the identifiers stable across plugin updates.

## Examples

You can find samples of this API on the [sample page](samples#search:contentSettings).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [ResourceIdentifier](#type-ResourceIdentifier) |
| [Scope](#type-Scope) |
| [ContentSetting](#type-ContentSetting) |
| [CookiesContentSetting](#type-CookiesContentSetting) |
| [ImagesContentSetting](#type-ImagesContentSetting) |
| [JavascriptContentSetting](#type-JavascriptContentSetting) |
| [LocationContentSetting](#type-LocationContentSetting) |
| [PluginsContentSetting](#type-PluginsContentSetting) |
| [PopupsContentSetting](#type-PopupsContentSetting) |
| [NotificationsContentSetting](#type-NotificationsContentSetting) |
| [FullscreenContentSetting](#type-FullscreenContentSetting) |
| [MouselockContentSetting](#type-MouselockContentSetting) |
| [MicrophoneContentSetting](#type-MicrophoneContentSetting) |
| [CameraContentSetting](#type-CameraContentSetting) |
| [PpapiBrokerContentSetting](#type-PpapiBrokerContentSetting) |
| [MultipleAutomaticDownloadsContentSetting](#type-MultipleAutomaticDownloadsContentSetting) |
| Properties |
| [cookies](#property-cookies) |
| [images](#property-images) |
| [javascript](#property-javascript) |
| [location](#property-location) |
| [plugins](#property-plugins) |
| [popups](#property-popups) |
| [notifications](#property-notifications) |
| [fullscreen](#property-fullscreen) |
| [mouselock](#property-mouselock) |
| [microphone](#property-microphone) |
| [camera](#property-camera) |
| [unsandboxedPlugins](#property-unsandboxedPlugins) |
| [automaticDownloads](#property-automaticDownloads) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### ResourceIdentifier

<dd>The only content type using resource identifiers is [contentSettings.plugins](/extensions/contentSettings#property-plugins). For more information, see [Resource Identifiers](contentSettings#resource-identifiers).</dd>

| properties |
|---|
| string | id | 

The resource identifier for the given content type.
 |
| string | <span class="optional">(optional)</span> description | 

A human readable description of the resource.
 |

</div>

<div>

### Scope

<dd>The scope of the ContentSetting. One of
<var>regular</var>: setting for regular profile (which is inherited by the incognito profile if not overridden elsewhere),
<var>incognito_session_only</var>: setting for incognito profile that can only be set during an incognito session and is deleted when the incognito session ends (overrides regular settings).</dd>

| Enum |
|---|
| `"regular"`, or `"incognito_session_only"` |

</div>

<div>

### ContentSetting

| methods |
|---|
| 

<div>

#### clear

<div class="summary">`ContentSetting.clear(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clear all content setting rules set by this extension.

| Parameters |
|---|
| object | details | 

| [Scope](/extensions/contentSettings#type-Scope) | <span class="optional">(optional)</span> scope | 
|---|---|

Where to clear the setting (default: regular).
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |
| 

<div>

#### get

<div class="summary">`ContentSetting.get(<span>object details</span>, <span>function callback</span>)`</div>

<div class="description">

Gets the current content setting for a given pair of URLs.

| Parameters |
|---|
| object | details | 

| string | primaryUrl | 
|---|---|

The primary URL for which the content setting should be retrieved. Note that the meaning of a primary URL depends on the content type.
 |
| string | <span class="optional">(optional)</span> secondaryUrl | 

The secondary URL for which the content setting should be retrieved. Defaults to the primary URL. Note that the meaning of a secondary URL depends on the content type, and not all content types use secondary URLs.
 |
| [ResourceIdentifier](/extensions/contentSettings#type-ResourceIdentifier) | <span class="optional">(optional)</span> resourceIdentifier | 

A more specific identifier of the type of content for which the settings should be retrieved.
 |
| boolean | <span class="optional">(optional)</span> incognito | 

Whether to check the content settings for an incognito session. (default false)
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| any | setting | 
|---|---|

The content setting. See the description of the individual ContentSetting objects for the possible values.
 |
 |
 |

</div>

</div>
 |
| 

<div>

#### set

<div class="summary">`ContentSetting.set(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Applies a new content setting rule.

| Parameters |
|---|
| object | details | 

| string | primaryPattern | 
|---|---|

The pattern for the primary URL. For details on the format of a pattern, see [Content Setting Patterns](contentSettings#patterns).
 |
| string | <span class="optional">(optional)</span> secondaryPattern | 

The pattern for the secondary URL. Defaults to matching all URLs. For details on the format of a pattern, see [Content Setting Patterns](contentSettings#patterns).
 |
| [ResourceIdentifier](/extensions/contentSettings#type-ResourceIdentifier) | <span class="optional">(optional)</span> resourceIdentifier | 

The resource identifier for the content type.
 |
| any | setting | 

The setting applied by this rule. See the description of the individual ContentSetting objects for the possible values.
 |
| [Scope](/extensions/contentSettings#type-Scope) | <span class="optional">(optional)</span> scope | 

Where to set the setting (default: regular).
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |
| 

<div>

#### getResourceIdentifiers

<div class="summary">`ContentSetting.getResourceIdentifiers(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [ResourceIdentifier](/extensions/contentSettings#type-ResourceIdentifier) resourceIdentifiers) <span class="subdued">{...}</span>;`

| array of [ResourceIdentifier](/extensions/contentSettings#type-ResourceIdentifier) | <span class="optional">(optional)</span> resourceIdentifiers | 
|---|---|

A list of resource identifiers for this content type, or <var>undefined</var> if this content type does not use resource identifiers.
 |
 |

</div>

</div>
 |

</div>

<div>

### CookiesContentSetting

| Enum |
|---|
| `"allow"`, `"block"`, or `"session_only"` |

</div>

<div>

### ImagesContentSetting

| Enum |
|---|
| `"allow"`, or `"block"` |

</div>

<div>

### JavascriptContentSetting

| Enum |
|---|
| `"allow"`, or `"block"` |

</div>

<div>

### LocationContentSetting

| Enum |
|---|
| `"allow"`, `"block"`, or `"ask"` |

</div>

<div>

### PluginsContentSetting

| Enum |
|---|
| `"allow"`, `"block"`, or `"detect_important_content"` |

</div>

<div>

### PopupsContentSetting

| Enum |
|---|
| `"allow"`, or `"block"` |

</div>

<div>

### NotificationsContentSetting

| Enum |
|---|
| `"allow"`, `"block"`, or `"ask"` |

</div>

<div>

### FullscreenContentSetting

| Enum |
|---|
| `"allow"` |

</div>

<div>

### MouselockContentSetting

| Enum |
|---|
| `"allow"` |

</div>

<div>

### MicrophoneContentSetting

| Enum |
|---|
| `"allow"`, `"block"`, or `"ask"` |

</div>

<div>

### CameraContentSetting

| Enum |
|---|
| `"allow"`, `"block"`, or `"ask"` |

</div>

<div>

### PpapiBrokerContentSetting

| Enum |
|---|
| `"allow"`, `"block"`, or `"ask"` |

</div>

<div>

### MultipleAutomaticDownloadsContentSetting

| Enum |
|---|
| `"allow"`, `"block"`, or `"ask"` |

</div>

## Properties

| <span class="type_name">[ContentSetting](/extensions/contentSettings#type-ContentSetting)</span> | `whale.contentSettings.cookies` | Whether to allow cookies and other local data to be set by websites. One of
<var>allow</var>: Accept cookies,
<var>block</var>: Block cookies,
<var>session_only</var>: Accept cookies only for the current session.
Default is <var>allow</var>.
The primary URL is the URL representing the cookie origin. The secondary URL is the URL of the top-level frame. |
| <span class="type_name">[ContentSetting](/extensions/contentSettings#type-ContentSetting)</span> | `whale.contentSettings.images` | Whether to show images. One of
<var>allow</var>: Show images,
<var>block</var>: Don't show images.
Default is <var>allow</var>.
The primary URL is the URL of the top-level frame. The secondary URL is the URL of the image. |
| <span class="type_name">[ContentSetting](/extensions/contentSettings#type-ContentSetting)</span> | `whale.contentSettings.javascript` | Whether to run JavaScript. One of
<var>allow</var>: Run JavaScript,
<var>block</var>: Don't run JavaScript.
Default is <var>allow</var>.
The primary URL is the URL of the top-level frame. The secondary URL is not used. |
| <span class="type_name">[ContentSetting](/extensions/contentSettings#type-ContentSetting)</span> | `whale.contentSettings.location` | 

Since Chrome 42.

Whether to allow Geolocation. One of
<var>allow</var>: Allow sites to track your physical location,
<var>block</var>: Don't allow sites to track your physical location,
<var>ask</var>: Ask before allowing sites to track your physical location.
Default is <var>ask</var>.
The primary URL is the URL of the document which requested location data. The secondary URL is the URL of the top-level frame (which may or may not differ from the requesting URL). |
| <span class="type_name">[ContentSetting](/extensions/contentSettings#type-ContentSetting)</span> | `whale.contentSettings.plugins` | Whether to run plugins. One of
<var>allow</var>: Run plugins automatically,
<var>block</var>: Don't run plugins automatically,
<var>detect_important_content</var>: Only run automatically those plugins that are detected as the website's main content.
The primary URL is the URL of the top-level frame. The secondary URL is not used. |
| <span class="type_name">[ContentSetting](/extensions/contentSettings#type-ContentSetting)</span> | `whale.contentSettings.popups` | Whether to allow sites to show pop-ups. One of
<var>allow</var>: Allow sites to show pop-ups,
<var>block</var>: Don't allow sites to show pop-ups.
Default is <var>block</var>.
The primary URL is the URL of the top-level frame. The secondary URL is not used. |
| <span class="type_name">[ContentSetting](/extensions/contentSettings#type-ContentSetting)</span> | `whale.contentSettings.notifications` | Whether to allow sites to show desktop notifications. One of
<var>allow</var>: Allow sites to show desktop notifications,
<var>block</var>: Don't allow sites to show desktop notifications,
<var>ask</var>: Ask when a site wants to show desktop notifications.
Default is <var>ask</var>.
The primary URL is the URL of the document which wants to show the notification. The secondary URL is not used. |
| <span class="type_name">[ContentSetting](/extensions/contentSettings#type-ContentSetting)</span> | `whale.contentSettings.fullscreen` | 

Since Chrome 42.

_Deprecated._ No longer has any effect. Fullscreen permission is now automatically granted for all sites. Value is always <var>allow</var>. |
| <span class="type_name">[ContentSetting](/extensions/contentSettings#type-ContentSetting)</span> | `whale.contentSettings.mouselock` | 

Since Chrome 42.

_Deprecated._ No longer has any effect. Mouse lock permission is now automatically granted for all sites. Value is always <var>allow</var>. |
| <span class="type_name">[ContentSetting](/extensions/contentSettings#type-ContentSetting)</span> | `whale.contentSettings.microphone` | 

Since Chrome 46.

Whether to allow sites to access the microphone. One of
<var>allow</var>: Allow sites to access the microphone,
<var>block</var>: Don't allow sites to access the microphone,
<var>ask</var>: Ask when a site wants to access the microphone.
Default is <var>ask</var>.
The primary URL is the URL of the document which requested microphone access. The secondary URL is not used.
NOTE: The 'allow' setting is not valid if both patterns are '<all_urls>'.</all_urls> |
| <span class="type_name">[ContentSetting](/extensions/contentSettings#type-ContentSetting)</span> | `whale.contentSettings.camera` | 

Since Chrome 46.

Whether to allow sites to access the camera. One of
<var>allow</var>: Allow sites to access the camera,
<var>block</var>: Don't allow sites to access the camera,
<var>ask</var>: Ask when a site wants to access the camera.
Default is <var>ask</var>.
The primary URL is the URL of the document which requested camera access. The secondary URL is not used.
NOTE: The 'allow' setting is not valid if both patterns are '<all_urls>'.</all_urls> |
| <span class="type_name">[ContentSetting](/extensions/contentSettings#type-ContentSetting)</span> | `whale.contentSettings.unsandboxedPlugins` | 

Since Chrome 42.

Whether to allow sites to run plugins unsandboxed. One of
<var>allow</var>: Allow sites to run plugins unsandboxed,
<var>block</var>: Don't allow sites to run plugins unsandboxed,
<var>ask</var>: Ask when a site wants to run a plugin unsandboxed.
Default is <var>ask</var>.
The primary URL is the URL of the top-level frame. The secondary URL is not used. |
| <span class="type_name">[ContentSetting](/extensions/contentSettings#type-ContentSetting)</span> | `whale.contentSettings.automaticDownloads` | 

Since Chrome 42.

Whether to allow sites to download multiple files automatically. One of
<var>allow</var>: Allow sites to download multiple files automatically,
<var>block</var>: Don't allow sites to download multiple files automatically,
<var>ask</var>: Ask when a site wants to download files automatically after the first file.
Default is <var>ask</var>.
The primary URL is the URL of the top-level frame. The secondary URL is not used. |

</div>

</section>