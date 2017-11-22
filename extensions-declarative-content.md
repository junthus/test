# whale.declarativeContent

| Description: | Use the `whale.declarativeContent` API to take actions depending on the content of a page, without requiring permission to read the page's content. |
|---|---|
| Availability: | Since Chrome 33. |
| Permissions: | <span class="code">"declarativeContent"</span> |
| Learn More: | [Declarative Events](events)
[activeTab](activeTab) |

<section>

## Usage

The Declarative Content API allows you to show your extension's [page action](/extensions/pageAction) depending on the URL of a web page and the CSS selectors its content matches, without needing to take a [host permission](declare_permissions#host-permissions) or inject a [content script](content_scripts). Use the [activeTab](activeTab) permission in order to be able to interact with a page after the user clicks on your page action.

If you need more precise control over when your page action appears or you need to change its appearance to match the current tab before the user clicks on it, you'll have to keep using the [pageAction](/extensions/pageAction) API.

## Rules

As a [declarative API](events#declarative), this API lets you register rules on the `[onPageChanged](/extensions/declarativeContent#event-onPageChanged)` [event](/extensions/events#type-Event) object which take an action (`[ShowPageAction](/extensions/declarativeContent#type-ShowPageAction)` and `[SetIcon](/extensions/declarativeContent#type-SetIcon)`) when a set of conditions, represented as a `[PageStateMatcher](/extensions/declarativeContent#type-PageStateMatcher)`, are met.

The `[PageStateMatcher](/extensions/declarativeContent#type-PageStateMatcher)` matches web pages if and only if all listed criteria are met. The following rule would show a page action for pages on `https://www.google.com/` when a password field is present on it:

<pre>      var rule1 = {
        conditions: [
          new [whale.declarativeContent.PageStateMatcher](/extensions/declarativeContent#type-PageStateMatcher)({
            [pageUrl](/extensions/declarativeContent#property-PageStateMatcher-pageUrl): { [hostEquals](/extensions/events#property-UrlFilter-hostEquals): 'www.google.com', [schemes](/extensions/events#property-UrlFilter-schemes): ['https'] },
            [css](/extensions/declarativeContent#property-PageStateMatcher-css): ["input[type='password']"]
          })
        ],
        actions: [ new [whale.declarativeContent.ShowPageAction](/extensions/declarativeContent#type-ShowPageAction)() ]
      };
      </pre>

**Note:** All conditions and actions are created via a constructor as shown in the example above.

In order to also show a page action for sites with a video, you can add a second condition, as each condition is sufficient to trigger all specified actions:

<pre>      var rule2 = {
        conditions: [
          new [whale.declarativeContent.PageStateMatcher](/extensions/declarativeContent#type-PageStateMatcher)({
            [pageUrl](/extensions/declarativeContent#property-PageStateMatcher-pageUrl): { [hostEquals](/extensions/events#property-UrlFilter-hostEquals): 'www.google.com', [schemes](/extensions/events#property-UrlFilter-schemes): ['https'] },
            [css](/extensions/declarativeContent#property-PageStateMatcher-css): ["input[type='password']"]
          })**,
          new [whale.declarativeContent.PageStateMatcher](/extensions/declarativeContent#type-PageStateMatcher)({
            [css](/extensions/declarativeContent#property-PageStateMatcher-css): ["video"]
          })**
        ],
        actions: [ new [whale.declarativeContent.ShowPageAction](/extensions/declarativeContent#type-ShowPageAction)() ]
      };
      </pre>

[Added rules](events#addingrules) are saved across browser restarts, so register them as follows:

<pre>      [whale.runtime.onInstalled](/extensions/runtime#event-onInstalled).addListener(function(details) {
        [whale.declarativeContent.onPageChanged](/extensions/declarativeContent#event-onPageChanged).[removeRules](events#removingrules)(undefined, function() {
          [whale.declarativeContent.onPageChanged](/extensions/declarativeContent#event-onPageChanged).[addRules](events#addingrules)([rule2]);
        });
      });
      </pre>

**Note:** You should always register or unregister rules in bulk rather than individually because each of these operations recreates internal data structures. This re-creation is computationally expensive but facilitates a faster matching algorithm.

Combine the above rule with the [activeTab](activeTab) permission to create an extension that doesn't need any install-time permissions but can invite the user to click its page action on relevant pages and can run on those pages when the user clicks the page action.

## CSS Matching

[PageStateMatcher.css](/extensions/declarativeContent#property-PageStateMatcher-css) conditions must be _[compound selectors](http://www.w3.org/TR/selectors4/#compound)_, meaning that you can't include [combinators](http://www.w3.org/community/webed/wiki/CSS/Selectors#Combinators) like whitespace or "`>`" in your selectors. This helps Chrome match the selectors more efficiently.

| Compound Selectors (OK) | Complex Selectors (Not OK) |
|---|---|
| `a` | `div p` |
| `iframe.special[src^='http']` | `p>span.highlight` |
| `ns|*` | `p + ol` |
| `#abcd:checked` | `p::first-line` |

CSS conditions only match displayed elements: if an element that matches your selector is `display:none` or one of its parent elements is `display:none`, it doesn't cause the condition to match. Elements styled with `visibility:hidden`, positioned off-screen, or hidden by other elements can still make your condition match.

## Bookmarked State Matching

The [PageStateMatcher.isBookmarked](/extensions/declarativeContent#property-PageStateMatcher-isBookmarked) condition allows matching of the bookmarked state of the current URL in the user's profile. To make use of this condition the "bookmarks" permission must be declared in the [extension manifest](manifest)

</section>

<section id="toc">

## Summary

| Types |
|---|
| [PageStateMatcherInstanceType](#type-PageStateMatcherInstanceType) |
| [ShowPageActionInstanceType](#type-ShowPageActionInstanceType) |
| [SetIconInstanceType](#type-SetIconInstanceType) |
| [RequestContentScriptInstanceType](#type-RequestContentScriptInstanceType) |
| [PageStateMatcher](#type-PageStateMatcher) |
| [ShowPageAction](#type-ShowPageAction) |
| [SetIcon](#type-SetIcon) |
| [RequestContentScript](#type-RequestContentScript) |
| Events |
| [onPageChanged](#event-onPageChanged) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### PageStateMatcherInstanceType

| Enum |
|---|
| `"declarativeContent.PageStateMatcher"` |

</div>

<div>

### ShowPageActionInstanceType

| Enum |
|---|
| `"declarativeContent.ShowPageAction"` |

</div>

<div>

### SetIconInstanceType

| Enum |
|---|
| `"declarativeContent.SetIcon"` |

</div>

<div>

### RequestContentScriptInstanceType

| Enum |
|---|
| `"declarativeContent.RequestContentScript"` |

</div>

<div>

### PageStateMatcher

<dd>Matches the state of a web page by various criteria.</dd>

| properties |
|---|
| object | <span class="optional">(optional)</span> pageUrl | 

Matches if the condition of the UrlFilter are fulfilled for the top-level URL of the page.

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
| array of string | <span class="optional">(optional)</span> css | 

Matches if all of the CSS selectors in the array match displayed elements in a frame with the same origin as the page's main frame. All selectors in this array must be [compound selectors](http://www.w3.org/TR/selectors4/#compound) to speed up matching. Note that listing hundreds of CSS selectors or CSS selectors that match hundreds of times per page can still slow down web sites.
 |
| boolean | <span class="optional">(optional)</span> isBookmarked | 

Since Chrome 45.

Matches if the bookmarked state of the page is equal to the specified value. Requres the [bookmarks permission](declare_permissions).
 |

</div>

<div>

### ShowPageAction

<dd>Declarative event action that shows the extension's [page action](/extensions/pageAction) while the corresponding conditions are met. This action can be used without [host permissions](declare_permissions#host-permissions), but the extension must have a page action. If the extension takes the [activeTab](activeTab.html) permission, a click on the page action will grant access to the active tab.</dd>

</div>

<div>

### SetIcon

Since Chrome 39.

<dd>Declarative event action that sets the n-<abbr title="device-independent pixel">dip</abbr> square icon for the extension's [page action](/extensions/pageAction) or [browser action](/extensions/browserAction) while the corresponding conditions are met. This action can be used without [host permissions](declare_permissions.html#host-permissions), but the extension must have page or browser action.

Exactly one of `imageData` or `path` must be specified. Both are dictionaries mapping a number of pixels to an image representation. The image representation in `imageData` is an[ImageData](https://developer.mozilla.org/en-US/docs/Web/API/ImageData) object, for example from a `<canvas>` element, while the image representation in `path` is the path to an image file relative to the extension's manifest. If `scale` screen pixels fit into a device-independent pixel, the `scale * n` icon will be used. If that scale is missing, another image will be resized to the needed size.

</dd>

| properties |
|---|
| ImageData or object | <span class="optional">(optional)</span> imageData | 

Either an ImageData object or a dictionary {size -> ImageData} representing icon to be set. If the icon is specified as a dictionary, the actual image to be used is chosen depending on screen's pixel density. If the number of image pixels that fit into one screen space unit equals `scale`, then image with size `scale` * n will be selected, where n is the size of the icon in the UI. At least one image must be specified. Note that 'details.imageData = foo' is equivalent to 'details.imageData = {'16': foo}'
 |

</div>

<div>

### RequestContentScript

Since Chrome 38.

<dd>Declarative event action that injects a content script.

**WARNING:** This action is still experimental and is not supported on stable builds of Chrome.

</dd>

| properties |
|---|
| array of string | <span class="optional">(optional)</span> css | 

Names of CSS files to be injected as a part of the content script.
 |
| array of string | <span class="optional">(optional)</span> js | 

Names of Javascript files to be injected as a part of the content script.
 |
| boolean | <span class="optional">(optional)</span> allFrames | 

Whether the content script runs in all frames of the matching page, or only the top frame. Default is false.
 |
| boolean | <span class="optional">(optional)</span> matchAboutBlank | 

Whether to insert the content script on about:blank and about:srcdoc. Default is false.
 |

</div>

## Events

<div>

### onPageChanged

<div class="description">

Provides the [Declarative Event API](events#declarative) consisting of [addRules](/extensions/events#method-Event-addRules), [removeRules](/extensions/events#method-Event-removeRules), and [getRules](/extensions/events#method-Event-addRules).

<div class="summary">`whale.declarativeContent.onPageChanged.addRules(<span>array of [Rule](/extensions/events#type-Rule) rules</span>, <span class="optional">function callback</span>)` `whale.declarativeContent.onPageChanged.removeRules(<span class="optional">array of string ruleIdentifiers</span>, <span class="optional">function callback</span>)` `whale.declarativeContent.onPageChanged.getRules(<span class="optional">array of string ruleIdentifiers</span>, <span>function callback</span>)`</div>

#### Supported conditions

<div>*   [declarativeContent.PageStateMatcher](/extensions/declarativeContent#type-PageStateMatcher)</div>

#### Supported actions

<div>*   [declarativeContent.RequestContentScript](/extensions/declarativeContent#type-RequestContentScript)</div>

<div>*   [declarativeContent.SetIcon](/extensions/declarativeContent#type-SetIcon)</div>

<div>*   [declarativeContent.ShowPageAction](/extensions/declarativeContent#type-ShowPageAction)</div>

</div>

</div>

</div>

</section>