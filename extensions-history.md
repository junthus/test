# whale.history

| Description: | Use the `whale.history` API to interact with the browser's record of visited pages. You can add, remove, and query for URLs in the browser's history. To override the history page with your own version, see [Override Pages](override). |
|---|---|
| Availability: | Since Chrome 20. |
| Permissions: | <span class="code">"history"</span> |

<section>

## Manifest

You must declare the "history" permission in the [extension manifest](manifest) to use the history API. For example:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"permissions": [
          "history"
        ]**,
        ...
      }
      </pre>

## Transition types

The history API uses a _transition type_ to describe how the browser navigated to a particular URL on a particular visit. For example, if a user visits a page by clicking a link on another page, the transition type is "link".

The following table describes each transition type.

| Transition type | Description |
|---|---|
| "link" | The user got to this page by clicking a link on another page. |
| "typed" | The user got this page by typing the URL in the address bar. Also used for other explicit navigation actions. See also [generated](#tt_generated), which is used for cases where the user selected a choice that didn't look at all like a URL. |
| "auto_bookmark" | The user got to this page through a suggestion in the UI — for example, through a menu item. |
| "auto_subframe" | Subframe navigation. This is any content that is automatically loaded in a non-top-level frame. For example, if a page consists of several frames containing ads, those ad URLs have this transition type. The user may not even realize the content in these pages is a separate frame, and so may not care about the URL (see also [manual_subframe](#tt_manual_subframe)). |
| "manual_subframe" | For subframe navigations that are explicitly requested by the user and generate new navigation entries in the back/forward list. An explicitly requested frame is probably more important than an automatically loaded frame because the user probably cares about the fact that the requested frame was loaded. |
| "generated" | The user got to this page by typing in the address bar and selecting an entry that did not look like a URL. For example, a match might have the URL of a Google search result page, but it might appear to the user as "Search Google for ...". These are not quite the same as [typed](#tt_typed) navigations because the user didn't type or see the destination URL. See also [keyword](#tt_keyword). |
| "auto_toplevel" | The page was specified in the command line or is the start page. |
| "form_submit" | The user filled out values in a form and submitted it. Note that in some situations — such as when a form uses script to submit contents — submitting a form does not result in this transition type. |
| "reload" | The user reloaded the page, either by clicking the reload button or by pressing Enter in the address bar. Session restore and Reopen closed tab use this transition type, too. |
| "keyword" | The URL was generated from a replaceable keyword other than the default search provider. See also [keyword_generated](#tt_keyword_generated). |
| "keyword_generated" | Corresponds to a visit generated for a keyword. See also [keyword](#tt_keyword). |

## Examples

For examples of using this API, see the [history sample directory](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/docs/examples/api/history/) and the [history API test directory](https://chromium.googlesource.com/chromium/src/+/master/chrome/test/data/extensions/api_test/history/). For other examples and for help in viewing the source code, see [Samples](samples).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [TransitionType](#type-TransitionType) |
| [HistoryItem](#type-HistoryItem) |
| [VisitItem](#type-VisitItem) |
| Methods |
| [search](#method-search) − `whale.history.search(<span>object query</span>, <span>function callback</span>)` |
| [getVisits](#method-getVisits) − `whale.history.getVisits(<span>object details</span>, <span>function callback</span>)` |
| [addUrl](#method-addUrl) − `whale.history.addUrl(<span>object details</span>, <span class="optional">function callback</span>)` |
| [deleteUrl](#method-deleteUrl) − `whale.history.deleteUrl(<span>object details</span>, <span class="optional">function callback</span>)` |
| [deleteRange](#method-deleteRange) − `whale.history.deleteRange(<span>object range</span>, <span>function callback</span>)` |
| [deleteAll](#method-deleteAll) − `whale.history.deleteAll(<span>function callback</span>)` |
| Events |
| [onVisited](#event-onVisited) |
| [onVisitRemoved](#event-onVisitRemoved) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### TransitionType

<dd>The [transition type](#transition_types) for this visit from its referrer.</dd>

| Enum |
|---|
| `"link"`, `"typed"`, `"auto_bookmark"`, `"auto_subframe"`, `"manual_subframe"`, `"generated"`, `"auto_toplevel"`, `"form_submit"`, `"reload"`, `"keyword"`, or `"keyword_generated"` |

</div>

<div>

### HistoryItem

<dd>An object encapsulating one result of a history query.</dd>

| properties |
|---|
| string | id | 

The unique identifier for the item.
 |
| string | <span class="optional">(optional)</span> url | 

The URL navigated to by a user.
 |
| string | <span class="optional">(optional)</span> title | 

The title of the page when it was last loaded.
 |
| double | <span class="optional">(optional)</span> lastVisitTime | 

When this page was last loaded, represented in milliseconds since the epoch.
 |
| integer | <span class="optional">(optional)</span> visitCount | 

The number of times the user has navigated to this page.
 |
| integer | <span class="optional">(optional)</span> typedCount | 

The number of times the user has navigated to this page by typing in the address.
 |

</div>

<div>

### VisitItem

<dd>An object encapsulating one visit to a URL.</dd>

| properties |
|---|
| string | id | 

The unique identifier for the item.
 |
| string | visitId | 

The unique identifier for this visit.
 |
| double | <span class="optional">(optional)</span> visitTime | 

When this visit occurred, represented in milliseconds since the epoch.
 |
| string | referringVisitId | 

The visit ID of the referrer.
 |
| [TransitionType](/extensions/history#type-TransitionType) | transition | 

The [transition type](#transition_types) for this visit from its referrer.
 |

</div>

## Methods

<div>

### search

<div class="summary">`whale.history.search(<span>object query</span>, <span>function callback</span>)`</div>

<div class="description">

Searches the history for the last visit time of each page matching the query.

| Parameters |
|---|
| object | query | 

| string | text | 
|---|---|

A free-text query to the history service. Leave empty to retrieve all pages.
 |
| double | <span class="optional">(optional)</span> startTime | 

Limit results to those visited after this date, represented in milliseconds since the epoch. If not specified, this defaults to 24 hours in the past.
 |
| double | <span class="optional">(optional)</span> endTime | 

Limit results to those visited before this date, represented in milliseconds since the epoch.
 |
| integer | <span class="optional">(optional)</span> maxResults | 

The maximum number of results to retrieve. Defaults to 100.
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [HistoryItem](/extensions/history#type-HistoryItem) results) <span class="subdued">{...}</span>;`

| array of [HistoryItem](/extensions/history#type-HistoryItem) | results |  |
|---|---|---|
 |

</div>

</div>

<div>

### getVisits

<div class="summary">`whale.history.getVisits(<span>object details</span>, <span>function callback</span>)`</div>

<div class="description">

Retrieves information about visits to a URL.

| Parameters |
|---|
| object | details | 

| string | url | 
|---|---|

The URL for which to retrieve visit information. It must be in the format as returned from a call to history.search.
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [VisitItem](/extensions/history#type-VisitItem) results) <span class="subdued">{...}</span>;`

| array of [VisitItem](/extensions/history#type-VisitItem) | results |  |
|---|---|---|
 |

</div>

</div>

<div>

### addUrl

<div class="summary">`whale.history.addUrl(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Adds a URL to the history at the current time with a [transition type](#transition_types) of "link".

| Parameters |
|---|
| object | details | 

| string | url | 
|---|---|

The URL to add.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### deleteUrl

<div class="summary">`whale.history.deleteUrl(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Removes all occurrences of the given URL from the history.

| Parameters |
|---|
| object | details | 

| string | url | 
|---|---|

The URL to remove.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### deleteRange

<div class="summary">`whale.history.deleteRange(<span>object range</span>, <span>function callback</span>)`</div>

<div class="description">

Removes all items within the specified date range from the history. Pages will not be removed from the history unless all visits fall within the range.

| Parameters |
|---|
| object | range | 

| double | startTime | 
|---|---|

Items added to history after this date, represented in milliseconds since the epoch.
 |
| double | endTime | 

Items added to history before this date, represented in milliseconds since the epoch.
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### deleteAll

<div class="summary">`whale.history.deleteAll(<span>function callback</span>)`</div>

<div class="description">

Deletes all items from the history.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

## Events

<div>

### onVisited

<div class="description">

Fired when a URL is visited, providing the HistoryItem data for that URL. This event fires before the page has loaded.

<div>

#### addListener

<div class="summary">`whale.history.onVisited.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [HistoryItem](/extensions/history#type-HistoryItem) result) <span class="subdued">{...}</span>;`

| [HistoryItem](/extensions/history#type-HistoryItem) | result |  |
|---|---|---|
 |

</div>

</div>

</div>

</div>

<div>

### onVisitRemoved

<div class="description">

Fired when one or more URLs are removed from the history service. When all visits have been removed the URL is purged from history.

<div>

#### addListener

<div class="summary">`whale.history.onVisitRemoved.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object removed) <span class="subdued">{...}</span>;`

| object | removed | 
|---|---|

| boolean | allHistory | 
|---|---|

True if all history was removed. If true, then urls will be empty.
 |
| array of string | <span class="optional">(optional)</span> urls |  |
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>