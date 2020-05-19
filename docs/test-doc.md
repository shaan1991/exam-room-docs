---
title: Getting started with android
date: 2020-05-17
slug: test-doc

---
## Getting Started with the Temasys SDK for Android

### **A step-by-step guide to embedding Real-Time Communication features into your Android application**

Fully encrypted, peer-to-peer and multiparty real-time communications features inside your app, in just a few lines of code? YES! Check it out!

This document is designed to help you start using the Temasys SDK for Android. This SDK supports features like video & voice calling, secure messaging, and more INSIDE any Android application.

## **Step 1: Create an Account on the Temasys Console and Generate an App Key**

[Here are instructions for creating an account and generating an App Key.](https://temasys.io/creating-an-account-generating-a-key/)

## **Step 2: Begin Your Project by Performing the Following Substeps**

1. **Add the Android SDK to your project**
   * [Review this document to setup Android Studio](https://cdn.temasys.io/skylink/skylinksdk/android/latest/SkylinkSDK_Android_Studio_Setup.md)
   * Note the [Android SDK version required](https://cdn.temasys.io/skylink/skylinksdk/android/latest/Android_SDK_Version_Required.md) for the Temasys SDK for Android
   1. **Implement Listeners** in the class receiving events from the Temasys SDK.**  
      Note:** A list of listeners available in the current release and the callbacks they provide can be found [here](http://cdn.temasys.io/skylink/skylinksdk/android/latest/doc/reference/packages.html). For older versions just replace “latest” in the URL with the SDK version.
   2. **Always implement LifeCycleListener and RemotePeerListener.** The “listeners” are as follows:
      1. Audio Call : Implement MediaListener
         1. Video Call : Implement MediaListener
         2. File Transfer : Implement FileTransferListener
         3. Data Transfer : Implement DataTransferListener
         4. Messaging : Implement MessageListener
         5. Recording : Implement RecordingListener


      1. public class VideoCallFragment extends Fragment implements
      2. LifeCycleListener, MediaListener, RemotePeerListener{
      3. 
      4. /**Implementation of callbacks provided the listeners**/
      5. ... .....
      6. }
   3. **Initialize SkylinkConfig** to specify the desired functionality from the Temasys SDK
      1. private SkylinkConfig getSkylinkConfig() {
      2. SkylinkConfig config = new SkylinkConfig();
      3. config.setAudioVideoSendConfig(SkylinkConfig.AudioVideoConfig.AUDIO_AND_VIDEO);
      4. config.setAudioVideoReceiveConfig(SkylinkConfig.AudioVideoConfig.AUDIO_AND_VIDEO);
      5. config.setHasPeerMessaging(true); config.setHasFileTransfer(true);
      6. config.setHasDataTransfer(true);
      7. config.setTimeout(60);
      8. return config;
      9. }

      | --- |
      | There are four configurations available for AudioVideoConfigSkylinkConfig.AudioVideoConfig.AUDIO_AND_VIDEOSkylinkConfig.AudioVideoConfig.NO_AUDIO_NO_VIDEOSkylinkConfig.AudioVideoConfig.AUDIO_ONLYSkylinkConfig.AudioVideoConfig.VIDEO_ONLY |

      ###### 
   4. **Initialize SkylinkConnection** object. You can initialize the SkylinkConnection object by providing the App key and secret obtained from the Temasys Console and the config object (obtained in step 3). This registers your application key with the Temasys server.
       1. SkylinkConnection skylinkConnection;
       2. .....
       3. .....
       4. public void onCreate(Bundle savedInstanceState) {
       5. super.onCreate();
       6. skylinkConnection = SkylinkConnection.getInstance();
       7. skylinkConnection.init(getString(R.string.app_key), getSkylinkConfig(),
       8. this.getActivity().getApplicationContext());
       9. 
      10. // register respective listeners
      11. 
      12. skylinkConnection.setLifeCycleListener(this);
      13. skylinkConnection.setMediaListener(this);
      14. skylinkConnection.setRemotePeerListener(this);
      15. .........
      16. .........
      17. }
   5. **Connect to a room** using the Temasys SDK  
      A room is where two peers can join and interact with each other. The roomName can be any alpha-numeric value. The next example shows that two peers will enter a room named “MyRoom”, where they can communicate with one another:
      1. // you will be connected to the room named "roomName" using a user name or user data object.
      2. skylinkConnection.connectToRoom("secret", "roomName", "userName");

      **Note**: The Temasys SDK also provides a more secure credentials-based method for connecting to a room. [Click here for information on generating credentials from your application server](http://support.temasys.com.sg/support/solutions/articles/5000644837-how-do-i-connect-to-a-room-using-credentials-). We strongly recommend using the credentials method for production applications.
   6. **Verify** connectivity by implementing debug logging in the callbacks of LifeCycleListener
       1. /***
       2. * Lifecycle Listener Callbacks -- triggered during events that happen during the SDK's lifecycle
       3. *//**
       4. * Triggered when connection is successful
       5. *
       6. * @param isSuccess
       7. * @param message
       8. */
       9. @Override
      10. public void onConnect(boolean isSuccess, String message) {
      11. if (isSuccess) {
      12. Toast.makeText(getActivity(), "Connected to room ").show();
      13. } else {
      14. Log.d(TAG, "Skylink Connection Failed");
      15. }
      16. }
      17. 
      18. @Override
      19. public void onWarning(int errorCode, String message) {
      20. String log = "Warning is errorCode " + errorCode + ". Message: " + message + ".";
      21. Log.d(TAG, log);
      22. }
      23. 
      24. @Override
      25. public void onDisconnect(int errorCode, String message) {
      26. String log = "";
      27. if (errorCode == Errors.DISCONNECT_FROM_ROOM) {
      28. log += "We have successfully disconnected from the room.";
      29. } else if (errorCode == Errors.DISCONNECT_UNEXPECTED_ERROR) {
      30. log += "WARNING! We have been unexpectedly disconnected from the room!";
      31. }
      32. log += " Server message: " + message;
      33. Log.d(TAG, log);
      34. }
      35. 
      36. @Override
      37. public void onReceiveLog(int infoCode, String message) {
      38. switch (infoCode) {
      39. case CAM_SWITCH_FRONT:
      40. case CAM_SWITCH_NON_FRONT:
      41. case CAM_SWITCH_NO:
      42. Toast.makeText(getActivity(), message, Toast.LENGTH_SHORT).show();
      43. break;
      44. default:
      45. Log.d(TAG, "Received SDK log: " + message);
      46. break;
      47. }
      48. }

We hope this helps you get started with the Temasys SDK for Android! Have fun and please [contact us](https://temasys.io/contact-us/) if you need any assistance!

### **ADDITIONAL RESOURCES**

To further demonstrate the possibilities and flexibility of the Temasys SDK for Android, please refer to our [simple demo application](https://github.com/Temasys/skylink-android-sample).

[Temasys Developer Console](https://console.temasys.io/)

[Getting Started Guide for the Web](https://temasys.io/temasys-rtc-getting-started-web-sdk/)

[Getting Started Guide for iOS](https://temasys.io/temasys-rtc-getting-started-ios-sdk/)