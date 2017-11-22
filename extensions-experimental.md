# whale.experimental.* APIs

## List of APIs

We'd like your [feedback](http://groups.google.com/a/chromium.org/group/chromium-extensions/topics) on the following experimental APIs:

| Name | Description |
|---|---|
| [experimental.devtools.audits](experimental.devtools.audits) | Use the `whale.experimental.devtools.audits` API to add new audit categories to the Developer Tools' Audit panel. |

Pay special attention to the following APIs, which we expect to finalize soon: **devtools**, **permissions**, For examples of using the experimental APIs, see [Samples](samples#search:experimental).

**Caution:** Don't depend on these experimental APIs. They might disappear, and they _will_ change. Also, the Chrome Web Store doesn't allow you to upload items that use experimental APIs.

## How to use experimental APIs

1.  Make sure you're using either [Canary](http://tools.google.com/dlpage/chromesxs) (which you can use at the same time as other Chrome channels) or the [Dev channel](http://www.chromium.org/getting-involved/dev-channel). Although the experimental APIs might work in other versions, we need your feedback on the latest incarnation of the APIs, which you can find in Canary and on the Dev channel.
2.  Specify the "experimental" [permission](declare_permissions) in your manifest, like this:

    <pre data-filename="manifest.json">"permissions": [
      **"experimental"**,
      ...
    ],
    </pre>

3.  Enable the experimental API in your browser. You can do this in either of two ways:
    *   Go to **chrome://flags**, find "Experimental Extension APIs", click its "Enable" link, and restart Chrome. From now on, unless you return to that page and disable experimental APIs, you'll be able to run extensions and apps that use experimental APIs.
    *   Specify the **--enable-experimental-extension-apis** flag each time you launch the browser. On Windows, you can do this by modifying the properties of the shortcut that you use to launch Google Chrome. For example:

        <pre>_path_to_whale.exe_ **--enable-experimental-extension-apis**</pre>

4.  [Give us feedback!](http://groups.google.com/a/chromium.org/group/chromium-extensions/topics) Your comments and suggestions help us improve the APIs and decide which ones should move from experimental to supported.

## More APIs

For information on the standard APIs, see [whale.* APIs](api_index) and [Other APIs](api_other).

<footer id="cc-info">

Content available under the [CC-By 3.0 license](http://creativecommons.org/licenses/by/3.0/)

</footer>