# whale.omnibox

| Description: | The omnibox API allows you to register a keyword with Google Chrome's address bar, which is also known as the omnibox. |
|---|---|
| Availability: | Since Chrome 20. |
| Manifest: | <span class="code">"omnibox": {...}</span> |

<section>

![A screenshot showing suggestions related to the keyword 'Chromium Search'](/static/images/omnibox.png)

When the user enters your extension's keyword, the user starts interacting solely with your extension. Each keystroke is sent to your extension, and you can provide suggestions in response.

The suggestions can be richly formatted in a variety of ways. When the user accepts a suggestion, your extension is notified and can take action.

## Manifest

You must include an `omnibox` `keyword` field in the [manifest](manifest) to use the omnibox API. You should also specify a 16x16-pixel icon, which will be displayed in the address bar when suggesting that users enter keyword mode.

For example:

<pre data-filename="manifest.json">      {
        "name": "Aaron's omnibox extension",
        "version": "1.0",
        **"omnibox": { "keyword" : "aaron" },**
        **"icons": {**
          **"16": "16-full-color.png"**
        **},**
        "background": {
          "persistent": false,
          "scripts": ["background.js"]
        }
      }
      </pre>

**Note:** Chrome automatically creates a grayscale version of your 16x16-pixel icon. You should provide a full-color version so that it can also be used in other situations that require color. For example, the [context menus API](contextMenus) also uses a 16x16-pixel icon, but it is displayed in color.

## Examples

You can find samples of this API on the [sample page](samples#search:omnibox).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [DescriptionStyleType](#type-DescriptionStyleType) |
| [OnInputEnteredDisposition](#type-OnInputEnteredDisposition) |
| [SuggestResult](#type-SuggestResult) |
| Methods |
| [setDefaultSuggestion](#method-setDefaultSuggestion) − `whale.omnibox.setDefaultSuggestion(<span>object suggestion</span>)` |
| Events |
| [onInputStarted](#event-onInputStarted) |
| [onInputChanged](#event-onInputChanged) |
| [onInputEntered](#event-onInputEntered) |
| [onInputCancelled](#event-onInputCancelled) |
| [onDeleteSuggestion](#event-onDeleteSuggestion) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### DescriptionStyleType

<dd>The style type.</dd>

| Enum |
|---|
| `"url"`, `"match"`, or `"dim"` |

</div>

<div>

### OnInputEnteredDisposition

<dd>The window disposition for the omnibox query. This is the recommended context to display results. For example, if the omnibox command is to navigate to a certain URL, a disposition of 'newForegroundTab' means the navigation should take place in a new selected tab.</dd>

| Enum |
|---|
| `"currentTab"`, `"newForegroundTab"`, or `"newBackgroundTab"` |

</div>

<div>

### SuggestResult

<dd>A suggest result.</dd>

| properties |
|---|
| string | content | 

The text that is put into the URL bar, and that is sent to the extension when the user chooses this entry.
 |
| string | description | 

The text that is displayed in the URL dropdown. Can contain XML-style markup for styling. The supported tags are 'url' (for a literal URL), 'match' (for highlighting text that matched what the user's query), and 'dim' (for dim helper text). The styles can be nested, eg. <dim><match>dimmed match</match></dim>. You must escape the five predefined entities to display them as text: stackoverflow.com/a/1091953/89484
 |
| boolean | <span class="optional">(optional)</span> deletable | 

Since Chrome 63. _Warning:_ this is the current **Dev** channel. [Learn more](api_index#dev_apis).

Whether the suggest result can be deleted by the user.
 |

</div>

## Methods

<div>

### setDefaultSuggestion

<div class="summary">`whale.omnibox.setDefaultSuggestion(<span>object suggestion</span>)`</div>

<div class="description">

Sets the description and styling for the default suggestion. The default suggestion is the text that is displayed in the first suggestion row underneath the URL bar.

| Parameters |
|---|
| object | suggestion | 

A partial SuggestResult object, without the 'content' parameter.

| string | description | 
|---|---|

The text that is displayed in the URL dropdown. Can contain XML-style markup for styling. The supported tags are 'url' (for a literal URL), 'match' (for highlighting text that matched what the user's query), and 'dim' (for dim helper text). The styles can be nested, eg. <dim><match>dimmed match</match></dim>.
 |
 |

</div>

</div>

## Events

<div>

### onInputStarted

<div class="description">

User has started a keyword input session by typing the extension's keyword. This is guaranteed to be sent exactly once per input session, and before any onInputChanged events.

<div>

#### addListener

<div class="summary">`whale.omnibox.onInputStarted.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

</div>

</div>

<div>

### onInputChanged

<div class="description">

User has changed what is typed into the omnibox.

<div>

#### addListener

<div class="summary">`whale.omnibox.onInputChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string text, function suggest) <span class="subdued">{...}</span>;`

| string | text |  |
|---|---|---|
| function | suggest | 

A callback passed to the onInputChanged event used for sending suggestions back to the browser.

The _suggest_ parameter should be a function that looks like this:

`function(array of [SuggestResult](/extensions/omnibox#type-SuggestResult) suggestResults) <span class="subdued">{...}</span>;`

| array of [SuggestResult](/extensions/omnibox#type-SuggestResult) | suggestResults | 
|---|---|

Array of suggest results
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onInputEntered

<div class="description">

User has accepted what is typed into the omnibox.

<div>

#### addListener

<div class="summary">`whale.omnibox.onInputEntered.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string text, [OnInputEnteredDisposition](/extensions/omnibox#type-OnInputEnteredDisposition) disposition) <span class="subdued">{...}</span>;`

| string | text |  |
|---|---|---|
| [OnInputEnteredDisposition](/extensions/omnibox#type-OnInputEnteredDisposition) | disposition | 

Since Chrome 28.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onInputCancelled

<div class="description">

User has ended the keyword input session without accepting the input.

<div>

#### addListener

<div class="summary">`whale.omnibox.onInputCancelled.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

</div>

</div>

<div>

### onDeleteSuggestion

<div class="description">

Since Chrome 63. _Warning:_ this is the current **Dev** channel. [Learn more](api_index#dev_apis).

User has deleted a suggested result.

<div>

#### addListener

<div class="summary">`whale.omnibox.onDeleteSuggestion.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string text) <span class="subdued">{...}</span>;`

| string | text | 
|---|---|

Text of the deleted suggestion.
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>