# Hayavo Meet Client

Official JavaScript SDK for **Hayavo Meet** — real-time video conferencing, audio/video communication, screen sharing, RTM messaging, chat, presence, reactions, attachments, moderation, and calls.

Build browser-based video meeting and real-time collaboration applications using the Hayavo Meet platform.

## Features

* 🎥 Real-time video conferencing
* 🎙️ Audio communication
* 📺 Screen sharing
* 💬 Real-time messaging
* 💬 Chat
* 👥 Presence
* 📎 File and attachment sharing
* 😀 Reactions
* ✋ Raise hand
* 📊 Poll interaction support
* 🔇 User moderation
* 📞 Real-time calls
* 🔄 Automatic RTC reconnection
* 📡 Adaptive streaming
* 🚀 Dynacast support
* 📱 Camera switching
* 🎤 Microphone switching
* 🔊 Microphone noise cancellation controls
* 📷 Camera facing-mode switching
* 🎞️ Multiple video quality profiles
* 🔐 Token-based authentication
* ⚡ Browser-ready UMD build
* 📦 ES module / npm support

---

# Installation

Install from npm:

```bash
npm install hayavo-meet-client
```

or:

```bash
yarn add hayavo-meet-client
```

or:

```bash
pnpm add hayavo-meet-client
```

---

# Browser CDN

The minified UMD build can be loaded directly from jsDelivr.

```html
<script src="https://cdn.jsdelivr.net/npm/hayavo-meet-client@1.1.2/dist/hayavo-meet-client.umd.min.js"></script>
```

For production applications, it is recommended to pin the SDK version instead of using a floating version.

After loading the UMD build, the SDK is available through the `HayavoMeet` browser global.

```html
<script>
  console.log(window.HayavoMeet);
</script>
```

---

# JavaScript / ESM

```javascript
import HayavoMeet from "hayavo-meet-client";
```

Create a client:

```javascript
const client = new HayavoMeet();
```

---

# Basic Setup

The JavaScript SDK connects to the Hayavo Meet RTC WebSocket endpoint.

```javascript
import HayavoMeet from "hayavo-meet-client";

const client = new HayavoMeet();

await client.init(
  "getting from server RTC URL"
);
```

You can also initialize during construction:

```javascript
const client = new HayavoMeet({
 "getting from server RTC URL"
});
```

---

# Join a Room

The browser/client application should receive a valid Hayavo Meet room token from your backend.

Do not expose your Hayavo API secret or application secret in browser JavaScript.

```javascript
const room = await client.join({
  token: ROOM_TOKEN,
  roomName: "my-room"
});
```

A typical application flow is:

```text
Your Application
       │
       │ Request room/token
       ▼
Your Backend
       │
       │ Hayavo API credentials
       ▼
Hayavo Meet
       │
       │ RTC token
       ▼
Browser
       │
       ▼
Hayavo Meet Client SDK
```

Your API credentials and secrets should remain server-side.

---

# Join With Media Options

You can specify the video quality when joining.

```javascript
await client.join({
  token: ROOM_TOKEN,

  media: {
    audio: true,

    video: {
      quality: "HD"
    }
  },

  autoPublish: true
});
```

Available quality profiles:

```text
SD
HD
FHD
QHD
UHD
```

Example:

```javascript
await client.join({
  token: ROOM_TOKEN,

  media: {
    audio: true,

    video: {
      quality: "FHD",
      codec: "vp8"
    }
  }
});
```

---

# Disable Automatic Publishing

You can join without automatically publishing microphone and camera tracks.

```javascript
await client.join({
  token: ROOM_TOKEN,
  autoPublish: false
});
```

Then publish manually:

```javascript
await client.publish({
  audio: true,

  video: {
    quality: "HD"
  }
});
```

---

# Audio

Enable microphone:

```javascript
await client.media.enableMic();
```

Disable microphone:

```javascript
await client.media.disableMic();
```

Toggle microphone:

```javascript
const enabled =
  await client.media.toggleMic();

console.log("Microphone enabled:", enabled);
```

Check microphone state:

```javascript
const enabled =
  client.media.isMicEnabled();
```

---

# Camera

Enable camera:

```javascript
await client.media.enableCamera();
```

Disable camera:

```javascript
await client.media.disableCamera();
```

Toggle camera:

```javascript
const enabled =
  await client.media.toggleCamera();

console.log("Camera enabled:", enabled);
```

Check camera state:

```javascript
const enabled =
  client.media.isCameraEnabled();
```

---

# Camera and Microphone Devices

Get all media devices:

```javascript
const devices =
  await client.media.getDevices();
```

Get cameras:

```javascript
const cameras =
  await client.media.getCameras();
```

Get microphones:

```javascript
const microphones =
  await client.media.getMicrophones();
```

---

# Switch Camera

```javascript
await client.media.switchCamera(
  cameraDeviceId
);
```

You can also specify video options:

```javascript
await client.media.switchCamera(
  cameraDeviceId,
  {
    quality: "HD",
    codec: "vp8"
  }
);
```

---

# Switch Camera Facing Mode

For supported mobile devices:

```javascript
await client.media.switchFacingMode(
  "environment"
);
```

Switch back:

```javascript
await client.media.switchFacingMode(
  "user"
);
```

Or automatically toggle:

```javascript
await client.media.toggleCameraFacing();
```

---

# Switch Microphone

```javascript
await client.media.switchMicrophone(
  microphoneDeviceId
);
```

---

# Noise Cancellation

The SDK provides microphone recreation with audio processing options.

```javascript
await client.media.setNoiseCancellation(
  true
);
```

Disable:

```javascript
await client.media.setNoiseCancellation(
  false
);
```

The SDK can configure:

* Echo cancellation
* Noise suppression
* Automatic gain control
* Krisp processing when supported
* Voice gate when supported

---

# Screen Sharing

Screen sharing is available through the SDK screen module.

Example:

```javascript
await client.screen.start();
```

Stop screen sharing:

```javascript
await client.screen.stop();
```

The exact screen-sharing options depend on the browser's supported media capture capabilities.

---

# RTC Events

The SDK provides room and media events.

Example:

```javascript
client.events.on(
  "connected",
  () => {
    console.log("Connected to room");
  }
);
```

Reconnection:

```javascript
client.events.on(
  "reconnecting",
  () => {
    console.log("Reconnecting...");
  }
);
```

Reconnected:

```javascript
client.events.on(
  "reconnected",
  () => {
    console.log("Connection restored");
  }
);
```

Disconnected:

```javascript
client.events.on(
  "disconnected",
  (reason) => {
    console.log(
      "Disconnected:",
      reason
    );
  }
);
```

Connection state:

```javascript
client.events.on(
  "connectionStateChanged",
  (state) => {
    console.log(
      "Connection state:",
      state
    );
  }
);
```

---

# Participant Events

Listen for participants joining:

```javascript
client.events.on(
  "participantConnected",
  (participant) => {
    console.log(
      "Participant joined:",
      participant
    );
  }
);
```

Participant leaving:

```javascript
client.events.on(
  "participantDisconnected",
  (participant) => {
    console.log(
      "Participant left:",
      participant
    );
  }
);
```

Track subscribed:

```javascript
client.events.on(
  "trackSubscribed",
  (
    track,
    publication,
    participant
  ) => {

    console.log(
      "Track subscribed:",
      track
    );
  }
);
```

Track unsubscribed:

```javascript
client.events.on(
  "trackUnsubscribed",
  (
    track,
    publication,
    participant
  ) => {

    console.log(
      "Track unsubscribed:",
      track
    );
  }
);
```

---

# Remote Participants

Get the current remote participants:

```javascript
const participants =
  client.core.getRemoteParticipants();

console.log(participants);
```

---

# Connection State

Check whether the client is connected:

```javascript
if (client.isConnected()) {
  console.log("Connected");
}
```

Get the current connection state:

```javascript
console.log(
  client.getConnectionState()
);
```

Possible states include:

```text
idle
connecting
connected
reconnecting
signalReconnecting
disconnecting
disconnected
```

---

# RTM

Hayavo Meet also provides a separate RTM layer for real-time application communication.

The RTM layer is exposed through:

```javascript
client.rtm
```

The SDK also provides higher-level modules:

```javascript
client.chat
client.presence
client.attachment
client.interaction
client.moderation
client.screenshare
client.call
```

---

# RTM Connection

RTM requires an RTM signal endpoint, token, and room ID.

```javascript
await client.rtm.connect({
  signal: "getting from server",
  token: RTM_TOKEN,
  roomId: ROOM_ID
});
```

Check connection:

```javascript
client.rtm.isConnected();
```

Get the RTM room:

```javascript
client.rtm.getRoomId();
```

Disconnect:

```javascript
client.rtm.disconnect();
```

---

# RTM Events

Listen for an RTM event:

```javascript
client.rtm.on(
  "connected",
  (data) => {
    console.log(
      "RTM connected:",
      data
    );
  }
);
```

Listen for every RTM event:

```javascript
client.rtm.on(
  "*",
  ({ event, payload }) => {

    console.log(
      "RTM event:",
      event
    );

    console.log(
      "Payload:",
      payload
    );
  }
);
```

---

# RTM Custom Messages

Send an RTM event:

```javascript
client.rtm.send(
  "my.custom.event",
  {
    message: "Hello"
  }
);
```

Receive it:

```javascript
client.rtm.on(
  "my.custom.event",
  (payload) => {

    console.log(
      payload.message
    );
  }
);
```

---

# Chat

Chat functionality is available through:

```javascript
client.chat
```

Chat supports the Hayavo Meet real-time messaging layer.

Typical application features include:

* Messages
* Message deletion
* Typing indicators
* Attachments
* Reactions
* Real-time delivery

Refer to the Hayavo Meet API documentation for the complete chat method signatures.

---

# Presence

Presence is available through:

```javascript
client.presence
```

Presence can be used to represent:

* Online users
* Offline users
* Room presence
* User presence changes

Example:

```javascript
client.presence
```

The SDK automatically handles room presence cleanup when leaving the RTM room.

---

# Attachments

Attachment functionality is available through:

```javascript
client.attachment
```

This can be used for real-time chat attachment workflows such as:

* Images
* Files
* Documents
* Chat attachments

Your application should obtain any required upload authorization from your backend.

---

# Reactions and Interactions

Interaction functionality is available through:

```javascript
client.interaction
```

Supported interaction concepts include:

* Raise hand
* Poll voting
* Reactions

---

# Moderation

Moderation functionality is available through:

```javascript
client.moderation
```

Typical moderation operations include:

* User mute
* User kick

Moderation permissions are controlled by the Hayavo Meet room/token authorization model.

---

# Calls

Real-time call functionality is available through:

```javascript
client.call
```

The RTM layer supports call events including:

```text
call.invite
call.accept
call.reject
```

---

# Screen Share Events

RTM screen-share signaling is available through:

```javascript
client.screenshare
```

Screen-share events include:

```text
screenshare.start
screenshare.stop
```

---

# Leave a Room

Leave the current room:

```javascript
await client.leave();
```

The SDK attempts to clean up:

* Published media
* Screen sharing
* RTM presence
* RTM connection
* RTC connection

---

# Destroy the Client

When the SDK instance is no longer required:

```javascript
await client.destroy();
```

This releases the SDK's internal resources and event handlers.

---

# Error Handling

Hayavo Meet provides SDK-specific error classes.

```javascript
import HayavoMeet, {
  HayavoMeetError,
  ConnectionError,
  MediaError,
  ScreenShareError,
  TokenError
} from "hayavo-meet-client";
```

Example:

```javascript
try {

  await client.join({
    token: ROOM_TOKEN
  });

} catch (error) {

  if (error instanceof ConnectionError) {
    console.error(
      "Connection failed:",
      error
    );
  }

  if (error instanceof MediaError) {
    console.error(
      "Media error:",
      error
    );
  }
}
```

Common error codes include:

```text
SDK_INITIALIZATION_FAILED
ROOM_JOIN_FAILED
ROOM_LEAVE_FAILED
MEDIA_PUBLISH_FAILED
INVALID_VIDEO_QUALITY
RTM_TOKEN_REQUIRED
RTM_ROOM_REQUIRED
RTM_NOT_CONNECTED
RTM_CONNECTION_FAILED
RTM_CONNECTION_CLOSED
```

---

# Complete Browser Example

```html
<!DOCTYPE html>

<html>

<head>
  <meta charset="UTF-8">
  <title>Hayavo Meet</title>
</head>

<body>

  <button id="join">
    Join Meeting
  </button>

  <button id="mic">
    Toggle Microphone
  </button>

  <button id="camera">
    Toggle Camera
  </button>

  <button id="leave">
    Leave
  </button>

  <script src="https://cdn.jsdelivr.net/npm/hayavo-meet-client@1.1.2/dist/hayavo-meet-client.umd.min.js"></script>

  <script>

    const client =
      new HayavoMeet.default();

    async function start() {

      /*
       * Initialize RTC.
       */
      await client.init(
        "getting from server RTC URL"
      );


      /*
       * The token must be generated
       * by your backend.
       */
      await client.join({

        token: "ROOM_TOKEN",

        media: {

          audio: true,

          video: {
            quality: "HD",
            codec: "vp8"
          }

        },

        autoPublish: true

      });

      console.log(
        "Joined Hayavo Meet"
      );
    }


    document
      .getElementById("join")
      .onclick = start;


    document
      .getElementById("mic")
      .onclick = async () => {

        await client.media.toggleMic();

      };


    document
      .getElementById("camera")
      .onclick = async () => {

        await client.media.toggleCamera();

      };


    document
      .getElementById("leave")
      .onclick = async () => {

        await client.leave();

      };


    client.events.on(
      "participantConnected",
      participant => {

        console.log(
          "Participant joined:",
          participant
        );

      }
    );


    client.events.on(
      "participantDisconnected",
      participant => {

        console.log(
          "Participant left:",
          participant
        );

      }
    );


    client.events.on(
      "reconnecting",
      () => {

        console.log(
          "Connection lost. Reconnecting..."
        );

      }
    );


    client.events.on(
      "reconnected",
      () => {

        console.log(
          "Connection restored."
        );

      }
    );

  </script>

</body>

</html>
```

> **Important:** The room token must be generated by your backend. Never put your Hayavo API secret, app secret, or other server credentials in browser code.

---

# Recommended Application Architecture

A production Hayavo Meet application should separate browser and server responsibilities.

```text
                         Your Application
                               │
              ┌────────────────┴────────────────┐
              │                                 │
          Browser                           Backend
              │                                 │
              │ hayavo-meet-client              │ hayavo-meet
              │                                 │
              │ RTC / RTM                      │ API
              │                                 │ Token generation
              │                                 │ Room management
              │                                 │ Authentication
              │                                 │ Server operations
              │                                 │
              └───────────────┬─────────────────┘
                              │
                         Hayavo Meet
                              │
                 ┌────────────┴────────────┐
                 │                         │
                RTC                       RTM
                 │                         │
              Video/Audio          Chat/Presence/
              Screen Share          Interactions
```

---

# Python SDK

Hayavo also provides a Python SDK for backend/server-side integration.

Install:

```bash
pip install hayavo-meet
```

Example:

```python
from hayavo_meet import HayavoClient

client = HayavoClient(
    api_key="YOUR_API_KEY",
    app_id="YOUR_APP_ID",
    app_secret="YOUR_APP_SECRET",
    app_platform="web",
    app_identifier="my-application"
)
```

The Python SDK is intended for backend operations such as:

* Room management
* Room creation
* Host token generation
* Guest token generation
* Server-side Hayavo Meet API operations
* Application authentication

Keep API secrets in your server environment and never expose them to browser clients.

---

# Environment Variables

For backend applications, credentials should normally be provided through environment variables.

Example:

```bash
HAYAVO_API_KEY=your_api_key
HAYAVO_APP_ID=your_app_id
HAYAVO_APP_SECRET=your_app_secret
```

Do not commit credentials to Git repositories.

---

# Package Information

Package:

```text
hayavo-meet-client
```

Current version:

```text
1.1.2
```

License:

```text
MIT
```

Homepage:

```text
https://meet.hayavo.com
```

---

# Clone Repository

Clone the repository:

```bash
git clone https://github.com/hayavo/hayavo-meet-client.git

```

---

# CDN Production Build

The recommended browser production build is:

```text
https://cdn.jsdelivr.net/npm/hayavo-meet-client@1.1.2/dist/hayavo-meet-client.umd.min.js
```

Example:

```html
<script src="https://cdn.jsdelivr.net/npm/hayavo-meet-client@1.1.2/dist/hayavo-meet-client.umd.min.js"></script>
```

---

# Browser Requirements

Hayavo Meet uses modern browser WebRTC and WebSocket capabilities.

The application requires browser support for features such as:

* WebRTC
* WebSocket
* MediaDevices
* getUserMedia
* Screen Capture API for screen sharing
* Modern JavaScript APIs

Camera and microphone access normally requires a secure context such as HTTPS.

---

# Security

Never expose the following in browser-side JavaScript:

```text
API Secret
App Secret
Private credentials
Server authentication credentials
```

Only short-lived, appropriately scoped client tokens should be provided to the browser.

Recommended architecture:

```text
Browser
   │
   │ authenticated request
   ▼
Your Backend
   │
   │ API credentials
   ▼
Hayavo Meet
   │
   │ temporary token
   ▼
Browser
```

---

# License

Copyright © Hayavo.

Released under the MIT License.
