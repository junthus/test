# whale.cookies

| Description: | Use the `whale.cookies` API to query and modify cookies, and to be notified when they change. |
|---|---|
| Availability: | Since Chrome 20. |
| Permissions: | <span class="code">"cookies"</span>
[host permissions](declare_permissions#host-permissions) |

<section>

## Manifest

To use the cookies API, you must declare the "cookies" permission in your manifest, along with [host permissions](declare_permissions) for any hosts whose cookies you want to access. For example:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"permissions": [
          "cookies",
          "*://*.google.com"
        ]**,
        ...
      }
      </pre>

## Examples

You can find a simple example of using the cookies API in the [examples/api/cookies](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/docs/examples/api/cookies/) directory. For other examples and for help in viewing the source code, see [Samples](samples).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [SameSiteStatus](#type-SameSiteStatus) |
| [Cookie](#type-Cookie) |
| [CookieStore](#type-CookieStore) |
| [OnChangedCause](#type-OnChangedCause) |
| Methods |
| [get](#method-get) − `whale.cookies.get(<span>object details</span>, <span>function callback</span>)` |
| [getAll](#method-getAll) − `whale.cookies.getAll(<span>object details</span>, <span>function callback</span>)` |
| [set](#method-set) − `whale.cookies.set(<span>object details</span>, <span class="optional">function callback</span>)` |
| [remove](#method-remove) − `whale.cookies.remove(<span>object details</span>, <span class="optional">function callback</span>)` |
| [getAllCookieStores](#method-getAllCookieStores) − `whale.cookies.getAllCookieStores(<span>function callback</span>)` |
| Events |
| [onChanged](#event-onChanged) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### SameSiteStatus

<dd>A cookie's 'SameSite' state (https://tools.ietf.org/html/draft-west-first-party-cookies). 'no_restriction' corresponds to a cookie set without a 'SameSite' attribute, 'lax' to 'SameSite=Lax', and 'strict' to 'SameSite=Strict'.</dd>

| Enum |
|---|
| `"no_restriction"`, `"lax"`, or `"strict"` |

</div>

<div>

### Cookie

<dd>Represents information about an HTTP cookie.</dd>

| properties |
|---|
| string | name | 

The name of the cookie.
 |
| string | value | 

The value of the cookie.
 |
| string | domain | 

The domain of the cookie (e.g. "www.google.com", "example.com").
 |
| boolean | hostOnly | 

True if the cookie is a host-only cookie (i.e. a request's host must exactly match the domain of the cookie).
 |
| string | path | 

The path of the cookie.
 |
| boolean | secure | 

True if the cookie is marked as Secure (i.e. its scope is limited to secure channels, typically HTTPS).
 |
| boolean | httpOnly | 

True if the cookie is marked as HttpOnly (i.e. the cookie is inaccessible to client-side scripts).
 |
| [SameSiteStatus](/extensions/cookies#type-SameSiteStatus) | sameSite | 

Since Chrome 51.

The cookie's same-site status (i.e. whether the cookie is sent with cross-site requests).
 |
| boolean | session | 

True if the cookie is a session cookie, as opposed to a persistent cookie with an expiration date.
 |
| double | <span class="optional">(optional)</span> expirationDate | 

The expiration date of the cookie as the number of seconds since the UNIX epoch. Not provided for session cookies.
 |
| string | storeId | 

The ID of the cookie store containing this cookie, as provided in getAllCookieStores().
 |

</div>

<div>

### CookieStore

<dd>Represents a cookie store in the browser. An incognito mode window, for instance, uses a separate cookie store from a non-incognito window.</dd>

| properties |
|---|
| string | id | 

The unique identifier for the cookie store.
 |
| array of integer | tabIds | 

Identifiers of all the browser tabs that share this cookie store.
 |

</div>

<div>

### OnChangedCause

<dd>The underlying reason behind the cookie's change. If a cookie was inserted, or removed via an explicit call to "whale.cookies.remove", "cause" will be "explicit". If a cookie was automatically removed due to expiry, "cause" will be "expired". If a cookie was removed due to being overwritten with an already-expired expiration date, "cause" will be set to "expired_overwrite". If a cookie was automatically removed due to garbage collection, "cause" will be "evicted". If a cookie was automatically removed due to a "set" call that overwrote it, "cause" will be "overwrite". Plan your response accordingly.</dd>

| Enum |
|---|
| `"evicted"`, `"expired"`, `"explicit"`, `"expired_overwrite"`, or `"overwrite"` |

</div>

## Methods

<div>

### get

<div class="summary">`whale.cookies.get(<span>object details</span>, <span>function callback</span>)`</div>

<div class="description">

Retrieves information about a single cookie. If more than one cookie of the same name exists for the given URL, the one with the longest path will be returned. For cookies with the same path length, the cookie with the earliest creation time will be returned.

| Parameters |
|---|
| object | details | 

Details to identify the cookie being retrieved.

| string | url | 
|---|---|

The URL with which the cookie to retrieve is associated. This argument may be a full URL, in which case any data following the URL path (e.g. the query string) is simply ignored. If host permissions for this URL are not specified in the manifest file, the API call will fail.
 |
| string | name | 

The name of the cookie to retrieve.
 |
| string | <span class="optional">(optional)</span> storeId | 

The ID of the cookie store in which to look for the cookie. By default, the current execution context's cookie store will be used.
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Cookie](/extensions/cookies#type-Cookie) cookie) <span class="subdued">{...}</span>;`

| [Cookie](/extensions/cookies#type-Cookie) | <span class="optional">(optional)</span> cookie | 
|---|---|

Contains details about the cookie. This parameter is null if no such cookie was found.
 |
 |

</div>

</div>

<div>

### getAll

<div class="summary">`whale.cookies.getAll(<span>object details</span>, <span>function callback</span>)`</div>

<div class="description">

Retrieves all cookies from a single cookie store that match the given information. The cookies returned will be sorted, with those with the longest path first. If multiple cookies have the same path length, those with the earliest creation time will be first.

| Parameters |
|---|
| object | details | 

Information to filter the cookies being retrieved.

| string | <span class="optional">(optional)</span> url | 
|---|---|

Restricts the retrieved cookies to those that would match the given URL.
 |
| string | <span class="optional">(optional)</span> name | 

Filters the cookies by name.
 |
| string | <span class="optional">(optional)</span> domain | 

Restricts the retrieved cookies to those whose domains match or are subdomains of this one.
 |
| string | <span class="optional">(optional)</span> path | 

Restricts the retrieved cookies to those whose path exactly matches this string.
 |
| boolean | <span class="optional">(optional)</span> secure | 

Filters the cookies by their Secure property.
 |
| boolean | <span class="optional">(optional)</span> session | 

Filters out session vs. persistent cookies.
 |
| string | <span class="optional">(optional)</span> storeId | 

The cookie store to retrieve cookies from. If omitted, the current execution context's cookie store will be used.
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [Cookie](/extensions/cookies#type-Cookie) cookies) <span class="subdued">{...}</span>;`

| array of [Cookie](/extensions/cookies#type-Cookie) | cookies | 
|---|---|

All the existing, unexpired cookies that match the given cookie info.
 |
 |

</div>

</div>

<div>

### set

<div class="summary">`whale.cookies.set(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Sets a cookie with the given cookie data; may overwrite equivalent cookies if they exist.

| Parameters |
|---|
| object | details | 

Details about the cookie being set.

| string | url | 
|---|---|

The request-URI to associate with the setting of the cookie. This value can affect the default domain and path values of the created cookie. If host permissions for this URL are not specified in the manifest file, the API call will fail.
 |
| string | <span class="optional">(optional)</span> name | 

The name of the cookie. Empty by default if omitted.
 |
| string | <span class="optional">(optional)</span> value | 

The value of the cookie. Empty by default if omitted.
 |
| string | <span class="optional">(optional)</span> domain | 

The domain of the cookie. If omitted, the cookie becomes a host-only cookie.
 |
| string | <span class="optional">(optional)</span> path | 

The path of the cookie. Defaults to the path portion of the url parameter.
 |
| boolean | <span class="optional">(optional)</span> secure | 

Whether the cookie should be marked as Secure. Defaults to false.
 |
| boolean | <span class="optional">(optional)</span> httpOnly | 

Whether the cookie should be marked as HttpOnly. Defaults to false.
 |
| [SameSiteStatus](/extensions/cookies#type-SameSiteStatus) | <span class="optional">(optional)</span> sameSite | 

Since Chrome 51.

The cookie's same-site status: defaults to 'no_restriction'.
 |
| double | <span class="optional">(optional)</span> expirationDate | 

The expiration date of the cookie as the number of seconds since the UNIX epoch. If omitted, the cookie becomes a session cookie.
 |
| string | <span class="optional">(optional)</span> storeId | 

The ID of the cookie store in which to set the cookie. By default, the cookie is set in the current execution context's cookie store.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [Cookie](/extensions/cookies#type-Cookie) cookie) <span class="subdued">{...}</span>;`

| [Cookie](/extensions/cookies#type-Cookie) | <span class="optional">(optional)</span> cookie | 
|---|---|

Contains details about the cookie that's been set. If setting failed for any reason, this will be "null", and "whale.runtime.lastError" will be set.
 |
 |

</div>

</div>

<div>

### remove

<div class="summary">`whale.cookies.remove(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Deletes a cookie by name.

| Parameters |
|---|
| object | details | 

Information to identify the cookie to remove.

| string | url | 
|---|---|

The URL associated with the cookie. If host permissions for this URL are not specified in the manifest file, the API call will fail.
 |
| string | name | 

The name of the cookie to remove.
 |
| string | <span class="optional">(optional)</span> storeId | 

The ID of the cookie store to look in for the cookie. If unspecified, the cookie is looked for by default in the current execution context's cookie store.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | <span class="optional">(optional)</span> details | 
|---|---|

Contains details about the cookie that's been removed. If removal failed for any reason, this will be "null", and "whale.runtime.lastError" will be set.

| string | url | 
|---|---|

The URL associated with the cookie that's been removed.
 |
| string | name | 

The name of the cookie that's been removed.
 |
| string | storeId | 

The ID of the cookie store from which the cookie was removed.
 |
 |
 |

</div>

</div>

<div>

### getAllCookieStores

<div class="summary">`whale.cookies.getAllCookieStores(<span>function callback</span>)`</div>

<div class="description">

Lists all existing cookie stores.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [CookieStore](/extensions/cookies#type-CookieStore) cookieStores) <span class="subdued">{...}</span>;`

| array of [CookieStore](/extensions/cookies#type-CookieStore) | cookieStores | 
|---|---|

All the existing cookie stores.
 |
 |

</div>

</div>

## Events

<div>

### onChanged

<div class="description">

Fired when a cookie is set or removed. As a special case, note that updating a cookie's properties is implemented as a two step process: the cookie to be updated is first removed entirely, generating a notification with "cause" of "overwrite" . Afterwards, a new cookie is written with the updated values, generating a second notification with "cause" "explicit".

<div>

#### addListener

<div class="summary">`whale.cookies.onChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object changeInfo) <span class="subdued">{...}</span>;`

| object | changeInfo | 
|---|---|

| boolean | removed | 
|---|---|

True if a cookie was removed.
 |
| [Cookie](/extensions/cookies#type-Cookie) | cookie | 

Information about the cookie that was set or removed.
 |
| [OnChangedCause](/extensions/cookies#type-OnChangedCause) | cause | 

The underlying reason behind the cookie's change.
 |
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>