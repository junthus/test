# whale.identity

| Description: | Use the `whale.identity` API to get OAuth2 access tokens. |
|---|---|
| Availability: | Since Chrome 29. |
| Permissions: | <span class="code">"identity"</span> |
| Learn More: | [Identify User](app_identity) |

<section id="toc">

## Summary

| Types |
|---|
| [AccountInfo](#type-AccountInfo) |
| Methods |
| [getAccounts](#method-getAccounts) − `whale.identity.getAccounts(<span>function callback</span>)` |
| [getAuthToken](#method-getAuthToken) − `whale.identity.getAuthToken(<span class="optional">object details</span>, <span class="optional">function callback</span>)` |
| [getProfileUserInfo](#method-getProfileUserInfo) − `whale.identity.getProfileUserInfo(<span>function callback</span>)` |
| [removeCachedAuthToken](#method-removeCachedAuthToken) − `whale.identity.removeCachedAuthToken(<span>object details</span>, <span class="optional">function callback</span>)` |
| [launchWebAuthFlow](#method-launchWebAuthFlow) − `whale.identity.launchWebAuthFlow(<span>object details</span>, <span>function callback</span>)` |
| [getRedirectURL](#method-getRedirectURL) − `string whale.identity.getRedirectURL(<span class="optional">string path</span>)` |
| Events |
| [onSignInChanged](#event-onSignInChanged) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### AccountInfo

Since Chrome 32.

| properties |
|---|
| string | id | 

A unique identifier for the account. This ID will not change for the lifetime of the account.
 |

</div>

## Methods

<div>

### getAccounts

<div class="summary">`whale.identity.getAccounts(<span>function callback</span>)`</div>

<div class="description">

**Dev** channel only. [Learn more](api_index#dev_apis).

Retrieves a list of AccountInfo objects describing the accounts present on the profile.

`getAccounts` is only supported on dev channel.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [AccountInfo](/extensions/identity#type-AccountInfo) accounts) <span class="subdued">{...}</span>;`

| array of [AccountInfo](/extensions/identity#type-AccountInfo) | accounts |  |
|---|---|---|
 |

</div>

</div>

<div>

### getAuthToken

<div class="summary">`whale.identity.getAuthToken(<span class="optional">object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Gets an OAuth2 access token using the client ID and scopes specified in the [`oauth2` section of manifest.json](app_identity.html#update_manifest).

The Identity API caches access tokens in memory, so it's ok to call `getAuthToken` non-interactively any time a token is required. The token cache automatically handles expiration.

For a good user experience it is important interactive token requests are initiated by UI in your app explaining what the authorization is for. Failing to do this will cause your users to get authorization requests, or Chrome sign in screens if they are not signed in, with with no context. In particular, do not use `getAuthToken` interactively when your app is first launched.

| Parameters |
|---|
| object | <span class="optional">(optional)</span> details | 

Token options.

| boolean | <span class="optional">(optional)</span> interactive | 
|---|---|

Fetching a token may require the user to sign-in to Chrome, or approve the application's requested scopes. If the interactive flag is `true`, `getAuthToken` will prompt the user as necessary. When the flag is `false` or omitted, `getAuthToken` will return failure any time a prompt would be required.
 |
| [AccountInfo](/extensions/identity#type-AccountInfo) | <span class="optional">(optional)</span> account | 

Since Chrome 37.

The account ID whose token should be returned. If not specified, the primary account for the profile will be used.

`account` is only supported when the "enable-new-profile-management" flag is set.
 |
| array of string | <span class="optional">(optional)</span> scopes | 

Since Chrome 37.

A list of OAuth2 scopes to request.

When the `scopes` field is present, it overrides the list of scopes specified in manifest.json.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called with an OAuth2 access token as specified by the manifest, or undefined if there was an error.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(string token) <span class="subdued">{...}</span>;`

| string | <span class="optional">(optional)</span> token |  |
|---|---|---|
 |

</div>

</div>

<div>

### getProfileUserInfo

<div class="summary">`whale.identity.getProfileUserInfo(<span>function callback</span>)`</div>

<div class="description">

Since Chrome 37.

Retrieves email address and obfuscated gaia id of the user signed into a profile.

This API is different from identity.getAccounts in two ways. The information returned is available offline, and it only applies to the primary account for the profile.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object userInfo) <span class="subdued">{...}</span>;`

| object | userInfo | 
|---|---|

| string | email | 
|---|---|

An email address for the user account signed into the current profile. Empty if the user is not signed in or the `identity.email` manifest permission is not specified.
 |
| string | id | 

A unique identifier for the account. This ID will not change for the lifetime of the account. Empty if the user is not signed in or (in M41+) the `identity.email` manifest permission is not specified.
 |
 |
 |

</div>

</div>

<div>

### removeCachedAuthToken

<div class="summary">`whale.identity.removeCachedAuthToken(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Removes an OAuth2 access token from the Identity API's token cache.

If an access token is discovered to be invalid, it should be passed to removeCachedAuthToken to remove it from the cache. The app may then retrieve a fresh token with `getAuthToken`.

| Parameters |
|---|
| object | details | 

Token information.

| string | token | 
|---|---|

The specific token that should be removed from the cache.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called when the token has been removed from the cache.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### launchWebAuthFlow

<div class="summary">`whale.identity.launchWebAuthFlow(<span>object details</span>, <span>function callback</span>)`</div>

<div class="description">

Starts an auth flow at the specified URL.

This method enables auth flows with non-Google identity providers by launching a web view and navigating it to the first URL in the provider's auth flow. When the provider redirects to a URL matching the pattern `https://<app-id>.chromiumapp.org/*`, the window will close, and the final redirect URL will be passed to the <var>callback</var> function.

For a good user experience it is important interactive auth flows are initiated by UI in your app explaining what the authorization is for. Failing to do this will cause your users to get authorization requests with no context. In particular, do not launch an interactive auth flow when your app is first launched.

| Parameters |
|---|
| object | details | 

WebAuth flow options.

| string | url | 
|---|---|

The URL that initiates the auth flow.
 |
| boolean | <span class="optional">(optional)</span> interactive | 

Whether to launch auth flow in interactive mode.

Since some auth flows may immediately redirect to a result URL, `launchWebAuthFlow` hides its web view until the first navigation either redirects to the final URL, or finishes loading a page meant to be displayed.

If the interactive flag is `true`, the window will be displayed when a page load completes. If the flag is `false` or omitted, `launchWebAuthFlow` will return with an error if the initial navigation does not complete the flow.
 |
 |
| function | callback | 

Called with the URL redirected back to your application.

The _callback_ parameter should be a function that looks like this:

`function(string responseUrl) <span class="subdued">{...}</span>;`

| string | <span class="optional">(optional)</span> responseUrl |  |
|---|---|---|
 |

</div>

</div>

<div>

### getRedirectURL

<div class="summary">`string whale.identity.getRedirectURL(<span class="optional">string path</span>)`</div>

<div class="description">

Since Chrome 33.

Generates a redirect URL to be used in |launchWebAuthFlow|.

The generated URLs match the pattern `https://<app-id>.chromiumapp.org/*`.

| Parameters |
|---|
| string | <span class="optional">(optional)</span> path | 

The path appended to the end of the generated URL.
 |

</div>

</div>

## Events

<div>

### onSignInChanged

<div class="description">

Since Chrome 33.

Fired when signin state changes for an account on the user's profile.

<div>

#### addListener

<div class="summary">`whale.identity.onSignInChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [AccountInfo](/extensions/identity#type-AccountInfo) account, boolean signedIn) <span class="subdued">{...}</span>;`

| [AccountInfo](/extensions/identity#type-AccountInfo) | account | 
|---|---|

Since Chrome 32.
 |
| boolean | signedIn | 

Since Chrome 32.
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>