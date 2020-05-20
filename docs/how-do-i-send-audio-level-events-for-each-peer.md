---
title: How do I send audio level events for each peer?
date: 2020-05-18
slug: audio-level

---
##   
How do I send audio level events for each peer?

***

Our SDK does not send audio level events. However, using the webAudio API you can easily access the audio streams to capture levels data and send this information to peers. A sample WebRTC application can be found here : [https://webrtc.github.io/samples/src/content/getusermedia/volume/](https://webrtc.github.io/samples/src/content/getusermedia/volume/ "https://webrtc.github.io/samples/src/content/getusermedia/volume/")

_Please note that the above is not a Skylink feature and we do not offer support for the above-mentioned demo._