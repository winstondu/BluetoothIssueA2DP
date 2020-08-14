# Illustrating Bluetooth A2DP issue on iOS 13.6
AVAudioSession does not register connection of Bluetooth A2DP audio during playAndRecord audio session. [FBFB8371852]

## Overview
This project means to illustrate an issue to Apple's developers about Bluetooth A2DP connection. It has been filed as feedback item FBFB8371852.

**Description:**

When I change my AudioSession to .playAndRecord via the following call:
```
AVAudioSession.sharedInstance().setCategory(.playAndRecord, mode: .default, options: [.defaultToSpeaker, .allowBluetoothA2DP, .mixWithOthers])
```
From these category options set, I expect that, once I pair bluetooth headphones with my phone, my running app should automatically route audio to my bluetooth headphones. Instead, my app still chooses to use the phone speaker. There is no indication at all, whether from querying the audio session instance or in the form of a route change notification, that I have bluetooth headphones connected.

**Why it is likely a bug**  
This behavior clearly has nothing to do with an incorrect line of code. as if I had bluetooth headphones paired before the I invoke audio-session category change, my audio will correctly continue to use the bluetooth headphones.  

Moreover, if I had instead used the `.allowBluetooth` category option (for HFP instead of A2DP) during the code invocation, and then paired bluetooth headphones, the headphones connect automatically just fine.  

**Behavior Reproduced on:**  
iPhone XR (iOS 13.6.1) with Apple AirPods Pro  
iPhone XR (iOS 13.6) with Senso wireless 44.1kHz bluetooth headphones

## Reproduction Steps

Before you run the sample code project in Xcode:

* This sample code project requires iOS 13

Step 1: Make sure your bluetooth headphones are currently not paired with the iPhone.

Step 2: Launch this app (“BluetoothIssue”). When you toggle the “FX Out” or the “Speech Out” toggles, the app should be playing sounds (out of iPhone speaker).

Step 3: now connect your bluetooth headphones to the phone. Even though the headphones are now connected and paired with phone, the app does not register the connection, and continues play out of the phones built-in speakers.

// Steps 4- 5 (Optional): to see that nothing is wrong with the app’s category itself

Step 4: now force kill the “BluetoothIssue” app, while your headphones are still connected

Step 5: now relaunch the “BluetoothIssue” app. The app plays sound correctly out to the bluetooth headphones.

- Note: This project is a modification of the sample code project associated with with WWDC 2019 session [510: What's New in AVAudioEngine](https://developer.apple.com/videos/play/wwdc19/510/).
