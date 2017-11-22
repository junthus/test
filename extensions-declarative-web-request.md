# whale.declarativeWebRequest

| Description: | _**Note:** this API is currently on hold, without concrete plans to move to stable._ Use the `whale.declarativeWebRequest` API to intercept, block, or modify requests in-flight. It is significantly faster than the [`whale.webRequest` API](webRequest) because you can register rules that are evaluated in the browser rather than the JavaScript engine with reduces roundtrip latencies and allows higher efficiency. |
|---|---|
| Availability: | **Beta** and **Dev** channels only. [Learn more](api_index#beta_apis). |
| Permissions: | <span class="code">"declarativeWebRequest"</span>
[host permissions](declare_permissions#host-permissions) |

<section>

## Manifest

You must declare the "declarativeWebRequest" permission in the [extension manifest](manifest) to use this API, along with [host permissions](declare_permissions).

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
      **  "permissions": [
          "declarativeWebRequest",
          "*://*/*"
        ]**,
        ...
      }
      </pre>

Note that certain types of non-sensitive actions do not require host permissions:

*   `CancelRequest`
*   `IgnoreRules`
*   `RedirectToEmptyDocument`
*   `RedirectToTransparentImage`

The `SendMessageToExtension` action requires host permissions for any hosts whose network requests you want to trigger a message.

All other actions require host permissions to all URLs.

As an example, if `"*://*.google.com/*"` is the only host permission an extension has, than such an extension may set up a rule to

*   cancel a request to "http://www.google.com" or "http://anything.else.com"
*   send a message when navigating to "http://www.google.com" but not to "http://something.else.com"

The extension cannot set up a rule to redirect "http://www.google.com" to "http://mail.google.com".

## Rules

The Declarative Web Request API follows the concepts of the [Declarative API](events#declarative). You can register rules to the `whale.declarativeWebRequest.onRequest` event object.

The Declarative Web Request API supports a single type of match criteria, the `RequestMatcher`. The `RequestMatcher` matches network requests if and only if all listed criteria are met. The following `RequestMatcher` would match a network request when the user enters "http://www.example.com" in the URL bar:

<pre>      var matcher = new whale.declarativeWebRequest.RequestMatcher({
        url: { hostSuffix: 'example.com', schemes: ['http'] },
        resourceType: ['main_frame']
        });
      </pre>

Requests to "https://www.example.com" would be rejected by the `RequestMatcher` due to the scheme. Also all requests for an embedded iframe would be rejected due to the `resourceType`.

**Note:** All conditions and actions are created via a constructor as shown in the example above.

In order to cancel all requests to "example.com", you can define a rule as follows:

<pre>      var rule = {
        conditions: [
          new whale.declarativeWebRequest.RequestMatcher({
            url: { hostSuffix: 'example.com' } })
        ],
        actions: [
          new whale.declarativeWebRequest.CancelRequest()
        ]};
      </pre>

In order to cancel all requests to "example.com" and "foobar.com", you can add a second condition, as each condition is sufficient to trigger all specified actions:

<pre>      var rule2 = {
        conditions: [
          new whale.declarativeWebRequest.RequestMatcher({
            url: { hostSuffix: 'example.com' } }),
          new whale.declarativeWebRequest.RequestMatcher({
            url: { hostSuffix: 'foobar.com' } })
        ],
        actions: [
          new whale.declarativeWebRequest.CancelRequest()
        ]};
      </pre>

Register rules as follows:

<pre>      whale.declarativeWebRequest.onRequest.addRules([rule2]);
      </pre>

**Note:** You should always register or unregister rules in bulk rather than individually because each of these operations recreates internal data structures. This re-creation is computationally expensive but facilitates a very fast URL matching algorithm for hundreds of thousands of URLs. The [Performance section](events#performance) of the [Events](/extensions/events) API provides further performance tips.

## Evaluation of conditions and actions

The Declarative Web Request API follows the [Life cycle model for web requests](webRequest#life_cycle) of the [Web Request API](webRequest). This means that conditions can only be tested at specific stages of a web request and, likewise, actions can also only be executed at specific stages. The following tables list the request stages that are compatible with conditions and actions.

| Request stages during which condition attributes can be processed. |
|---|
| Condition attribute | onBeforeRequest | onBeforeSendHeaders | onHeadersReceived | onAuthRequired |
| url | ✓ | ✓ | ✓ | ✓ |
| resourceType | ✓ | ✓ | ✓ | ✓ |
| contentType |  |  | ✓ |  |
| excludeContentType |  |  | ✓ |  |
| responseHeaders |  |  | ✓ |  |
| excludeResponseHeaders |  |  | ✓ |  |
| requestHeaders |  | ✓ |  |  |
| excludeRequestHeaders |  | ✓ |  |  |
| thirdPartyForCookies | ✓ | ✓ | ✓ | ✓ |
| Request stages during which actions can be executed. |
| Event | onBeforeRequest | onBeforeSendHeaders | onHeadersReceived | onAuthRequired |
| AddRequestCookie |  | ✓ |  |  |
| AddResponseCookie |  |  | ✓ |  |
| AddResponseHeader |  |  | ✓ |  |
| CancelRequest | ✓ | ✓ | ✓ | ✓ |
| EditRequestCookie |  | ✓ |  |  |
| EditResponseCookie |  |  | ✓ |  |
| IgnoreRules | ✓ | ✓ | ✓ | ✓ |
| RedirectByRegEx | ✓ |  | ✓ |  |
| RedirectRequest | ✓ |  | ✓ |  |
| RedirectToEmptyDocument | ✓ |  | ✓ |  |
| RedirectToTransparentImage | ✓ |  | ✓ |  |
| RemoveRequestCookie |  | ✓ |  |  |
| RemoveRequestHeader |  | ✓ |  |  |
| RemoveResponseCookie |  |  | ✓ |  |
| RemoveResponseHeader |  |  | ✓ |  |
| SendMessageToExtension | ✓ | ✓ | ✓ | ✓ |
| SetRequestHeader |  | ✓ |  |  |

**Note:** Applicable stages can be further constrained by using the "stages" attribute.

**Note:** Redirects initiated by a redirect action use the original request method for the redirect, with one exception: If the redirect is initiated at the onHeadersReceived stage, then the redirect will be issued using the GET method.

**Example:** It is possible to combine a `new whale.declarativeWebRequest.RequestMatcher({contentType: ["image/jpeg"]})` condition with a `new whale.declarativeWebRequest.CancelRequest()` action because both of them can be evaluated in the onHeadersReceived stage. It is, however, impossible to combine the request matcher with a `new whale.declarativeWebRequest.SetRequestHeader()` because request headers cannot be set any more by the time the content type has been terminated.

## Using priorities to override rules

Rules can be associated with priorities as described in the [Events API](events#declarative). This mechanism can be used to express exceptions. The following example will block all requests to images named "evil.jpg" except on the server "myserver.com".

<pre>      var rule1 = {
        priority: 100,
        conditions: [
          new whale.declarativeWebRequest.RequestMatcher({
              url: { pathEquals: 'evil.jpg' } })
        ],
        actions: [
          new whale.declarativeWebRequest.CancelRequest()
        ]
      };
      var rule2 = {
        priority: 1000,
        conditions: [
          new whale.declarativeWebRequest.RequestMatcher({
            url: { hostSuffix: '.myserver.com' } })
        ],
        actions: [
          new whale.declarativeWebRequest.IgnoreRules({
            lowerPriorityThan: 1000 })
        ]
      };
      whale.declarativeWebRequest.onRequest.addRules([rule1, rule2]);
      </pre>

It is important to recognize that the `IgnoreRules` action is not persisted across [request stages](#evaluation). All conditions of all rules are evaluated at each stage of a web request. If an `IgnoreRules` action is executed, it applies only to other actions that are executed for the same web request in the same stage.

</section>

<section id="toc">

## Summary

| Types |
|---|
| [Stage](#type-Stage) |
| [HeaderFilter](#type-HeaderFilter) |
| [RequestMatcher](#type-RequestMatcher) |
| [CancelRequest](#type-CancelRequest) |
| [RedirectRequest](#type-RedirectRequest) |
| [RedirectToTransparentImage](#type-RedirectToTransparentImage) |
| [RedirectToEmptyDocument](#type-RedirectToEmptyDocument) |
| [RedirectByRegEx](#type-RedirectByRegEx) |
| [SetRequestHeader](#type-SetRequestHeader) |
| [RemoveRequestHeader](#type-RemoveRequestHeader) |
| [AddResponseHeader](#type-AddResponseHeader) |
| [RemoveResponseHeader](#type-RemoveResponseHeader) |
| [IgnoreRules](#type-IgnoreRules) |
| [SendMessageToExtension](#type-SendMessageToExtension) |
| [RequestCookie](#type-RequestCookie) |
| [ResponseCookie](#type-ResponseCookie) |
| [FilterResponseCookie](#type-FilterResponseCookie) |
| [AddRequestCookie](#type-AddRequestCookie) |
| [AddResponseCookie](#type-AddResponseCookie) |
| [EditRequestCookie](#type-EditRequestCookie) |
| [EditResponseCookie](#type-EditResponseCookie) |
| [RemoveRequestCookie](#type-RemoveRequestCookie) |
| [RemoveResponseCookie](#type-RemoveResponseCookie) |
| Events |
| [onRequest](#event-onRequest) |
| [onMessage](#event-onMessage) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### Stage

| Enum |
|---|
| `"onBeforeRequest"`, `"onBeforeSendHeaders"`, `"onHeadersReceived"`, or `"onAuthRequired"` |

</div>

<div>

### HeaderFilter

<dd>Filters request headers for various criteria. Multiple criteria are evaluated as a conjunction.</dd>

| properties |
|---|
| string | <span class="optional">(optional)</span> namePrefix | 

Matches if the header name starts with the specified string.
 |
| string | <span class="optional">(optional)</span> nameSuffix | 

Matches if the header name ends with the specified string.
 |
| array of string or string | <span class="optional">(optional)</span> nameContains | 

Matches if the header name contains all of the specified strings.
 |
| string | <span class="optional">(optional)</span> nameEquals | 

Matches if the header name is equal to the specified string.
 |
| string | <span class="optional">(optional)</span> valuePrefix | 

Matches if the header value starts with the specified string.
 |
| string | <span class="optional">(optional)</span> valueSuffix | 

Matches if the header value ends with the specified string.
 |
| array of string or string | <span class="optional">(optional)</span> valueContains | 

Matches if the header value contains all of the specified strings.
 |
| string | <span class="optional">(optional)</span> valueEquals | 

Matches if the header value is equal to the specified string.
 |

</div>

<div>

### RequestMatcher

<dd>Matches network events by various criteria.</dd>

| properties |
|---|
| object | <span class="optional">(optional)</span> url | 

Matches if the conditions of the UrlFilter are fulfilled for the URL of the request.

| string | <span class="optional">(optional)</span> hostContains | 
|---|---|

Matches if the host name of the URL contains a specified string. To test whether a host name component has a prefix 'foo', use hostContains: '.foo'. This matches 'www.foobar.com' and 'foo.com', because an implicit dot is added at the beginning of the host name. Similarly, hostContains can be used to match against component suffix ('foo.') and to exactly match against components ('.foo.'). Suffix- and exact-matching for the last components need to be done separately using hostSuffix, because no implicit dot is added at the end of the host name.
 |
| string | <span class="optional">(optional)</span> hostEquals | 

Matches if the host name of the URL is equal to a specified string.
 |
| string | <span class="optional">(optional)</span> hostPrefix | 

Matches if the host name of the URL starts with a specified string.
 |
| string | <span class="optional">(optional)</span> hostSuffix | 

Matches if the host name of the URL ends with a specified string.
 |
| string | <span class="optional">(optional)</span> pathContains | 

Matches if the path segment of the URL contains a specified string.
 |
| string | <span class="optional">(optional)</span> pathEquals | 

Matches if the path segment of the URL is equal to a specified string.
 |
| string | <span class="optional">(optional)</span> pathPrefix | 

Matches if the path segment of the URL starts with a specified string.
 |
| string | <span class="optional">(optional)</span> pathSuffix | 

Matches if the path segment of the URL ends with a specified string.
 |
| string | <span class="optional">(optional)</span> queryContains | 

Matches if the query segment of the URL contains a specified string.
 |
| string | <span class="optional">(optional)</span> queryEquals | 

Matches if the query segment of the URL is equal to a specified string.
 |
| string | <span class="optional">(optional)</span> queryPrefix | 

Matches if the query segment of the URL starts with a specified string.
 |
| string | <span class="optional">(optional)</span> querySuffix | 

Matches if the query segment of the URL ends with a specified string.
 |
| string | <span class="optional">(optional)</span> urlContains | 

Matches if the URL (without fragment identifier) contains a specified string. Port numbers are stripped from the URL if they match the default port number.
 |
| string | <span class="optional">(optional)</span> urlEquals | 

Matches if the URL (without fragment identifier) is equal to a specified string. Port numbers are stripped from the URL if they match the default port number.
 |
| string | <span class="optional">(optional)</span> urlMatches | 

Matches if the URL (without fragment identifier) matches a specified regular expression. Port numbers are stripped from the URL if they match the default port number. The regular expressions use the [RE2 syntax](https://github.com/google/re2/blob/master/doc/syntax.txt).
 |
| string | <span class="optional">(optional)</span> originAndPathMatches | 

Matches if the URL without query segment and fragment identifier matches a specified regular expression. Port numbers are stripped from the URL if they match the default port number. The regular expressions use the [RE2 syntax](https://github.com/google/re2/blob/master/doc/syntax.txt).
 |
| string | <span class="optional">(optional)</span> urlPrefix | 

Matches if the URL (without fragment identifier) starts with a specified string. Port numbers are stripped from the URL if they match the default port number.
 |
| string | <span class="optional">(optional)</span> urlSuffix | 

Matches if the URL (without fragment identifier) ends with a specified string. Port numbers are stripped from the URL if they match the default port number.
 |
| array of string | <span class="optional">(optional)</span> schemes | 

Matches if the scheme of the URL is equal to any of the schemes specified in the array.
 |
| array of integer or array of integer | <span class="optional">(optional)</span> ports | 

Matches if the port of the URL is contained in any of the specified port lists. For example `[80, 443, [1000, 1200]]` matches all requests on port 80, 443 and in the range 1000-1200.
 |
 |
| object | <span class="optional">(optional)</span> firstPartyForCookiesUrl | 

Matches if the conditions of the UrlFilter are fulfilled for the 'first party' URL of the request. The 'first party' URL of a request, when present, can be different from the request's target URL, and describes what is considered 'first party' for the sake of third-party checks for cookies.

| string | <span class="optional">(optional)</span> hostContains | 
|---|---|

Matches if the host name of the URL contains a specified string. To test whether a host name component has a prefix 'foo', use hostContains: '.foo'. This matches 'www.foobar.com' and 'foo.com', because an implicit dot is added at the beginning of the host name. Similarly, hostContains can be used to match against component suffix ('foo.') and to exactly match against components ('.foo.'). Suffix- and exact-matching for the last components need to be done separately using hostSuffix, because no implicit dot is added at the end of the host name.
 |
| string | <span class="optional">(optional)</span> hostEquals | 

Matches if the host name of the URL is equal to a specified string.
 |
| string | <span class="optional">(optional)</span> hostPrefix | 

Matches if the host name of the URL starts with a specified string.
 |
| string | <span class="optional">(optional)</span> hostSuffix | 

Matches if the host name of the URL ends with a specified string.
 |
| string | <span class="optional">(optional)</span> pathContains | 

Matches if the path segment of the URL contains a specified string.
 |
| string | <span class="optional">(optional)</span> pathEquals | 

Matches if the path segment of the URL is equal to a specified string.
 |
| string | <span class="optional">(optional)</span> pathPrefix | 

Matches if the path segment of the URL starts with a specified string.
 |
| string | <span class="optional">(optional)</span> pathSuffix | 

Matches if the path segment of the URL ends with a specified string.
 |
| string | <span class="optional">(optional)</span> queryContains | 

Matches if the query segment of the URL contains a specified string.
 |
| string | <span class="optional">(optional)</span> queryEquals | 

Matches if the query segment of the URL is equal to a specified string.
 |
| string | <span class="optional">(optional)</span> queryPrefix | 

Matches if the query segment of the URL starts with a specified string.
 |
| string | <span class="optional">(optional)</span> querySuffix | 

Matches if the query segment of the URL ends with a specified string.
 |
| string | <span class="optional">(optional)</span> urlContains | 

Matches if the URL (without fragment identifier) contains a specified string. Port numbers are stripped from the URL if they match the default port number.
 |
| string | <span class="optional">(optional)</span> urlEquals | 

Matches if the URL (without fragment identifier) is equal to a specified string. Port numbers are stripped from the URL if they match the default port number.
 |
| string | <span class="optional">(optional)</span> urlMatches | 

Matches if the URL (without fragment identifier) matches a specified regular expression. Port numbers are stripped from the URL if they match the default port number. The regular expressions use the [RE2 syntax](https://github.com/google/re2/blob/master/doc/syntax.txt).
 |
| string | <span class="optional">(optional)</span> originAndPathMatches | 

Matches if the URL without query segment and fragment identifier matches a specified regular expression. Port numbers are stripped from the URL if they match the default port number. The regular expressions use the [RE2 syntax](https://github.com/google/re2/blob/master/doc/syntax.txt).
 |
| string | <span class="optional">(optional)</span> urlPrefix | 

Matches if the URL (without fragment identifier) starts with a specified string. Port numbers are stripped from the URL if they match the default port number.
 |
| string | <span class="optional">(optional)</span> urlSuffix | 

Matches if the URL (without fragment identifier) ends with a specified string. Port numbers are stripped from the URL if they match the default port number.
 |
| array of string | <span class="optional">(optional)</span> schemes | 

Matches if the scheme of the URL is equal to any of the schemes specified in the array.
 |
| array of integer or array of integer | <span class="optional">(optional)</span> ports | 

Matches if the port of the URL is contained in any of the specified port lists. For example `[80, 443, [1000, 1200]]` matches all requests on port 80, 443 and in the range 1000-1200.
 |
 |
| array of [webRequest.ResourceType](/extensions/webRequest#type-ResourceType) | <span class="optional">(optional)</span> resourceType | 

Matches if the request type of a request is contained in the list. Requests that cannot match any of the types will be filtered out.
 |
| array of string | <span class="optional">(optional)</span> contentType | 

Matches if the MIME media type of a response (from the HTTP Content-Type header) is contained in the list.
 |
| array of string | <span class="optional">(optional)</span> excludeContentType | 

Matches if the MIME media type of a response (from the HTTP Content-Type header) is _not_ contained in the list.
 |
| array of [HeaderFilter](/extensions/declarativeWebRequest#type-HeaderFilter) | <span class="optional">(optional)</span> requestHeaders | 

Matches if some of the request headers is matched by one of the HeaderFilters.
 |
| array of [HeaderFilter](/extensions/declarativeWebRequest#type-HeaderFilter) | <span class="optional">(optional)</span> excludeRequestHeaders | 

Matches if none of the request headers is matched by any of the HeaderFilters.
 |
| array of [HeaderFilter](/extensions/declarativeWebRequest#type-HeaderFilter) | <span class="optional">(optional)</span> responseHeaders | 

Matches if some of the response headers is matched by one of the HeaderFilters.
 |
| array of [HeaderFilter](/extensions/declarativeWebRequest#type-HeaderFilter) | <span class="optional">(optional)</span> excludeResponseHeaders | 

Matches if none of the response headers is matched by any of the HeaderFilters.
 |
| boolean | <span class="optional">(optional)</span> thirdPartyForCookies | 

If set to true, matches requests that are subject to third-party cookie policies. If set to false, matches all other requests.
 |
| array of [Stage](/extensions/declarativeWebRequest#type-Stage) | <span class="optional">(optional)</span> stages | 

Contains a list of strings describing stages. Allowed values are 'onBeforeRequest', 'onBeforeSendHeaders', 'onHeadersReceived', 'onAuthRequired'. If this attribute is present, then it limits the applicable stages to those listed. Note that the whole condition is only applicable in stages compatible with all attributes.
 |

</div>

<div>

### CancelRequest

<dd>Declarative event action that cancels a network request.</dd>

</div>

<div>

### RedirectRequest

<dd>Declarative event action that redirects a network request.</dd>

| properties |
|---|
| string | redirectUrl | 

Destination to where the request is redirected.
 |

</div>

<div>

### RedirectToTransparentImage

<dd>Declarative event action that redirects a network request to a transparent image.</dd>

</div>

<div>

### RedirectToEmptyDocument

<dd>Declarative event action that redirects a network request to an empty document.</dd>

</div>

<div>

### RedirectByRegEx

<dd>Redirects a request by applying a regular expression on the URL. The regular expressions use the [RE2 syntax](https://github.com/google/re2/blob/master/doc/syntax.txt).</dd>

| properties |
|---|
| string | from | 

A match pattern that may contain capture groups. Capture groups are referenced in the Perl syntax ($1, $2, ...) instead of the RE2 syntax (\1, \2, ...) in order to be closer to JavaScript Regular Expressions.
 |
| string | to | 

Destination pattern.
 |

</div>

<div>

### SetRequestHeader

<dd>Sets the request header of the specified name to the specified value. If a header with the specified name did not exist before, a new one is created. Header name comparison is always case-insensitive. Each request header name occurs only once in each request.</dd>

| properties |
|---|
| string | name | 

HTTP request header name.
 |
| string | value | 

HTTP request header value.
 |

</div>

<div>

### RemoveRequestHeader

<dd>Removes the request header of the specified name. Do not use SetRequestHeader and RemoveRequestHeader with the same header name on the same request. Each request header name occurs only once in each request.</dd>

| properties |
|---|
| string | name | 

HTTP request header name (case-insensitive).
 |

</div>

<div>

### AddResponseHeader

<dd>Adds the response header to the response of this web request. As multiple response headers may share the same name, you need to first remove and then add a new response header in order to replace one.</dd>

| properties |
|---|
| string | name | 

HTTP response header name.
 |
| string | value | 

HTTP response header value.
 |

</div>

<div>

### RemoveResponseHeader

<dd>Removes all response headers of the specified names and values.</dd>

| properties |
|---|
| string | name | 

HTTP request header name (case-insensitive).
 |
| string | <span class="optional">(optional)</span> value | 

HTTP request header value (case-insensitive).
 |

</div>

<div>

### IgnoreRules

<dd>Masks all rules that match the specified criteria.</dd>

| properties |
|---|
| integer | <span class="optional">(optional)</span> lowerPriorityThan | 

If set, rules with a lower priority than the specified value are ignored. This boundary is not persisted, it affects only rules and their actions of the same network request stage.
 |
| string | <span class="optional">(optional)</span> hasTag | 

If set, rules with the specified tag are ignored. This ignoring is not persisted, it affects only rules and their actions of the same network request stage. Note that rules are executed in descending order of their priorities. This action affects rules of lower priority than the current rule. Rules with the same priority may or may not be ignored.
 |

</div>

<div>

### SendMessageToExtension

<dd>Triggers the [declarativeWebRequest.onMessage](/extensions/declarativeWebRequest#event-onMessage) event.</dd>

| properties |
|---|
| string | message | 

The value that will be passed in the `message` attribute of the dictionary that is passed to the event handler.
 |

</div>

<div>

### RequestCookie

<dd>A filter or specification of a cookie in HTTP Requests.</dd>

| properties |
|---|
| string | <span class="optional">(optional)</span> name | 

Name of a cookie.
 |
| string | <span class="optional">(optional)</span> value | 

Value of a cookie, may be padded in double-quotes.
 |

</div>

<div>

### ResponseCookie

<dd>A specification of a cookie in HTTP Responses.</dd>

| properties |
|---|
| string | <span class="optional">(optional)</span> name | 

Name of a cookie.
 |
| string | <span class="optional">(optional)</span> value | 

Value of a cookie, may be padded in double-quotes.
 |
| string | <span class="optional">(optional)</span> expires | 

Value of the Expires cookie attribute.
 |
| double | <span class="optional">(optional)</span> maxAge | 

Value of the Max-Age cookie attribute
 |
| string | <span class="optional">(optional)</span> domain | 

Value of the Domain cookie attribute.
 |
| string | <span class="optional">(optional)</span> path | 

Value of the Path cookie attribute.
 |
| string | <span class="optional">(optional)</span> secure | 

Existence of the Secure cookie attribute.
 |
| string | <span class="optional">(optional)</span> httpOnly | 

Existence of the HttpOnly cookie attribute.
 |

</div>

<div>

### FilterResponseCookie

<dd>A filter of a cookie in HTTP Responses.</dd>

| properties |
|---|
| string | <span class="optional">(optional)</span> name | 

Name of a cookie.
 |
| string | <span class="optional">(optional)</span> value | 

Value of a cookie, may be padded in double-quotes.
 |
| string | <span class="optional">(optional)</span> expires | 

Value of the Expires cookie attribute.
 |
| double | <span class="optional">(optional)</span> maxAge | 

Value of the Max-Age cookie attribute
 |
| string | <span class="optional">(optional)</span> domain | 

Value of the Domain cookie attribute.
 |
| string | <span class="optional">(optional)</span> path | 

Value of the Path cookie attribute.
 |
| string | <span class="optional">(optional)</span> secure | 

Existence of the Secure cookie attribute.
 |
| string | <span class="optional">(optional)</span> httpOnly | 

Existence of the HttpOnly cookie attribute.
 |
| integer | <span class="optional">(optional)</span> ageUpperBound | 

Inclusive upper bound on the cookie lifetime (specified in seconds after current time). Only cookies whose expiration date-time is in the interval [now, now + ageUpperBound] fulfill this criterion. Session cookies and cookies whose expiration date-time is in the past do not meet the criterion of this filter. The cookie lifetime is calculated from either 'max-age' or 'expires' cookie attributes. If both are specified, 'max-age' is used to calculate the cookie lifetime.
 |
| integer | <span class="optional">(optional)</span> ageLowerBound | 

Inclusive lower bound on the cookie lifetime (specified in seconds after current time). Only cookies whose expiration date-time is set to 'now + ageLowerBound' or later fulfill this criterion. Session cookies do not meet the criterion of this filter. The cookie lifetime is calculated from either 'max-age' or 'expires' cookie attributes. If both are specified, 'max-age' is used to calculate the cookie lifetime.
 |
| boolean | <span class="optional">(optional)</span> sessionCookie | 

Filters session cookies. Session cookies have no lifetime specified in any of 'max-age' or 'expires' attributes.
 |

</div>

<div>

### AddRequestCookie

<dd>Adds a cookie to the request or overrides a cookie, in case another cookie of the same name exists already. Note that it is preferred to use the Cookies API because this is computationally less expensive.</dd>

| properties |
|---|
| [declarativeWebRequest.RequestCookie](/extensions/declarativeWebRequest#type-RequestCookie) | cookie | 

Cookie to be added to the request. No field may be undefined.
 |

</div>

<div>

### AddResponseCookie

<dd>Adds a cookie to the response or overrides a cookie, in case another cookie of the same name exists already. Note that it is preferred to use the Cookies API because this is computationally less expensive.</dd>

| properties |
|---|
| [declarativeWebRequest.ResponseCookie](/extensions/declarativeWebRequest#type-ResponseCookie) | cookie | 

Cookie to be added to the response. The name and value need to be specified.
 |

</div>

<div>

### EditRequestCookie

<dd>Edits one or more cookies of request. Note that it is preferred to use the Cookies API because this is computationally less expensive.</dd>

| properties |
|---|
| [declarativeWebRequest.RequestCookie](/extensions/declarativeWebRequest#type-RequestCookie) | filter | 

Filter for cookies that will be modified. All empty entries are ignored.
 |
| [declarativeWebRequest.RequestCookie](/extensions/declarativeWebRequest#type-RequestCookie) | modification | 

Attributes that shall be overridden in cookies that machted the filter. Attributes that are set to an empty string are removed.
 |

</div>

<div>

### EditResponseCookie

<dd>Edits one or more cookies of response. Note that it is preferred to use the Cookies API because this is computationally less expensive.</dd>

| properties |
|---|
| [declarativeWebRequest.FilterResponseCookie](/extensions/declarativeWebRequest#type-FilterResponseCookie) | filter | 

Filter for cookies that will be modified. All empty entries are ignored.
 |
| [declarativeWebRequest.ResponseCookie](/extensions/declarativeWebRequest#type-ResponseCookie) | modification | 

Attributes that shall be overridden in cookies that machted the filter. Attributes that are set to an empty string are removed.
 |

</div>

<div>

### RemoveRequestCookie

<dd>Removes one or more cookies of request. Note that it is preferred to use the Cookies API because this is computationally less expensive.</dd>

| properties |
|---|
| [declarativeWebRequest.RequestCookie](/extensions/declarativeWebRequest#type-RequestCookie) | filter | 

Filter for cookies that will be removed. All empty entries are ignored.
 |

</div>

<div>

### RemoveResponseCookie

<dd>Removes one or more cookies of response. Note that it is preferred to use the Cookies API because this is computationally less expensive.</dd>

| properties |
|---|
| [declarativeWebRequest.FilterResponseCookie](/extensions/declarativeWebRequest#type-FilterResponseCookie) | filter | 

Filter for cookies that will be removed. All empty entries are ignored.
 |

</div>

## Events

<div>

### onRequest

<div class="description">

Provides the [Declarative Event API](events#declarative) consisting of [addRules](/extensions/events#method-Event-addRules), [removeRules](/extensions/events#method-Event-removeRules), and [getRules](/extensions/events#method-Event-addRules).

<div class="summary">`whale.declarativeWebRequest.onRequest.addRules(<span>array of [Rule](/extensions/events#type-Rule) rules</span>, <span class="optional">function callback</span>)` `whale.declarativeWebRequest.onRequest.removeRules(<span class="optional">array of string ruleIdentifiers</span>, <span class="optional">function callback</span>)` `whale.declarativeWebRequest.onRequest.getRules(<span class="optional">array of string ruleIdentifiers</span>, <span>function callback</span>)`</div>

#### Supported conditions

<div>*   [declarativeWebRequest.RequestMatcher](/extensions/declarativeWebRequest#type-RequestMatcher)</div>

#### Supported actions

<div>*   [declarativeWebRequest.AddRequestCookie](/extensions/declarativeWebRequest#type-AddRequestCookie)</div>

<div>*   [declarativeWebRequest.AddResponseCookie](/extensions/declarativeWebRequest#type-AddResponseCookie)</div>

<div>*   [declarativeWebRequest.AddResponseHeader](/extensions/declarativeWebRequest#type-AddResponseHeader)</div>

<div>*   [declarativeWebRequest.CancelRequest](/extensions/declarativeWebRequest#type-CancelRequest)</div>

<div>*   [declarativeWebRequest.EditRequestCookie](/extensions/declarativeWebRequest#type-EditRequestCookie)</div>

<div>*   [declarativeWebRequest.EditResponseCookie](/extensions/declarativeWebRequest#type-EditResponseCookie)</div>

<div>*   [declarativeWebRequest.RedirectRequest](/extensions/declarativeWebRequest#type-RedirectRequest)</div>

<div>*   [declarativeWebRequest.RedirectToTransparentImage](/extensions/declarativeWebRequest#type-RedirectToTransparentImage)</div>

<div>*   [declarativeWebRequest.RedirectToEmptyDocument](/extensions/declarativeWebRequest#type-RedirectToEmptyDocument)</div>

<div>*   [declarativeWebRequest.RedirectByRegEx](/extensions/declarativeWebRequest#type-RedirectByRegEx)</div>

<div>*   [declarativeWebRequest.RemoveRequestCookie](/extensions/declarativeWebRequest#type-RemoveRequestCookie)</div>

<div>*   [declarativeWebRequest.RemoveResponseCookie](/extensions/declarativeWebRequest#type-RemoveResponseCookie)</div>

<div>*   [declarativeWebRequest.RemoveRequestHeader](/extensions/declarativeWebRequest#type-RemoveRequestHeader)</div>

<div>*   [declarativeWebRequest.RemoveResponseHeader](/extensions/declarativeWebRequest#type-RemoveResponseHeader)</div>

<div>*   [declarativeWebRequest.SetRequestHeader](/extensions/declarativeWebRequest#type-SetRequestHeader)</div>

<div>*   [declarativeWebRequest.SendMessageToExtension](/extensions/declarativeWebRequest#type-SendMessageToExtension)</div>

<div>*   [declarativeWebRequest.IgnoreRules](/extensions/declarativeWebRequest#type-IgnoreRules)</div>

</div>

</div>

<div>

### onMessage

<div class="description">

Fired when a message is sent via [declarativeWebRequest.SendMessageToExtension](/extensions/declarativeWebRequest#type-SendMessageToExtension) from an action of the declarative web request API.

<div>

#### addListener

<div class="summary">`whale.declarativeWebRequest.onMessage.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| string | message | 
|---|---|

The message sent by the calling script.
 |
| [Stage](/extensions/declarativeWebRequest#type-Stage) | stage | 

The stage of the network request during which the event was triggered.
 |
| string | requestId | 

The ID of the request. Request IDs are unique within a browser session. As a result, they could be used to relate different events of the same request.
 |
| string | url |  |
| string | method | 

Standard HTTP method.
 |
| integer | frameId | 

The value 0 indicates that the request happens in the main frame; a positive value indicates the ID of a subframe in which the request happens. If the document of a (sub-)frame is loaded (`type` is `main_frame` or `sub_frame`), `frameId` indicates the ID of this frame, not the ID of the outer frame. Frame IDs are unique within a tab.
 |
| integer | parentFrameId | 

ID of frame that wraps the frame which sent the request. Set to -1 if no parent frame exists.
 |
| integer | tabId | 

The ID of the tab in which the request takes place. Set to -1 if the request isn't related to a tab.
 |
| [webRequest.ResourceType](/extensions/webRequest#type-ResourceType) | type | 

How the requested resource will be used.
 |
| double | timeStamp | 

The time when this signal is triggered, in milliseconds since the epoch.
 |
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>