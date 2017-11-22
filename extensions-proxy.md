# whale.proxy

| Description: | Use the `whale.proxy` API to manage Chrome's proxy settings. This API relies on the [ChromeSetting prototype of the type API](types#ChromeSetting) for getting and setting the proxy configuration. |
|---|---|
| Availability: | Since Chrome 20. |
| Permissions: | <span class="code">"proxy"</span> |

<section>

## Manifest

You must declare the "proxy" permission in the [extension manifest](manifest) to use the proxy settings API. For example:

<pre data-filename="manifest.json">      {
        "name": "My extension",
        ...
        **"permissions": [
          "proxy"
        ]**,
        ...
      }
      </pre>

## Objects and properties

Proxy settings are defined in a [proxy.ProxyConfig](/extensions/proxy#type-ProxyConfig) object. Depending on Chrome's proxy settings, the settings may contain [proxy.ProxyRules](/extensions/proxy#type-ProxyRules) or a [proxy.PacScript](/extensions/proxy#type-PacScript).

### Proxy modes

A ProxyConfig object's `mode` attribute determines the overall behavior of Chrome with regards to proxy usage. It can take the following values:

<dl>

<dt>`direct`</dt>

<dd>In `direct` mode all connections are created directly, without any proxy involved. This mode allows no further parameters in the `ProxyConfig` object.</dd>

<dt>`auto_detect`</dt>

<dd>In `auto_detect` mode the proxy configuration is determined by a PAC script that can be downloaded at [http://wpad/wpad.dat](http://wpad/wpad.dat). This mode allows no further parameters in the `ProxyConfig` object.</dd>

<dt>`pac_script`</dt>

<dd>In `pac_script` mode the proxy configuration is determined by a PAC script that is either retrieved from the URL specified in the [proxy.PacScript](/extensions/proxy#type-PacScript) object or taken literally from the `data` element specified in the [proxy.PacScript](/extensions/proxy#type-PacScript) object. Besides this, this mode allows no further parameters in the `ProxyConfig` object.</dd>

<dt>`fixed_servers`</dt>

<dd>In `fixed_servers` mode the proxy configuration is codified in a [proxy.ProxyRules](/extensions/proxy#type-ProxyRules) object. Its structure is described in [Proxy rules](#proxy_rules). Besides this, the `fixed_servers` mode allows no further parameters in the `ProxyConfig` object.</dd>

<dt>`system`</dt>

<dd>In `system` mode the proxy configuration is taken from the operating system. This mode allows no further parameters in the `ProxyConfig` object. Note that the `system` mode is different from setting no proxy configuration. In the latter case, Chrome falls back to the system settings only if no command-line options influence the proxy configuration.</dd>

</dl>

### Proxy rules

The [proxy.ProxyRules](/extensions/proxy#type-ProxyRules) object can contain either a `singleProxy` attribute or a subset of `proxyForHttp`, `proxyForHttps`, `proxyForFtp`, and `fallbackProxy`.

In the first case, HTTP, HTTPS and FTP traffic is proxied through the specified proxy server. Other traffic is sent directly. In the latter case the behavior is slightly more subtle: If a proxy server is configured for the HTTP, HTTPS or FTP protocol, the respective traffic is proxied through the specified server. If no such proxy server is specified or traffic uses a different protocol than HTTP, HTTPS or FTP, the `fallbackProxy` is used. If no `fallbackProxy` is specified, traffic is sent directly without a proxy server.

### Proxy server objects

A proxy server is configured in a [proxy.ProxyServer](/extensions/proxy#type-ProxyServer) object. The connection to the proxy server (defined by the `host` attribute) uses the protocol defined in the `scheme` attribute. If no `scheme` is specified, the proxy connection defaults to `http`.

If no `port` is defined in a [proxy.ProxyServer](/extensions/proxy#type-ProxyServer) object, the port is derived from the scheme. The default ports are:

| Scheme | Port |
|---|---|
| http | 80 |
| https | 443 |
| socks4 | 1080 |
| socks5 | 1080 |

### Bypass list

Individual servers may be excluded from being proxied with the `bypassList`. This list may contain the following entries:

<dl>

<dt>`[_<scheme>_://]_<host-pattern>_[:_<port>_]`</dt>

<dd>Match all hostnames that match the pattern _<host-pattern>_. A leading `"."` is interpreted as a `"*."`.
Examples: `"foobar.com", "*foobar.com", "*.foobar.com", "*foobar.com:99", "https://x.*.y.com:99"`.

| Pattern | Matches | Does not match |
|---|---|---|
| `".foobar.com"` | `"www.foobar.com"` | `"foobar.com"` |
| `"*.foobar.com"` | `"www.foobar.com"` | `"foobar.com"` |
| `"foobar.com"` | `"foobar.com"` | `"www.foobar.com"` |
| `"*foobar.com"` | `"foobar.com"`, `"www.foobar.com"`, `"foofoobar.com"` |  |

</dd>

<dt>`[_<scheme>_://]_<ip-literal>_[:_<port>_]`</dt>

<dd>Match URLs that are IP address literals.
Conceptually this is the similar to the first case, but with special cases to handle IP literal canonicalization. For example, matching on "[0:0:0::1]" is the same as matching on "[::1]" because the IPv6 canonicalization is done internally.
Examples: `"127.0.1", "[0:0::1]", "[::1]", "http://[::1]:99"`</dd>

<dt>`_<ip-literal>_/_<prefix-length-in-bits>_`</dt>

<dd>Match any URL containing an IP literal within the given range. The IP range is specified using CIDR notation.
Examples: `"192.168.1.1/16", "fefe:13::abc/33"`</dd>

<dt>`<local>`</dt>

<dd>Match local addresses. An address is local if the host is "127.0.0.1", "::1", or "localhost".
Example: `"<local>"`</dd>

</dl>

## Examples

The following code sets a SOCKS 5 proxy for HTTP connections to all servers but foobar.com and uses direct connections for all other protocols. The settings apply to regular and incognito windows, as incognito windows inherit settings from regular windows. Please also consult the [Types API](types#ChromeSetting) documentation.

<pre>      var config = {
        mode: "fixed_servers",
        rules: {
          proxyForHttp: {
            scheme: "socks5",
            host: "1.2.3.4"
          },
          bypassList: ["foobar.com"]
        }
      };
      whale.proxy.settings.set(
          {value: config, scope: 'regular'},
          function() {});
      </pre>

The following code sets a custom PAC script.

<pre>      var config = {
        mode: "pac_script",
        pacScript: {
          data: "function FindProxyForURL(url, host) {\n" +
                "  if (host == 'foobar.com')\n" +
                "    return 'PROXY blackhole:80';\n" +
                "  return 'DIRECT';\n" +
                "}"
        }
      };
      whale.proxy.settings.set(
          {value: config, scope: 'regular'},
          function() {});
      </pre>

The next snippet queries the currently effective proxy settings. The effective proxy settings can be determined by another extension or by a policy. See the [Types API](types#ChromeSetting) documentation for details.

<pre>      whale.proxy.settings.get(
          {'incognito': false},
          function(config) {console.log(JSON.stringify(config));});
      </pre>

Note that the `value` object passed to `set()` is not identical to the `value` object passed to callback function of `get()`. The latter will contain a `rules.proxyForHttp.port` element.

</section>

<section id="toc">

## Summary

| Types |
|---|
| [Scheme](#type-Scheme) |
| [Mode](#type-Mode) |
| [ProxyServer](#type-ProxyServer) |
| [ProxyRules](#type-ProxyRules) |
| [PacScript](#type-PacScript) |
| [ProxyConfig](#type-ProxyConfig) |
| Properties |
| [settings](#property-settings) |
| Events |
| [onProxyError](#event-onProxyError) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### Scheme

| Enum |
|---|
| `"http"`, `"https"`, `"quic"`, `"socks4"`, or `"socks5"` |

</div>

<div>

### Mode

| Enum |
|---|
| `"direct"`, `"auto_detect"`, `"pac_script"`, `"fixed_servers"`, or `"system"` |

</div>

<div>

### ProxyServer

<dd>An object encapsulating a single proxy server's specification.</dd>

| properties |
|---|
| [Scheme](/extensions/proxy#type-Scheme) | <span class="optional">(optional)</span> scheme | 

The scheme (protocol) of the proxy server itself. Defaults to 'http'.
 |
| string | host | 

The URI of the proxy server. This must be an ASCII hostname (in Punycode format). IDNA is not supported, yet.
 |
| integer | <span class="optional">(optional)</span> port | 

The port of the proxy server. Defaults to a port that depends on the scheme.
 |

</div>

<div>

### ProxyRules

<dd>An object encapsulating the set of proxy rules for all protocols. Use either 'singleProxy' or (a subset of) 'proxyForHttp', 'proxyForHttps', 'proxyForFtp' and 'fallbackProxy'.</dd>

| properties |
|---|
| [ProxyServer](/extensions/proxy#type-ProxyServer) | <span class="optional">(optional)</span> singleProxy | 

The proxy server to be used for all per-URL requests (that is http, https, and ftp).
 |
| [ProxyServer](/extensions/proxy#type-ProxyServer) | <span class="optional">(optional)</span> proxyForHttp | 

The proxy server to be used for HTTP requests.
 |
| [ProxyServer](/extensions/proxy#type-ProxyServer) | <span class="optional">(optional)</span> proxyForHttps | 

The proxy server to be used for HTTPS requests.
 |
| [ProxyServer](/extensions/proxy#type-ProxyServer) | <span class="optional">(optional)</span> proxyForFtp | 

The proxy server to be used for FTP requests.
 |
| [ProxyServer](/extensions/proxy#type-ProxyServer) | <span class="optional">(optional)</span> fallbackProxy | 

The proxy server to be used for everthing else or if any of the specific proxyFor... is not specified.
 |
| array of string | <span class="optional">(optional)</span> bypassList | 

List of servers to connect to without a proxy server.
 |

</div>

<div>

### PacScript

<dd>An object holding proxy auto-config information. Exactly one of the fields should be non-empty.</dd>

| properties |
|---|
| string | <span class="optional">(optional)</span> url | 

URL of the PAC file to be used.
 |
| string | <span class="optional">(optional)</span> data | 

A PAC script.
 |
| boolean | <span class="optional">(optional)</span> mandatory | 

If true, an invalid PAC script will prevent the network stack from falling back to direct connections. Defaults to false.
 |

</div>

<div>

### ProxyConfig

<dd>An object encapsulating a complete proxy configuration.</dd>

| properties |
|---|
| [ProxyRules](/extensions/proxy#type-ProxyRules) | <span class="optional">(optional)</span> rules | 

The proxy rules describing this configuration. Use this for 'fixed_servers' mode.
 |
| [PacScript](/extensions/proxy#type-PacScript) | <span class="optional">(optional)</span> pacScript | 

The proxy auto-config (PAC) script for this configuration. Use this for 'pac_script' mode.
 |
| [Mode](/extensions/proxy#type-Mode) | mode | 

'direct' = Never use a proxy
'auto_detect' = Auto detect proxy settings
'pac_script' = Use specified PAC script
'fixed_servers' = Manually specify proxy servers
'system' = Use system proxy settings
 |

</div>

## Properties

| <span class="type_name">object</span> | `whale.proxy.settings` | Proxy settings to be used. The value of this setting is a ProxyConfig object.

| Functions |
|---|
| --- |
| 

<div>

### get

<div class="summary">`settings.get(<span>object details</span>, <span>function callback</span>)`</div>

<div class="description">

Since Chrome 21.

Gets the value of a setting.

| Parameters |
|---|
| object | details | 

Which setting to consider.

| boolean | <span class="optional">(optional)</span> incognito | 
|---|---|

Whether to return the value that applies to the incognito session (default false).
 |
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

Details of the currently effective value.

| any | value | 
|---|---|

The value of the setting.
 |
| enum of `"not_controllable"`, `"controlled_by_other_extensions"`, `"controllable_by_this_extension"`, or `"controlled_by_this_extension"` | levelOfControl | 

The level of control of the setting.
 |
| boolean | <span class="optional">(optional)</span> incognitoSpecific | 

Whether the effective value is specific to the incognito session.
This property will _only_ be present if the <var>incognito</var> property in the <var>details</var> parameter of `get()` was true.
 |
 |
 |

</div>

</div>
 |
| 

<div>

### set

<div class="summary">`settings.set(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 21.

Sets the value of a setting.

| Parameters |
|---|
| object | details | 

Which setting to change.

| any | value | 
|---|---|

The value of the setting.
Note that every setting has a specific value type, which is described together with the setting. An extension should _not_ set a value of a different type.
 |
| enum of `"regular"`, `"regular_only"`, `"incognito_persistent"`, or `"incognito_session_only"` | <span class="optional">(optional)</span> scope | 

Where to set the setting (default: regular).
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called at the completion of the set operation.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |
| 

<div>

### clear

<div class="summary">`settings.clear(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Since Chrome 21.

Clears the setting, restoring any default value.

| Parameters |
|---|
| object | details | 

Which setting to clear.

| enum of `"regular"`, `"regular_only"`, `"incognito_persistent"`, or `"incognito_session_only"` | <span class="optional">(optional)</span> scope | 
|---|---|

Where to clear the setting (default: regular).
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called at the completion of the clear operation.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |
 |

## Events

<div>

### onProxyError

<div class="description">

Notifies about proxy errors.

<div>

#### addListener

<div class="summary">`whale.proxy.onProxyError.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| boolean | fatal | 
|---|---|

If true, the error was fatal and the network transaction was aborted. Otherwise, a direct connection is used instead.
 |
| string | error | 

The error description.
 |
| string | details | 

Additional details about the error such as a JavaScript runtime error.
 |
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>