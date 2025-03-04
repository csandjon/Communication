# ACS Calling Web (JavaScript) SDK - Release History
- [Sample Applications](https://docs.microsoft.com/azure/communication-services/samples/overview)
- [API usage documentation](https://docs.microsoft.com/en-us/azure/communication-services/quickstarts/voice-video-calling/calling-client-samples?pivots=platform-web)
- [API reference documentation](https://docs.microsoft.com/en-us/javascript/api/azure-communication-services/@azure/communication-calling/?view=azure-communication-services-js)

## v1.6.0-beta.1 (2022-07-5)
Available in NPM - https://www.npmjs.com/package/@azure/communication-calling/v/1.6.0-beta.1

Features

- Audio media access enables application developers to access the incoming call audio stream and send custom outgoing audio stream during the call. 
	* Incoming audio stream, can be accessed right on the call object.

		```js
		call.remoteAudioStreams;
		```
	* Outgoing audio stream, application can create custom [mediaStreamTrack](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrack) and set it as outgoing source stream when in a call.

		```js
		const createAudioTrackToSend = () => {
			...
		};

		const localAudioStream = new LocalAudioStream(createAudioTrackToSend());
		call.startAudio(localAudioStream);
		```
	
- Mute incoming audio feature will help to mute / unmute the incoming audio. So that, the speaker will not playback the incoming call audio directly. With raw media access and mute incoming audio features developers can add custom filter and play filtered audio in client side. 
	* `Call.isIncomingAudioMuted` property will be `true` when the incoming audio is muted otherwise `false`.
         	
		```js
		 if (Call.isIncomingAudioMuted) {
		    // Incoming audio is muted. Participant will NOT listen the call audio. Time to play filtered audio!
		 } else {
		    // Incoming audio is unmuted. Participant will be able to listen the call audio.
		 }
	 	```
	* Property change event `isIncomingAudioMutedChanged` will raise when `Call.isIncomingAudioMuted` value updated.
	 	```js
		 // isIncomingAudioMutedChangedHandler is the listener to handle PropertyChangedEvent 
		 Call.on('isIncomingAudioMutedChanged', isIncomingAudioMutedChangedHandler);
	 	```
   	* `Call.muteIncomingAudio()` and `Call.unmuteIncomingAudio()` API will mute / unmute incoming audio respectly.
         
		 ```js
		 // To mute incoming audio
		 Call.muteIncomingAudio();
		 // To unmute incoming audio
		 Call.unmuteIncomingAudio();
		 ```

Other Changes

- Telemetry additions and improvements.

## v1.5.4 (2022-06-03)
Available in NPM - https://www.npmjs.com/package/@azure/communication-calling/v/1.5.4
### Azure Communication Services for Government

Azure Communication Services (ACS) in [Azure Government](https://azure.microsoft.com/en-us/global-infrastructure/government/) provides compliance with US government requirements for cloud services. In addition to enjoying the features and capabilities of Messaging, Voice and Video calling, developers benefit from the following features that are unique to Azure Government:
- Your personal data is logically segregated from customer content in the commercial Azure cloud.
- Your resource’s customer content is stored within the United States.
- Access to your organization's customer content is restricted to screened Microsoft personnel. 
- Complies with certifications and accreditations that are required for US Public Sector customers, specifically those offered to Office 365 Government - GCC High offering.

You can find more information about the Office 365 Government – GCC High offering for US Government customers at [Office 365 Government plans](https://products.office.com/government/compare-office-365-government-plans), including [eligibility requirements]().

Features
- iOS and Android, when there is an active Azure Communication Services call and there is an interruption*, audio and video shall auto recover on most of the cases. On some edge cases, to unmute, an api to 'unmute' must be called by the application (can be as a result of user action) to recover the outgoing audio.

Bugfixes
-  Fixes on call recovery after an interruption* on iOS 15.4+.
    * Incoming video streams won't stop rendering.
    * One to one calls won't go to remote hold state.
-  CreateView on remote video stream will fail with correct error code if application tries to have more than the supported number of active remote video streams renderered at the same time.
- Fixed a bug that caused localVideoStream to be removed when the call goes on hold.
- Telemetry additions and improvements.
\* Interruption could be anything that takes over physical devices like microphone and camera. Examples are enabling Siri, playing YouTube videos, accepting PSTN calls.

## v1.5.4-beta.1 (2022-5-17)
Available in NPM - https://www.npmjs.com/package/@azure/communication-calling/v/1.5.4-beta.1
Features
- Remote stream - introduced 'IsReceiving' property, which reflects if the remote stream is being received. For example: 
    * When the remote mobile participant is sending video and put the browser application in the background, OS will stop sending video stream data until the application is brought back to the foreground. Same behavior when mobile device locks/unlocks.
    * When the remote participant is sending video and have bad network connectivity, video is cutting off / lagging.
    * Application can subscribe to updates via 'isReceivingChanged' event, render a spinner over the video while the IsReceiving is false and remove the spinner while the isReceiving is true.
    * Documentation: [IsReceiving property docs](https://docs.microsoft.com/en-us/azure/communication-services/how-tos/calling-sdk/manage-video?pivots=platform-web#remote-video-stream-properties) and [Remote Video rendering docs](https://docs.microsoft.com/en-us/azure/communication-services/how-tos/calling-sdk/manage-video?pivots=platform-web#render-remote-participant-video-streams)
- Debug-Information feature, enables application to gather debuging information needed when a problem does reproduce. Those information will have the call and participant identifiers and the latest log lines.
```js
const debugInfo = callClient.feature(SDK.Features.DebugInfo).dumpDebugInfo();
```
- Get-Environment-Info exposed on the callClient provides environment details and enables the application running on an environment to know if it is supported by the Azure Communication Services or not. A supported environment is a combination of an operating system, a browser, and the minimum version required for that browser.
```js
const environmentInfo = await callClient.getEnvironmentInfo();
environmentInfo.isSupportedBrowser // browser is supported.
```
- iOS and Android, when there is an active Azure Communication Services call and there is an interruption*, audio and video shall auto recover on most of the cases. On some edge cases, to unmute, an api to 'unmute' must be called by the application (can be as a result of user action) to recover the outgoing audio.

Bugfixes
-  Fixes on call recovery after an interruption* on iOS 15.4+.
    * Incoming video streams won't stop rendering.
    * One to one calls won't go to remote hold state.
-  CreateView on remote video stream will fail with correct error code if application tries to have more than the supported number of active remote video streams renderered at the same time.
- Fixed a bug that caused localVideoStream to be removed when the call goes on hold.
- Telemetry additions and improvements.
\* Interruption could be anything that takes over physical devices like microphone and camera. Examples are enabling Siri, playing YouTube videos, accepting PSTN calls.


## v1.4.3-beta.1 (2022-2-18)
## v1.4.4 (2022-3-9)

Available in NPM

Beta: https://www.npmjs.com/package/@azure/communication-calling/v/1.4.3-beta.1
<br />
Stable: https://www.npmjs.com/package/@azure/communication-calling/v/1.4.4

Bugfixes
- Fixed a race condition that in specific cases(<0.5%) causes a failure to join to a meeting or group call.
- Fixed a race condition that in specific cases would return zero devices on enumeration if that happens right after 'askDevicePermission' api call.

# v1.4.2-beta.1 (2022-2-17)
Available in NPM - https://www.npmjs.com/package/@azure/communication-calling/v/1.4.2-beta.1

Bugfixes
- Fixed issue which caused an invisible video tag to be added to the DOM on browsers other than Safari 15.1 on iOS - it was originally introduced as a workaround for iOS 15.1 regression (which we addressed with our 1.3.2). On Safari versions < 14 it could cause a call drop when joining to a group call, due to usage of browser APIs that are not supported by old Safari versions.
- Fixed call drop on Safari versions 14/15 which occurred sometimes (<5%) due to race condition inside the library when user rejoined multiple times.
- Workaround for an [issue](https://docs.microsoft.com/en-us/azure/communication-services/concepts/known-issues#some-android-devices-failing-to-join-calls-and-meetings) with starting/joining/accepting calls or meetings on Samsung A devices - calls would not connect on these devices.
- Fixed wrong device permission state on Safari browsers, when application asks for permissions (deviceManager.askDevicePermission) for the second time with opposite permission for audio/video - wrong result was reported back despite user having granted originally permission -

```js
adpResult1 = deviceManager.askDevicePermission({audio:true, video:false});
// 1st call returns Promise<{audio:true,video:false}>
adpResult2 = deviceManager.askDevicePermission({audio:false, video:true});
// 2nd call returned wrongly Promise<{audio:false,video:true}>

// expected is Promise<{audio:true,video:true}> which is now fixed in 1.4.2-beta.1
```

# v1.4.1-beta.1 (2022-2-1)
Available in NPM - https://www.npmjs.com/package/@azure/communication-calling/v/1.4.1-beta.1

Features

1. Remote stream - introduced 'streamSize' property, which reflects what is the received resolution of a give stream, application can subscribe to updates via 'sizeChanged' event.

Other Changes

1. On mobile platforms, will take into account 'devicePixelRatio' to control resolution of the video stream that is requested from the sender, this will improve video stream resolution for all participants in a calls where at least one of the participants is on mobile platform.
2. Audio volume improvements on iOS 15.3+. Related WebKit [bug](https://bugs.webkit.org/show_bug.cgi?id=230902).
3. Improvements on askDevicePermission API to increase reliability across browsers.
	
	* Ability to split permission request to two for audio/video independently of the browser.
	* Better error handling if one request fail to still trigger the other one.
	* Accurate permissions state after the api call independently of previous calls and states.
	* Detailed permissions break down when a failure happens.

5. Improvements on call recovery after an interruption (Siri, PSTN call etc.) on iOS 15.2+.

	* Incoming video streams won't stop rendering.
	* One to one calls won't go to remote hold state.
	* Recovery on unmute will just unmute the audio stream instead of re-creating it.

5. Improvements on camera recovery when device is taken from another process.
6. Other bug fixes and improvements.
7. Documentation updates.

## v1.3.2-beta.1 (2021-12-9)
## v1.3.2 (2021-12-9)

Available in NPM

Beta: https://www.npmjs.com/package/@azure/communication-calling/v/1.3.2-beta.1
<br />
Stable: https://www.npmjs.com/package/@azure/communication-calling/v/1.3.2

Features

1. Public preview of Microsoft Teams identities in Calling SDK. [Documentation](https://docs.microsoft.com/en-us/azure/communication-services/concepts/interop/teams-user-calling).

2. General availability of Microsoft Teams meeting join. [Documentation](https://docs.microsoft.com/en-us/azure/communication-services/concepts/join-teams-meeting).

3. General availability of User-facing diagnostics. [Documentation](https://docs.microsoft.com/en-us/azure/communication-services/concepts/voice-video-calling/user-facing-diagnostics).

4. General availability of Dominant speaker feature. [Documentation](https://docs.microsoft.com/en-us/azure/communication-services/how-tos/calling-sdk/dominant-speaker).

5. General availability of Client options diagnostic information. [Documentation](https://docs.microsoft.com/en-us/javascript/api/azure-communication-services/@azure/communication-calling/diagnosticoptions?view=azure-communication-services-js).

6. General availability of Calling SDK Emergency calling.

Bugfixes

1. Fixed, Android 12 video artifacts on Chromium browsers. Known regression on Chromium on Android 12 more information [here](https://github.com/Azure/Communication/issues/412)
2. Fixed, iOS 15.1 crashes and wrong orientation when video is on, in calls and Microsoft Teams meetings. Known regression introduced by Apple on iOS 15.1 more information [here](https://github.com/Azure/Communication/issues/413)


Other changes

1. Documentation updates.
2. Internal telemetry updates.
3. Quality and reliability fixes.

Known issues

iOS 15.1 users joining group calls or Microsoft Teams meetings.

* Low volume. Known regression introduced by Apple on iOS 15.1. Related webkit bug [here](https://bugs.webkit.org/show_bug.cgi?id=230902).
* Sometimes when incoming PSTN is received the tab with the call or meeting will hang. Related webkit bugs [here](https://bugs.webkit.org/show_bug.cgi?id=233707) and [here](https://bugs.webkit.org/show_bug.cgi?id=233708#c0).


## v1.3.1-beta.1 (2021-11-17)
Available in NPM - https://www.npmjs.com/package/@azure/communication-calling/v/1.3.1-beta.1

Features

1. Media statistics.
  	* Application will be able to collect aggregated media statistics on specified intervals.
2. Consultative transfer.
  	* Application will be able to consultative transfer a call with the transferee to a call with the transfer target.
3. Meeting join via meeting identifier.
  	* Application will be able to join Microsoft Teams meetings using a unique meeting identifier. This functionality will extend the existing meeting join options with meeting link and meeting coordinates.
4. Client options diagnostic information.
  	* Application will be able to pass custom 'appName', 'appVersion', and additionally set of 'tags' to the SDK that will be sent with telemetry pipeline.

Other changes

1. Call "apis" got renamed to call "features".
2. Documentation updates.
3. Internal telemetry updates.
4. Bug fixes.

Known issues

iOS 15.1 users joining group calls or Microsoft Teams meetings with video on.

	* Will result into broken orientation on the receiver's end. Mitigation: Switch device orientation to horizontal.
	* Going to background will refresh your call. Mitigation: Stop video before going to background.

**This is a known regression introduced by Apple on iOS 15.1 release. More information can be found [here](https://github.com/Azure/Communication/issues/413) and [here](https://docs.microsoft.com/en-us/azure/communication-services/concepts/known-issues#ios-with-safari-crashes-and-refreshes-the-page-if-a-user-tries-to-send-video-in-a-call).**


## v1.2.3-beta.1 (2021-10-20)
Available in NPM - https://www.npmjs.com/package/@azure/communication-calling/v/1.2.3-beta.1

Bugfixes
1. Fixes on iOS Safari when call gets interrupted from PSTN call, Siri, or a native application taking over the device (Microphone or Camera) access.

    * When user is muted would break audio/video permanently.
    * When user is in lobby would break audio/video permanently.
    * When user received multiple interruptions, would break audio/video permanently.

2. Fixes on other OS platforms that received DeviceCaptureNotFunctioning during a call would break audio/video permanently.
3. Fixes on iOS/Android to stop outgoing video when screen locks or browser moves to the background, instead of sending the last frame. User will have to start video again when back to the application

Other changes

1. Feature “Diagnostics” got renamed to “User Facing Diagnostics” for clarity and better understanding.
2. User facing diagnostics won’t expose mediaType anymore, rather a flattened list of possible options.
3. New User facing diagnostics added and are explained bellow:

    * capturerStartFailed: Triggered when failing to start screen-share or recovery.
    * capturerStoppedUnexpectedly: Triggered when screen-share will stop working unexpectedly or recovery.
    * cameraStoppedUnexpectedly: Triggered when camera will stop working unexpectedly or recovery.
    * networkSendQuality: Triggered when network send quality is bad or recovery.
    
4. Documentation updates.
5. Internal telemetry updates.

## v1.2.2-beta.1 (2021-09-15)
Available in NPM - https://www.npmjs.com/package/@azure/communication-calling/v/1.2.2-beta.1

#### Bugfixes
1. Fixes on iOS Safari when application goes to background during an active call.
    - Affected were all iOS users on Safari.
    - Incoming and outgoing audio will work uninterrupted.
3. Fixes on iOS Safari when call interrupted with another PSTN call.
    - Affected were all iOS users on Safari.
    - Audio and video will reconnect when the PSTN call is declined on hanged up. User will have to "unmute" to reconnect the previous call and "start video" to re-start the video.

#### Other changes
1. Documentation updates
2. Internal telemetry and bug fixes.

## v1.2.1-beta.1 (2021-08-26)
Available in NPM - https://www.npmjs.com/package/@azure/communication-calling/v/1.2.1-beta.1 

#### Bugfixes
1. Fixes for incoming video from certain iOS and Android devices not rendering.
    - Affected devices include Samsung M3, Samsung S21, Oppo A5, A7, R17 Pro, Xiaomi Redmi, iPhone SE, Motorolas, etc, where outgoing video streams from these devices were not being sent out correctly to the remote participants in the call. 
2. Fixes for quick video turn on/off switching.
    - Now when you turn your local video on and off quickly, Call.localVideoStream[0] object is updated (added/removed) correctly and remote participants will see your video correctly based off this state.
3. Fixes for UFDs on iOS Safari, MacOS Safari, and MacOS Chrome.
    - isSpeakingWhileDeviceIsMuted UFD was not working properly on Safari nor MacOS Chrome. GH Issue: https://github.com/Azure/Communication/issues/335
4. Fixed issue where incoming video from Pixel devices, shows distorted.
    - Now if the incoming video is rendered in a small screen such as in a portrait mode mobile browser, then the incoming video will not show distorted. But if the incoming video is rendered on a big screen like desktop where the video renderer can be big, then video artifacts may still show.
5. Fixes for camera and microphone names not showing correctly after permissions are granted on Android.
    - GH Issue: https://github.com/Azure/Communication/issues/46
6. Fixes for calling sdk not refreshing the token correctly.
7. Fixes issue where incoming video from Pixel devices, shows distorted. Now if the incoming video is rendered in a small screen such as in a portrait mode mobile browser, then the incoming video will not show distorted. But if the incoming video is rendered on a big screen like desktop where the video renderer can be big, then video artifacts may still show.

#### Other changes
1. Documentation updates
2. Internal telemetry and bug fixes.


## v1.2.0-beta.1 (2021-06-24)
Available in NPM - https://www.npmjs.com/package/@azure/communication-calling/v/1.2.0-beta.1

### New features

1. Call diagnostics feature will allow the developers to diagnose an active call. It is an extended feature of the core `Call` API. ACS Calling Web SDK will raise appropriate Media and Network call diagnostic depending on the active call and device status. For example- `speakingWhileMicrophoneIsMuted` call diagnostic will be raised when user speaks, and the microphone is muted. Developer can show the notification message to alert the end user when this call diagnostic raised.
2. ACS Web Calling SDK extended the `file` protocol support for local developement. Remote access still supports `https` protocol only. ( https://github.com/Azure/Communication/issues/297 )

#### Bugfixes
1. Build failure when using @azure/communication-calling in Vite/Vue app (https://github.com/Azure/azure-sdk-for-js/issues/15479)


## v1.1.0 (2021-06-17)
Available in NPM - https://www.npmjs.com/package/@azure/communication-calling/v/1.1.0

### New features
**Support for WebRTC Unified Plan SDP format.**

Azure Communication Services Calling JavaScript SDK for browsers relies on WebRTC APIs that are being deprecated, specifically the Plan B Session Description Protocol (SDP) API. Google, Microsoft, Apple, and other browser vendors will be removing this functionality starting in August 2021.

**To avoid compatibility risk and potential impact to end-users, we recommend developers leveraging browser calling to upgrade to the latest version of the Azure Communication Services Calling JavaScript SDK as soon as possible. Download the latest Calling JavaScript SDK at NPM or using the commands below.**

Any version higher than - v1.1.0 for stable and v1.1.0-beta.2 for beta releases - supports new WebRTC standards and will be compatible throughout the Plan B deprecation. 
Prior versions of Azure Communication Services Calling JavaScript SDK will continue to be supported by the service; however they will be marked deprecated in NPM and other repositories given the limited browser compatibility of those libraires going forward.  


## v1.1.0-beta.2 (2021-06-10)
Available in NPM - https://www.npmjs.com/package/@azure/communication-calling/v/1.1.0-beta.2

### New features
**Support for WebRTC Unified Plan SDP format.**

Azure Communication Services Calling JavaScript SDK for browsers relies on WebRTC APIs that are being deprecated, specifically the Plan B Session Description Protocol (SDP) API. Google, Microsoft, Apple, and other browser vendors will be removing this functionality starting in August 2021.

**To avoid compatibility risk and potential impact to end-users, we recommend developers leveraging browser calling to upgrade to the latest version of the Azure Communication Services Calling JavaScript SDK as soon as possible. Download the latest Calling JavaScript SDK at NPM or using the commands below.**

Any version higher than - v1.1.0-beta.2 - supports new WebRTC standards and will be compatible throughout the Plan B deprecation. 
Prior versions of Azure Communication Services Calling JavaScript SDK will continue to be supported by the service; however they will be marked deprecated in NPM and other repositories given the limited browser compatibility of those libraires going forward.  



## v1.1.0-beta.1 (2021-05-25)
Available in NPM - https://www.npmjs.com/package/@azure/communication-calling/v/1.1.0-beta.1

### New features
1. Dominant speakers for a call is an extended feature of the core `Call` API and allows you obtain a list of the active speakers in the call.

   This is a ranked list, where the first element in the list represents the last active speaker on the call and so on.
   In order to obtain the dominant speakers in a call, you first need to obtain the call dominant speakers feature API object:
   ```js
   const callDominantSpeakersApi = call.api(Features.CallDominantSpeakers);
   ```
   
   Then, obtain the list of the dominant speakers by calling `dominantSpeakers`. This has a type of `DominantSpeakersInfo`, which has the following members:
     - `speakersList` contains the list of the ranked dominant speakers in the call. These are represented by their participant id.
     - `timestamp` is the latest update time for the dominant speakers in the call.


         ```js
          let dominantSpeakers: DominantSpeakersInfo = callDominantSpeakersApi.dominantSpeakers;
          ```
   Also, you can subscribe to the `dominantSpeakersChanged` event to know when the dominant speakers list has changed
   ```js
   const dominantSpeakersChangedHandler = () => {
      // Get the most up to date list of dominant speakers
      let dominantSpeakers = callDominantSpeakersApi.dominantSpeakers;
   };
   callDominantSpeakersApi.api(SDK.Features.CallDominantSpeakers).on('dominantSpeakersChanged', dominantSpeakersChangedHandler);
   ```
### Breaking API Changes
1. Call.CallInfo.getConversationUrl() renamed with Call.CallInfo.getServerCallId() - returns server call id

#### Bugfixes
1. Prevent creating multiple Call Agents with same ACS user Id
2. Fixed VideoStreamRenderer disposal logic
3. SelectedSpeaker retuns correct value immediately after selectSpeaker is called and resolved
4. Latest media quality improvements
5. VideoRenderer.createView will reject with code 408/Timeout if video wasn't rendered before timeout

## v1.0.1-beta.2 (2021-04-15)

Available in NPM - [https://www.npmjs.com/package/@azure/communication-calling/v/1.0.1-beta.2](https://www.npmjs.com/package/@azure/communication-calling/v/1.0.1-beta.2)

### Other changes
1. New interface `CallInfo` is added to `Call` and `IncomingCall`:

    ```js
        export interface CallInfo {
            getConversationUrl(): Promise<string>; // needed for ACS Server calling API
            readonly groupId: string | undefined; // used in join group scenario
        }
    ```

### Bug fixes
1. Fixed Participant's Call End Reason Doesn't Match Call's Call End Reason

## v1.0.1-beta.1 (2021-03-30)

This release notes contain new changes for ACS Calling Web (JavaScript) SDK v1.0.1-beta.1

Available in NPM - [https://www.npmjs.com/package/@azure/communication-calling/v/1.0.1-beta.1](https://www.npmjs.com/package/@azure/communication-calling/v/1.0.1-beta.1)

**Beta release**
 
Please note about the [feature set](https://docs.microsoft.com/en-us/azure/communication-services/concepts/voice-video-calling/calling-sdk-features) and [known issues](https://docs.microsoft.com/en-us/azure/communication-services/concepts/known-issues)

Moving forward, we will be having two versions of the SDKs:

* v x.x.x-Beta.x - the SDK versions provide access to new functionality, which is not at the General Availability milestone. E.g., **Teams Interoperability is supported in the Beta versions.**
* v x.x.x - the version of the SDK that at General Availability. Azure Communication Services provide full SLA for these SDKs


### Bugfixes
1. Fixed [Web 1.0.0-beta.10 - remoteParticipantsUpdated event is not triggered when Teams user is leaving a meeting](https://github.com/Azure/Communication/issues/238)

## v1.0.0 (2021-04-04)

- Our first GA SDK was released, v1.0.0
- APIs for preview features, such as Teams Interop, are only available in the new libraries marked with the *-beta* suffix. This week we released 1.0.1-beta.1.
- Both of these are available through [NPM](https://www.npmjs.com/package/@azure/communication-calling)

## v1.0.0-beta.10 (2021-03-24)

## Breaking API Changes
1. v1.0.0-beta.10 takes dependency on @azure/communication-common@1.0.0-beta.6 and @azure/logger@^1.0.2
2. `CallingApplicationIdentifier` and `CallingApplicationKind`were removed starting from @azure/communication-common@1.0.0-beta.6
Applications that were relying on them can still leverage generic `UnknownIdentifier` and `UnknownIdentifierKind`

    2.1 Call `addParticipant` method doesn't accept `CallingApplicationIdentifier` anymore

    2.2 Call `removeParticipant` method doesn't accept `CallingApplicationIdentifier` anymore

    2.3 CallAgent `startCall`  method doesn't accept `CallingApplicationIdentifier` anymore

    2.4 `CallerInfo` doesn't expose `CallingApplicationIdentifier` anymore as one of the `identifier` types, it will use `UnknownIdentifier` as a fallback

    2.5 `RemoteParticipant` `identifier` property doesn't expose `CallingApplicationIdentifier` anymore, it will use `UnknownIdentifier` as a fallback

3. `isMicrophoneMuted` changed to `isMuted`, if `isMuted` is set to `true` it indicates that local user is muted because of
* application called `mute()` method on the Call object and it resolved
* in Teams interop scenarios - Teams user muted ACS user
* in Teams interop scenarios - Teams user triggered 'mute all' feature for all participants in the meeting

    3.1 `isMutedChanged` event was introduced on the `Call` object

4. `VideoDeviceInfo` - `cameraFacing` property was removed

5. `Renderer` class was renamed to `VideoStreamRenderer`
6. `RendererOptions` type used in `createView` method on `Renderer` is now called `CreateViewOptions`
7. `createView` now returns `VideoStreamRendererView` type instead of `RendererView`



## Other changes

### Logger
CallClient doesn't accept `AzureLogger` instance in options, instead application can control logs by importing `setLogLevel` function and 
calling it with appropriate log level
**Example**
```js
import { setLogLevel } from '@azure/logger';

// configure log level
setLogLevel('verbose');

const callClient = new CallClient();
```

[More about @azure/logger](https://www.npmjs.com/package/@azure/logger)

### Errors
Whenever ACS Calling library throws an error, it will contain improved error message, with additional details.

## IncomingCall
`IncomingCall` changes
* added `callEndReason` property of a `CallEndReason` type that indicates why call ended* 

## Prevent race conditions when rendering stream
To prevent race condition where `VideoStreamRendererView` is used to render a `RemoveVideoStream` that becomes unavailable ( `isAvailable` changes to `false` )
`VideoStreamRendererView` will be automatically disposed
* when stream `isAvailable` changes to `false` while `createView()` promise is still pending, `createView()` promise will be rejected

## v1.0.0-beta.8 and v1.0.0-beta.9 (2021-03-08)

## Bug fixes
### v1.0.0-beta.8
Media quality improvements - more stabile and uniform quality in all scenarios

### v1.0.0-beta.9
Fixed issue for Safari - DeviceManager.askDevicePermission - when called with both audio:true, video: true, after stream is already acquired for a given device, was loosing track of acquired stream. Since 1.0.0-beta.9, ACS Calling SDK will split these calls into 2 prompts on Safari.

## Breaking API Changes
1. CallState - removed 'Incoming' state, this state is now invalid since 1.0.0-beta.4 introduced IncomingCall


## New features

## Support for multiple CallClient instances
CallClient can now be instantiated multiple times
* each instance can then be used to create new CallAgent instance using `async createCallAgent` API
* you can create only one CallAgent instance from each CallClient instance
* each CallAgent instance must be initialized for a different user, by implementing and passing user specific `CommunicationTokenCredential`

**Example**
```js
const callClientUser1 = new CallClient();
const callAgentUser1 = await callClientUser1.createCallAgent(communicationTokenCredentialForUser1, {displayName: 'User1'});
const callClientUser2 = new CallClient();
const callAgentUser2 = await callClientUser2.createCallAgent(communicationTokenCredentialForUser2, {displayName: 'User2'});
```

## Access DeviceManager without creating CallAgent instance
CallClient API to get DeviceManager instance - `async getDeviceManager()` can now be called prior to initialization of CallAgent instance

**Example**
```js
const callClient = new CallClient();
const deviceManager = await callClient.getDeviceManager();
await deviceManager.getCamears();
```
This allows developers to implement UX where user can manage devices or create preview of local camera before user context is known.

## Call transcription 

Call transcription is an extended feature of the core `Call` API. 
In the Teams interop scenarios, when a Teams user starts 'Transcription' on a Teams side,
your application must notify the user that call is being actively transcribed due to privacy reasons.

You first need to obtain the transcription feature API object

**Example**
```js
const callTranscriptionApi = call.api(Features.Transcription);
```

Then, you can check if the audio in the call is being transcribed, inspect the `isTranscriptionActive` property of `callTranscriptionApi`, it returns `Boolean`

**Example**
```js
const isTranscriptionActive = callTranscriptionApi.isTranscriptionActive;
```

You can also subscribe to transcription state changes

**Example**
```js
const isTranscriptionActiveChangedHandler = () => {
  console.log(callTranscriptionApi.isTranscriptionActive);
};
callRecordingApi.on('isTranscriptionActiveChanged', isTranscriptionActiveChangedHandler);
```

To unsubscribe your event handler call

**Example**
```js
callRecordingApi.off('isTranscriptionActiveChanged', isTranscriptionActiveChangedHandler);
```

## v1.0.0-beta.7 (2021-03-01)
## ACS Calling Web (JavaScript) SDK v1.0.0-beta.7 was deprecated due to critical bug that could affect main scenarios.

## v1.0.0-beta.6 (2021-02-18)

## Breaking API Changes
1. CallerInfo interface changes:
    1. identity property has been replaced by `identifier`
    2. optional `displayName` property which is useful for incoming call scenario
2. DeviceAccess interface changes:
    1. `audio` property is not optional anymore, it will be always returned
    2. `video` property is not optional anymore, it will be always returned
3. New interface `PermissionConstraints`
    ```js
        export interface PermissionConstraints {
            audio: boolean;
            video: boolean;
        }
    ```
4. DeviceManager interface changes:
    1. askDevicePermission method uses the new `PermissionConstraints` interface:  
       askDevicePermission(audio: boolean, video: boolean) -> askDevicePermission(permissionConstraints: PermissionConstraints)
5. RendererView interface changes:
    1. mirrored property has been replaced by `isMirrored`
6. RendererOptions interface changes:
    1. mirrored property has been replaced by `isMirrored`
7. IncomingCall interface changes:
    1. new readonly property `callerInfo` of type CallerInfo has been added

## v1.0.0-beta.5 (2021-02-17)

## Bug fixes
1. Fixed https://github.com/Azure/Communication/issues/46
    - Device names do not appear on Android Chrome after calling DeviceManager.AskDevicePermission(), then enumerating the Microphone List
2. Fixed https://github.com/Azure/Communication/issues/47
    - After permissions are granted by the user (call deviceManager.askDevicePermission), the desktop version of Chrome fires the audioDevicesUpdated and videoDevicesUpdated events. These events are not called when accessed from Mobile Chrome
3. Fixed https://github.com/Azure/Communication/issues/144
    - For Android Chrome: The getSpeakerList return empty list. But getMicrophoneList and getCameraList return correct devices 
4. Fixed https://github.com/Azure/Communication/issues/174
    - Occur unhandled runtime error for below conditions:
        1. Could not find Callee
        2. Callee rejected an incoming call

## Breaking API Changes
1. Call interface changes:
    1. Call.unhold() has been changed to call.resume()
    2. 'callStateChanged' event is renamed to 'stateChanged'
    3. 'callIdChanged' event is renamed to 'idChanged'
2. CallAgent.call() has been changed to CallAgent.startCall()
3. CallState.Hold has been removed and splitted in 2 new states:
    1. 'LocalHold' - indicates that call is put on hold by local participant
    2. 'RemoteHold' - indicates that call is put on hold by remote participant
4. DeviceManager interface changes:
    1. getCameraList() has been changed to getCameras() which is now **async**
    2. getMicrophoneList() has been changed to getMicrophones() which is now **async**
    3. getSpeakerList() has been changed to getSpeakers() which is now **async**
    4. setMicrophone() has been changed to selectMicrophone() which is now **async**
    5. setSpeaker() has been changed to selectSpeaker() which is now **async**
    6. getMicrophone() has been changed to readonly **property** selectedMicrophone
    7. getSpeaker() has been changed to readonly **property** selectedSpeaker
    8. 'permissionStateChanged' event has been removed
    9. New event 'selectedMicrophoneChanged' which will be raised when selectedMicrophone is changed
    10. New event 'selectedSpeakerChanged' which will be raised when selectedSpeaker is changed
    11. New property 'isSpeakerSelectionAvailable' which indicates whether the host device supports speaker enumeration and selection
5. HangupCallOptions has been renamed to HangUpOptions
6. PermissionState and PermissionType both types are removed
7. RemoteParticipant interface changes:
    1. 'participantStateChanged' event is renamed to 'stateChanged'
8. RemoteParticipantState now includes 'Ringing' state
9. RemoteVideoStream interface changes:
    1. 'availabilityChanged' event is renamed to 'isAvailableChanged'
10. Transfer interface changes:
    1. 'transferStateChanged' event is renamed to 'stateChanged'
11. LocalVideoStream interface chnages:
    1. getMediaStreamType() has been changed to **getter method** mediaStreamType()
    2. getSource() has been changed to **getter method** source()
12. RemoteVideoStream interface changes:
    1. type property is renamed to mediaStreamType

## Other changes
1. Call.remoteParticipants type change: RemoteParticipant[] -> ReadonlyArray<RemoteParticipant>
2. Call.localVideoStreams type change: LocalVideoStream[] -> ReadonlyArray<LocalVideoStream>
3. CallAgent.calls type change:  Call[] -> ReadonlyArray<Call>
4. RemoteParticipant.videoStreams type change:  RemoteVideoStream[] -> ReadonlyArray<RemoteVideoStream>


## v1.0.0-beta.4 (2021-02-03)

## Bug fixes
1. Fix CallEndReason.subCode lower case 'c' typo 
2. Fix CallAgent.join() api accepting undefined CallContext. Api must now take a GroupLocator or MeetingLocator 

## Breaking API Changes
1. New flow for properties/collection subscriptions.
Application has to inspect current values and subscribe to update events for future values
```js
    // Subscribe to participants currently in a Call
    call.remoteParticipants.forEach(p => {
        subscribeToRemoteParticipant(p);
    })

    call.on('remoteParticipantsUpdated', e => {
        // Subscribe to new participants in the call
        e.added.forEach( p=> {
            subscribeToRemoteParticipant(p)
        })
        e.removed.forEach( p => {
            unsubscribeToRemoteParticipant(p)
        })
    });

    function subscribeToParticipant(p) {
        // Subscribe to participant's current video/screensharing streams
        p.videoStreams.forEach(v => { handleRemoteStream(v) });
        // Subscribe to particiapnt's new video/screensharing streams
        p.on('videoStreamsUpdated', e => {
            e.added.forEach(v => { handleRemoteStream(v) })
        })
    }
}
```
2. Changed Call.callerIdentity variable to Call.callerInfo which is now of type CallerInfo
```js
export declare interface CallerInfo {
    /**
    * Identity of the caller
    */
    identity: CommunicationUserIdentifier | PhoneNumberIdentifier | CallingApplicationIdentifier | MicrosoftTeamsUserIdentifier | UnknownIdentifier | undefined;
}
```

4. Changed Call.isIncoming: boolean to Call.direction: CallDirection
```js
    export declare type CallDirection = 'Incoming' | 'Outgoing';
```

3. Type name changes from @azure/communication-common version beta4
```js
    CommunicationUserCredential -> CommunicationTokenCredential
    CommunicationUser -> CommunicationUserIdentifier
    PhoneNumber -> PhoneNumberIdentifier
    CallingApplication -> CallingApplicationIdentifier
```
Note: @azure/communication-calling beta4 depends on @azure/communication-common beta4

## New features

## Call recording management

Call recording is an extended feature of the core `Call` API. You first need to obtain the recording feature API object:

```js
const callRecordingApi = call.api(Features.Recording);
```

Then, to can check if the call is being recorded, inspect the `isRecordingActive` property of `callRecordingApi`, it returns `Boolean`.

```js
const isResordingActive = callRecordingApi.isRecordingActive;
```

You can also subscribe to recording changes:

```js
const isRecordingActiveChangedHandler = () => {
  console.log(callRecordingApi.isRecordingActive);
};

callRecordingApi.on('isRecordingActiveChanged', isRecordingActiveChangedHandler);
```

## Call Transfer management

Call transfer is an extended feature of the core `Call` API. You first need to obtain the transfer feature API object:

```js
const callTransferApi = call.api(Features.Transfer);
```

Call transfer involves three parties *transferor*, *transferee*, and *transfer target*. Transfer flow is working as following:

1. There is already a connected call between *transferor* and *transferee*
2. *transferor* decide to transfer the call (*transferee* -> *transfer target*)
3. *transferor* should put the call on hold first before invoking `transfer` API
4. *transferor* calls `transfer` API
5. *transferee* decide to whether `accept` or `reject` the transfer request to *transfer target* via `transferRequested` event.
6. *transfer target* will receive an incoming call only if *transferee* did `accept` the transfer request

### Transfer terminology

- Transferor - The one who initiates the transfer request
- Transferee - The one who is being transferred by the transferor to the transfer target
- Transfer target - The one who is the target that is being transferred to

To transfer current call, you can use `transfer` synchronous API. `transfer` takes optional `TransferCallOptions` which allows you to set `disableForwardingAndUnanswered` flag:

- `disableForwardingAndUnanswered` = false - if *transfer target* doesn't answer the transfer call, then it will follow the *transfer target* forwarding and unanswered settings
- `disableForwardingAndUnanswered` = true - if *transfer target* doesn't answer the transfer call, then the transfer attempt will end

```js
// transfer target can be ACS user
const id = { communicationUserId: <ACS_USER_ID> };
```

```js
// call transfer API
const transfer = callTransferApi.transfer({targetParticipant: id});
```

Transfer allows you to subscribe to `transferStateChanged` and `transferRequested` events. `transferRequsted` event comes from `call` instance, `transferStateChanged` event and transfer `state` and `error` comes from `transfer` instance

```js
// transfer state
const transferState = transfer.state; // None | Transferring | Transferred | Failed

// to check the transfer failure reason
const transferError = transfer.error; // transfer error code that describes the failure if transfer request failed
```

Transferee can accept or reject the transfer request initiated by transferor in `transferRequested` event via `accept()` or `reject()` in `transferRequestedEventArgs`. You can access `targetParticipant` information, `accept`, `reject` methods in `transferRequestedEventArgs`.

```js
// Transferee to accept the transfer request
callTransferApi.on('transferRequested', args => {
  args.accept();
});

// Transferee to reject the transfer request
callTransferApi.on('transferRequested', args => {
  args.reject();
});
```


## New 'incomingCall' event for CallAgent
The `CallAgent` instance emits an `incomingCall` event when the logged in identity is receiving an incoming call. To listen to this event subscibe in the following way

```js
const incomingCallHander = async (args: { incomingCall: IncomingCall }) => {
	//accept the call
	var call = await incomingCall.accept();

	//reject the call
	incomingCall.reject();
};
callAgentInstance.on('incomingCall', incomingCallHander);
```
The `incomingCall` event will provide with an instance of `IncomingCall` on which you can accept or reject a call. Please note that Call interface no longer has accept() nor reject() method. These two methods are now part of the IncomingCall interface.
```js
export declare interface IncomingCall {
    readonly id: string;
    accept(options?: AcceptCallOptions): Promise<Call>;
    reject(): Promise<void>;
    on(event: 'callEnded', listener: CallEndedEvent): void;
    off(event: 'callEnded', listener: CallEndedEvent): void;
}
```

## New 'callEnded' event for Call obj
```js
    call.on('callEnded', args => {
        console.log(args.callEndReason);
    });
```

## Other changes
1. New package structure:
    * /dist
        * sdk.bundle.js
    * /dist-esm
        * sdk.bundle.js
    * /types
        * communication-calling.d.ts
    * EULA.txt
    * Notices.txt
    * package.json (Updated)
    * README.md (Updated)
2. Added new package dependencies:
    * @azure/logger version 1.0.0
    * @azure/communication-common version beta4 (calling beta4 depends on common beta4)

## v1.0.0-beta.3 (2020-12-16)

## Bug fixes
1. Safari/iOS 14+ - fixed an issue with video being stopped if user starts or joins a call with enabled video preview.
2. Call.unhold() API can now be called right after Call.hold() API without having to wait for Call.hold() API to resolve.
3. Fixes to allow consumption of library with as a ES5 bundle ( enables support for EmberJs framework )

## Breaking API changes
Display name of an ACS user is now passed in CallClient.createCallAgent() API as an optional argument and it is immutable.
`callAgent.updateDisplayName` API is deprecated.
### New interface
```ts
export class CallClient {
  // ...
  public async createCallAgent(tokenCredential: CommunicationUserCredential, options?: CallAgentOptions): Promise<CallAgent>
}

export interface CallAgentOptions {
    /**
     * Specify the display name of the local participant for all new calls.
     */
    displayName?: string;
}
```
### Usage example
```ts
    const tokenCredential = new AzureCommunicationUserCredential("<USER ACCESS TOKEN>");
    callAgent = await callClient.createCallAgent(tokenCredential, {displayName: 'optional ACS user name'});
```
Changed join group call API to now take a GroupLocator:

### New interface
```ts
export class CallAgent {
  // ...
  join(groupLocator: GroupLocator, options?: JoinCallOptions): Call;
}
/**
 * Locator used for joining a group call.
 * @public
 */
export interface GroupCallLocator {
    groupId: string;
}
```
### Usage example
```ts
    const groupLocator = { groupId: '<GROUP_ID>'};
    callAgent.join(groupLocaltor, {});
```
Changed join Teams meeting api to now take a MeetingLocaltor:
### New interface
```ts
/**
 * @beta
 * Locator used for joining a meeting with meeting link.
 */
export interface TeamsMeetingLinkLocator {
    meetingLink: string;
}

/**
 * @beta
 * Locator used for joining a meeting with meeting coordinates.
 */
export interface TeamsMeetingCoordinatesLocator {
    threadId: string;
    organizerId: string;
    tenantId: string;
    messageId: string;
}

/**
 * @beta
 * Type of Meeting locator.
 */
export type MeetingLocator = TeamsMeetingLinkLocator | TeamsMeetingCoordinatesLocator;

export class CallAgent {
  // ...
  /**
     * @beta
     * Join a meeting.
     * @param meetingLocator - Meeting information.
     * @param options - Call start options.
     * @returns The Call object associated with the call.
     */
    join(meetingLocator: MeetingLocator, options?: JoinCallOptions): Call;
}
```

### Usage example

```ts
  const teamsMeetingLinkLocator = { meetingLink: '<TEAMS_MEETING_URL>'};
  callAgent.join(teamsMeetingLinkLocator, {});
```
## Other changes
1.  Teams/ACS meeting interop - ACS Calling SDK will not expose any Teams bots ( e.g. Recording ) through 'remoteParticipants' collection.
    Application can inspect & subscribe to isRecordingActive property to discover if current call is recorded
2.  Call.startVideo(localVideoStream: LocalVideoStream) API must now take a LocalVideoStream object as an argument
3.  LocalVideoStream constructor must now take VideoDeviceInfo as an argument


## v1.0.0-beta.2 (2020-11-10)

## Bug fixes
* Call.callEndReason is undefined when Call terminates issue was fixed and now will be populated with a code and sub-code which determine the reason why the call terminated.
* Call terminates when there is only 1 participant left in the call ([GitHub Issue 68](https://github.com/Azure/Communication/issues/68)) was fixed and now call will stay connected if there is only 1 participant left in the call.


## New features

* New APIs to join Teams meetings by using meeting link or meeting coordinates:
<br/>`CallAgent.join(context: MeetingLinkContext, options?: JoinCallOptions): Call;`
<br/>`CallAgent.join(context: MeetingCoordinatesContext, options?: JoinCallOptions): Call;`

## v1.0.0-beta.1 (2020-09-22)

This is the initial release of Azure Communication Services for public preview for JavaScript (Web) SDK. The following features are not available yet:

1. Acquiring phone numbers
2. Using phone numbers to send and receive SMS messages
3. Using the Calling SDK to drive voice calls with the traditional phone network (PSTN)

### Features available
*   Place and receive 1-1 audio/video call
*   Place and receive 1-N (group) audio/video call
*   Join 1-N (group) audio/video call
*   Escalate a 1-1 call to a group call - by adding voip participant
*   Allow to join call with microphone mute
*   Allow to set camera device before call and to switch camera device during call
*   Allow to enable/disable camera in-call and while call is ringing/incoming
*   DeviceManager support to list audio and video devices
*   DeviceManager support to set audio devices
*   Remote video and screen-sharing rendering
*   Screensharing support
*   Local camera device preview rendering
*   Mid-call operations (mute/unmute)
*   Mid-call operations (Turn Video on/Turn Video off)
*   Mid-call operations (Add\Remove Participant)
*   Call and RemoteParticipant states
*   Property changed and collection updated events
*   Place a call to PSTN participant
*   Test your mic, speaker, and camera via audio testing service (available by calling 8:echo123) 
*   Hold/unhold

### Onboarding
Please refer to the [Microsoft docs](https://docs.microsoft.com/en-us/azure/communication-services/quickstarts/voice-video-calling/getting-started-with-calling?pivots=platform-web) of how to bootstrap a Calling sample application on web.

### Limitations
* While the ability to make PSTN calls is available in the SDK, you need a PSTN number in ACS to make the call. PSTN functionality is available under private preview; follow this [document](https://docs.microsoft.com/azure/communication-services/quickstarts/telephony-sms/get-phone-number) for availability updates
* On web mobile and web desktop at any given time client multiple receive video streams might result in a greater number of video freezes and/or quality fluctuations. We are working on improving the experience. We recommend limiting number of incoming streams to one if you experience such issues
* The PSTN and VOIP access token scopes are currently not enforced. All-access tokens implicitly authorize users to access VOIP and PSTN calling functionality until further notice. 
