# whale.types

| Description: | The `whale.types` API contains type declarations for Chrome. |
|---|---|
| Availability: | Since Chrome 20. |

<section>

## Chrome settings

The `ChromeSetting` prototype provides a common set of functions (`get()`, `set()`, and `clear()`) as well as an event publisher (`onChange`) for settings of the Chrome browser. The [proxy settings examples](proxy#overview-examples) demonstrate how these functions are intended to be used.

### Scope and life cycle

Chrome distinguishes between three different scopes of browser settings:

<dl>

<dt>`regular`</dt>

<dd>Settings set in the `regular` scope apply to regular browser windows and are inherited by incognito windows if they are not overwritten. These settings are stored to disk and remain in place until they are cleared by the governing extension, or the governing extension is disabled or uninstalled.</dd>

<dt>`incognito_persistent`</dt>

<dd>Settings set in the `incognito_persistent` scope apply only to incognito windows. For these, they override `regular` settings. These settings are stored to disk and remain in place until they are cleared by the governing extension, or the governing extension is disabled or uninstalled.</dd>

<dt>`incognito_session_only`</dt>

<dd>Settings set in the `incognito_session_only` scope apply only to incognito windows. For these, they override `regular` and `incognito_persistent` settings. These settings are not stored to disk and are cleared when the last incognito window is closed. They can only be set when at least one incognito window is open.</dd>

</dl>

### Precedence

Chrome manages settings on different layers. The following list describes the layers that may influence the effective settings, in increasing order of precedence.

1.  System settings provided by the operating system
2.  Command-line parameters
3.  Settings provided by extensions
4.  Policies

As the list implies, policies might overrule any changes that you specify with your extension. You can use the `get()` function to determine whether your extension is capable of providing a setting or whether this setting would be overridden.

As discussed above, Chrome allows using different settings for regular windows and incognito windows. The following example illustrates the behavior. Assume that no policy overrides the settings and that an extension can set settings for regular windows **(R)** and settings for incognito windows **(I)**.

*   If only **(R)** is set, these settings are effective for both regular and incognito windows.
*   If only **(I)** is set, these settings are effective for only incognito windows. Regular windows use the settings determined by the lower layers (command-line options and system settings).
*   If both **(R)** and **(I)** are set, the respective settings are used for regular and incognito windows.

If two or more extensions want to set the same setting to different values, the extension installed most recently takes precedence over the other extensions. If the most recently installed extension sets only **(I)**, the settings of regular windows can be defined by previously installed extensions.

The _effective_ value of a setting is the one that results from considering the precedence rules. It is used by Chrome.

</section>

<section id="toc">

## Summary

| Types |
|---|
| [ChromeSettingScope](#type-ChromeSettingScope) |
| [LevelOfControl](#type-LevelOfControl) |
| [ChromeSetting](#type-ChromeSetting) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### ChromeSettingScope

<dd>The scope of the ChromeSetting. One of

*   <var>regular</var>: setting for the regular profile (which is inherited by the incognito profile if not overridden elsewhere),
*   <var>regular_only</var>: setting for the regular profile only (not inherited by the incognito profile),
*   <var>incognito_persistent</var>: setting for the incognito profile that survives browser restarts (overrides regular preferences),
*   <var>incognito_session_only</var>: setting for the incognito profile that can only be set during an incognito session and is deleted when the incognito session ends (overrides regular and incognito_persistent preferences).

</dd>

| Enum |
|---|
| `"regular"`, `"regular_only"`, `"incognito_persistent"`, or `"incognito_session_only"` |

</div>

<div>

### LevelOfControl

<dd>One of

*   <var>not_controllable</var>: cannot be controlled by any extension
*   <var>controlled_by_other_extensions</var>: controlled by extensions with higher precedence
*   <var>controllable_by_this_extension</var>: can be controlled by this extension
*   <var>controlled_by_this_extension</var>: controlled by this extension

</dd>

| Enum |
|---|
| `"not_controllable"`, `"controlled_by_other_extensions"`, `"controllable_by_this_extension"`, or `"controlled_by_this_extension"` |

</div>

<div>

### ChromeSetting

<dd>An interface that allows access to a Chrome browser setting. See [accessibilityFeatures](/extensions/accessibilityFeatures) for an example.</dd>

<div>

### onChange

<div class="description">

Fired after the setting changes.

</div>

</div>

| methods |
|---|
| 

<div>

#### get

<div class="summary">`ChromeSetting.get(<span>object details</span>, <span>function callback</span>)`</div>

<div class="description">

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

#### set

<div class="summary">`ChromeSetting.set(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

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

#### clear

<div class="summary">`ChromeSetting.clear(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

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
| events |
| 

<div>

#### addListener

<div class="summary">`onChange.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| any | value | 
|---|---|

The value of the setting after the change.
 |
| enum of `"not_controllable"`, `"controlled_by_other_extensions"`, `"controllable_by_this_extension"`, or `"controlled_by_this_extension"` | levelOfControl | 

The level of control of the setting.
 |
| boolean | <span class="optional">(optional)</span> incognitoSpecific | 

Whether the value that has changed is specific to the incognito session.
This property will _only_ be present if the user has enabled the extension in incognito mode.
 |
 |
 |

</div>

</div>
 |

</div>

</div>

</section>