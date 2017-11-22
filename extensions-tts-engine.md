# whale.ttsEngine

| Description: | Use the `whale.ttsEngine` API to implement a text-to-speech(TTS) engine using an extension. If your extension registers using this API, it will receive events containing an utterance to be spoken and other parameters when any extension or Chrome App uses the [tts](tts) API to generate speech. Your extension can then use any available web technology to synthesize and output the speech, and send events back to the calling function to report the status. |
|---|---|
| Availability: | Since Chrome 19. |
| Permissions: | <span class="code">"ttsEngine"</span> |

<section>

## Overview

An extension can register itself as a speech engine. By doing so, it can intercept some or all calls to functions such as [tts.speak](/extensions/tts#method-speak) and [tts.stop](/extensions/tts#method-stop) and provide an alternate implementation. Extensions are free to use any available web technology to provide speech, including streaming audio from a server, HTML5 audio, Native Client, or Flash. An extension could even do something different with the utterances, like display closed captions in a pop-up window or send them as log messages to a remote server.

## Manifest

To implement a TTS engine, an extension must declare the "ttsEngine" permission and then declare all voices it provides in the extension manifest, like this:

<pre data-filename="manifest.json">      {
        "name": "My TTS Engine",
        "version": "1.0",
        **"permissions": ["ttsEngine"],
        "tts_engine": {
          "voices": [
            {
              "voice_name": "Alice",
              "lang": "en-US",
              "gender": "female",
              "event_types": ["start", "marker", "end"]
            },
            {
              "voice_name": "Pat",
              "lang": "en-US",
              "event_types": ["end"]
            }
          ]
        },**
        "background": {
          "page": "background.html",
          "persistent": false
        }
      }
      </pre>

An extension can specify any number of voices.

The `voice_name` parameter is required. The name should be descriptive enough that it identifies the name of the voice and the engine used. In the unlikely event that two extensions register voices with the same name, a client can specify the ID of the extension that should do the synthesis.

The `gender` parameter is optional. If your voice corresponds to a male or female voice, you can use this parameter to help clients choose the most appropriate voice for their application.

The `lang` parameter is optional, but highly recommended. Almost always, a voice can synthesize speech in just a single language. When an engine supports more than one language, it can easily register a separate voice for each language. Under rare circumstances where a single voice can handle more than one language, it's easiest to just list two separate voices and handle them using the same logic internally. However, if you want to create a voice that will handle utterances in any language, leave out the `lang` parameter from your extension's manifest.

Finally, the `event_types` parameter is required if the engine can send events to update the client on the progress of speech synthesis. At a minimum, supporting the `'end'` event type to indicate when speech is finished is highly recommended, otherwise Chrome cannot schedule queued utterances.

**Note:** If your TTS engine does not support the `'end'` event type, Chrome cannot queue utterances because it has no way of knowing when your utterance has finished. To help mitigate this, Chrome passes an additional boolean `enqueue` option to your engine's onSpeak handler, giving you the option of implementing your own queueing. This is discouraged because then clients are unable to queue utterances that should get spoken by different speech engines.

The possible event types that you can send correspond to the event types that the `speak()` method receives:

*   `'start'`: The engine has started speaking the utterance.
*   `'word'`: A word boundary was reached. Use `event.charIndex` to determine the current speech position.
*   `'sentence'`: A sentence boundary was reached. Use `event.charIndex` to determine the current speech position.
*   `'marker'`: An SSML marker was reached. Use `event.charIndex` to determine the current speech position.
*   `'end'`: The engine has finished speaking the utterance.
*   `'error'`: An engine-specific error occurred and this utterance cannot be spoken. Pass more information in `event.errorMessage`.

The `'interrupted'` and `'cancelled'` events are not sent by the speech engine; they are generated automatically by Chrome.

Text-to-speech clients can get the voice information from your extension's manifest by calling [tts.getVoices](/extensions/tts#method-getVoices), assuming you've registered speech event listeners as described below.

## Handling speech events

To generate speech at the request of clients, your extension must register listeners for both `onSpeak` and `onStop`, like this:

<pre>var speakListener = function(utterance, options, sendTtsEvent) {
        sendTtsEvent({'event_type': 'start', 'charIndex': 0})

        // (start speaking)

        sendTtsEvent({'event_type': 'end', 'charIndex': utterance.length})
      };

      var stopListener = function() {
        // (stop all speech)
      };

      whale.ttsEngine.onSpeak.addListener(speakListener);
      whale.ttsEngine.onStop.addListener(stopListener);</pre>

**Important:** If your extension does not register listeners for both `onSpeak` and `onStop`, it will not intercept any speech calls, regardless of what is in the manifest.

The decision of whether or not to send a given speech request to an extension is based solely on whether the extension supports the given voice parameters in its manifest and has registered listeners for `onSpeak` and `onStop`. In other words, there's no way for an extension to receive a speech request and dynamically decide whether to handle it.

</section>

<section id="toc">

## Summary

| Types |
|---|
| [VoiceGender](#type-VoiceGender) |
| Events |
| [onSpeak](#event-onSpeak) |
| [onStop](#event-onStop) |
| [onPause](#event-onPause) |
| [onResume](#event-onResume) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### VoiceGender

| Enum |
|---|
| `"male"`, or `"female"` |

</div>

## Events

<div>

### onSpeak

<div class="description">

Called when the user makes a call to tts.speak() and one of the voices from this extension's manifest is the first to match the options object.

<div>

#### addListener

<div class="summary">`whale.ttsEngine.onSpeak.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(string utterance, object options, function sendTtsEvent) <span class="subdued">{...}</span>;`

| string | utterance | 
|---|---|

The text to speak, specified as either plain text or an SSML document. If your engine does not support SSML, you should strip out all XML markup and synthesize only the underlying text content. The value of this parameter is guaranteed to be no more than 32,768 characters. If this engine does not support speaking that many characters at a time, the utterance should be split into smaller chunks and queued internally without returning an error.
 |
| object | options | 

Options specified to the tts.speak() method.

| string | <span class="optional">(optional)</span> voiceName | 
|---|---|

The name of the voice to use for synthesis.
 |
| string | <span class="optional">(optional)</span> lang | 

The language to be used for synthesis, in the form _language_-_region_. Examples: 'en', 'en-US', 'en-GB', 'zh-CN'.
 |
| [VoiceGender](/extensions/ttsEngine#type-VoiceGender) | <span class="optional">(optional)</span> gender | 

Gender of voice for synthesized speech.
 |
| double | <span class="optional">(optional)</span> rate | 

Speaking rate relative to the default rate for this voice. 1.0 is the default rate, normally around 180 to 220 words per minute. 2.0 is twice as fast, and 0.5 is half as fast. This value is guaranteed to be between 0.1 and 10.0, inclusive. When a voice does not support this full range of rates, don't return an error. Instead, clip the rate to the range the voice supports.
 |
| double | <span class="optional">(optional)</span> pitch | 

Speaking pitch between 0 and 2 inclusive, with 0 being lowest and 2 being highest. 1.0 corresponds to this voice's default pitch.
 |
| double | <span class="optional">(optional)</span> volume | 

Speaking volume between 0 and 1 inclusive, with 0 being lowest and 1 being highest, with a default of 1.0.
 |
 |
| function | sendTtsEvent | 

Call this function with events that occur in the process of speaking the utterance.

The _sendTtsEvent_ parameter should be a function that looks like this:

`function( [tts.TtsEvent](/extensions/tts#type-TtsEvent) event) <span class="subdued">{...}</span>;`

| [tts.TtsEvent](/extensions/tts#type-TtsEvent) | event | 
|---|---|

The event from the text-to-speech engine indicating the status of this utterance.
 |
 |
 |

</div>

</div>

</div>

</div>

<div>

### onStop

<div class="description">

Fired when a call is made to tts.stop and this extension may be in the middle of speaking. If an extension receives a call to onStop and speech is already stopped, it should do nothing (not raise an error). If speech is in the paused state, this should cancel the paused state.

<div>

#### addListener

<div class="summary">`whale.ttsEngine.onStop.addListener(<span>function callback</span>)`</div>

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

### onPause

<div class="description">

Since Chrome 29.

Optional: if an engine supports the pause event, it should pause the current utterance being spoken, if any, until it receives a resume event or stop event. Note that a stop event should also clear the paused state.

<div>

#### addListener

<div class="summary">`whale.ttsEngine.onPause.addListener(<span>function callback</span>)`</div>

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

### onResume

<div class="description">

Since Chrome 29.

Optional: if an engine supports the pause event, it should also support the resume event, to continue speaking the current utterance, if any. Note that a stop event should also clear the paused state.

<div>

#### addListener

<div class="summary">`whale.ttsEngine.onResume.addListener(<span>function callback</span>)`</div>

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