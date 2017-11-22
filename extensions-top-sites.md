# whale.topSites

| Description: | Use the `whale.topSites` API to access the top sites that are displayed on the new tab page. |
|---|---|
| Availability: | Since Chrome 19. |
| Permissions: | <span class="code">"topSites"</span> |

<section>

## Manifest

You must declare the "topSites" permission in your extension's manifest to use this API.

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
      **  "permissions": [
          "topSites",
        ]**,
        ...
      }
      </pre>

## Examples

You can find samples of this API in [Samples](samples#search:topsites).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [MostVisitedURL](#type-MostVisitedURL) |
| Methods |
| [get](#method-get) − `whale.topSites.get(<span>function callback</span>)` |

</section>

<section>

<div class="api-reference">

## Types

<div>

### MostVisitedURL

<dd>An object encapsulating a most visited URL, such as the URLs on the new tab page.</dd>

| properties |
|---|
| string | url | 

The most visited URL.
 |
| string | title | 

The title of the page
 |

</div>

## Methods

<div>

### get

<div class="summary">`whale.topSites.get(<span>function callback</span>)`</div>

<div class="description">

Gets a list of top sites.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [MostVisitedURL](/extensions/topSites#type-MostVisitedURL) data) <span class="subdued">{...}</span>;`

| array of [MostVisitedURL](/extensions/topSites#type-MostVisitedURL) | data |  |
|---|---|---|
 |

</div>

</div>

</div>

</section>