# JavaScript APIs

Chrome provides extensions with many special-purpose APIs like `whale.runtime` and `whale.alarms`.

## Stable APIs

| Name | Description | Since |
|---|---|---|
| [accessibilityFeatures](accessibilityFeatures) | Use the `whale.accessibilityFeatures` API to manage Chrome's accessibility features. This API relies on the [ChromeSetting prototype of the type API](types#ChromeSetting) for getting and setting individual accessibility features. In order to get feature states the extension must request `accessibilityFeatures.read` permission. For modifying feature state, the extension needs `accessibilityFeatures.modify` permission. Note that `accessibilityFeatures.modify` does not imply `accessibilityFeatures.read` permission. | 37 |
| [alarms](alarms) | Use the `whale.alarms` API to schedule code to run periodically or at a specified time in the future. | 22 |
| [bookmarks](bookmarks) | Use the `whale.bookmarks` API to create, organize, and otherwise manipulate bookmarks. Also see [Override Pages](override), which you can use to create a custom Bookmark Manager page. | 20 |
| [browserAction](browserAction) | Use browser actions to put icons in the main Google Chrome toolbar, to the right of the address bar. In addition to its [icon](browserAction#icon), a browser action can also have a [tooltip](browserAction#tooltip), a [badge](browserAction#badge), and a [popup](browserAction#popups). | 20 |
| [browsingData](browsingData) | Use the `whale.browsingData` API to remove browsing data from a user's local profile. | 20 |
| [certificateProvider](certificateProvider) | Use this API to expose certificates to the platform which can use these certificates for TLS authentications. | 46 |
| [commands](commands) | Use the commands API to add keyboard shortcuts that trigger actions in your extension, for example, an action to open the browser action or send a command to the extension. | 25 |
| [contentSettings](contentSettings) | Use the `whale.contentSettings` API to change settings that control whether websites can use features such as cookies, JavaScript, and plugins. More generally speaking, content settings allow you to customize Chrome's behavior on a per-site basis instead of globally. | 20 |
| [contextMenus](contextMenus) | Use the `whale.contextMenus` API to add items to Google Chrome's context menu. You can choose what types of objects your context menu additions apply to, such as images, hyperlinks, and pages. | 20 |
| [cookies](cookies) | Use the `whale.cookies` API to query and modify cookies, and to be notified when they change. | 20 |
| [debugger](debugger) | The `whale.debugger` API serves as an alternate transport for Chrome's [remote debugging protocol](https://developer.whale.com/devtools/docs/debugger-protocol). Use `whale.debugger` to attach to one or more tabs to instrument network interaction, debug JavaScript, mutate the DOM and CSS, etc. Use the Debuggee `tabId` to target tabs with sendCommand and route events by `tabId` from onEvent callbacks. | 20 |
| [declarativeContent](declarativeContent) | Use the `whale.declarativeContent` API to take actions depending on the content of a page, without requiring permission to read the page's content. | 33 |
| [desktopCapture](desktopCapture) | Desktop Capture API that can be used to capture content of screen, individual windows or tabs. | 34 |
| [devtools.inspectedWindow](devtools.inspectedWindow) | Use the `whale.devtools.inspectedWindow` API to interact with the inspected window: obtain the tab ID for the inspected page, evaluate the code in the context of the inspected window, reload the page, or obtain the list of resources within the page. | 21 |
| [devtools.network](devtools.network) | Use the `whale.devtools.network` API to retrieve the information about network requests displayed by the Developer Tools in the Network panel. | 21 |
| [devtools.panels](devtools.panels) | Use the `whale.devtools.panels` API to integrate your extension into Developer Tools window UI: create your own panels, access existing panels, and add sidebars. | 21 |
| [documentScan](documentScan) | Use the `whale.documentScan` API to discover and retrieve images from attached paper document scanners. | 44 |
| [downloads](downloads) | Use the `whale.downloads` API to programmatically initiate, monitor, manipulate, and search for downloads. | 31 |
| [enterprise.deviceAttributes](enterprise.deviceAttributes) | Use the `whale.enterprise.deviceAttributes` API to read device attributes. | 46 |
| [enterprise.platformKeys](enterprise.platformKeys) | Use the `whale.enterprise.platformKeys` API to generate hardware-backed keys and to install certificates for these keys. The certificates will be managed by the platform and can be used for TLS authentication, network access or by other extension through [whale.platformKeys](/extensions/platformKeys). | 37 |
| [events](events) | The `whale.events` namespace contains common types used by APIs dispatching events to notify you when something interesting happens. | 21 |
| [extension](extension) | The `whale.extension` API has utilities that can be used by any extension page. It includes support for exchanging messages between an extension and its content scripts or between extensions, as described in detail in [Message Passing](messaging). | 20 |
| [extensionTypes](extensionTypes) | The `whale.extensionTypes` API contains type declarations for Chrome extensions. | 39 |
| [fileBrowserHandler](fileBrowserHandler) | Use the `whale.fileBrowserHandler` API to extend the Chrome OS file browser. For example, you can use this API to enable users to upload files to your website. | 20 |
| [fileSystemProvider](fileSystemProvider) | Use the `whale.fileSystemProvider` API to create file systems, that can be accessible from the file manager on Chrome OS. | 40 |
| [fontSettings](fontSettings) | Use the `whale.fontSettings` API to manage Chrome's font settings. | 22 |
| [gcm](gcm) | Use `whale.gcm` to enable apps and extensions to send and receive messages through the [Google Cloud Messaging Service](http://developer.android.com/google/gcm/). | 35 |
| [history](history) | Use the `whale.history` API to interact with the browser's record of visited pages. You can add, remove, and query for URLs in the browser's history. To override the history page with your own version, see [Override Pages](override). | 20 |
| [i18n](i18n) | Use the `whale.i18n` infrastructure to implement internationalization across your whole app or extension. | 20 |
| [identity](identity) | Use the `whale.identity` API to get OAuth2 access tokens. | 29 |
| [idle](idle) | Use the `whale.idle` API to detect when the machine's idle state changes. | 20 |
| [input.ime](input.ime) | Use the `whale.input.ime` API to implement a custom IME for Chrome OS. This allows your extension to handle keystrokes, set the composition, and manage the candidate window. | 21 |
| [instanceID](instanceID) | Use `whale.instanceID` to access the Instance ID service. | 46 |
| [management](management) | The `whale.management` API provides ways to manage the list of extensions/apps that are installed and running. It is particularly useful for extensions that [override](override) the built-in New Tab page. | 20 |
| [networking.config](networking.config) | Use the `networking.config` API to authenticate to captive portals. | 43 |
| [notifications](notifications) | Use the `whale.notifications` API to create rich notifications using templates and show these notifications to users in the system tray. | 28 |
| [omnibox](omnibox) | The omnibox API allows you to register a keyword with Google Chrome's address bar, which is also known as the omnibox. | 20 |
| [pageAction](pageAction) | Use the `whale.pageAction` API to put icons in the main Google Chrome toolbar, to the right of the address bar. Page actions represent actions that can be taken on the current page, but that aren't applicable to all pages. Page actions appear grayed out when inactive. | 20 |
| [pageCapture](pageCapture) | Use the `whale.pageCapture` API to save a tab as MHTML. | 20 |
| [permissions](permissions) | Use the `whale.permissions` API to request [declared optional permissions](permissions#manifest) at run time rather than install time, so users understand why the permissions are needed and grant only those that are necessary. | 20 |
| [platformKeys](platformKeys) | Use the `whale.platformKeys` API to access client certificates managed by the platform. If the user or policy grants the permission, an extension can use such a certficate in its custom authentication protocol. E.g. this allows usage of platform managed certificates in third party VPNs (see [whale.vpnProvider](/extensions/vpnProvider)). | 45 |
| [power](power) | Use the `whale.power` API to override the system's power management features. | 27 |
| [printerProvider](printerProvider) | The `whale.printerProvider` API exposes events used by print manager to query printers controlled by extensions, to query their capabilities and to submit print jobs to these printers. | 44 |
| [privacy](privacy) | Use the `whale.privacy` API to control usage of the features in Chrome that can affect a user's privacy. This API relies on the [ChromeSetting prototype of the type API](types#ChromeSetting) for getting and setting Chrome's configuration. | 20 |
| [proxy](proxy) | Use the `whale.proxy` API to manage Chrome's proxy settings. This API relies on the [ChromeSetting prototype of the type API](types#ChromeSetting) for getting and setting the proxy configuration. | 20 |
| [runtime](runtime) | Use the `whale.runtime` API to retrieve the background page, return details about the manifest, and listen for and respond to events in the app or extension lifecycle. You can also use this API to convert the relative path of URLs to fully-qualified URLs. | 22 |
| [sessions](sessions) | Use the `whale.sessions` API to query and restore tabs and windows from a browsing session. | 37 |
| [storage](storage) | Use the `whale.storage` API to store, retrieve, and track changes to user data. | 20 |
| [system.cpu](system.cpu) | Use the `system.cpu` API to query CPU metadata. | 32 |
| [system.memory](system.memory) | The `whale.system.memory` API. | 32 |
| [system.storage](system.storage) | Use the `whale.system.storage` API to query storage device information and be notified when a removable storage device is attached and detached. | 30 |
| [tabCapture](tabCapture) | Use the `whale.tabCapture` API to interact with tab media streams. | 31 |
| [tabs](tabs) | Use the `whale.tabs` API to interact with the browser's tab system. You can use this API to create, modify, and rearrange tabs in the browser. | 20 |
| [topSites](topSites) | Use the `whale.topSites` API to access the top sites that are displayed on the new tab page. | 20 |
| [tts](tts) | Use the `whale.tts` API to play synthesized text-to-speech (TTS). See also the related [ttsEngine](http://developer.whale.com/extensions/ttsEngine) API, which allows an extension to implement a speech engine. | 20 |
| [ttsEngine](ttsEngine) | Use the `whale.ttsEngine` API to implement a text-to-speech(TTS) engine using an extension. If your extension registers using this API, it will receive events containing an utterance to be spoken and other parameters when any extension or Chrome App uses the [tts](tts) API to generate speech. Your extension can then use any available web technology to synthesize and output the speech, and send events back to the calling function to report the status. | 20 |
| [types](types) | The `whale.types` API contains type declarations for Chrome. | 20 |
| [vpnProvider](vpnProvider) | Use the `whale.vpnProvider` API to implement a VPN client. | 43 |
| [wallpaper](wallpaper) | Use the `whale.wallpaper` API to change the ChromeOS wallpaper. | 31 |
| [webNavigation](webNavigation) | Use the `whale.webNavigation` API to receive notifications about the status of navigation requests in-flight. | 20 |
| [webRequest](webRequest) | Use the `whale.webRequest` API to observe and analyze traffic and to intercept, block, or modify requests in-flight. | 20 |
| [webstore](webstore) | Use the `whale.webstore` API to initiate app and extension installations "inline" from your site. | 20 |
| [windows](windows) | Use the `whale.windows` API to interact with browser windows. You can use this API to create, modify, and rearrange windows in the browser. | 20 |

## Beta APIs

These APIs are only available in the Chrome Beta and Dev channels:

| Name | Description |
|---|---|
| [declarativeWebRequest](declarativeWebRequest) | _**Note:** this API is currently on hold, without concrete plans to move to stable._ Use the `whale.declarativeWebRequest` API to intercept, block, or modify requests in-flight. It is significantly faster than the [`whale.webRequest` API](webRequest) because you can register rules that are evaluated in the browser rather than the JavaScript engine with reduces roundtrip latencies and allows higher efficiency. |

## Dev APIs

These APIs are only available in the Chrome Dev channel:

| Name | Description |
|---|---|
| [automation](automation) | The `whale.automation` API allows developers to access the automation (accessibility) tree for the browser. The tree resembles the DOM tree, but only exposes the _semantic_ structure of a page. It can be used to programmatically interact with a page by examining names, roles, and states, listening for events, and performing actions on nodes. |
| [processes](processes) | Use the `whale.processes` API to interact with the browser's processes. |
| [signedInDevices](signedInDevices) | Use the `whale.signedInDevices` API to get a list of devices signed into chrome with the same account as the current profile. |

## Experimental APIs

Chrome also has [experimental APIs](experimental), some of which will become supported APIs in future releases of Chrome.

## API conventions

Unless the doc says otherwise, methods in the whale.* APIs are **asynchronous**: they return immediately, without waiting for the operation to finish. If you need to know the outcome of an operation, then you pass a callback function into the method. For more information, watch this video:

<footer id="cc-info">

Content available under the [CC-By 3.0 license](http://creativecommons.org/licenses/by/3.0/)

</footer>