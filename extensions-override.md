# Override Pages

<style>#pics { margin:2em 1em 1.5em; } #pics td { text-align:center; width:50%!important; border:none; padding:0 1em; font-size:90%; } #pics img { width:188; height:246; border:none; }</style>

Override pages are a way to substitute an HTML file from your extension for a page that Google Chrome normally provides. In addition to HTML, an override page usually has CSS and JavaScript code.

An extension can replace any one of the following pages:

*   **Bookmark Manager:** The page that appears when the user chooses the Bookmark Manager menu item from the Chrome menu or, on Mac, the Bookmark Manager item from the Bookmarks menu. You can also get to this page by entering the URL **chrome://bookmarks**.
*   **History:** The page that appears when the user chooses the History menu item from the Chrome menu or, on Mac, the Show Full History item from the History menu. You can also get to this page by entering the URL **chrome://history**.
*   **New Tab:** The page that appears when the user creates a new tab or window. You can also get to this page by entering the URL **chrome://newtab**.

**Note:** A single extension can override **only one page**. For example, an extension can't override both the Bookmark Manager and History pages.

Incognito windows are treated specially. New Tab pages cannot be overridden in incognito windows. Other override pages work in incognito windows as long as the [incognito](manifest/incognito) manifest property is set to "spanning" (which is the default value). See [Saving data and incognito mode](overview#incognito) in the Overview for more details on how you should treat incognito windows.

The following screenshots show the default New Tab page next to a custom New Tab page.

| **The default New Tab page** | **An alternative New Tab page** |
|---|---|
| ![default New Tab page](/static/images/ntp-default.png) | ![a blank New Tab page](/static/images/ntp-blank.png) |

## Manifest

Register an override page in the [extension manifest](manifest) like this:

<pre>{
  "name": "My extension",
  ...

 **"chrome_url_overrides" : {
    "_pageToOverride_": "_myPage.html_"
  }**,
  ...
}</pre>

For `_pageToOverride_`, substitute one of the following:

*   `bookmarks`
*   `history`
*   `newtab`

## Tips

For an effective override page, follow these guidelines:

*   **Make your page quick and small.**
    Users expect built-in browser pages to open instantly. Avoid doing things that might take a long time. For example, avoid synchronous fetches of network or database resources.

*   **Include a title in your page.**
    Otherwise people might see the URL of the page, which could be confusing. Here's an example of specifying the title: `<title>New Tab</title>`

*   **Don't rely on the page having the keyboard focus.**
    The address bar always gets the focus first when the user creates a new tab.

*   **Don't try to emulate the default New Tab page.**
    The APIs necessary to create a slightly modified version of the default New Tab page — with top pages, recently closed pages, tips, a theme background image, and so on — don't exist yet. Until they do, you're better off trying to make something completely different.

## Examples

See the [override samples](samples#search:chrome_url_overrides).

<footer id="cc-info">

Content available under the [CC-By 3.0 license](http://creativecommons.org/licenses/by/3.0/)

</footer>