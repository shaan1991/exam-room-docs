---
title: Getting Started Guide for IOS
date: 2020-05-19
slug: start-sdk-for-ios

---
## GETTING STARTED WITH THE SKYLINK SDK FOR IOS

## A step-by-step guide to embedding Real-Time Communication features into your iOS application

This document is designed to help developers get started using the Skylink SDK for iOS to add video & voice calling, secure messaging, and more to their iOS application. Let’s get started!

### Step 1: Create an Account on the Skylink Console and Generate an App Key

[Here are instructions for creating an account and generating an App Key.](https://qdex-hnn1dcqna.instant.forestry.io/get-your-api-key)

### Step 2: Set up a new Xcode project workspace

These are the simple steps to set up an empty Xcode project.

We recommend you install the SDK via _Cocoapods_. If you don’t have _Cocoapods_ installed, please follow these steps:

* Check that you have Xcode command line tools installed (**Xcode** > **Preferences** > **Locations** > **Command line tools**).
* If not, open the terminal and run **-xcode-select –install** . You can find more details [here](http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/) if needed:
* Install cocoa pods in the terminal: $ **sudo gem install cocoapods.** Cocoapods website: cocoapods.org. Create new Xcode project
* Close _Xcode_ (._xcodeproj_ file)

### Step 3: Install the Skylink SDK for iOS

* Run `pod init` in the terminal, where the ._xcodeprojfile_ file is located. This will create the podfile.
* Add `pod "SKYLINK"` to your pod file, for the appropriate(s) target(s).
* Run `pod install`. Wait for the terminal to output a message that will read something like “Pod installation complete!”
* You will see that there is 1 dependency from the Podfile, and 3 total pods installed.

### Step 4: Configure Settings

* Open the ._xcworkspace_ file and always work with this from now on (instead of the .xcodeproj file).
* For each target where you plan to use the Skylink SDK, go to **Build settings** (make sure “all” is selected) > **Build** **Options** > **Enable bit code** and set it to _NO_. This will avoid the “_…does not contain_ bitcode” message.
* If you get the error “The resource could not be loaded because the App Transport Security policy requires the use of a secure connection”, edit your `info.plist` by adding an `NSAppTransportSecurity` key as Dictionary. Then, add a sub-key named NSAllowsArbitraryLoads as boolean set to YES.
* Optionally, if you want your app to be able to process audio even when the users leaves the app or locks the device, just enable the VoIP background capability or the audio background capability in the target’s “capabilities” tab.

### Step 5: Initializing the Skylink SDK for iOS

The main idea is to prepare and create a connection to a room via the Skylink platform. After that, you will be able to send messages to the connection and implement the desired protocols to control what happens between the local device and the peers.

Skylink SDK for iOS provides 3 classes:

* **SKYLINKConnection** – the main class, the one you will be sending messages to
* **SKYLINKConnectionConfig** – to configure the SKYLINKConnection instance before you connect to a room
* **SKYLINKPeerMediaProperties** – used in delegates methods to get more information about the peers

Here is an example connection code:

    // Creating configuration
    
    SKYLINKConnectionConfig *config = [SKYLINKConnectionConfig new];
    
    config.video = YES;
    
    config.audio = YES;
    
    // Creating SKYLINKConnection
    
    self.skylinkConnection = [[SKYLINKConnection alloc] initWithConfig:config appKey:self.skylinkApiKey];
    
    self.skylinkConnection.lifeCycleDelegate = self;
    
    self.skylinkConnection.mediaDelegate = self;
    
    self.skylinkConnection.remotePeerDelegate = self;
    
    // Connecting to a room
    
    [self.skylinkConnection connectToRoomWithSecret:self.skylinkApiSecret roomName:ROOM_NAME userInfo:nil];

### Step 6: Implement Protocols

The common next step is to implement protocols. The Skylink SDK for iOS provides 6 protocols:

* `SKYLINKConnectionLifeCycleDelegate`
* `SKYLINKConnectionRemotePeerDelegate`
* `SKYLINKConnectionMediaDelegate`
* `SKYLINKConnectionMessagesDelegate`
* `SKYLINKConnectionFileTransferDelegate`
* `SKYLINKConnectionRecordingDelegate`

The `SKYLINKConnectionLifeCycleDelegate` and the `SKYLINKConnectionRemotePeerDelegate` are the most important. These will give you general details about the lifecycle and the peers that are joining and leaving your application. It is advised to implement them for all applications.

**_MediaDelegates_** need to be implemented if you are using the Skylink SDK for simply adding video/voice communication capabilities. Similarly, **_MessagesDelegate_** and **_FileTransferDelegate_** can be implemented based on whether you require messaging and data transfer capabilities for your application. You get the idea!

That’s about it. We can’t wait to see what you’ll create with Skylink technology!