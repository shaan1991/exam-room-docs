---
title: Get started with using SkylinkSDK for Android
date: 2020-05-18
slug: get-started-android

---
# How do I get started with using SkylinkSDK for Android?

***

### Here is a step-by-step walkthrough that helps you get started with using SkylinkSDK for Android

## 1. **Add** 

the SkylinkSDK to your project. Please view the following guide on how to add the SDK to your project.

* [Get started with SkylinkSDK using Android Studio](http://support.temasys.com.sg/solution/categories/5000159224/folders/5000255584/articles/5000650923-using-skylinksdk-for-android-with-android-studio)

## 2. **Register** 

at [Temasys Console](https://console.temasys.io/) to receive your application key and secret.

## 3. **Implement Listeners** 

in the class receiving events from the SkylinkSDK

* **Note:** A list of listeners available in the current release and the callbacks they provide can be found [here](http://cdn.temasys.com.sg/skylink/skylinksdk/android/dev/latest/doc/index.html), for older versions replace "latest" in the URL with the SDK version.

| --- |
| LifeCycleListener and RemotePeerListener are always required.Listener requirements for additional functionality are outlined below For audio support: Implement MediaListener For video support: Implement MediaListener For file transfer: Implement FileTransferListenerFor messaging: Implement MessageListener |

Java

    public class VideoCallFragment extends Fragment implements LifeCycleListener, MediaListener,    RemotePeerListener {
    /**Implementation of callbacks provided the listeners**/
    .....
    .....
    }

## 4**. Initialize SkylinkConfig** 

specifying desired functionality from the Skylink SDK

Java

    private SkylinkConfig getSkylinkConfig() {
    SkylinkConfig config = new SkylinkConfig();
    
    config.setAudioVideoSendConfig(SkylinkConfig.AudioVideoConfig.AUDIO_AND_VIDEO);
    config.setHasPeerMessaging(true);
    config.setHasFileTransfer(true);
    config.setTimeout(60);
    
    return config;
    }

| --- |
| There are four configurations available for AudioVideoConfigSkylinkConfig.AudioVideoConfig.AUDIO_AND_VIDEO SkylinkConfig.AudioVideoConfig.NO_AUDIO_NO_VIDEOSkylinkConfig.AudioVideoConfig.AUDIO_ONLYSkylinkConfig.AudioVideoConfig.VIDEO_ONLY |

## 5. **Initialize SkylinkConnection** 

object, providing the AppKey and secret obtained from the Temasys console (step 2) and config object from step 4.  
 Java

    SkylinkConnection skylinkConnection;
    .....
    .....
    public void onCreate(Bundle savedInstanceState) {
    super.onCreate(); 
    skylinkConnection = SkylinkConnection.getInstance();
    skylinkConnection.init(getString(R.string.app_key),
    getString(R.string.app_secret), getSkylinkConfig(), this.getActivity().getApplicationContext());
    
    //register respective listeners
    skylinkConnection.setLifeCycleListener(this);
    skylinkConnection.setMediaListener(this);
    skylinkConnection.setRemotePeerListener(this);
    .....
    .....
    }

## 6. **Connect to a room** 

using SkylinkSDK

    //you will be connected to the room named "MyRoom" using the name "userDisplayName"
    skylinkConnection.connectToRoom("MyRoom", "userDisplayName"); 

**Note:** The SkylinkSDK also provides a more secure credentials method for connecting to rooms.  
Click [here](http://support.temasys.com.sg/solution/articles/5000644837-how-do-i-connect-to-a-room-using-credentials-) for information on generating credentials from your application server.  
The credentials method is recommended for production applications.

## 7. **Verify** 

connectivity by implementing debug logging in the callbacks of LifeCycleListener Java

    /***
    * Lifecycle Listener Callbacks -- triggered during events that happen during the SDK's lifecycle
    */
    
    /**
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
    Log.d(TAG, message + "warning");
    }
    
    @Override
    public void onDisconnect(int errorCode, String message) {
    Log.d(TAG, message + " SkylinkConnection has been disconnected");
    }
    
    @Override
    public void onReceiveLog(String message) {
    Log.d(TAG, message + " on receive log");
    }

  
For more information on the SDK usage, please refer to our [simple demo application](https://github.com/Temasys/skylink-android-sample).