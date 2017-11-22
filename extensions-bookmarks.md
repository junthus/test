# whale.bookmarks

| Description: | Use the `whale.bookmarks` API to create, organize, and otherwise manipulate bookmarks. Also see [Override Pages](override), which you can use to create a custom Bookmark Manager page. |
|---|---|
| Availability: | Since Chrome 20. |
| Permissions: | <span class="code">"bookmarks"</span> |

<section>![Clicking the star adds a bookmark](/static/images/bookmarks.png)

## Manifest

You must declare the "bookmarks" permission in the [extension manifest](manifest) to use the bookmarks API. For example:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"permissions": [
          "bookmarks"
        ]**,
        ...
      }
      </pre>

## Objects and properties

Bookmarks are organized in a tree, where each node in the tree is either a bookmark or a folder (sometimes called a _group_). Each node in the tree is represented by a [bookmarks.BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) object.

`BookmarkTreeNode` properties are used throughout the `whale.bookmarks` API. For example, when you call [bookmarks.create](/extensions/bookmarks#method-create), you pass in the new node's parent (`parentId`), and, optionally, the node's `index`, `title`, and `url` properties. See [bookmarks.BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) for information about the properties a node can have.

**Note:** You cannot use this API to add or remove entries in the root folder. You also cannot rename, move, or remove the special "Bookmarks Bar" and "Other Bookmarks" folders.

## Examples

The following code creates a folder with the title "Extension bookmarks". The first argument to `create()` specifies properties for the new folder. The second argument defines a function to be executed after the folder is created.

<pre>      whale.bookmarks.create({'parentId': bookmarkBar.id,
                               'title': 'Extension bookmarks'},
                              function(newFolder) {
        console.log("added folder: " + newFolder.title);
      });
      </pre>

The next snippet creates a bookmark pointing to the developer documentation for extensions. Since nothing bad will happen if creating the bookmark fails, this code doesn't bother to define a callback function.

<pre>      whale.bookmarks.create({'parentId': extensionsFolderId,
                               'title': 'Extensions doc',
                               'url': 'http://code.google.com/chrome/extensions'});
      </pre>

For an example of using this API, see the [basic bookmarks sample](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/docs/examples/api/bookmarks/basic/). For other examples and for help in viewing the source code, see [Samples](samples).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [BookmarkTreeNodeUnmodifiable](#type-BookmarkTreeNodeUnmodifiable) |
| [BookmarkTreeNode](#type-BookmarkTreeNode) |
| Properties |
| [MAX_WRITE_OPERATIONS_PER_HOUR](#property-MAX_WRITE_OPERATIONS_PER_HOUR) |
| [MAX_SUSTAINED_WRITE_OPERATIONS_PER_MINUTE](#property-MAX_SUSTAINED_WRITE_OPERATIONS_PER_MINUTE) |
| Methods |
| [get](#method-get) − `whale.bookmarks.get(<span>string or array of string idOrIdList</span>, <span>function callback</span>)` |
| [getChildren](#method-getChildren) − `whale.bookmarks.getChildren(<span>string id</span>, <span>function callback</span>)` |
| [getRecent](#method-getRecent) − `whale.bookmarks.getRecent(<span>integer numberOfItems</span>, <span>function callback</span>)` |
| [getTree](#method-getTree) − `whale.bookmarks.getTree(<span>function callback</span>)` |
| [getSubTree](#method-getSubTree) − `whale.bookmarks.getSubTree(<span>string id</span>, <span>function callback</span>)` |
| [search](#method-search) − `whale.bookmarks.search(<span>string or object query</span>, <span>function callback</span>)` |
| [create](#method-create) − `whale.bookmarks.create(<span>object bookmark</span>, <span class="optional">function callback</span>)` |
| [move](#method-move) − `whale.bookmarks.move(<span>string id</span>, <span>object destination</span>, <span class="optional">function callback</span>)` |
| [update](#method-update) − `whale.bookmarks.update(<span>string id</span>, <span>object changes</span>, <span class="optional">function callback</span>)` |
| [remove](#method-remove) − `whale.bookmarks.remove(<span>string id</span>, <span class="optional">function callback</span>)` |
| [removeTree](#method-removeTree) − `whale.bookmarks.removeTree(<span>string id</span>, <span class="optional">function callback</span>)` |
| Events |
| [onCreated](#event-onCreated) |
| [onRemoved](#event-onRemoved) |
| [onChanged](#event-onChanged) |
| [onMoved](#event-onMoved) |
| [onChildrenReordered](#event-onChildrenReordered) |
| [onImportBegan](#event-onImportBegan) |
| [onImportEnded](#event-onImportEnded) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### BookmarkTreeNodeUnmodifiable

<dd>Indicates the reason why this node is unmodifiable. The <var>managed</var> value indicates that this node was configured by the system administrator. Omitted if the node can be modified by the user and the extension (default).</dd>

| Enum |
|---|
| `"managed"` |

</div>

<div>

### BookmarkTreeNode

<dd>A node (either a bookmark or a folder) in the bookmark tree. Child nodes are ordered within their parent folder.</dd>

| properties |
|---|
| string | id | 

The unique identifier for the node. IDs are unique within the current profile, and they remain valid even after the browser is restarted.
 |
| string | <span class="optional">(optional)</span> parentId | 

The `id` of the parent folder. Omitted for the root node.
 |
| integer | <span class="optional">(optional)</span> index | 

The 0-based position of this node within its parent folder.
 |
| string | <span class="optional">(optional)</span> url | 

The URL navigated to when a user clicks the bookmark. Omitted for folders.
 |
| string | title | 

The text displayed for the node.
 |
| double | <span class="optional">(optional)</span> dateAdded | 

When this node was created, in milliseconds since the epoch (`new Date(dateAdded)`).
 |
| double | <span class="optional">(optional)</span> dateGroupModified | 

When the contents of this folder last changed, in milliseconds since the epoch.
 |
| [BookmarkTreeNodeUnmodifiable](/extensions/bookmarks#type-BookmarkTreeNodeUnmodifiable) | <span class="optional">(optional)</span> unmodifiable | 

Since Chrome 37.

Indicates the reason why this node is unmodifiable. The <var>managed</var> value indicates that this node was configured by the system administrator or by the custodian of a supervised user. Omitted if the node can be modified by the user and the extension (default).
 |
| array of [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) | <span class="optional">(optional)</span> children | 

An ordered list of children of this node.
 |

</div>

## Properties

| <span class="type_name">`1,000,000`</span> | `whale.bookmarks.MAX_WRITE_OPERATIONS_PER_HOUR` | 
|---|---|

**Deprecated** since Chrome 38. Bookmark write operations are no longer limited by Chrome.
 |
| <span class="type_name">`1,000,000`</span> | `whale.bookmarks.MAX_SUSTAINED_WRITE_OPERATIONS_PER_MINUTE` | 

**Deprecated** since Chrome 38. Bookmark write operations are no longer limited by Chrome.
 |

## Methods

<div>

### get

<div class="summary">`whale.bookmarks.get(<span>string or array of string idOrIdList</span>, <span>function callback</span>)`</div>

<div class="description">

Retrieves the specified BookmarkTreeNode(s).

| Parameters |
|---|
| string or array of string | idOrIdList | 

A single string-valued id, or an array of string-valued ids
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) results) <span class="subdued">{...}</span>;`

| array of [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) | results |  |
|---|---|---|
 |

</div>

</div>

<div>

### getChildren

<div class="summary">`whale.bookmarks.getChildren(<span>string id</span>, <span>function callback</span>)`</div>

<div class="description">

Retrieves the children of the specified BookmarkTreeNode id.

| Parameters |
|---|
| string | id |  |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) results) <span class="subdued">{...}</span>;`

| array of [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) | results |  |
|---|---|---|
 |

</div>

</div>

<div>

### getRecent

<div class="summary">`whale.bookmarks.getRecent(<span>integer numberOfItems</span>, <span>function callback</span>)`</div>

<div class="description">

Retrieves the recently added bookmarks.

| Parameters |
|---|
| integer | numberOfItems | 

The maximum number of items to return.
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) results) <span class="subdued">{...}</span>;`

| array of [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) | results |  |
|---|---|---|
 |

</div>

</div>

<div>

### getTree

<div class="summary">`whale.bookmarks.getTree(<span>function callback</span>)`</div>

<div class="description">

Retrieves the entire Bookmarks hierarchy.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) results) <span class="subdued">{...}</span>;`

| array of [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) | results |  |
|---|---|---|
 |

</div>

</div>

<div>

### getSubTree

<div class="summary">`whale.bookmarks.getSubTree(<span>string id</span>, <span>function callback</span>)`</div>

<div class="description">

Retrieves part of the Bookmarks hierarchy, starting at the specified node.

| Parameters |
|---|
| string | id | 

The ID of the root of the subtree to retrieve.
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) results) <span class="subdued">{...}</span>;`

| array of [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) | results |  |
|---|---|---|
 |

</div>

</div>

<div>

### search

<div class="summary">`whale.bookmarks.search(<span>string or object query</span>, <span>function callback</span>)`</div>

<div class="description">

Searches for BookmarkTreeNodes matching the given query. Queries specified with an object produce BookmarkTreeNodes matching all specified properties.

| Parameters |
|---|
| string or object | query | 

Either a string of words and quoted phrases that are matched against bookmark URLs and titles, or an object. If an object, the properties `query`, `url`, and `title` may be specified and bookmarks matching all specified properties will be produced.
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) results) <span class="subdued">{...}</span>;`

| array of [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) | results |  |
|---|---|---|
 |

</div>

</div>

<div>

### create

<div class="summary">`whale.bookmarks.create(<span>object bookmark</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Creates a bookmark or folder under the specified parentId. If url is NULL or missing, it will be a folder.

| Parameters |
|---|
| object | bookmark | 

| string | <span class="optional">(optional)</span> parentId | 
|---|---|

Defaults to the Other Bookmarks folder.
 |
| integer | <span class="optional">(optional)</span> index |  |
| string | <span class="optional">(optional)</span> title |  |
| string | <span class="optional">(optional)</span> url |  |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) result) <span class="subdued">{...}</span>;`

| [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) | result |  |
|---|---|---|
 |

</div>

</div>

<div>

### move

<div class="summary">`whale.bookmarks.move(<span>string id</span>, <span>object destination</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Moves the specified BookmarkTreeNode to the provided location.

| Parameters |
|---|
| string | id |  |
| object | destination | 

| string | <span class="optional">(optional)</span> parentId |  |
|---|---|---|
| integer | <span class="optional">(optional)</span> index |  |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) result) <span class="subdued">{...}</span>;`

| [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) | result |  |
|---|---|---|
 |

</div>

</div>

<div>

### update

<div class="summary">`whale.bookmarks.update(<span>string id</span>, <span>object changes</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Updates the properties of a bookmark or folder. Specify only the properties that you want to change; unspecified properties will be left unchanged. **Note:** Currently, only 'title' and 'url' are supported.

| Parameters |
|---|
| string | id |  |
| object | changes | 

| string | <span class="optional">(optional)</span> title |  |
|---|---|---|
| string | <span class="optional">(optional)</span> url |  |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function( [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) result) <span class="subdued">{...}</span>;`

| [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) | result |  |
|---|---|---|
 |

</div>

</div>

<div>

### remove

<div class="summary">`whale.bookmarks.remove(<span>string id</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Removes a bookmark or an empty bookmark folder.

| Parameters |
|---|
| string | id |  |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### removeTree

<div class="summary">`whale.bookmarks.removeTree(<span>string id</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Recursively removes a bookmark folder.

| Parameters |
|---|
| string | id |  |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

## Events

<div>

### onCreated

<div class="description">

Fired when a bookmark or folder is created.

<div>

#### addListener

<div class="summary">`whale.bookmarks.onCreated.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string id, [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) bookmark) <span class="subdued">{...}</span>;`

| string | id |  |
|---|---|---|
| [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) | bookmark |  |
 |

</div>

</div>

</div>

</div>

<div>

### onRemoved

<div class="description">

Fired when a bookmark or folder is removed. When a folder is removed recursively, a single notification is fired for the folder, and none for its contents.

<div>

#### addListener

<div class="summary">`whale.bookmarks.onRemoved.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string id, object removeInfo) <span class="subdued">{...}</span>;`

| string | id |  |
|---|---|---|
| object | removeInfo | 

| string | parentId |  |
|---|---|---|
| integer | index |  |
| [BookmarkTreeNode](/extensions/bookmarks#type-BookmarkTreeNode) | node | 

Since Chrome 48.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onChanged

<div class="description">

Fired when a bookmark or folder changes. **Note:** Currently, only title and url changes trigger this.

<div>

#### addListener

<div class="summary">`whale.bookmarks.onChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string id, object changeInfo) <span class="subdued">{...}</span>;`

| string | id |  |
|---|---|---|
| object | changeInfo | 

| string | title |  |
|---|---|---|
| string | <span class="optional">(optional)</span> url |  |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onMoved

<div class="description">

Fired when a bookmark or folder is moved to a different parent folder.

<div>

#### addListener

<div class="summary">`whale.bookmarks.onMoved.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string id, object moveInfo) <span class="subdued">{...}</span>;`

| string | id |  |
|---|---|---|
| object | moveInfo | 

| string | parentId |  |
|---|---|---|
| integer | index |  |
| string | oldParentId |  |
| integer | oldIndex |  |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onChildrenReordered

<div class="description">

Fired when the children of a folder have changed their order due to the order being sorted in the UI. This is not called as a result of a move().

<div>

#### addListener

<div class="summary">`whale.bookmarks.onChildrenReordered.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string id, object reorderInfo) <span class="subdued">{...}</span>;`

| string | id |  |
|---|---|---|
| object | reorderInfo | 

| array of string | childIds |  |
|---|---|---|
 |
 |

</div>

</div>

</div>

</div>

<div>

### onImportBegan

<div class="description">

Fired when a bookmark import session is begun. Expensive observers should ignore onCreated updates until onImportEnded is fired. Observers should still handle other notifications immediately.

<div>

#### addListener

<div class="summary">`whale.bookmarks.onImportBegan.addListener(<span>function callback</span>)`</div>

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

### onImportEnded

<div class="description">

Fired when a bookmark import session is ended.

<div>

#### addListener

<div class="summary">`whale.bookmarks.onImportEnded.addListener(<span>function callback</span>)`</div>

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

</div>

</section>