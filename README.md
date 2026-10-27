# Group Voice

A free group voice chat app for Android (like Discord / in-game voice), built to work with **Bluetooth A2DP headphones**: your voice is recorded with the phone's own mic and everything you hear plays in normal high-quality stereo-headphone mode (no low-quality "call mode"). It works over the internet through a small relay server that you host for free.

> Built entirely on a phone with the AIDE app.

## Features

- **Group rooms over the internet**: type a room name, press **Join**; everyone who uses the same room name and server is in the same call.
- **Bluetooth A2DP friendly**: plays as media, records from the phone mic, so Bluetooth headphones keep their normal sound quality.
- **Clear voice**: Opus voice codec at 48 kHz, about 33 kbps per speaker, so it works on mobile data and weak Wi-Fi.
- **Noise removal**: RNNoise removes fans, AC and background noise, plus a smart noise gate that blocks taps, rubbing and scratching on the phone.
- **Adaptive jitter buffer**: grows automatically when the network is unstable to avoid choppy voice, and shrinks again to keep the delay low. The status line shows buffer size and gap counts.
- **Member list**: see who is in the room by name, with a glowing outline around whoever is speaking.
- **Mute / Unmute** from the app or from the notification, and a **Leave** button in the notification.
- **Join / leave sounds** when someone enters or leaves the room; people who leave are removed from the list quickly.
- **Voice volume 0-100** (set it lower than the game volume) and **Mic sensitivity** buttons.
- **Keeps working in the background**: foreground service with a notification, so the call continues with the screen off or while you play a game.
- **Settings**: server address, port and your display name.

## How to use the app

1. Install the APK (allow "install from unknown sources" if Android asks) and open the app.
2. Tap the **gear icon**. Enter your **server** (the address of your relay, see below), the **port** (`443` for a Render server), and your **display name**. Press back or Save.
3. Allow the **microphone** permission (and notifications on Android 13+, so you get the Mute / Leave buttons).
4. Type a **room name** and press **Join**. Share the same room name, server and port with your friends.
5. During the call:
   - **Mute / Unmute** your mic (also available in the notification).
   - **Mic sensitivity** `-` / `+`: raise it if people can't hear you, lower it if you are too loud.
   - **Voice volume** `-` / `+` (0-100): how loud the other people are. Tip: set your game's media volume first, then lower the voice volume until the voices sit nicely under the game.
   - Press the red **Leave** button (the Join button turns into Leave), or **Leave** in the notification, to end the call.
6. Use headphones to avoid echo.

## Server setup

The app needs a small relay server. It only forwards audio between the people in a room (nothing is recorded or stored). The server is two files inside **Server-setup.zip**: `server.js` and `package.json`. The steps below use the free **Render** hosting plan. You need a free GitHub account and a free Render account.

### 1. Put the server files on GitHub

1. Unzip **Server-setup.zip**. You get `server.js` and `package.json`.
2. On GitHub create a new repository (for example `voice-server`).
3. Upload `server.js` and `package.json` to the **root** of the repository (**Add file -> Upload files**), then commit.

### 2. Deploy on Render

1. Sign in at <https://render.com> and choose **New -> Web Service**.
2. Connect your GitHub account and select the repository from step 1.
3. Fill in:
   - **Language / Runtime:** Node
   - **Build Command:** `npm install`
   - **Start Command:** `npm start` (same as `node server.js`)
   - **Instance Type:** Free
4. Press **Create Web Service** and wait until the deploy finishes. Render shows your address, like `https://voice-server-abcd.onrender.com`. Opening it in a browser should show `voice relay ok`.

### 3. Connect the app

In the app: **Settings -> Server** = your address without `https://` (for example `voice-server-abcd.onrender.com`), **Port** = `443`. Port 443 uses a secure connection (wss); any other port uses plain ws.

### Notes

- Render's free plan **goes to sleep after about 15 minutes without use**. The first Join after that can take up to a minute while the server wakes up. After that it is fast.
- Free servers are shared, so network speed can vary; the app's jitter buffer is there to cope with that.
- The server forwards audio only. Everyone in a room must use the same server and port.

### Other ways to run the server (optional)

On a PC (or a phone with Termux), install Node.js 18 or newer, then in the folder with the two files run:

```
npm install
node server.js
```

It listens on port 8080 (or the `PORT` environment variable). In the app use your machine's IP address and port `8080`. For use over the internet you need to forward that port on your router or use a hosting service.

## Build from source

The project is a normal Android project (package `com.rega.groupvoice`) that I build in AIDE. The native libraries (Opus and RNNoise) are compiled from the `jni` folder with `Android.mk` / `Application.mk` for `armeabi-v7a` and `arm64-v8a`.

## Credits and third-party software

This app stands on the work of others. Thank you to:

- **Opus codec** (libopus 1.4): Xiph.Org Foundation, Skype Limited, Octasic, Jean-Marc Valin, Timothy B. Terriberry, CSIRO, Gregory Maxwell, Mark Borgerding, Erik de Castro Lopo and the Opus contributors. BSD 3-clause license. <https://opus-codec.org>
- **RNNoise** (noise suppression): Jean-Marc Valin, Xiph.Org Foundation and Mozilla (copyright 2017 Mozilla, 2007-2017 Jean-Marc Valin, 2005-2017 Xiph.Org Foundation, 2003-2004 Mark Borgerding). BSD 3-clause license. <https://github.com/xiph/rnnoise>
- **ws** (WebSocket library used by the server): Einar Otto Stangvik, Luigi Pinca and contributors. MIT license. <https://github.com/websockets/ws>
- **Node.js** runtime for the relay server: <https://nodejs.org>
- **Render** for the free hosting used in the setup guide: <https://render.com>
- **AIDE** (Android IDE) used to build the app.
- Parts of the code were written with the help of **Claude**, an AI assistant by Anthropic.


## License

Proprietary License

Copyright (c) 2026 [Soyam Vishwakarma]

All rights reserved.

The original source code and associated materials of this application are proprietary. No permission is granted to copy, modify, distribute, or reuse these materials without prior written permission from the copyright holder.

Third-party components remain subject to their respective licenses. See the relevant third-party license notices for details.
