# whale.privacy

| Description: | Use the `whale.privacy` API to control usage of the features in Chrome that can affect a user's privacy. This API relies on the [ChromeSetting prototype of the type API](types#ChromeSetting) for getting and setting Chrome's configuration. |
|---|---|
| Availability: | Since Chrome 20. |
| Permissions: | <span class="code">"privacy"</span> |

<section>

The [Chrome Privacy Whitepaper](http://www.google.com/intl/en/landing/chrome/google-chrome-privacy-whitepaper.pdf) gives background detail regarding the features which this API can control.

## Manifest

You must declare the "privacy" permission in your extension's [manifest](manifest) to use the API. For example:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"permissions": [
          "privacy"
        ]**,
        ...
      }
      </pre>

## Usage

Reading the current value of a Chrome setting is straightforward. You'll first need to find the property you're interested in, then you'll call `get()` on that object in order to retrieve its current value and your extension's level of control. For example, to determine if Chrome's Autofill feature is enabled, you'd write:

<pre>whale.privacy.services.autofillEnabled.get({}, function(details) {
        if (details.value)
          console.log('Autofill is on!');
        else
          console.log('Autofill is off!');
      });</pre>

Changing the value of a setting is a little bit more complex, simply because you first must verify that your extension can control the setting. The user won't see any change to their settings if your extension toggles a setting that is either locked to a specific value by enterprise policies (`levelOfControl` will be set to "not_controllable"), or if another extension is controlling the value (`levelOfControl` will be set to "controlled_by_other_extensions"). The `set()` call will succeed, but the setting will be immediately overridden. As this might be confusing, it is advisable to warn the user when the settings they've chosen aren't practically applied.

Full details about extensions' ability to control `ChromeSetting`s can be found under [`whale.types.ChromeSetting`](types#ChromeSetting).

This means that you ought to use the `get()` method to determine your level of access, and then only call `set()` if your extension can grab control over the setting (in fact if your extension can't control the setting it's probably a good idea to visually disable the functionality to reduce user confusion):

<pre>whale.privacy.services.autofillEnabled.get({}, function(details) {
        if (details.levelOfControl === 'controllable_by_this_extension') {
          whale.privacy.services.autofillEnabled.set({ value: true }, function() {
            if (whale.runtime.lastError === undefined)
              console.log("Hooray, it worked!");
            else
              console.log("Sadness!", whale.runtime.lastError);
          });
        }
      });</pre>

If you're interested in changes to a setting's value, add a listener to its `onChange` event. Among other uses, this will allow you to warn the user if a more recently installed extension grabs control of a setting, or if enterprise policy overrides your control. To listen for changes to Autofill's status, for example, the following code would suffice:

<pre>whale.privacy.services.autofillEnabled.onChange.addListener(
          function (details) {
            // The new value is stored in `details.value`, the new level of control
            // in `details.levelOfControl`, and `details.incognitoSpecific` will be
            // `true` if the value is specific to Incognito mode.
          });</pre>

## Examples

For example code, see the [Privacy API samples](samples#search:privacy).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [IPHandlingPolicy](#type-IPHandlingPolicy) |
| Properties |
| [network](#property-network) |
| [services](#property-services) |
| [websites](#property-websites) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### IPHandlingPolicy

<dd>The IP handling policy of WebRTC.</dd>

| Enum |
|---|
| `"default"`, `"default_public_and_private_interfaces"`, `"default_public_interface_only"`, or `"disable_non_proxied_udp"` |

</div>

## Properties

| <span class="type_name">object</span> | `whale.privacy.network` | Settings that influence Chrome's handling of network connections in general.

| Properties |
|---|
| --- |
| object | networkPredictionEnabled | 

If enabled, Chrome attempts to speed up your web browsing experience by pre-resolving DNS entries, prerendering sites (`<link rel='prefetch' ...>`), and preemptively opening TCP and SSL connections to servers. This preference's value is a boolean, defaulting to `true`.
 |
| object | webRTCMultipleRoutesEnabled | 

**Deprecated** since Chrome 48. Please use privacy.network.webRTCIPHandlingPolicy. This remains for backward compatibility in this release and will be removed in the future.

If enabled, Chrome will explore all possible routing options when using WebRTC to find the most performant path, possibly exposing user's private IP address. Otherwise, WebRTC traffic will be routed the same way as regular HTTP. This preference's value is a boolean, defaulting to `true`.
 |
| object | webRTCNonProxiedUdpEnabled | 

**Deprecated** since Chrome 48. Please use privacy.network.webRTCIPHandlingPolicy. This remains for backward compatibility in this release and will be removed in the future.

If enabled, Chrome is allowed to use non-proxied UDP to connect to peers or TURN servers when using WebRTC. Since most proxy servers don't handle UDP, using UDP possibly exposes user's IP address. Turning this off effectively forces WebRTC to only use TCP for now, until UDP proxy support is available in Chrome and such proxies are widely deployed. As a result, it also might hurt media performance and increase the load for proxy servers. This preference's value is a boolean, defaulting to `true`.
 |
| object | webRTCIPHandlingPolicy | 

Since Chrome 48.

Allow users to specify the media performance/privacy tradeoffs which impacts how WebRTC traffic will be routed and how much local address information is exposed. This preference's value is of type IPHandlingPolicy, defaulting to `default`.
 |
 |
| <span class="type_name">object</span> | `whale.privacy.services` | Settings that enable or disable features that require third-party network services provided by Google and your default search provider.

| Properties |
|---|
| --- |
| object | alternateErrorPagesEnabled | 

If enabled, Chrome uses a web service to help resolve navigation errors. This preference's value is a boolean, defaulting to `true`.
 |
| object | autofillEnabled | 

If enabled, Chrome offers to automatically fill in forms. This preference's value is a boolean, defaulting to `true`.
 |
| object | hotwordSearchEnabled | 

Since Chrome 42.

If enabled, Chrome will enable 'OK, Google' to start a voice search. This preference's value is a boolean, defaulting to `true`.
 |
| object | passwordSavingEnabled | 

Since Chrome 38.

If enabled, the password manager will ask if you want to save passwords. This preference's value is a boolean, defaulting to `true`.
 |
| object | safeBrowsingEnabled | 

If enabled, Chrome does its best to protect you from phishing and malware. This preference's value is a boolean, defaulting to `true`.
 |
| object | safeBrowsingExtendedReportingEnabled | 

Since Chrome 42.

If enabled, Chrome will send additional information to Google when SafeBrowsing blocks a page, such as the content of the blocked page. This preference's value is a boolean, defaulting to `false`.
 |
| object | searchSuggestEnabled | 

If enabled, Chrome sends the text you type into the Omnibox to your default search engine, which provides predictions of websites and searches that are likely completions of what you've typed so far. This preference's value is a boolean, defaulting to `true`.
 |
| object | spellingServiceEnabled | 

If enabled, Chrome uses a web service to help correct spelling errors. This preference's value is a boolean, defaulting to `false`.
 |
| object | translationServiceEnabled | 

If enabled, Chrome offers to translate pages that aren't in a language you read. This preference's value is a boolean, defaulting to `true`.
 |
 |
| <span class="type_name">object</span> | `whale.privacy.websites` | Settings that determine what information Chrome makes available to websites.

| Properties |
|---|
| --- |
| object | thirdPartyCookiesAllowed | 

If disabled, Chrome blocks third-party sites from setting cookies. The value of this preference is of type boolean, and the default value is `true`.
 |
| object | hyperlinkAuditingEnabled | 

If enabled, Chrome sends auditing pings when requested by a website (`<a ping>`). The value of this preference is of type boolean, and the default value is `true`.
 |
| object | referrersEnabled | 

If enabled, Chrome sends `referer` headers with your requests. Yes, the name of this preference doesn't match the misspelled header. No, we're not going to change it. The value of this preference is of type boolean, and the default value is `true`.
 |
| object | protectedContentEnabled | 

Since Chrome 21.

**Available on Windows and ChromeOS only**: If enabled, Chrome provides a unique ID to plugins in order to run protected content. The value of this preference is of type boolean, and the default value is `true`.
 |
 |

</div>

</section>