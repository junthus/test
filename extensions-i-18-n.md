# whale.i18n

| Description: | Use the `whale.i18n` infrastructure to implement internationalization across your whole app or extension. |
|---|---|
| Availability: | Since Chrome 20. |
| Content Scripts: | Fully supported. [Learn more](content_scripts) |

<section>

You need to put all of its user-visible strings into a file named [`messages.json`](i18n-messages). Each time you add a new locale, you add a messages file under a directory named `_locales/_localeCode_`, where _localeCode_ is a code such as `en` for English.

Here's the file hierarchy for an internationalized extension that supports English (`en`), Spanish (`es`), and Korean (`ko`):

![In the extension directory: manifest.json, *.html, *.js, _locales directory. In the _locales directory: en, es, and ko directories, each with a messages.json file.](/static/images/i18n-hierarchy.gif)

## How to support multiple languages

Say you have an extension with the files shown in the following figure:

![A manifest.json file and a file with JavaScript. The .json file has "name": "Hello World". The JavaScript file has title = "Hello World";](/static/images/i18n-before.gif)

To internationalize this extension, you name each user-visible string and put it into a messages file. The extension's manifest, CSS files, and JavaScript code use each string's name to get its localized version.

Here's what the extension looks like when it's internationalized (note that it still has only English strings):

![In the manifest.json file, "Hello World" has been changed to "__MSG_extName__", and a new "default_locale" item has the value "en". In the JavaScript file, "Hello World" has been changed to whale.i18n.getMessage("extName"). A new file named _locales/en/messages.json defines "extName".](/static/images/i18n-after-1.gif)

**Important:** If an extension has a `_locales` directory, the [manifest](manifest) **must** define "default_locale".

Some notes about internationalizing:

*   You can use any of the [supported locales](#overview-locales). If you use an unsupported locale, Google Chrome ignores it.

*   In `manifest.json` and CSS files, refer to a string named _messagename_ like this:

    <pre>__MSG__messagename___</pre>

*   In your extension or app's JavaScript code, refer to a string named _messagename_ like this:

    <pre>whale.i18n.getMessage("_messagename_")</pre>

*   In each call to `getMessage()`, you can supply up to 9 strings to be included in the message. See [Examples: getMessage](#examples-getMessage) for details.

*   Some messages, such as `@@bidi_dir` and `@@ui_locale`, are provided by the internationalization system. See the [Predefined messages](#overview-predefined) section for a full list of predefined message names.

*   In `messages.json`, each user-visible string has a name, a "message" item, and an optional "description" item. The name is a key such as "extName" or "search_string" that identifies the string. The "message" specifies the value of the string in this locale. The optional "description" provides help to translators, who might not be able to see how the string is used in your extension. For example:

    <pre data-filename="messages.json">      {
            "search_string": {
              "message": "hello%20world",
              "description": "The string we search for. Put %20 between words that go together."
            },
            ...
          }</pre>

    For more information, see [Formats: Locale-Specific Messages](i18n-messages).

Once an extension or app is internationalized, translating it is simple. You copy `messages.json`, translate it, and put the copy into a new directory under `_locales`. For example, to support Spanish, just put a translated copy of `messages.json` under `_locales/es`. The following figure shows the previous extension with a new Spanish translation.

![This looks the same as the previous figure, but with a new file at _locales/es/messages.json that contains a Spanish translation of the messages.](/static/images/i18n-after-2.gif)

## Predefined messages

The internationalization system provides a few predefined messages to help you localize. These include `@@ui_locale`, so you can detect the current UI locale, and a few `@@bidi_...` messages that let you detect the text direction. The latter messages have similar names to constants in the [gadgets BIDI (bi-directional) API](http://code.google.com/apis/gadgets/docs/i18n.html#BIDI).

The special message `@@extension_id` can be used in the CSS and JavaScript files, whether or not the extension or app is localized. This message doesn't work in manifest files.

The following table describes each predefined message.

| Message name | Description |
|---|---|
| `@@extension_id` | The extension or app ID; you might use this string to construct URLs for resources inside the extension. Even unlocalized extensions can use this message.
**Note:** You can't use this message in a manifest file. |
| `@@ui_locale` | The current locale; you might use this string to construct locale-specific URLs. |
| `@@bidi_dir` | The text direction for the current locale, either "ltr" for left-to-right languages such as English or "rtl" for right-to-left languages such as Japanese. |
| `@@bidi_reversed_dir` | If the `@@bidi_dir` is "ltr", then this is "rtl"; otherwise, it's "ltr". |
| `@@bidi_start_edge` | If the `@@bidi_dir` is "ltr", then this is "left"; otherwise, it's "right". |
| `@@bidi_end_edge` | If the `@@bidi_dir` is "ltr", then this is "right"; otherwise, it's "left". |

Here's an example of using `@@extension_id` in a CSS file to construct a URL:

<pre>      body {
        **background-image:url('chrome-extension://__MSG_@@extension_id__/background.png');**
      }
      </pre>

If the extension ID is abcdefghijklmnopqrstuvwxyzabcdef, then the bold line in the previous code snippet becomes:

<pre>      background-image:url('chrome-extension://abcdefghijklmnopqrstuvwxyzabcdef/background.png');
      </pre>

Here's an example of using `@@bidi_*` messages in a CSS file:

<pre>      body {
        **direction: __MSG_@@bidi_dir__;**
      }

      div#header {
        margin-bottom: 1.05em;
        overflow: hidden;
        padding-bottom: 1.5em;
        **padding-__MSG_@@bidi_start_edge__: 0;**
        **padding-__MSG_@@bidi_end_edge__: 1.5em;**
        position: relative;
      }
      </pre>

For left-to-right languages such as English, the bold lines become:

<pre>      dir: ltr;
      padding-left: 0;
      padding-right: 1.5em;
      </pre>

## Locales

You can choose from many locales, including some (such as `en`) that let a single translation support multiple variations of a language (such as `en_GB` and `en_US`).

### Supported locales

You can use any of the [locales that the Chrome Web Store supports](http://code.google.com/chrome/webstore/docs/i18n.html#localeTable).

### Searching for messages

You don't have to define every string for every supported locale. As long as the default locale's `messages.json` file has a value for every string, your extension or app will run no matter how sparse a translation is. Here's how the extension system searches for a message:

1.  Search the messages file (if any) for the user's preferred locale. For example, when Google Chrome's locale is set to British English (`en_GB`), the system first looks for the message in `_locales/en_GB/messages.json`. If that file exists and the message is there, the system looks no further.
2.  If the user's preferred locale has a region (that is, the locale has an underscore: _), search the locale without that region. For example, if the `en_GB` messages file doesn't exist or doesn't contain the message, the system looks in the `en` messages file. If that file exists and the message is there, the system looks no further.
3.  Search the messages file for the default locale. For example, if the extension's "default_locale" is set to "es", and neither `_locales/en_GB/messages.json` nor `_locales/en/messages.json` contains the message, the extension uses the message from `_locales/es/messages.json`.

In the following figure, the message named "colores" is in all three locales that the extension supports, but "extName" is in only two of the locales. Wherever a user running Google Chrome in US English sees the label "Colors", a user of British English sees "Colours". Both US English and British English users see the extension name "Hello World". Because the default language is Spanish, users running Google Chrome in any non-English language see the label "Colores" and the extension name "Hola mundo".

![Four files: manifest.json and three messages.json files (for es, en, and en_GB).  The es and en files show entries for messages named "extName" and "colores"; the en_GB file has just one entry (for "colores").](/static/images/i18n-strings.gif)

### How to set your browser's locale

To test translations, you might want to set your browser's locale. This section tells you how to set the locale in [Windows](#testing-win), [Mac OS X](#testing-mac), and [Linux](#testing-linux).

#### Windows

You can change the locale using either a locale-specific shortcut or the Google Chrome UI. The shortcut approach is quicker, once you've set it up, and it lets you use several languages at once.

##### Using a locale-specific shortcut

To create and use a shortcut that launches Google Chrome with a particular locale:

1.  Make a copy of the Google Chrome shortcut that's already on your desktop.
2.  Rename the new shortcut to match the new locale.
3.  Change the shortcut's properties so that the Target field specifies the `--lang` and `--user-data-dir` flags. The target should look something like this:

    <pre>_path_to_whale.exe_ --lang=_locale_ --user-data-dir=c:\_locale_profile_dir_</pre>

4.  Launch Google Chrome by double-clicking the shortcut.

For example, to create a shortcut that launches Google Chrome in Spanish (`es`), you might create a shortcut named `chrome-es` that has the following target:

<pre>_path_to_whale.exe_ --lang=es --user-data-dir=c:\chrome-profile-es</pre>

You can create as many shortcuts as you like, making it easy to test in multiple languages. For example:

<pre>_path_to_whale.exe_ --lang=en --user-data-dir=c:\chrome-profile-en
      _path_to_whale.exe_ --lang=en_GB --user-data-dir=c:\chrome-profile-en_GB
      _path_to_whale.exe_ --lang=ko --user-data-dir=c:\chrome-profile-ko</pre>

**Note:** Specifying `--user-data-dir` is optional but handy. Having one data directory per locale lets you run the browser in several languages at the same time. A disadvantage is that because the locales' data isn't shared, you have to install your extension multiple times — once per locale, which can be challenging when you don't speak the language. For more information, see [Creating and Using Profiles](http://www.chromium.org/developers/creating-and-using-profiles).

##### Using the UI

Here's how to change the locale using the UI on Google Chrome for Windows:

1.  App icon > **Options**
2.  Choose the **Under the Hood** tab
3.  Scroll down to **Web Content**
4.  Click **Change font and language settings**
5.  Choose the **Languages** tab
6.  Use the drop down to set the **Google Chrome language**
7.  Restart Chrome

#### Mac OS X

To change the locale on Mac, you use the system preferences.

1.  From the Apple menu, choose **System Preferences**
2.  Under the **Personal** section, choose **International**
3.  Choose your language and location
4.  Restart Chrome

#### Linux

To change the locale on Linux, first quit Google Chrome. Then, all in one line, set the LANGUAGE environment variable and launch Google Chrome. For example:

<pre>      LANGUAGE=es ./chrome
      </pre>

## Examples

You can find simple examples of internationalization in the [examples/api/i18n](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/docs/examples/api/i18n/) directory. For a complete example, see [examples/extensions/news](https://chromium.googlesource.com/chromium/src/+/master/chrome/common/extensions/docs/examples/extensions/news/). For other examples and for help in viewing the source code, see [Samples](samples).

### Examples: getMessage

The following code gets a localized message from the browser and displays it as a string. It replaces two placeholders within the message with the strings "string1" and "string2".

<pre>      function getMessage() {
        var message = whale.i18n.getMessage("click_here", ["string1", "string2"]);
        document.getElementById("languageSpan").innerHTML = message;
      }
      </pre>

Here's how you'd supply and use a single string:

<pre>      _// In JavaScript code_
      status.innerText = whale.i18n.getMessage("error", errorDetails);
      </pre>

<pre data-filename="messages.json">      "error": {
        "message": "Error: $details$",
        "description": "Generic error template. Expects error parameter to be passed in.",
        "placeholders": {
          "details": {
            "content": "$1",
            "example": "Failed to fetch RSS feed."
          }
        }
      }
      </pre>

For more information about placeholders, see the [Locale-Specific Messages](i18n-messages) page. For details on calling `getMessage()`, see the [API reference](/extensions/i18n#method-getMessage).

### Example: getAcceptLanguages

The following code gets accept-languages from the browser and displays them as a string by separating each accept-language with ','.

<pre>      function getAcceptLanguages() {
        whale.i18n.getAcceptLanguages(function(languageList) {
          var languages = languageList.join(",");
          document.getElementById("languageSpan").innerHTML = languages;
        })
      }
      </pre>

For details on calling `getAcceptLanguages()`, see the [API reference](/extensions/i18n#method-getAcceptLanguages).

### Example: detectLanguage

The following code detects up to 3 languages from the given string and displays the result as strings separated by new lines.

<pre>      function detectLanguage(inputText) {
        whale.i18n.detectLanguage(inputText, function(result) {
          var outputLang = "Detected Language: ";
          var outputPercent = "Language Percentage: ";
          for(i = 0; i < result.languages.length; i++) {
            outputLang += result.languages[i].language + " ";
            outputPercent +=result.languages[i].percentage + " ";
          }
          document.getElementById("languageSpan").innerHTML = outputLang + "\n" + outputPercent + "\nReliable: " + result.isReliable;
        });
      }
      </pre>

For more details on calling `detectLanguage(inputText)`, see the [API reference](/extensions/i18n#method-detectLanguage).

</section>

<section id="toc">

## Summary

| Types |
|---|
| [LanguageCode](#type-LanguageCode) |
| Methods |
| [getAcceptLanguages](#method-getAcceptLanguages) − `whale.i18n.getAcceptLanguages(<span>function callback</span>)` |
| [getMessage](#method-getMessage) − `string whale.i18n.getMessage(<span>string messageName</span>, <span class="optional">any substitutions</span>)` |
| [getUILanguage](#method-getUILanguage) − `string whale.i18n.getUILanguage()` |
| [detectLanguage](#method-detectLanguage) − `whale.i18n.detectLanguage(<span>string text</span>, <span>function callback</span>)` |

</section>

<section>

<div class="api-reference">

## Types

<div>

### LanguageCode

Since Chrome 47.

<dd>string</dd>

<dd>An ISO language code such as `en` or `fr`. For a complete list of languages supported by this method, see [kLanguageInfoTable](http://src.chromium.org/viewvc/chrome/trunk/src/third_party/cld/languages/internal/languages.cc). For an unknown language, `und` will be returned, which means that [percentage] of the text is unknown to CLD</dd>

</div>

## Methods

<div>

### getAcceptLanguages

<div class="summary">`whale.i18n.getAcceptLanguages(<span>function callback</span>)`</div>

<div class="description">

Gets the accept-languages of the browser. This is different from the locale used by the browser; to get the locale, use [i18n.getUILanguage](/extensions/i18n#method-getUILanguage).

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(array of [LanguageCode](/extensions/i18n#type-LanguageCode) languages) <span class="subdued">{...}</span>;`

| array of [LanguageCode](/extensions/i18n#type-LanguageCode) | languages | 
|---|---|

Array of LanguageCode
 |
 |

</div>

</div>

<div>

### getMessage

<div class="summary">`string whale.i18n.getMessage(<span>string messageName</span>, <span class="optional">any substitutions</span>)`</div>

<div class="description">

Gets the localized string for the specified message. If the message is missing, this method returns an empty string (''). If the format of the `getMessage()` call is wrong — for example, _messageName_ is not a string or the _substitutions_ array has more than 9 elements — this method returns `undefined`.

| Parameters |
|---|
| string | messageName | 

The name of the message, as specified in the [`messages.json`](i18n-messages) file.
 |
| any | <span class="optional">(optional)</span> substitutions | 

Up to 9 substitution strings, if the message requires any.
 |

</div>

</div>

<div>

### getUILanguage

<div class="summary">`string whale.i18n.getUILanguage()`</div>

<div class="description">

Since Chrome 35.

Gets the browser UI language of the browser. This is different from [i18n.getAcceptLanguages](/extensions/i18n#method-getAcceptLanguages) which returns the preferred user languages.

</div>

</div>

<div>

### detectLanguage

<div class="summary">`whale.i18n.detectLanguage(<span>string text</span>, <span>function callback</span>)`</div>

<div class="description">

Since Chrome 47.

Detects the language of the provided text using CLD.

| Parameters |
|---|
| string | text | 

User input string to be translated.
 |
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object result) <span class="subdued">{...}</span>;`

| object | result | 
|---|---|

LanguageDetectionResult object that holds detected langugae reliability and array of DetectedLanguage

| boolean | isReliable | 
|---|---|

CLD detected language reliability
 |
| array of object | languages | 

array of detectedLanguage

#### Properties of each object

<dl>

<div>

<dd>DetectedLanguage object that holds detected ISO language code and its percentage in the input string</dd>

| [LanguageCode](/extensions/i18n#type-LanguageCode) | language |  |
|---|---|---|
| integer | percentage | 

The percentage of the detected language
 |

</div>

</dl>
 |
 |
 |

</div>

</div>

</div>

</section>