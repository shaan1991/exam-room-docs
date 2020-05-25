---
title: Getting Started Guide for the Web
date: 2020-05-19
slug: sdk-for-web

---
## GETTING STARTED WITH THE SKYLINK SDK FOR WEB

## A step-by-step guide to embedding Real-Time Communication features into your webapp or website

This document is designed to help developers get started using the Skylink SDK for the Web to add video & voice calling, secure messaging, file sharing and screen sharing features to any website.

### Step 1: Create an Account on the Skylink Console and Generate an App Key

[Here are instructions for creating an account and generating an App Key.](https://skylink-docs.netlify.app/get-your-api-key/)

### Step 2: Include Skylink SkylinkJS code into your website

#### Include Video and Audio Elements in HTML file

    <html>
    
    <head>
    
    <title>WebRTC with Skylink SkylinkJS</title>
    
    <script><script>
    
    </head>
    
    <body>
    
    <video id="myvideo" style="transform: rotateY(-180deg);" autoplay muted</video>
    
    <audio id="myaudio" autoplay muted</audio>
    
    </body>
    
    </html>

The body contains a video tag to attach the video stream from the camera and an audio tag to attach the audio stream. We used a CSS transform call to mirror the image, so it looks and feels more natural. We muted the audio, so you don’t hear yourself speaking. The autoplay attribute is needed in some browsers where there are restrictions on autoplay. [https://developers.google.com/web/updates/2017/09/autoplay-policy-changes](https://developers.google.com/web/updates/2017/09/autoplay-policy-changes "https://developers.google.com/web/updates/2017/09/autoplay-policy-changes")

### Step 3: Import

#### Import from a path

Add to main.js

    import Skylink, { SkylinkEventManager, SkylinkLogger, SkylinkConstants } from './path/to/skylink.complete.js'

#### Include as a script tag with type as Module

Add to main.js

    <script src="./path/to/skylink.complete.js" type="module"></script>

### Step 4: Initialise and then listen for events

#### Instantiate Skylink and init with config

For appKey: ‘XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX’, use the appKey you generated in the Skylink Console.

Full list of init configuration options here: [initOptions](http://cdn.temasys.io/skylink/skylinkjs/2.x/docs/global.html#initOptions)

    const config = {
    
    appKey: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX',
    
    defaultRoom: 'skylinkRoom',
    
    enableIceTrickle: true,
    
    enableDataChannel: true,
    
    forceSSL: true,
    
    };
    
    skylink = new Skylink(config);

#### Declare joinRoomOptions and pass as argument in joinRoom method. MediaStreams can be obtained from the resolved Promise

Configure by setting audio: true and video: true to access camera and microphone.

Full list of joinRoom config options here: [joinRoomOptions](http://cdn.temasys.io/skylink/skylinkjs/2.x/docs/global.html#joinRoomOptions)

    const joinRoomOptions = {
    
    audio: { stereo: true },
    
    video: true,
    
    };
    
    skylink.joinRoom(joinRoomOptions)
    
    .then((streams) => {
    
    // if there is an audio stream
    
    if (streams[0]) {
    
    window.attachMediaStream(audioEl, streams[0]);
    
    }
    
    // if there is a video stream
    
    if (streams[1]) {
    
    window.attachMediaStream(videoEl, streams[1]);
    
    }
    
    })
    
    .catch();

### Step 5:Subscribing and Unsubscribing to an event

#### Listen on ‘peerJoined’ event with isSelf=true for self join room success outcome

[**peerJoined**](http://cdn.temasys.io/skylink/skylinkjs/latest/doc/classes/Skylink.html#event_peerJoined)**:** informs you that a peer has joined the room and shares their peerID and peerInfo with you. In this example, we create a new video element for this peer and use the peerId to identify this element in the DOM of our website.

    SkylinkEventManager.addEventListener(SkylinkConstants.EVENTS.PEER_JOINED, (evt) => {
    
    const { isSelf, peerId, peerInfo } = evt.detail;
    
    if (isSelf) {
    
    return: // We already have a video and audio element for our video and audio and don't need to create a new one.
    
    }
    
    const vid = document.createElement('video');
    
    vid.autoplay = true;
    
    vid.id = <code data-enlighter-language="generic" class="EnlighterJSRAW" style="display: none;">${peerId_video}</code><span class="wpcustomEnlighterJS EnlighterJS"><span class="">$</span><span class="br0">{</span><span class="">peerId_video</span><span class="br0">}</span></span>;
    
    document.body.appendChild(vid);
    
    const aud = document.createElement('audio');
    
    aud.autoplay = true;
    
    aud.muted = true; // Added to avoid feedback when testing locally
    
    aud.id = <code data-enlighter-language="generic" class="EnlighterJSRAW" style="display: none;">${peerId_audio}</code><span class="wpcustomEnlighterJS EnlighterJS"><span class="">$</span><span class="br0">{</span><span class="">peerId_audio</span><span class="br0">}</span></span>;
    
    document.body.appendChild(aud);
    
    });

#### Listen on ‘incomingStream’ event with isSelf=true for self MediaStream

Skylink 2.x incoming stream will be either an audio stream or a video stream but not both.

[**incomingStream**](http://cdn.temasys.io/skylink/skylinkjs/latest/doc/classes/Skylink.html#event_incomingStream)**:** This event is fired after peerJoined when Skylink SkylinkJS begins to receive the audio and video streams from that peer. This peer could also be yourself (the local user) – in which case the event is fired when the user grants access to his microphone and/or camera and joins a room successfully. In this example, we use the _attachMediaStream()_ function of our enhanced [AdapterJS](http://github.com/Temasys/AdapterJS) library to feed this stream into our previously created video tag in Step 5. Why do we use this function? The different browser vendors have slightly different ways to do this and attachMediaStream() enables us to abstract this.

    SkylinkEventManager.addEventListener(SkylinkConstants.EVENTS.ON_INCOMING_STREAM, (evt) => {
    
    const { isSelf, stream, peerInfo, isVideo, isAudio } = evt.detail;
    
    if (isSelf) {
    
    return; // We already attached our local audio and video streams from the resolved promise in joinRoom.
    
    }
    
    if(isAudio) {
    
    const aud = document.getElementById(<code data-enlighter-language="generic" class="EnlighterJSRAW" style="display: none;">${peerId_audio}</code><span class="wpcustomEnlighterJS EnlighterJS"><span class="">$</span><span class="br0">{</span><span class="">peerId_audio</span><span class="br0">}</span></span>);
    
    attachMediaStream(aud, stream);
    
    }
    
    if(isVideo) {
    
    const vid = document.getElementById(<code data-enlighter-language="generic" class="EnlighterJSRAW" style="display: none;">${peerId_audio}</code><span class="wpcustomEnlighterJS EnlighterJS"><span class="">$</span><span class="br0">{</span><span class="">peerId_audio</span><span class="br0">}</span></span>);
    
    attachMediaStream(vid, stream);
    
    }
    
    });

#### Listen on ‘peerLeft’ event to handle peers leaving the room

[**peerLeft**](http://cdn.temasys.io/skylink/skylinkjs/latest/doc/classes/Skylink.html#method_peerLeft)**:** informs you that a peer has left the room. In our example, we look in the DOM for the video element with the events peerId and remove it. Subscribe to the peer joined event with the code below.

    SkylinkEventManager.addEventListener(SkylinkConstants.EVENTS.PEER_LEFT, (evt) => {
    
    const { isSelf, peerInfo, peerId } = evt.detail;
    
    const videoElId = isSelf ? 'myvideo' : <code data-enlighter-language="generic" class="EnlighterJSRAW" style="display: none;">${peerId_video}</code><span class="wpcustomEnlighterJS EnlighterJS"><span class="">$</span><span class="br0">{</span><span class="">peerId_video</span><span class="br0">}</span></span>
    
    const audioElId = isSelf ? 'myaudio' : <code data-enlighter-language="generic" class="EnlighterJSRAW" style="display: none;">${peerId_audio}</code><span class="wpcustomEnlighterJS EnlighterJS"><span class="">$</span><span class="br0">{</span><span class="">peerId_audio</span><span class="br0">}</span></span>
    
    const vid = document.getElementById(videoElId);
    
    const audio = document.getElementById(audioElId);
    
    document.body.removeChild(vid);
    
    document.body.removeChild(aud);
    
    });SkylinkEventManager.addEventListener(SkylinkConstants.EVENTS.PEER_LEFT, (evt) => {
    
    const { isSelf, peerInfo, peerId } = evt.detail;
    
    const videoElId = isSelf ? 'myvideo' : <code data-enlighter-language="generic" class="EnlighterJSRAW" style="display: none;">${peerId_video}</code><span class="wpcustomEnlighterJS EnlighterJS"><span class="">$</span><span class="br0">{</span><span class="">peerId_video</span><span class="br0">}</span></span>
    
    const audioElId = isSelf ? 'myaudio' : <code data-enlighter-language="generic" class="EnlighterJSRAW" style="display: none;">${peerId_audio}</code><span class="wpcustomEnlighterJS EnlighterJS"><span class="">$</span><span class="br0">{</span><span class="">peerId_audio</span><span class="br0">}</span></span>
    
    const vid = document.getElementById(videoElId);
    
    const audio = document.getElementById(audioElId);
    
    document.body.removeChild(vid);
    
    document.body.removeChild(aud);
    
    });

#### Unsubscribing to events

Unsubscribe to the peer joined event with the code below.

    SkylinkEventManager.removeEventListener(SkylinkConstants.EVENTS.PEER_JOINED, peerJoinedHandler);

#### Logging

Toggle console logging on and off with the setLevel method. For more logger options refer to: [SkylinkLogger](https://cdn.temasys.io/skylink/skylinkjs/latest/docs/SkylinkLogger.html)

    SkylinkLogger.setLevel(SkylinkLogger.logLevels.DEBUG, storeLogs);

### Step 6: Get ready to impress!

You’ve created a simple video conference app. Easy, right? Now explore all the ways that you can real-time interactions to your website. You can find an overview of all the methods and events Skylink offers (like lockRoom, disableAudio or disableVideo) in the [API documentation.](https://cdn.temasys.io/skylink/skylinkjs/latest/docs/index.html)

Here is another example Codepen that we’ve created that shows how you can create a very simple audio/video conference with JavaScript client-side code, with no additional server code required

See the Pen [WebRTC Audio/Video conference demo with SkylinkJS](https://codepen.io/temasys/pen/GogabE/) by Skylink ([@skylink](https://codepen.io/temasys)) on CodePen.

To further demonstrate the possibilities and flexibility of SkylinkJS, we have also created a more elaborate demo created with the help of [Facebook’s React](http://facebook.github.io/react/) at [http://getaroom.io.](http://getaroom.io. "http://getaroom.io.") Check it out, share it, and use it if you like it.

We hope you’ve enjoyed getting started with the Skylink Web SDK! Have fun, share this and let us know if you run into any [issues!](home.html)