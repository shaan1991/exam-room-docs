---
title: Getting Started Guide for Android
date: 2020-05-20
slug: start-sdk-for-android

---
## GETTING STARTED WITH THE SKYLINK SDK FOR ANDROID

## A step-by-step guide to embedding Real-Time Communication features into your Android application

Fully encrypted, peer-to-peer and multiparty real-time communications features inside your app, in just a few lines of code? YES! Check it out!

This document is designed to help you start using the Skylink SDK for Android. This SDK supports features like video & voice calling, secure messaging, and more INSIDE any Android application.

### Step 1: Create an Account on the Skylink Console and Generate an App Key

[Here are instructions for creating an account and generating an App Key.](https://qdex-hnn1dcqna.instant.forestry.io/get-your-api-key)

### Step 2: Begin Your Project by Performing the Following Substeps

1. **Add the Android SDK to your project**
   * [Review this document to setup Android Studio](https://cdn.temasys.io/skylink/skylinksdk/android/latest/SkylinkSDK_Android_Studio_Setup.md)
   * Note the [Android SDK version required](https://cdn.temasys.io/skylink/skylinksdk/android/latest/Android_SDK_Version_Required.md) for the Skylink SDK for Android
2. **Implement Listeners** in the class receiving events from the Skylink SDK.

   **Note:** A list of listeners available in the current release and the callbacks they provide can be found [here](http://cdn.temasys.io/skylink/skylinksdk/android/latest/doc/reference/packages.html). For older versions just replace “latest” in the URL with the SDK version.
3. **Always implement LifeCycleListener and RemotePeerListener.** The “listeners” are as follows:
   1. Audio Call : Implement `MediaListener`
   2. Video Call : Implement `MediaListener`
   3. File Transfer : Implement `FileTransferListener`
   4. Data Transfer : Implement `DataTransferListener`
   5. Messaging : Implement `MessageListener`
   6. Recording : Implement `RecordingListener`

      public class VideoCallFragment extends Fragment implements

      LifeCycleListener, MediaListener, RemotePeerListener{

      /Implementation of callbacks provided the listeners/

      ... .....

      }
4. **Initialize SkylinkConnection** object. You can initialize the SkylinkConnection object by providing the App key and secret obtained from the Skylink Console and the config object (obtained in step 3). This registers your application key with the Skylink server.

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
5. **Connect to a room** using the Skylink SDK  
   A room is where two peers can join and interact with each other. The roomName can be any alpha-numeric value. The next example shows that two peers will enter a room named “MyRoom”, where they can communicate with one another:

       // you will be connected to the room named "roomName" using a user name or user data object.
       
       skylinkConnection.connectToRoom("secret", "roomName", "userName");

   **Note** : The Skylink SDK also provides a more secure credentials-based method for connecting to a room. [Click here for information on generating credentials from your application server ](http://support.temasys.com.sg/support/solutions/articles/5000644837-how-do-i-connect-to-a-room-using-credentials-). We strongly recommend using the credentials method for production applications.
6. **Verify** connectivity by implementing debug logging in the callbacks of LifeCycleListener

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

We hope this helps you get started with the Skylink SDK for Android! Have fun and please [contact us](https://temasys.io/contact-us/) if you need any assistance!

### ADDITIONAL RESOURCES

[Getting Started Guide for the Web](https://qdex-hnn1dcqna.instant.forestry.io/start-sdk-for-web)

[Getting Started Guide for iOS](https://qdex-hnn1dcqna.instant.forestry.io/start-sdk-for-ios)