# A ROS service that provides speech recognition using a KALDI server

See also: https://github.com/HackRoboy/RoboyOSR
(you can find a setup instruction for the KALDI docker module in [LucidaContainer/Hackroboy3-notizen-shuttle.txt](https://github.com/HackRoboy/RoboyOSR/blob/master/LucidaContainer/Hackroboy3-notizen-shuttle.txt9))

This module is based on the https://github.com/Roboy/roboy_speech_recognition but uses a local KALDI server instead of the Bing API

## Functionality
This module creates a ROS node `roboy_local_speech_recognition` that provides a ROS service `/roboy/cognition/speech/recognition`.
Calling this service triggers the speech recognition which will record audio on the PC that runs the node and will send that audio to the (currently hardcoded) KALDI server.

It will return the recognized text as a string.

## Versions

### master
The default version will record the audio until a speech pause is detected. It will then send the audio to the KALDI server

### audio_streaming
This version (on the branch of the same name) will take advantage of the audio streaming feature a local KALDI server provides.
It will start sending packages during the recording which gives the speech recognition a headstart.


## Known issues

The KALDI server provides preliminary hypotheses while the audio is still processed.
These are received through the websocket and printed to the console.
Usually, after each part, a final hypothesis should be sent to the client.

During our tests, this unfortunately didn't happen which resulted in no recognized text being returned ever (both versions) since only the final hypothesis is actually relevant.

Also the audio_streaming branch needs adjustment for the package sizes and delays; eg. larger packages should be collected to be sent to the server instead of sending each hunk individually.