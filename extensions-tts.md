# whale.tts

| Description: | Use the `whale.tts` API to play synthesized text-to-speech (TTS). See also the related [ttsEngine](http://developer.whale.com/extensions/ttsEngine) API, which allows an extension to implement a speech engine. |
|---|---|
| Availability: | Since Chrome 20. |
| Permissions: | <span class="code">"tts"</span> |
| Learn More: | [Chrome Office Hours: Text to Speech API](https://developers.google.com/live/shows/7320022-7001/) |

<section>

## Overview

Chrome provides native support for speech on Windows (using SAPI 5), Mac OS X, and Chrome OS, using speech synthesis capabilities provided by the operating system. On all platforms, the user can install extensions that register themselves as alternative speech engines.

## Generating speech

Call `speak()` from your extension or Chrome App to speak. For example:

<pre>whale.tts.speak('Hello, world.');</pre>

To stop speaking immediately, just call `stop()`:

<pre>whale.tts.stop();</pre>

You can provide options that control various properties of the speech, such as its rate, pitch, and more. For example:

<pre>whale.tts.speak('Hello, world.', {'rate': 2.0});</pre>

It's also a good idea to specify the language so that a synthesizer supporting that language (and regional dialect, if applicable) is chosen.

<pre>whale.tts.speak(
          'Hello, world.', {'lang': 'en-US', 'rate': 2.0});</pre>

By default, each call to `speak()` interrupts any ongoing speech and speaks immediately. To determine if a call would be interrupting anything, you can call `isSpeaking()`. In addition, you can use the `enqueue` option to cause this utterance to be added to a queue of utterances that will be spoken when the current utterance has finished.

<pre>whale.tts.speak(
          'Speak this first.');
      whale.tts.speak(
          'Speak this next, when the first sentence is done.', {'enqueue': true});
      </pre>

A complete description of all options can be found in the [tts.speak](/extensions/tts#method-speak) below. Not all speech engines will support all options.

To catch errors and make sure you're calling `speak()` correctly, pass a callback function that takes no arguments. Inside the callback, check [runtime.lastError](/extensions/runtime#property-lastError) to see if there were any errors.

<pre>whale.tts.speak(
          utterance,
          options,
          function() {
            if (whale.runtime.lastError) {
              console.log('Error: ' + whale.runtime.lastError.message);
            }
          });</pre>

The callback returns right away, before the engine has started generating speech. The purpose of the callback is to alert you to syntax errors in your use of the TTS API, not to catch all possible errors that might occur in the process of synthesizing and outputting speech. To catch these errors too, you need to use an event listener, described below.

## Listening to events

To get more real-time information about the status of synthesized speech, pass an event listener in the options to `speak()`, like this:

<pre>whale.tts.speak(
          utterance,
          {
            onEvent: function(event) {
              console.log('Event ' + event.type ' at position ' + event.charIndex);
              if (event.type == 'error') {
                console.log('Error: ' + event.errorMessage);
              }
            }
          },
          callback);</pre>

Each event includes an event type, the character index of the current speech relative to the utterance, and for error events, an optional error message. The event types are:

*   `'start'`: The engine has started speaking the utterance.
*   `'word'`: A word boundary was reached. Use `event.charIndex` to determine the current speech position.
*   `'sentence'`: A sentence boundary was reached. Use `event.charIndex` to determine the current speech position.
*   `'marker'`: An SSML marker was reached. Use `event.charIndex` to determine the current speech position.
*   `'end'`: The engine has finished speaking the utterance.
*   `'interrupted'`: This utterance was interrupted by another call to `speak()` or `stop()` and did not finish.
*   `'cancelled'`: This utterance was queued, but then cancelled by another call to `speak()` or `stop()` and never began to speak at all.
*   `'error'`: An engine-specific error occurred and this utterance cannot be spoken. Check `event.errorMessage` for details.

Four of the event types—`'end'`, `'interrupted'`, `'cancelled'`, and `'error'`—are _final_. After one of those events is received, this utterance will no longer speak and no new events from this utterance will be received.

Some voices may not support all event types, and some voices may not send any events at all. If you do not want to use a voice unless it sends certain events, pass the events you require in the `requiredEventTypes` member of the options object, or use `getVoices()` to choose a voice that meets your requirements. Both are documented below.

## SSML markup

Utterances used in this API may include markup using the [Speech Synthesis Markup Language (SSML)](http://www.w3.org/TR/speech-synthesis). If you use SSML, the first argument to `speak()` should be a complete SSML document with an XML header and a top-level `<speak>` tag, not a document fragment.

For example:

<pre>whale.tts.speak(
          '<?xml version="1.0"?>' +
          '<speak>' +
          '  The <emphasis>second</emphasis> ' +
          '  word of this sentence was emphasized.' +
          '</speak>');</pre>

Not all speech engines will support all SSML tags, and some may not support SSML at all, but all engines are required to ignore any SSML they don't support and to still speak the underlying text.

## Choosing a voice

By default, Chrome chooses the most appropriate voice for each utterance you want to speak, based on the language and gender. On most Windows, Mac OS X, and Chrome OS systems, speech synthesis provided by the operating system should be able to speak any text in at least one language. Some users may have a variety of voices available, though, from their operating system and from speech engines implemented by other Chrome extensions. In those cases, you can implement custom code to choose the appropriate voice, or to present the user with a list of choices.

To get a list of all voices, call `getVoices()` and pass it a function that receives an array of `TtsVoice` objects as its argument:

<pre>whale.tts.getVoices(
          function(voices) {
            for (var i = 0; i < voices.length; i++) {
              console.log('Voice ' + i + ':');
              console.log('  name: ' + voices[i].voiceName);
              console.log('  lang: ' + voices[i].lang);
              console.log('  gender: ' + voices[i].gender);
              console.log('  extension id: ' + voices[i].extensionId);
              console.log('  event types: ' + voices[i].eventTypes);
            }
          });</pre>

</section>

<section id="toc">

## Summary

| Types |
|---|
| [EventType](#type-EventType) |
| [VoiceGender](#type-VoiceGender) |
| [TtsEvent](#type-TtsEvent) |
| [TtsVoice](#type-TtsVoice) |
| Methods |
| [speak](#method-speak) − `whale.tts.speak(<span>string utterance</span>, <span class="optional">object options</span>, <span class="optional">function callback</span>)` |
| [stop](#method-stop) − `whale.tts.stop()` |
| [pause](#method-pause) − `whale.tts.pause()` |
| [resume](#method-resume) − `whale.tts.resume()` |
| [isSpeaking](#method-isSpeaking) − `whale.tts.isSpeaking(<span class="optional">function callback</span>)` |
| [getVoices](#method-getVoices) − `whale.tts.getVoices(<span class="optional">function callback</span>)` |

</section>

<section>

<div class="api-reference">

## Types

<div>

### EventType

| Enum |
|---|
| `"start"`, `"end"`, `"word"`, `"sentence"`, `"marker"`, `"interrupted"`, `"cancelled"`, `"error"`, `"pause"`, or `"resume"` |

</div>

<div>

### VoiceGender

| Enum |
|---|
| `"male"`, or `"female"` |

</div>

<div>

### TtsEvent

<dd>An event from the TTS engine to communicate the status of an utterance.</dd>

| properties |
|---|
| [EventType](/extensions/tts#type-EventType) | type | 

The type can be 'start' as soon as speech has started, 'word' when a word boundary is reached, 'sentence' when a sentence boundary is reached, 'marker' when an SSML mark element is reached, 'end' when the end of the utterance is reached, 'interrupted' when the utterance is stopped or interrupted before reaching the end, 'cancelled' when it's removed from the queue before ever being synthesized, or 'error' when any other error occurs. When pausing speech, a 'pause' event is fired if a particular utterance is paused in the middle, and 'resume' if an utterance resumes speech. Note that pause and resume events may not fire if speech is paused in-between utterances.
 |
| integer | <span class="optional">(optional)</span> charIndex | 

The index of the current character in the utterance.
 |
| string | <span class="optional">(optional)</span> errorMessage | 

The error description, if the event type is 'error'.
 |

</div>

<div>

### TtsVoice

<dd>A description of a voice available for speech synthesis.</dd>

| properties |
|---|
| string | <span class="optional">(optional)</span> voiceName | 

The name of the voice.
 |
| string | <span class="optional">(optional)</span> lang | 

The language that this voice supports, in the form _language_-_region_. Examples: 'en', 'en-US', 'en-GB', 'zh-CN'.
 |
| [VoiceGender](/extensions/tts#type-VoiceGender) | <span class="optional">(optional)</span> gender | 

This voice's gender.
 |
| boolean | <span class="optional">(optional)</span> remote | 

Since Chrome 33.

If true, the synthesis engine is a remote network resource. It may be higher latency and may incur bandwidth costs.
 |
| string | <span class="optional">(optional)</span> extensionId | 

The ID of the extension providing this voice.
 |
| array of string | <span class="optional">(optional)</span> eventTypes | 

All of the callback event types that this voice is capable of sending.
 |

</div>

## Methods

<div>

### speak

<div class="summary">`whale.tts.speak(<span>string utterance</span>, <span class="optional">object options</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Speaks text using a text-to-speech engine.

| Parameters |
|---|
| string | utterance | 

The text to speak, either plain text or a complete, well-formed SSML document. Speech engines that do not support SSML will strip away the tags and speak the text. The maximum length of the text is 32,768 characters.
 |
| object | <span class="optional">(optional)</span> options | 

The speech options.

| boolean | <span class="optional">(optional)</span> enqueue | 
|---|---|

If true, enqueues this utterance if TTS is already in progress. If false (the default), interrupts any current speech and flushes the speech queue before speaking this new utterance.
 |
| string | <span class="optional">(optional)</span> voiceName | 

The name of the voice to use for synthesis. If empty, uses any available voice.
 |
| string | <span class="optional">(optional)</span> extensionId | 

The extension ID of the speech engine to use, if known.
 |
| string | <span class="optional">(optional)</span> lang | 

The language to be used for synthesis, in the form _language_-_region_. Examples: 'en', 'en-US', 'en-GB', 'zh-CN'.
 |
| [VoiceGender](/extensions/tts#type-VoiceGender) | <span class="optional">(optional)</span> gender | 

Gender of voice for synthesized speech.
 |
| double | <span class="optional">(optional)</span> rate | 

Speaking rate relative to the default rate for this voice. 1.0 is the default rate, normally around 180 to 220 words per minute. 2.0 is twice as fast, and 0.5 is half as fast. Values below 0.1 or above 10.0 are strictly disallowed, but many voices will constrain the minimum and maximum rates further—for example a particular voice may not actually speak faster than 3 times normal even if you specify a value larger than 3.0.
 |
| double | <span class="optional">(optional)</span> pitch | 

Speaking pitch between 0 and 2 inclusive, with 0 being lowest and 2 being highest. 1.0 corresponds to a voice's default pitch.
 |
| double | <span class="optional">(optional)</span> volume | 

Speaking volume between 0 and 1 inclusive, with 0 being lowest and 1 being highest, with a default of 1.0.
 |
| array of string | <span class="optional">(optional)</span> requiredEventTypes | 

The TTS event types the voice must support.
 |
| array of string | <span class="optional">(optional)</span> desiredEventTypes | 

The TTS event types that you are interested in listening to. If missing, all event types may be sent.
 |
| function | <span class="optional">(optional)</span> onEvent | 

This function is called with events that occur in the process of speaking the utterance.

| Parameters |
|---|
| [TtsEvent](/extensions/tts#type-TtsEvent) | event | 

The update event from the text-to-speech engine indicating the status of this utterance.
 |
 |
 |
| function | <span class="optional">(optional)</span> callback | 

Called right away, before speech finishes. Check whale.runtime.lastError to make sure there were no errors. Use options.onEvent to get more detailed feedback.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>

<div>

### stop

<div class="summary">`whale.tts.stop()`</div>

<div class="description">

Stops any current speech and flushes the queue of any pending utterances. In addition, if speech was paused, it will now be un-paused for the next call to speak.

</div>

</div>

<div>

### pause

<div class="summary">`whale.tts.pause()`</div>

<div class="description">

Since Chrome 29.

Pauses speech synthesis, potentially in the middle of an utterance. A call to resume or stop will un-pause speech.

</div>

</div>

<div>

### resume

<div class="summary">`whale.tts.resume()`</div>

<div class="description">

Since Chrome 29.

If speech was paused, resumes speaking where it left off.

</div>

</div>

<div>

### isSpeaking

<div class="summary">`whale.tts.isSpeaking(<span class="optional">function callback</span>)`</div>

<div class="description">

Checks whether the engine is currently speaking. On Mac OS X, the result is true whenever the system speech engine is speaking, even if the speech wasn't initiated by Chrome.

| Parameters |
|---|
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(boolean speaking) <span class="subdued">{...}</span>;`

| boolean | speaking | 
|---|---|

True if speaking, false otherwise.
 |
 |

</div>

</div>

<div>

### getVoices

<div class="summary">`whale.tts.getVoices(<span class="optional">function callback</span>)`</div>

<div class="description">

Gets an array of all available voices.

| Parameters |
|---|
| function | <span class="optional">(optional)</span> callback | 

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(array of [TtsVoice](/extensions/tts#type-TtsVoice) voices) <span class="subdued">{...}</span>;`

| array of [TtsVoice](/extensions/tts#type-TtsVoice) | voices | 
|---|---|

Array of [tts.TtsVoice](/extensions/tts#type-TtsVoice) objects representing the available voices for speech synthesis.
 |
 |

</div>

</div>

</div>

</section>