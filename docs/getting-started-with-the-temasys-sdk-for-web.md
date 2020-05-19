---
title: sdk for web
date: 2020-05-17
slug: sdk-for-web

---
Getting Started with the Temasys SDK for Web

## **A step-by-step guide to embedding Real-Time Communication features into your webapp or website**

This document is designed to help developers get started using the Temasys SDK for the Web to add video & voice calling, secure messaging, file sharing and screen sharing features to any website.

## **Step 1: Create an Account on the Temasys Console and Generate an App Key**

[Here are instructions for creating an account and generating an App Key.](https://temasys.io/creating-an-account-generating-a-key/)

## **Step 2: Include Temasys SkylinkJS code into your website**

### Include Video and Audio Elements in HTML file

 1. <html>
 2. <head>
 3. <title>WebRTC with Temasys SkylinkJS</title>
 4. <script><script>
 5. </head>
 6. <body>
 7. <video id="myvideo" style="transform: rotateY(-180deg);" autoplay muted</video>
 8. <audio id="myaudio" autoplay muted</audio>
 9. </body>
10. </html>

The body contains a video tag to attach the video stream from the camera and an audio tag to attach the audio stream. We used a CSS transform call to mirror the image, so it looks and feels more natural. We muted the audio, so you don’t hear yourself speaking. The autoplay attribute is needed in some browsers where there are restrictions on autoplay. [https://developers.google.com/web/updates/2017/09/autoplay-policy-changes](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes "https://developers.google.com/web/updates/2017/09/autoplay-policy-changes")

## **Step 3: Import**

### Import from a path

Add to main.js

1. import Skylink, { SkylinkEventManager, SkylinkLogger, SkylinkConstants } from './path/to/skylink.complete.js'

### Include as a script tag with type as Module

Add to index.html

1. <script src="./path/to/skylink.complete.js" type="module"></script>

## **Step 4: Initialise and then listen for events**

### Instantiate Skylink and init with config

For appKey: ‘XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX’, use the appKey you generated in the Temasys Console.

Full list of init configuration options here: [initOptions](http://cdn.temasys.io/skylink/skylinkjs/2.x/docs/global.html#initOptions)

1. const config = {
2. appKey: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX',
3. defaultRoom: 'skylinkRoom',
4. enableIceTrickle: true,
5. enableDataChannel: true,
6. forceSSL: true,
7. };
8. 
9. skylink = new Skylink(config);

### 

### Declare joinRoomOptions and pass as argument in joinRoom method. MediaStreams can be obtained from the resolved Promise

Configure by setting audio: true and video: true to access camera and microphone.

Full list of joinRoom config options here: [joinRoomOptions](http://cdn.temasys.io/skylink/skylinkjs/2.x/docs/global.html#joinRoomOptions)

 1. const joinRoomOptions = {
 2. audio: { stereo: true },
 3. video: true,
 4. };
 5. 
 6. skylink.joinRoom(joinRoomOptions)
 7. .then((streams) => {
 8. // if there is an audio stream
 9. if (streams\[0\]) {
10. window.attachMediaStream(audioEl, streams\[0\]);
11. }
12. // if there is a video stream
13. if (streams\[1\]) {
14. window.attachMediaStream(videoEl, streams\[1\]);
15. }
16. })
17. .catch();

### 

## **Step 5: Subscribing and Unsubscribing to an event**

### Listen on ‘peerJoined’ event with isSelf=true for self join room success outcome

[**peerJoined**](http://cdn.temasys.io/skylink/skylinkjs/latest/doc/classes/Skylink.html#event_peerJoined)**:** informs you that a peer has joined the room and shares their peerID and peerInfo with you. In this example, we create a new video element for this peer and use the peerId to identify this element in the DOM of our website.

 1. SkylinkEventManager.addEventListener(SkylinkConstants.EVENTS.PEER_JOINED, (evt) => {
 2. const { isSelf, peerId, peerInfo } = evt.detail;
 3. if (isSelf) {
 4. return: // We already have a video and audio element for our video and audio and don't need to create a new one.
 5. }
 6. 
 7. 
 8. const vid = document.createElement('video');
 9. vid.autoplay = true;
10. vid.id = <code data-enlighter-language="generic" class="EnlighterJSRAW">${peerId_video}</code>;
11. document.body.appendChild(vid);
12. 
13. const aud = document.createElement('audio');
14. aud.autoplay = true;
15. aud.muted = true; // Added to avoid feedback when testing locally
16. aud.id = <code data-enlighter-language="generic" class="EnlighterJSRAW">${peerId_audio}</code>;
17. document.body.appendChild(aud);
18. });

### Listen on ‘incomingStream’ event with isSelf=true for self MediaStream

Skylink 2.x incoming stream will be either an audio stream or a video stream but not both.

[**incomingStream**](http://cdn.temasys.io/skylink/skylinkjs/latest/doc/classes/Skylink.html#event_incomingStream)**:** This event is fired after peerJoined when Temasys SkylinkJS begins to receive the audio and video streams from that peer. This peer could also be yourself (the local user) – in which case the event is fired when the user grants access to his microphone and/or camera and joins a room successfully. In this example, we use the _attachMediaStream()_ function of our enhanced [AdapterJS](http://github.com/Temasys/AdapterJS) library to feed this stream into our previously created video tag in Step 5. Why do we use this function? The different browser vendors have slightly different ways to do this and attachMediaStream() enables us to abstract this.

 1. SkylinkEventManager.addEventListener(SkylinkConstants.EVENTS.ON_INCOMING_STREAM, (evt) => {
 2. const { isSelf, stream, peerInfo, isVideo, isAudio } = evt.detail;
 3. if (isSelf) {
 4. return; // We already attached our local audio and video streams from the resolved promise in joinRoom.
 5. }
 6. 
 7. if(isAudio) {
 8. const aud = document.getElementById(<code data-enlighter-language="generic" class="EnlighterJSRAW">${peerId_audio}</code>);
 9. attachMediaStream(aud, stream);
10. }
11. if(isVideo) {
12. const vid = document.getElementById(<code data-enlighter-language="generic" class="EnlighterJSRAW">${peerId_audio}</code>);
13. attachMediaStream(vid, stream);
14. }
15. });

### Listen on ‘peerLeft’ event to handle peers leaving the room

[**peerLeft**](http://cdn.temasys.io/skylink/skylinkjs/latest/doc/classes/Skylink.html#method_peerLeft)**:** informs you that a peer has left the room. In our example, we look in the DOM for the video element with the events peerId and remove it. Subscribe to the peer joined event with the code below.

1. SkylinkEventManager.addEventListener(SkylinkConstants.EVENTS.PEER_LEFT, (evt) => {
2. const { isSelf, peerInfo, peerId } = evt.detail;
3. const videoElId = isSelf ? 'myvideo' : <code data-enlighter-language="generic" class="EnlighterJSRAW">${peerId_video}</code>
4. const audioElId = isSelf ? 'myaudio' : <code data-enlighter-language="generic" class="EnlighterJSRAW">${peerId_audio}</code>
5. const vid = document.getElementById(videoElId);
6. const audio = document.getElementById(audioElId);
7. document.body.removeChild(vid);
8. document.body.removeChild(aud);
9. });

### Unsubscribing to events

Unsubscribe to the peer joined event with the code below.

1. SkylinkEventManager.removeEventListener(SkylinkConstants.EVENTS.PEER_JOINED, peerJoinedHandler);

### Logging

Toggle console logging on and off with the setLevel method. For more logger options refer to: [SkylinkLogger](https://cdn.temasys.io/skylink/skylinkjs/latest/docs/SkylinkLogger.html)

1. SkylinkLogger.setLevel(SkylinkLogger.logLevels.DEBUG, storeLogs);

## 

## **Step 6: Get ready to impress!**

You’ve created a simple video conference app. Easy, right? Now explore all the ways that you can real-time interactions to your website. You can find an overview of all the methods and events Skylink offers (like lockRoom, disableAudio or disableVideo) in the [API documentation.](https://cdn.temasys.io/skylink/skylinkjs/latest/docs/index.html)

Here is another example Codepen that we’ve created that shows how you can create a very simple audio/video conference with JavaScript client-side code, with no additional server code required

See the Pen [WebRTC Audio/Video conference demo with Temasys SkylinkJS](https://codepen.io/temasys/pen/GogabE/) by Temasys ([@temasys](https://codepen.io/temasys)) on [CodePen](https://temasys.io/temasys-rtc-getting-started-web-sdk/www.codepen.io).

To further demonstrate the possibilities and flexibility of Temasys SkylinkJS, we have also created a more elaborate demo created with the help of [Facebook’s React](http://facebook.github.io/react/) at [http://getaroom.io.](http://getaroom.io. "http://getaroom.io.") Check it out, share it, and use it if you like it.

We hope you’ve enjoyed getting started with the Temasys Web SDK! Have fun, share this and let us know if you run into any [issues!](http://support.temasys.io/)