# whale.webNavigation

| Description: | Use the `whale.webNavigation` API to receive notifications about the status of navigation requests in-flight. |
|---|---|
| Availability: | Since Chrome 19. |
| Permissions: | <span class="code">"webNavigation"</span> |

<section>

## Manifest

All `whale.webNavigation` methods and events require you to declare the "webNavigation" permission in the [extension manifest](manifest). For example:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"permissions": [
          "webNavigation"
        ]**,
        ...
      }
      </pre>

## Examples

You can find simple examples of using the tabs module in the [examples/api/webNavigation](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/docs/examples/api/webNavigation/) directory. For other examples and for help in viewing the source code, see [Samples](samples).

## Event order

For a navigation that is successfully completed, events are fired in the following order:

<pre>      onBeforeNavigate -> onCommitted -> onDOMContentLoaded -> onCompleted
      </pre>

Any error that occurs during the process results in an `onErrorOccurred` event. For a specific navigation, there are no further events fired after `onErrorOccurred`.

If a navigating frame contains subframes, its `onCommitted` is fired before any of its children's `onBeforeNavigate`; while `onCompleted` is fired after all of its children's `onCompleted`.

If the reference fragment of a frame is changed, a `onReferenceFragmentUpdated` event is fired. This event can fire any time after `onDOMContentLoaded`, even after `onCompleted`.

If the history API is used to modify the state of a frame (e.g. using `history.pushState()`, a `onHistoryStateUpdated` event is fired. This event can fire any time after `onDOMContentLoaded`.

If a navigation was triggered via [Chrome Instant](https://support.google.com/chrome/answer/177873) or [Instant Pages](https://support.google.com/chrome/answer/1385029), a completely loaded page is swapped into the current tab. In that case, an `onTabReplaced` event is fired.

## Relation to webRequest events

There is no defined ordering between events of the [webRequest API](webRequest) and the events of the webNavigation API. It is possible that webRequest events are still received for frames that already started a new navigation, or that a navigation only proceeds after the network resources are already fully loaded.

In general, the webNavigation events are closely related to the navigation state that is displayed in the UI, while the webRequest events correspond to the state of the network stack which is generally opaque to the user.

## A note about tab IDs

Not all navigating tabs correspond to actual tabs in Chrome's UI, e.g., a tab that is being pre-rendered. Such tabs are not accessible via the [tabs API](tabs) nor can you request information about them via `webNavigation.getFrame` or `webNavigation.getAllFrames`. Once such a tab is swapped in, an `onTabReplaced` event is fired and they become accessible via these APIs.

## A note about timestamps

It's important to note that some technical oddities in the OS's handling of distinct Chrome processes can cause the clock to be skewed between the browser itself and extension processes. That means that WebNavigation's events' `timeStamp` property is only guaranteed to be _internally_ consistent. Comparing one event to another event will give you the correct offset between them, but comparing them to the current time inside the extension (via `(new Date()).getTime()`, for instance) might give unexpected results.

## A note about frame IDs

Frames within a tab can be identified by a frame ID. The frame ID of the main frame is always 0, the ID of child frames is a positive number. Once a document is constructed in a frame, its frame ID remains constant during the lifetime of the document. As of Chrome 49, this ID is also constant for the lifetime of the frame (across multiple navigations).

Due to the multi-process nature of Chrome, a tab might use different processes to render the source and destination of a web page. Therefore, if a navigation takes place in a new process, you might receive events both from the new and the old page until the new navigation is committed (i.e. the `onCommitted` event is send for the new main frame). In other words, it is possible to have more than one pending sequence of webNavigation events with the same `frameId`. The sequences can be distinguished by the `processId` key.

Also note that during a provisional load the process might be switched several times. This happens when the load is redirected to a different site. In this case, you will receive repeated `onBeforeNavigate` and `onErrorOccurred` events, until you receive the final `onCommitted` event.

## Transition types and qualifiers

The webNavigation API's `onCommitted` event has a `transitionType` and a `transitionQualifiers` property. The _transition type_ is the same as used in the [history API](history#transition_types) describing how the browser navigated to this particular URL. In addition, several _transition qualifiers_ can be returned that further define the navigation.

The following transition qualifiers exist:

| Transition qualifier | Description |
|---|---|
| "client_redirect" | One or more redirects caused by JavaScript or meta refresh tags on the page happened during the navigation. |
| "server_redirect" | One or more redirects caused by HTTP headers sent from the server happened during the navigation. |
| "forward_back" | The user used the Forward or Back button to initiate the navigation. |
| "from_address_bar" | The user initiated the navigation from the address bar (aka Omnibox). |

</section>

<section id="toc">

## Summary

| Types |
|---|
| [TransitionType](#type-TransitionType) |
| [TransitionQualifier](#type-TransitionQualifier) |
| Methods |
| [getFrame](#method-getFrame) − `whale.webNavigation.getFrame(<span>object details</span>, <span>function callback</span>)` |
| [getAllFrames](#method-getAllFrames) − `whale.webNavigation.getAllFrames(<span>object details</span>, <span>function callback</span>)` |
| Events |
| [onBeforeNavigate](#event-onBeforeNavigate) |
| [onCommitted](#event-onCommitted) |
| [onDOMContentLoaded](#event-onDOMContentLoaded) |
| [onCompleted](#event-onCompleted) |
| [onErrorOccurred](#event-onErrorOccurred) |
| [onCreatedNavigationTarget](#event-onCreatedNavigationTarget) |
| [onReferenceFragmentUpdated](#event-onReferenceFragmentUpdated) |
| [onTabReplaced](#event-onTabReplaced) |
| [onHistoryStateUpdated](#event-onHistoryStateUpdated) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### TransitionType

<dd>Cause of the navigation. The same transition types as defined in the history API are used. These are the same transition types as defined in the [history API](history#transition_types) except with `"start_page"` in place of `"auto_toplevel"` (for backwards compatibility).</dd>

| Enum |
|---|
| `"link"`, `"typed"`, `"auto_bookmark"`, `"auto_subframe"`, `"manual_subframe"`, `"generated"`, `"start_page"`, `"form_submit"`, `"reload"`, `"keyword"`, or `"keyword_generated"` |

</div>

<div>

### TransitionQualifier

| Enum |
|---|
| `"client_redirect"`, `"server_redirect"`, `"forward_back"`, or `"from_address_bar"` |

</div>

## Methods

<div>

### getFrame

<div class="summary">`whale.webNavigation.getFrame(<span>object details</span>, <span>function callback</span>)`</div>

<div class="description">

Retrieves information about the given frame. A frame refers to an <iframe> or a <frame> of a web page and is identified by a tab ID and a frame ID.

| Parameters |
|---|
| object | details | 

Information about the frame to retrieve information about.

| integer | tabId | 
|---|---|

The ID of the tab in which the frame is.
 |
| integer | <span class="optional">(optional)</span> processId | 

**Deprecated** since Chrome 49. Frames are now uniquely identified by their tab ID and frame ID; the process ID is no longer needed and therefore ignored.

The ID of the process that runs the renderer for this tab.
 |
| integer | frameId | 

The ID of the frame in the given tab.
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | <span class="optional">(optional)</span> details | 
|---|---|

Information about the requested frame, null if the specified frame ID and/or tab ID are invalid.

| boolean | errorOccurred | 
|---|---|

True if the last navigation in this frame was interrupted by an error, i.e. the onErrorOccurred event fired.
 |
| string | url | 

The URL currently associated with this frame, if the frame identified by the frameId existed at one point in the given tab. The fact that an URL is associated with a given frameId does not imply that the corresponding frame still exists.
 |
| integer | parentFrameId | 

ID of frame that wraps the frame. Set to -1 of no parent frame exists.
 |
 |
 |

</div>

</div>

<div>

### getAllFrames

<div class="summary">`whale.webNavigation.getAllFrames(<span>object details</span>, <span>function callback</span>)`</div>

<div class="description">

Retrieves information about all frames of a given tab.

| Parameters |
|---|
| object | details | 

Information about the tab to retrieve all frames from.

| integer | tabId | 
|---|---|

The ID of the tab.
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of object details) <span class="subdued">{...}</span>;`

| array of object | <span class="optional">(optional)</span> details | 
|---|---|

A list of frames in the given tab, null if the specified tab ID is invalid.

#### Properties of each object

<dl>

<div>

| boolean | errorOccurred | 
|---|---|

True if the last navigation in this frame was interrupted by an error, i.e. the onErrorOccurred event fired.
 |
| integer | processId | 

The ID of the process that runs the renderer for this frame.
 |
| integer | frameId | 

The ID of the frame. 0 indicates that this is the main frame; a positive value indicates the ID of a subframe.
 |
| integer | parentFrameId | 

ID of frame that wraps the frame. Set to -1 of no parent frame exists.
 |
| string | url | 

The URL currently associated with this frame.
 |

</div>

</dl>
 |
 |

</div>

</div>

## Events

<div>

### onBeforeNavigate

<div class="description">

Fired when a navigation is about to occur.

#### Filters

<dl>array of object url

Conditions that the URL being navigated to must satisfy. The 'schemes' and 'ports' fields of UrlFilter are ignored for this event.

#### Properties of each object

<dl>

<div>

<dd>Filters URLs for various criteria. See [event filtering](events#filtered). All criteria are case sensitive.</dd>

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

</div>

</dl>

</dl>

<div>

#### addListener

<div class="summary">`whale.webNavigation.onBeforeNavigate.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| integer | tabId | 
|---|---|

The ID of the tab in which the navigation is about to occur.
 |
| string | url |  |
| integer | processId | 

**Deprecated** since Chrome 50. The processId is no longer set for this event, since the process which will render the resulting document is not known until onCommit.

The value of -1.
 |
| integer | frameId | 

0 indicates the navigation happens in the tab content window; a positive value indicates navigation in a subframe. Frame IDs are unique for a given tab and process.
 |
| integer | parentFrameId | 

Since Chrome 24.

ID of frame that wraps the frame. Set to -1 of no parent frame exists.
 |
| double | timeStamp | 

The time when the browser was about to start the navigation, in milliseconds since the epoch.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onCommitted

<div class="description">

Fired when a navigation is committed. The document (and the resources it refers to, such as images and subframes) might still be downloading, but at least part of the document has been received from the server and the browser has decided to switch to the new document.

#### Filters

<dl>array of object url

Conditions that the URL being navigated to must satisfy. The 'schemes' and 'ports' fields of UrlFilter are ignored for this event.

#### Properties of each object

<dl>

<div>

<dd>Filters URLs for various criteria. See [event filtering](events#filtered). All criteria are case sensitive.</dd>

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

</div>

</dl>

</dl>

<div>

#### addListener

<div class="summary">`whale.webNavigation.onCommitted.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| integer | tabId | 
|---|---|

The ID of the tab in which the navigation occurs.
 |
| string | url |  |
| integer | processId | 

Since Chrome 22.

The ID of the process that runs the renderer for this frame.
 |
| integer | frameId | 

0 indicates the navigation happens in the tab content window; a positive value indicates navigation in a subframe. Frame IDs are unique within a tab.
 |
| [TransitionType](/extensions/webNavigation#type-TransitionType) | transitionType | 

Cause of the navigation.
 |
| array of [TransitionQualifier](/extensions/webNavigation#type-TransitionQualifier) | transitionQualifiers | 

A list of transition qualifiers.
 |
| double | timeStamp | 

The time when the navigation was committed, in milliseconds since the epoch.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onDOMContentLoaded

<div class="description">

Fired when the page's DOM is fully constructed, but the referenced resources may not finish loading.

#### Filters

<dl>array of object url

Conditions that the URL being navigated to must satisfy. The 'schemes' and 'ports' fields of UrlFilter are ignored for this event.

#### Properties of each object

<dl>

<div>

<dd>Filters URLs for various criteria. See [event filtering](events#filtered). All criteria are case sensitive.</dd>

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

</div>

</dl>

</dl>

<div>

#### addListener

<div class="summary">`whale.webNavigation.onDOMContentLoaded.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| integer | tabId | 
|---|---|

The ID of the tab in which the navigation occurs.
 |
| string | url |  |
| integer | processId | 

Since Chrome 22.

The ID of the process that runs the renderer for this frame.
 |
| integer | frameId | 

0 indicates the navigation happens in the tab content window; a positive value indicates navigation in a subframe. Frame IDs are unique within a tab.
 |
| double | timeStamp | 

The time when the page's DOM was fully constructed, in milliseconds since the epoch.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onCompleted

<div class="description">

Fired when a document, including the resources it refers to, is completely loaded and initialized.

#### Filters

<dl>array of object url

Conditions that the URL being navigated to must satisfy. The 'schemes' and 'ports' fields of UrlFilter are ignored for this event.

#### Properties of each object

<dl>

<div>

<dd>Filters URLs for various criteria. See [event filtering](events#filtered). All criteria are case sensitive.</dd>

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

</div>

</dl>

</dl>

<div>

#### addListener

<div class="summary">`whale.webNavigation.onCompleted.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| integer | tabId | 
|---|---|

The ID of the tab in which the navigation occurs.
 |
| string | url |  |
| integer | processId | 

Since Chrome 22.

The ID of the process that runs the renderer for this frame.
 |
| integer | frameId | 

0 indicates the navigation happens in the tab content window; a positive value indicates navigation in a subframe. Frame IDs are unique within a tab.
 |
| double | timeStamp | 

The time when the document finished loading, in milliseconds since the epoch.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onErrorOccurred

<div class="description">

Fired when an error occurs and the navigation is aborted. This can happen if either a network error occurred, or the user aborted the navigation.

#### Filters

<dl>array of object url

Conditions that the URL being navigated to must satisfy. The 'schemes' and 'ports' fields of UrlFilter are ignored for this event.

#### Properties of each object

<dl>

<div>

<dd>Filters URLs for various criteria. See [event filtering](events#filtered). All criteria are case sensitive.</dd>

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

</div>

</dl>

</dl>

<div>

#### addListener

<div class="summary">`whale.webNavigation.onErrorOccurred.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| integer | tabId | 
|---|---|

The ID of the tab in which the navigation occurs.
 |
| string | url |  |
| integer | processId | 

**Deprecated** since Chrome 50. The processId is no longer set for this event.

The value of -1.
 |
| integer | frameId | 

0 indicates the navigation happens in the tab content window; a positive value indicates navigation in a subframe. Frame IDs are unique within a tab.
 |
| string | error | 

The error description.
 |
| double | timeStamp | 

The time when the error occurred, in milliseconds since the epoch.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onCreatedNavigationTarget

<div class="description">

Fired when a new window, or a new tab in an existing window, is created to host a navigation.

#### Filters

<dl>array of object url

Conditions that the URL being navigated to must satisfy. The 'schemes' and 'ports' fields of UrlFilter are ignored for this event.

#### Properties of each object

<dl>

<div>

<dd>Filters URLs for various criteria. See [event filtering](events#filtered). All criteria are case sensitive.</dd>

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

</div>

</dl>

</dl>

<div>

#### addListener

<div class="summary">`whale.webNavigation.onCreatedNavigationTarget.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| integer | sourceTabId | 
|---|---|

The ID of the tab in which the navigation is triggered.
 |
| integer | sourceProcessId | 

Since Chrome 22.

The ID of the process that runs the renderer for the source frame.
 |
| integer | sourceFrameId | 

The ID of the frame with sourceTabId in which the navigation is triggered. 0 indicates the main frame.
 |
| string | url | 

The URL to be opened in the new window.
 |
| integer | tabId | 

The ID of the tab in which the url is opened
 |
| double | timeStamp | 

The time when the browser was about to create a new view, in milliseconds since the epoch.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onReferenceFragmentUpdated

<div class="description">

Fired when the reference fragment of a frame was updated. All future events for that frame will use the updated URL.

#### Filters

<dl>array of object url

Conditions that the URL being navigated to must satisfy. The 'schemes' and 'ports' fields of UrlFilter are ignored for this event.

#### Properties of each object

<dl>

<div>

<dd>Filters URLs for various criteria. See [event filtering](events#filtered). All criteria are case sensitive.</dd>

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

</div>

</dl>

</dl>

<div>

#### addListener

<div class="summary">`whale.webNavigation.onReferenceFragmentUpdated.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| integer | tabId | 
|---|---|

The ID of the tab in which the navigation occurs.
 |
| string | url |  |
| integer | processId | 

Since Chrome 22.

The ID of the process that runs the renderer for this frame.
 |
| integer | frameId | 

0 indicates the navigation happens in the tab content window; a positive value indicates navigation in a subframe. Frame IDs are unique within a tab.
 |
| [TransitionType](/extensions/webNavigation#type-TransitionType) | transitionType | 

Cause of the navigation.
 |
| array of [TransitionQualifier](/extensions/webNavigation#type-TransitionQualifier) | transitionQualifiers | 

A list of transition qualifiers.
 |
| double | timeStamp | 

The time when the navigation was committed, in milliseconds since the epoch.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onTabReplaced

<div class="description">

Since Chrome 22.

Fired when the contents of the tab is replaced by a different (usually previously pre-rendered) tab.

<div>

#### addListener

<div class="summary">`whale.webNavigation.onTabReplaced.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| integer | replacedTabId | 
|---|---|

The ID of the tab that was replaced.
 |
| integer | tabId | 

The ID of the tab that replaced the old tab.
 |
| double | timeStamp | 

The time when the replacement happened, in milliseconds since the epoch.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onHistoryStateUpdated

<div class="description">

Since Chrome 22.

Fired when the frame's history was updated to a new URL. All future events for that frame will use the updated URL.

#### Filters

<dl>array of object url

Conditions that the URL being navigated to must satisfy. The 'schemes' and 'ports' fields of UrlFilter are ignored for this event.

#### Properties of each object

<dl>

<div>

<dd>Filters URLs for various criteria. See [event filtering](events#filtered). All criteria are case sensitive.</dd>

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

</div>

</dl>

</dl>

<div>

#### addListener

<div class="summary">`whale.webNavigation.onHistoryStateUpdated.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| integer | tabId | 
|---|---|

The ID of the tab in which the navigation occurs.
 |
| string | url |  |
| integer | processId | 

The ID of the process that runs the renderer for this frame.
 |
| integer | frameId | 

0 indicates the navigation happens in the tab content window; a positive value indicates navigation in a subframe. Frame IDs are unique within a tab.
 |
| [TransitionType](/extensions/webNavigation#type-TransitionType) | transitionType | 

Cause of the navigation.
 |
| array of [TransitionQualifier](/extensions/webNavigation#type-TransitionQualifier) | transitionQualifiers | 

A list of transition qualifiers.
 |
| double | timeStamp | 

The time when the navigation was committed, in milliseconds since the epoch.
 |
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>