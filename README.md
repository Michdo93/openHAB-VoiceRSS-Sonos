# openHAB-VoiceRSS-Sonos
A simple openHAB rule which uses Voice RSS on Sonos speakers.

## The Rule

The rule looks like this:

```
import java.net.URLEncoder

val String VOICERSS_API_KEY = "<your_voicerss_api_key>"

rule "Text-to-Speech with VoiceRSS and Sonos"
when
    Item <tts_buffer_item> received update
then
    var String ttsText = <tts_buffer_item>.state.toString // Save the current state in the variable
    ttsText = URLEncoder::encode(ttsText, 'UTF-8')

    // Text-to-Speech with VoiceRSS
    var String ttsUrl = "x-rincon-mp3radio://api.voicerss.org/?key=" + VOICERSS_API_KEY + "&hl=de-de&c=MP3&f=16khz_16bit_stereo&src=" + ttsText

    logInfo("Text-to-Speech", "ttsUrl: " + ttsUrl)

    // Playback via Sonos speakers
    <sonos_playuri_item>.sendCommand(ttsUrl)
end
```

If you are using multiple sonos speakers it looks like this:

```
import java.net.URLEncoder

val String VOICERSS_API_KEY = "<your_voicerss_api_key>"

rule "Text-to-Speech with VoiceRSS and Sonos device 1"
when
    Item <tts_buffer_item1> received update
then
    var String ttsText = <tts_buffer_item1>.state.toString // Save the current state in the variable
    ttsText = URLEncoder::encode(ttsText, 'UTF-8')

    // Text-to-Speech with VoiceRSS
    var String ttsUrl = "x-rincon-mp3radio://api.voicerss.org/?key=" + VOICERSS_API_KEY + "&hl=de-de&c=MP3&f=16khz_16bit_stereo&src=" + ttsText

    logInfo("Text-to-Speech", "ttsUrl: " + ttsUrl)

    // Playback via Sonos speakers
    <sonos_playuri_item1>.sendCommand(ttsUrl)
end

rule "Text-to-Speech with VoiceRSS and Sonos device 2"
when
    Item <tts_buffer_item2> received update
then
    var String ttsText = <tts_buffer_item2>.state.toString // Save the current state in the variable
    ttsText = URLEncoder::encode(ttsText, 'UTF-8')

    // Text-to-Speech with VoiceRSS
    var String ttsUrl = "x-rincon-mp3radio://api.voicerss.org/?key=" + VOICERSS_API_KEY + "&hl=de-de&c=MP3&f=16khz_16bit_stereo&src=" + ttsText

    logInfo("Text-to-Speech", "ttsUrl: " + ttsUrl)

    // Playback via Sonos speakers
    <sonos_playuri_item2>.sendCommand(ttsUrl)
end
```


## Installation

Please make sure you have installed the [Sonos Binding](https://www.openhab.org/addons/bindings/sonos/). You must create a minimum `<sonos_playuri_item>` for your Sonos speaker. Also you have to create a unbound item for your `<tts_buffer_item>`. This should be a string item. Each time this buffer receives a new word or phrase, it will be sent to VoiceRSS and output through the Sonos speaker and its PlayURI function.

Finally, this means that you need to register at [Voice RSS](https://www.voicerss.org/) and you can usually add the API key. Free of charge, 350 requests can be made daily to the Voice RSS service.

The rules file is structured so that the API key can be reused for several different Sonos devices (e.g. you may have some in the living room, bedroom or bathroom).

## Customize the rule

For example, with `hl` you specify the language, with `v` the voice, etc. You can find more about this [here](https://www.voicerss.org/api/).
