---
title: GETTING STARTED WITH THE TEMASYS SDK FOR ANDROID
date: 2020-05-21
slug: temasys-sdk-for-android

---
## GETTING STARTED WITH THE TEMASYS SDK FOR ANDROID

## A step-by-step guide to embedding Real-Time Communication features into your Android application

Fully encrypted, peer-to-peer and multiparty real-time communications features inside your app, in just a few lines of code? YES! Check it out!

This document is designed to help you start using the Temasys SDK for Android. This SDK supports features like video & voice calling, secure messaging, and more INSIDE any Android application.

### Step 1: Create an Account on the Temasys Console and Generate an App Key

[Here are instructions for creating an account and generating an App Key.](creating-an-account-generating-a-key.html)

### Step 2: Begin Your Project by Performing the Following Substeps

1. **=Add the Android SDK to your project**
   * [Review this document to setup Android Studio](https://cdn.temasys.io/skylink/skylinksdk/android/latest/SkylinkSDK_Android_Studio_Setup.md)
   * Note the [Android SDK version required](https://cdn.temasys.io/skylink/skylinksdk/android/latest/Android_SDK_Version_Required.md) for the Temasys SDK for Android
2. **Implement Listeners** in the class receiving events from the Temasys SDK.

   **Note:** A list of listeners available in the current release and the callbacks they provide can be found [here](http://cdn.temasys.io/skylink/skylinksdk/android/latest/doc/reference/packages.html). For older versions just replace “latest” in the URL with the SDK version.
3. **Always implement LifeCycleListener and RemotePeerListener.** The “listeners” are as follows:
   1. Audio Call : Implement `MediaListener`
      1. Video Call : Implement `MediaListener`
      2. File Transfer : Implement `FileTransferListener`
      3. Data Transfer : Implement `DataTransferListener`
      4. Messaging : Implement `MessageListener`
      5. Recording : Implement `RecordingListener`

       public class VideoCallFragment extends Fragment implements
       
       LifeCycleListener, MediaListener, RemotePeerListener{
       
       /**Implementation of callbacks provided the listeners**/
       
       ... .....
       
       }
4. **Initialize SkylinkConfig** to specify the desired functionality from the Temasys SDK

       private SkylinkConfig getSkylinkConfig() {
       
       SkylinkConfig config = new SkylinkConfig();
       
       config.setAudioVideoSendConfig(SkylinkConfig.AudioVideoConfig.AUDIO_AND_VIDEO);
       
       config.setAudioVideoReceiveConfig(SkylinkConfig.AudioVideoConfig.AUDIO_AND_VIDEO);
       
       config.setHasPeerMessaging(true); config.setHasFileTransfer(true);
       
       config.setHasDataTransfer(true);
       
       config.setTimeout(60);
       
       return config;
       
       }

   **There are four configurations available for AudioVideoConfig**
   * `SkylinkConfig.AudioVideoConfig.AUDIO_AND_VIDEO`
   * `SkylinkConfig.AudioVideoConfig.NO_AUDIO_NO_VIDEO`
   * `SkylinkConfig.AudioVideoConfig.AUDIO_ONLY`
   * `SkylinkConfig.AudioVideoConfig.VIDEO_ONLY`
5. **Initialize SkylinkConnection** object. You can initialize the SkylinkConnection object by providing the App key and secret obtained from the Temasys Console and the config object (obtained in step 3). This registers your application key with the Temasys server.

       SkylinkConnection skylinkConnection;
       
       .....
       
       .....
       
       public void onCreate(Bundle savedInstanceState) {
       
       super.onCreate();
       
       skylinkConnection = SkylinkConnection.getInstance();
       
       skylinkConnection.init(getString(R.string.app_key), getSkylinkConfig(),
       
       this.getActivity().getApplicationContext());
       
       // register respective listeners
       
       skylinkConnection.setLifeCycleListener(this);
       
       skylinkConnection.setMediaListener(this);
       
       skylinkConnection.setRemotePeerListener(this);
       
       .........
       
       .........
       
       }
6. **Connect to a room** using the Temasys SDK  
   A room is where two peers can join and interact with each other. The roomName can be any alpha-numeric value. The next example shows that two peers will enter a room named “MyRoom”, where they can communicate with one another:

       // you will be connected to the room named "roomName" using a user name or user data object.
       
       skylinkConnection.connectToRoom("secret", "roomName", "userName");

   **Note** : The Temasys SDK also provides a more secure credentials-based method for connecting to a room. [Click here for information on generating credentials from your application server ](http://support.temasys.com.sg/support/solutions/articles/5000644837-how-do-i-connect-to-a-room-using-credentials-). We strongly recommend using the credentials method for production applications.
7. **Verify** connectivity by implementing debug logging in the callbacks of LifeCycleListener

       /***
       * Lifecycle Listener Callbacks -- triggered during events that happen during the SDK's lifecycle
       *//**
       * Triggered when connection is successful
       *
       * @param isSuccess
       * @param message
       */
       @Override
       public void onConnect(boolean isSuccess, String message) {
       if (isSuccess) {
       Toast.makeText(getActivity(), "Connected to room ").show();
       } else {
       Log.d(TAG, "Skylink Connection Failed");
       }
       }
       
       @Override
       public void onWarning(int errorCode, String message) {
       String log = "Warning is errorCode " + errorCode + ". Message: " + message + ".";
       Log.d(TAG, log);
       }
       
       @Override
       public void onDisconnect(int errorCode, String message) {
       String log = "";
       if (errorCode == Errors.DISCONNECT_FROM_ROOM) {
       log += "We have successfully disconnected from the room.";
       } else if (errorCode == Errors.DISCONNECT_UNEXPECTED_ERROR) {
       log += "WARNING! We have been unexpectedly disconnected from the room!";
       }
       log += " Server message: " + message;
       Log.d(TAG, log);
       }
       
       @Override
       public void onReceiveLog(int infoCode, String message) {
       switch (infoCode) {
       case CAM_SWITCH_FRONT:
       case CAM_SWITCH_NON_FRONT:
       case CAM_SWITCH_NO:
       Toast.makeText(getActivity(), message, Toast.LENGTH_SHORT).show();
       break;
       default:
       Log.d(TAG, "Received SDK log: " + message);
       break;
       }
       }

We hope this helps you get started with the Temasys SDK for Android! Have fun and please [contact us](https://temasys.io/contact-us/) if you need any assistance!

### ADDITIONAL RESOURCES

To further demonstrate the possibilities and flexibility of the Temasys SDK for Android, please refer to our [simple demo application](https://github.com/Temasys/skylink-android-sample)

[Temasys Developer Console](https://console.temasys.io/)

[Getting Started Guide for the Web](temasys-rtc-getting-started-web-sdk.html)

[Getting Started Guide for iOS](temasys-rtc-getting-started-ios-sdk.html)