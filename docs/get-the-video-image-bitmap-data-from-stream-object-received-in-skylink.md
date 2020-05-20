---
title: Get the video image bitmap data from Stream object received in Skylink
date: 2020-05-19
slug: 'video-image-bitmap '

---
##   
How can I get the video image bitmap data from Stream object received in Skylink

***

To obtain the bitmap data of the Stream object received in Events like incomingStream, mediaAccessSuccess, getUserMedia() _callback success_ and shareScreen() _callback success_.

###### 1. Attach the Stream object to a <video> element using attachMediaStream function.

    skylink.on('mediaAccessSuccess', function (stream) {
       var video = document.createElement('video');
       video.autoplay = true;
    
       attachMediaStream(video, stream);
    });

###### 2. After attaching the Stream object to a <video> element. You may attach the <video> element to a new <canvas> element.

Guide and documentation are available here: [https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Manipulating_video_using_canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Manipulating_video_using_canvas "https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Manipulating_video_using_canvas").

###### 3. After you have the <canvas> element that syncs with the <canvas> element, you can obtain the bitmap data with getImageData().

Guide and documentation are available here: [https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Pixel_manipulation_with_canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Pixel_manipulation_with_canvas "https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Pixel_manipulation_with_canvas").