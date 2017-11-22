# Message Passing

Since content scripts run in the context of a web page and not the extension, they often need some way of communicating with the rest of the extension. For example, an RSS reader extension might use content scripts to detect the presence of an RSS feed on a page, then notify the background page in order to display a page action icon for that page.

Communication between extensions and their content scripts works by using message passing. Either side can listen for messages sent from the other end, and respond on the same channel. A message can contain any valid JSON object (null, boolean, number, string, array, or object). There is a simple API for [one-time requests](#simple) and a more complex API that allows you to have [long-lived connections](#connect) for exchanging multiple messages with a shared context. It is also possible to send a message to another extension if you know its ID, which is covered in the [cross-extension messages](#external) section.

## Simple one-time requests

If you only need to send a single message to another part of your extension (and optionally get a response back), you should use the simplified [runtime.sendMessage](/extensions/runtime#method-sendMessage) or [tabs.sendMessage](/extensions/tabs#method-sendMessage) . This lets you send a one-time JSON-serializable message from a content script to extension , or vice versa, respectively . An optional callback parameter allows you handle the response from the other side, if there is one.

Sending a request from a content script looks like this:

<pre data-filename="contentscript.js">whale.runtime.sendMessage({greeting: "hello"}, function(response) {
  console.log(response.farewell);
});
</pre>

Sending a request from the extension to a content script looks very similar, except that you need to specify which tab to send it to. This example demonstrates sending a message to the content script in the selected tab.

<pre data-filename="background.html">whale.tabs.query({active: true, currentWindow: true}, function(tabs) {
  whale.tabs.sendMessage(tabs[0].id, {greeting: "hello"}, function(response) {
    console.log(response.farewell);
  });
});
</pre>

On the receiving end, you need to set up an [runtime.onMessage](/extensions/runtime#event-onMessage) event listener to handle the message. This looks the same from a content script or extension page.

<pre>whale.runtime.onMessage.addListener(
  function(request, sender, sendResponse) {
    console.log(sender.tab ?
                "from a content script:" + sender.tab.url :
                "from the extension");
    if (request.greeting == "hello")
      sendResponse({farewell: "goodbye"});
  });
</pre>

In the above example, `sendResponse` was called synchronously. If you want to asynchronously use `sendResponse`, add `return true;` to the `onMessage` event handler.

**Note:** If multiple pages are listening for onMessage events, only the first to call sendResponse() for a particular event will succeed in sending the response. All other responses to that event will be ignored.

**Note:** The `sendResponse` callback is only valid if used synchronously, or if the event handler returns `true` to indicate that it will respond asynchronously. The `sendMessage` function's callback will be invoked automatically if no handlers return true or if the `sendResponse` callback is garbage-collected.

## Long-lived connections

Sometimes it's useful to have a conversation that lasts longer than a single request and response. In this case, you can open a long-lived channel from your content script to an extension page , or vice versa, using [runtime.connect](/extensions/runtime#method-connect) or [tabs.connect](/extensions/tabs#method-connect), respectively . The channel can optionally have a name, allowing you to distinguish between different types of connections.

One use case might be an automatic form fill extension. The content script could open a channel to the extension page for a particular login, and send a message to the extension for each input element on the page to request the form data to fill in. The shared connection allows the extension to keep shared state linking the several messages coming from the content script.

When establishing a connection, each end is given a [runtime.Port](/extensions/runtime#type-Port) object which is used for sending and receiving messages through that connection.

Here is how you open a channel from a content script, and send and listen for messages:

<pre data-filename="contentscript.js">var port = whale.runtime.connect({name: "knockknock"});
port.postMessage({joke: "Knock knock"});
port.onMessage.addListener(function(msg) {
  if (msg.question == "Who's there?")
    port.postMessage({answer: "Madame"});
  else if (msg.question == "Madame who?")
    port.postMessage({answer: "Madame... Bovary"});
});
</pre>

Sending a request from the extension to a content script looks very similar, except that you need to specify which tab to connect to. Simply replace the call to connect in the above example with [tabs.connect](/extensions/tabs#method-connect).

In order to handle incoming connections, you need to set up a [runtime.onConnect](/extensions/runtime#event-onConnect) event listener. This looks the same from a content script or an extension page. When another part of your extension calls "connect()", this event is fired, along with the [runtime.Port](/extensions/runtime#type-Port) object you can use to send and receive messages through the connection. Here's what it looks like to respond to incoming connections:

<pre>whale.runtime.onConnect.addListener(function(port) {
  console.assert(port.name == "knockknock");
  port.onMessage.addListener(function(msg) {
    if (msg.joke == "Knock knock")
      port.postMessage({question: "Who's there?"});
    else if (msg.answer == "Madame")
      port.postMessage({question: "Madame who?"});
    else if (msg.answer == "Madame... Bovary")
      port.postMessage({question: "I don't get it."});
  });
});
</pre>

### Port lifetime

Ports are designed as a two-way communication method between different parts of the extension, where a (top-level) frame is viewed as the smallest part.
Upon calling [tabs.connect](/extensions/tabs#method-connect), [runtime.connect](/extensions/runtime#method-connect) or [runtime.connectNative](/extensions/runtime#method-connectNative), a [Port](/extensions/runtime#type-Port) is created. This port can immediately be used for sending messages to the other end via [postMessage](/extensions/runtime#property-Port-postMessage).

If there are multiple frames in a tab, calling [tabs.connect](/extensions/tabs#method-connect) results in multiple invocations of the [runtime.onConnect](/extensions/runtime#event-onConnect) event (once for each frame in the tab). Similarly, if [runtime.connect](/extensions/runtime#method-connect) is used, then the onConnect event may be fired multiple times (once for every frame in the extension process).

You may want to find out when a connection is closed, for example if you are maintaining separate state for each open port. For this you can listen to the [runtime.Port.onDisconnect](/extensions/runtime#property-Port-onDisconnect) event. This event is fired when there are no valid ports at the other side of the channel. This happens in the following situations:

*   There are no listeners for [runtime.onConnect](/extensions/runtime#event-onConnect) at the other end.
*   The tab containing the port is unloaded (e.g. if the tab is navigated).
*   The frame from where `connect` was called has unloaded.
*   All frames that received the port (via [runtime.onConnect](/extensions/runtime#event-onConnect)) have unloaded.
*   [runtime.Port.disconnect](/extensions/runtime#property-Port-disconnect) is called by _the other end_. Note that if a `connect` call results in multiple ports at the receiver's end, and `disconnect()` is called on any of these ports, then the `onDisconnect` event is only fired at the port of the sender, and not at the other ports.

## Cross-extension messaging

In addition to sending messages between different components in your extension, you can use the messaging API to communicate with other extensions. This lets you expose a public API that other extensions can take advantage of.

Listening for incoming requests and connections is similar to the internal case, except you use the [runtime.onMessageExternal](/extensions/runtime#event-onMessageExternal) or [runtime.onConnectExternal](/extensions/runtime#event-onConnectExternal) methods. Here's an example of each:

<pre>// For simple requests:
whale.runtime.onMessageExternal.addListener(
  function(request, sender, sendResponse) {
    if (sender.id == blacklistedExtension)
      return;  // don't allow this extension access
    else if (request.getTargetData)
      sendResponse({targetData: targetData});
    else if (request.activateLasers) {
      var success = activateLasers();
      sendResponse({activateLasers: success});
    }
  });

// For long-lived connections:
whale.runtime.onConnectExternal.addListener(function(port) {
  port.onMessage.addListener(function(msg) {
    // See other examples for sample onMessage handlers.
  });
});
</pre>

Likewise, sending a message to another extension is similar to sending one within your extension. The only difference is that you must pass the ID of the extension you want to communicate with. For example:

<pre>// The ID of the extension we want to talk to.
var laserExtensionId = "abcdefghijklmnoabcdefhijklmnoabc";

// Make a simple request:
whale.runtime.sendMessage(laserExtensionId, {getTargetData: true},
  function(response) {
    if (targetInRange(response.targetData))
      whale.runtime.sendMessage(laserExtensionId, {activateLasers: true});
  });

// Start a long-running conversation:
var port = whale.runtime.connect(laserExtensionId);
port.postMessage(...);
</pre>

## Sending messages from web pages

Similar to [cross-extension messaging](#external), your app or extension can receive and respond to messages from regular web pages. To use this feature, you must first specify in your manifest.json which web sites you want to communicate with. For example:

<pre data-filename="manifest.json">"externally_connectable": {
  "matches": ["*://*.example.com/*"]
}
</pre>

This will expose the messaging API to any page which matches the URL patterns you specify. The URL pattern must contain at least a [second-level domain](http://en.wikipedia.org/wiki/Second-level_domain) - that is, hostname patterns like "*", "*.com", "*.co.uk", and "*.appspot.com" are prohibited. From the web page, use the [runtime.sendMessage](/extensions/runtime#method-sendMessage) or [runtime.connect](/extensions/runtime#method-connect) APIs to send a message to a specific app or extension. For example:

<pre>// The ID of the extension we want to talk to.
var editorExtensionId = "abcdefghijklmnoabcdefhijklmnoabc";

// Make a simple request:
whale.runtime.sendMessage(editorExtensionId, {openUrlInEditor: url},
  function(response) {
    if (!response.success)
      handleError(url);
  });
</pre>

From your app or extension, you may listen to messages from web pages via the [runtime.onMessageExternal](/extensions/runtime#event-onMessageExternal) or [runtime.onConnectExternal](/extensions/runtime#event-onConnectExternal) APIs, similar to [cross-extension messaging](#external). Only the web page can initiate a connection. Here is an example:

<pre>whale.runtime.onMessageExternal.addListener(
  function(request, sender, sendResponse) {
    if (sender.url == blacklistedWebsite)
      return;  // don't allow this web page access
    if (request.openUrlInEditor)
      openUrl(request.openUrlInEditor);
  });
</pre>

<a id="native-messaging-host"></a><a id="native-messaging-client"></a>

## Native messaging

Extensions and apps [can exchange messages](nativeMessaging#native-messaging-client) with native applications that are registered as a [native messaging host](nativeMessaging#native-messaging-host). To learn more about this feature, see [Native messaging](nativeMessaging).

## Security considerations

When receiving a message from a content script or another extension, your background page should be careful not to fall victim to [cross-site scripting](http://en.wikipedia.org/wiki/Cross-site_scripting). Specifically, avoid using dangerous APIs such as the below:

<pre data-filename="background.js">whale.tabs.sendMessage(tab.id, {greeting: "hello"}, function(response) {
  // WARNING! Might be evaluating an evil script!
  var resp = eval("(" + response.farewell + ")");
});
</pre>

<pre data-filename="background.js">whale.tabs.sendMessage(tab.id, {greeting: "hello"}, function(response) {
  // WARNING! Might be injecting a malicious script!
  document.getElementById("resp").innerHTML = response.farewell;
});
</pre>

Instead, prefer safer APIs that do not run scripts:

<pre data-filename="background.js">whale.tabs.sendMessage(tab.id, {greeting: "hello"}, function(response) {
  // JSON.parse does not evaluate the attacker's scripts.
  var resp = JSON.parse(response.farewell);
});
</pre>

<pre data-filename="background.js">whale.tabs.sendMessage(tab.id, {greeting: "hello"}, function(response) {
  // innerText does not let the attacker inject HTML elements.
  document.getElementById("resp").innerText = response.farewell;
});
</pre>

## Examples

You can find simple examples of communication via messages in the [examples/api/messaging](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/docs/examples/api/messaging/) directory. The [native messaging sample](nativeMessaging#examples) demonstrates how a Chrome app can communicate with a native app. For more examples and for help in viewing the source code, see [Samples](samples).

<footer id="cc-info">

Content available under the [CC-By 3.0 license](http://creativecommons.org/licenses/by/3.0/)

</footer>