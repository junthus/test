# whale.fontSettings

| Description: | Use the `whale.fontSettings` API to manage Chrome's font settings. |
|---|---|
| Availability: | Since Chrome 22. |
| Permissions: | <span class="code">"fontSettings"</span> |

<section>

## Manifest

To use the Font Settings API, you must declare the "fontSettings" permission in the [extension manifest](manifest). For example:

<pre data-filename="manifest.json">      {
        "name": "My Font Settings Extension",
        "description": "Customize your fonts",
        "version": "0.2",
        **"permissions": [
          "fontSettings"
        ]**,
        ...
      }
      </pre>

## Generic Font Families and Scripts

Chrome allows for some font settings to depend on certain generic font families and language scripts. For example, the font used for sans-serif Simplified Chinese may be different than the font used for serif Japanese.

The generic font families supported by Chrome are based on [CSS generic font families](http://www.w3.org/TR/CSS21/fonts.html#generic-font-families) and are listed in the API reference below. When a webpage specifies a generic font family, Chrome selects the font based on the corresponding setting. If no generic font family is specified, Chrome uses the setting for the "standard" generic font family.

When a webpage specifies a language, Chrome selects the font based on the setting for the corresponding language script. If no language is specified, Chrome uses the setting for the default, or global, script.

The supported language scripts are specified by ISO 15924 script code and listed in the API reference below. Technically, Chrome settings are not strictly per-script but also depend on language. For example, Chrome chooses the font for Cyrillic (ISO 15924 script code "Cyrl") when a webpage specifies the Russian language, and uses this font not just for Cyrillic script but for everything the font covers, such as Latin.

## Examples

The following code gets the standard font for Arabic.

<pre>      whale.fontSettings.getFont(
        { genericFamily: 'standard', script: 'Arab' },
        function(details) { console.log(details.fontId); }
      );
      </pre>

The next snippet sets the sans-serif font for Japanese.

<pre>      whale.fontSettings.setFont(
        { genericFamily: 'sansserif', script: 'Jpan', fontId: 'MS PGothic' }
      );
      </pre>

You can find a sample extension using the Font Settings API in the [examples/api/fontSettings](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/docs/examples/api/fontSettings/) directory. For other examples and for help in viewing the source code, see [Samples](samples).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [FontName](#type-FontName) |
| [ScriptCode](#type-ScriptCode) |
| [GenericFamily](#type-GenericFamily) |
| [LevelOfControl](#type-LevelOfControl) |
| Methods |
| [clearFont](#method-clearFont) − `whale.fontSettings.clearFont(<span>object details</span>, <span class="optional">function callback</span>)` |
| [getFont](#method-getFont) − `whale.fontSettings.getFont(<span>object details</span>, <span class="optional">function callback</span>)` |
| [setFont](#method-setFont) − `whale.fontSettings.setFont(<span>object details</span>, <span class="optional">function callback</span>)` |
| [getFontList](#method-getFontList) − `whale.fontSettings.getFontList(<span>function callback</span>)` |
| [clearDefaultFontSize](#method-clearDefaultFontSize) − `whale.fontSettings.clearDefaultFontSize(<span class="optional">object details</span>, <span class="optional">function callback</span>)` |
| [getDefaultFontSize](#method-getDefaultFontSize) − `whale.fontSettings.getDefaultFontSize(<span class="optional">object details</span>, <span class="optional">function callback</span>)` |
| [setDefaultFontSize](#method-setDefaultFontSize) − `whale.fontSettings.setDefaultFontSize(<span>object details</span>, <span class="optional">function callback</span>)` |
| [clearDefaultFixedFontSize](#method-clearDefaultFixedFontSize) − `whale.fontSettings.clearDefaultFixedFontSize(<span class="optional">object details</span>, <span class="optional">function callback</span>)` |
| [getDefaultFixedFontSize](#method-getDefaultFixedFontSize) − `whale.fontSettings.getDefaultFixedFontSize(<span class="optional">object details</span>, <span class="optional">function callback</span>)` |
| [setDefaultFixedFontSize](#method-setDefaultFixedFontSize) − `whale.fontSettings.setDefaultFixedFontSize(<span>object details</span>, <span class="optional">function callback</span>)` |
| [clearMinimumFontSize](#method-clearMinimumFontSize) − `whale.fontSettings.clearMinimumFontSize(<span class="optional">object details</span>, <span class="optional">function callback</span>)` |
| [getMinimumFontSize](#method-getMinimumFontSize) − `whale.fontSettings.getMinimumFontSize(<span class="optional">object details</span>, <span class="optional">function callback</span>)` |
| [setMinimumFontSize](#method-setMinimumFontSize) − `whale.fontSettings.setMinimumFontSize(<span>object details</span>, <span class="optional">function callback</span>)` |
| Events |
| [onFontChanged](#event-onFontChanged) |
| [onDefaultFontSizeChanged](#event-onDefaultFontSizeChanged) |
| [onDefaultFixedFontSizeChanged](#event-onDefaultFixedFontSizeChanged) |
| [onMinimumFontSizeChanged](#event-onMinimumFontSizeChanged) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### FontName

<dd>Represents a font name.</dd>

| properties |
|---|
| string | fontId | 

The font ID.
 |
| string | displayName | 

The display name of the font.
 |

</div>

<div>

### ScriptCode

<dd>An ISO 15924 script code. The default, or global, script is represented by script code "Zyyy".</dd>

| Enum |
|---|
| `"Afak"`, `"Arab"`, `"Armi"`, `"Armn"`, `"Avst"`, `"Bali"`, `"Bamu"`, `"Bass"`, `"Batk"`, `"Beng"`, `"Blis"`, `"Bopo"`, `"Brah"`, `"Brai"`, `"Bugi"`, `"Buhd"`, `"Cakm"`, `"Cans"`, `"Cari"`, `"Cham"`, `"Cher"`, `"Cirt"`, `"Copt"`, `"Cprt"`, `"Cyrl"`, `"Cyrs"`, `"Deva"`, `"Dsrt"`, `"Dupl"`, `"Egyd"`, `"Egyh"`, `"Egyp"`, `"Elba"`, `"Ethi"`, `"Geor"`, `"Geok"`, `"Glag"`, `"Goth"`, `"Gran"`, `"Grek"`, `"Gujr"`, `"Guru"`, `"Hang"`, `"Hani"`, `"Hano"`, `"Hans"`, `"Hant"`, `"Hebr"`, `"Hluw"`, `"Hmng"`, `"Hung"`, `"Inds"`, `"Ital"`, `"Java"`, `"Jpan"`, `"Jurc"`, `"Kali"`, `"Khar"`, `"Khmr"`, `"Khoj"`, `"Knda"`, `"Kpel"`, `"Kthi"`, `"Lana"`, `"Laoo"`, `"Latf"`, `"Latg"`, `"Latn"`, `"Lepc"`, `"Limb"`, `"Lina"`, `"Linb"`, `"Lisu"`, `"Loma"`, `"Lyci"`, `"Lydi"`, `"Mand"`, `"Mani"`, `"Maya"`, `"Mend"`, `"Merc"`, `"Mero"`, `"Mlym"`, `"Moon"`, `"Mong"`, `"Mroo"`, `"Mtei"`, `"Mymr"`, `"Narb"`, `"Nbat"`, `"Nkgb"`, `"Nkoo"`, `"Nshu"`, `"Ogam"`, `"Olck"`, `"Orkh"`, `"Orya"`, `"Osma"`, `"Palm"`, `"Perm"`, `"Phag"`, `"Phli"`, `"Phlp"`, `"Phlv"`, `"Phnx"`, `"Plrd"`, `"Prti"`, `"Rjng"`, `"Roro"`, `"Runr"`, `"Samr"`, `"Sara"`, `"Sarb"`, `"Saur"`, `"Sgnw"`, `"Shaw"`, `"Shrd"`, `"Sind"`, `"Sinh"`, `"Sora"`, `"Sund"`, `"Sylo"`, `"Syrc"`, `"Syre"`, `"Syrj"`, `"Syrn"`, `"Tagb"`, `"Takr"`, `"Tale"`, `"Talu"`, `"Taml"`, `"Tang"`, `"Tavt"`, `"Telu"`, `"Teng"`, `"Tfng"`, `"Tglg"`, `"Thaa"`, `"Thai"`, `"Tibt"`, `"Tirh"`, `"Ugar"`, `"Vaii"`, `"Visp"`, `"Wara"`, `"Wole"`, `"Xpeo"`, `"Xsux"`, `"Yiii"`, `"Zmth"`, `"Zsym"`, or `"Zyyy"` |

</div>

<div>

### GenericFamily

<dd>A CSS generic font family.</dd>

| Enum |
|---|
| `"standard"`, `"sansserif"`, `"serif"`, `"fixed"`, `"cursive"`, or `"fantasy"` |

</div>

<div>

### LevelOfControl

<dd>One of
<var>not_controllable</var>: cannot be controlled by any extension
<var>controlled_by_other_extensions</var>: controlled by extensions with higher precedence
<var>controllable_by_this_extension</var>: can be controlled by this extension
<var>controlled_by_this_extension</var>: controlled by this extension</dd>

| Enum |
|---|
| `"not_controllable"`, `"controlled_by_other_extensions"`, `"controllable_by_this_extension"`, or `"controlled_by_this_extension"` |

</div>

## Methods

<div>

### clearFont

<div class="summary">`whale.fontSettings.clearFont(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears the font set by this extension, if any.

| Parameters |
|---|
| object | details | 

| [ScriptCode](/extensions/fontSettings#type-ScriptCode) | <span class="optional">(optional)</span> script | 
|---|---|

The script for which the font should be cleared. If omitted, the global script font setting is cleared.
 |
| [GenericFamily](/extensions/fontSettings#type-GenericFamily) | genericFamily | 

The generic font family for which the font should be cleared.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### getFont

<div class="summary">`whale.fontSettings.getFont(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Gets the font for a given script and generic font family.

| Parameters |
|---|
| object | details | 

| [ScriptCode](/extensions/fontSettings#type-ScriptCode) | <span class="optional">(optional)</span> script | 
|---|---|

The script for which the font should be retrieved. If omitted, the font setting for the global script (script code "Zyyy") is retrieved.
 |
| [GenericFamily](/extensions/fontSettings#type-GenericFamily) | genericFamily | 

The generic font family for which the font should be retrieved.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| string | fontId | 
|---|---|

The font ID. Rather than the literal font ID preference value, this may be the ID of the font that the system resolves the preference value to. So, <var>fontId</var> can differ from the font passed to `setFont`, if, for example, the font is not available on the system. The empty string signifies fallback to the global script font setting.
 |
| [LevelOfControl](/extensions/fontSettings#type-LevelOfControl) | levelOfControl | 

The level of control this extension has over the setting.
 |
 |
 |

</div>

</div>

<div>

### setFont

<div class="summary">`whale.fontSettings.setFont(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Sets the font for a given script and generic font family.

| Parameters |
|---|
| object | details | 

| [ScriptCode](/extensions/fontSettings#type-ScriptCode) | <span class="optional">(optional)</span> script | 
|---|---|

The script code which the font should be set. If omitted, the font setting for the global script (script code "Zyyy") is set.
 |
| [GenericFamily](/extensions/fontSettings#type-GenericFamily) | genericFamily | 

The generic font family for which the font should be set.
 |
| string | fontId | 

The font ID. The empty string means to fallback to the global script font setting.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### getFontList

<div class="summary">`whale.fontSettings.getFontList(<span>function callback</span>)`</div>

<div class="description">

Gets a list of fonts on the system.

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [FontName](/extensions/fontSettings#type-FontName) results) <span class="subdued">{...}</span>;`

| array of [FontName](/extensions/fontSettings#type-FontName) | results |  |
|---|---|---|
 |

</div>

</div>

<div>

### clearDefaultFontSize

<div class="summary">`whale.fontSettings.clearDefaultFontSize(<span class="optional">object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears the default font size set by this extension, if any.

| Parameters |
|---|
| object | <span class="optional">(optional)</span> details | 

This parameter is currently unused.
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### getDefaultFontSize

<div class="summary">`whale.fontSettings.getDefaultFontSize(<span class="optional">object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Gets the default font size.

| Parameters |
|---|
| object | <span class="optional">(optional)</span> details | 

This parameter is currently unused.
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| integer | pixelSize | 
|---|---|

The font size in pixels.
 |
| [LevelOfControl](/extensions/fontSettings#type-LevelOfControl) | levelOfControl | 

The level of control this extension has over the setting.
 |
 |
 |

</div>

</div>

<div>

### setDefaultFontSize

<div class="summary">`whale.fontSettings.setDefaultFontSize(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Sets the default font size.

| Parameters |
|---|
| object | details | 

| integer | pixelSize | 
|---|---|

The font size in pixels.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### clearDefaultFixedFontSize

<div class="summary">`whale.fontSettings.clearDefaultFixedFontSize(<span class="optional">object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears the default fixed font size set by this extension, if any.

| Parameters |
|---|
| object | <span class="optional">(optional)</span> details | 

This parameter is currently unused.
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### getDefaultFixedFontSize

<div class="summary">`whale.fontSettings.getDefaultFixedFontSize(<span class="optional">object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Gets the default size for fixed width fonts.

| Parameters |
|---|
| object | <span class="optional">(optional)</span> details | 

This parameter is currently unused.
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| integer | pixelSize | 
|---|---|

The font size in pixels.
 |
| [LevelOfControl](/extensions/fontSettings#type-LevelOfControl) | levelOfControl | 

The level of control this extension has over the setting.
 |
 |
 |

</div>

</div>

<div>

### setDefaultFixedFontSize

<div class="summary">`whale.fontSettings.setDefaultFixedFontSize(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Sets the default size for fixed width fonts.

| Parameters |
|---|
| object | details | 

| integer | pixelSize | 
|---|---|

The font size in pixels.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### clearMinimumFontSize

<div class="summary">`whale.fontSettings.clearMinimumFontSize(<span class="optional">object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Clears the minimum font size set by this extension, if any.

| Parameters |
|---|
| object | <span class="optional">(optional)</span> details | 

This parameter is currently unused.
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### getMinimumFontSize

<div class="summary">`whale.fontSettings.getMinimumFontSize(<span class="optional">object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Gets the minimum font size.

| Parameters |
|---|
| object | <span class="optional">(optional)</span> details | 

This parameter is currently unused.
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| integer | pixelSize | 
|---|---|

The font size in pixels.
 |
| [LevelOfControl](/extensions/fontSettings#type-LevelOfControl) | levelOfControl | 

The level of control this extension has over the setting.
 |
 |
 |

</div>

</div>

<div>

### setMinimumFontSize

<div class="summary">`whale.fontSettings.setMinimumFontSize(<span>object details</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Sets the minimum font size.

| Parameters |
|---|
| object | details | 

| integer | pixelSize | 
|---|---|

The font size in pixels.
 |
 |
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

## Events

<div>

### onFontChanged

<div class="description">

Fired when a font setting changes.

<div>

#### addListener

<div class="summary">`whale.fontSettings.onFontChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| string | fontId | 
|---|---|

The font ID. See the description in `getFont`.
 |
| [ScriptCode](/extensions/fontSettings#type-ScriptCode) | <span class="optional">(optional)</span> script | 

The script code for which the font setting has changed.
 |
| [GenericFamily](/extensions/fontSettings#type-GenericFamily) | genericFamily | 

The generic font family for which the font setting has changed.
 |
| [LevelOfControl](/extensions/fontSettings#type-LevelOfControl) | levelOfControl | 

The level of control this extension has over the setting.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onDefaultFontSizeChanged

<div class="description">

Fired when the default font size setting changes.

<div>

#### addListener

<div class="summary">`whale.fontSettings.onDefaultFontSizeChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| integer | pixelSize | 
|---|---|

The font size in pixels.
 |
| [LevelOfControl](/extensions/fontSettings#type-LevelOfControl) | levelOfControl | 

The level of control this extension has over the setting.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onDefaultFixedFontSizeChanged

<div class="description">

Fired when the default fixed font size setting changes.

<div>

#### addListener

<div class="summary">`whale.fontSettings.onDefaultFixedFontSizeChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| integer | pixelSize | 
|---|---|

The font size in pixels.
 |
| [LevelOfControl](/extensions/fontSettings#type-LevelOfControl) | levelOfControl | 

The level of control this extension has over the setting.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onMinimumFontSizeChanged

<div class="description">

Fired when the minimum font size setting changes.

<div>

#### addListener

<div class="summary">`whale.fontSettings.onMinimumFontSizeChanged.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object details) <span class="subdued">{...}</span>;`

| object | details | 
|---|---|

| integer | pixelSize | 
|---|---|

The font size in pixels.
 |
| [LevelOfControl](/extensions/fontSettings#type-LevelOfControl) | levelOfControl | 

The level of control this extension has over the setting.
 |
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>