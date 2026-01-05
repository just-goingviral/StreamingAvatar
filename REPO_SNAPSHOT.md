# Repository Snapshot - StreamingAvatar

This document contains a complete snapshot of all files in the StreamingAvatar repository.

**Generated on:** 2026-01-05

---

## Table of Contents

1. [.gitignore](#gitignore)
2. [.prettierrc](#prettierrc)
3. [README.md](#readmemd)
4. [package.json](#packagejson)
5. [index.html](#indexhtml)
6. [index.css](#indexcss)
7. [index.js](#indexjs)
8. [server.js](#serverjs)
9. [Binary Files](#binary-files)

---

## .gitignore

```gitignore
node_modules
.idea
.vscode
```

---

## .prettierrc

```json
{
  "singleQuote": true,
  "jsxSingleQuote": false,
  "trailingComma": "all",
  "printWidth": 100,
  "useTabs": false,
  "tabWidth": 2,
  "semi": true,
  "quoteProps": "as-needed",
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "always",
  "singleAttributePerLine": false,
  "overrides": [
    {
      "files": ".prettierrc",
      "options": { "parser": "json" }
    }
  ]
}
```

---

## README.md

```markdown
# Streaming Avatar Demo

NEW: We have an SDK now! The SDK is an NPM package that you can easily add to your website and use for Streaming Avatar functionality. Please check it out at https://www.npmjs.com/package/@heygen/streaming-avatar

We also have an example React app demonstrating the SDK's functionality. That is located at: https://github.com/HeyGen-Official/StreamingAvatarTSDemo

## Introduction

This HeyGen Streaming Avatar demo is a starting point from which developers can adapt and build streaming sessions into their own websites and experiences.

Below, we have outlined a few questions that frequently pop up when users first try out this demo. Below that FAQ, you will find directions for installing and running this demo.

## Getting Started FAQ

### How do I get an API Key?

Either an an API Key or Trial Token from HeyGen is required to run this Streaming API demo. API Keys are reserved for Enterprise customers, whereas both Creator and Teams plan users can activate and use a Trial token. You can retrieve either the API Key or Trial Token by logging in to HeyGen and navigating to this page in your settings: https://app.heygen.com/settings?nav=API

### Which Avatars can I use with this project?

By default, there are several Public Avatars that can be used in Streaming. (AKA Streaming Avatars.) You can find the Avatar IDs for these Public Avatars by navigating to app.heygen.com/streaming-avatar and clicking 'Select Avatar'.

In order to use a private Avatar created under your own account in Streaming, it must be upgraded to be a Streaming Avatar. Only 1. Finetune Instant Avatars and 2. Studio Avatars are able to be upgraded to Streaming Avatars. This upgrade is a one-time fee and can be purchased by navigating to app.heygen.com/streaming-avatar and clicking 'Select Avatar'.

Photo Avatars are not compatible with Streaming and cannot be used.

### Which voices can I use?

Most of HeyGen's AI Voices can be used with the Streaming API. To find the Voice IDs that you can use, please use the List Voices v2 endpoint from HeyGen: https://docs.heygen.com/reference/list-voices-v2

### Why am I encountering issues with testing?

Most likely, you are hitting your concurrent session limit. While testing the Streaming API, your account is limited to 3 concurrent sessions. Please endeavor to close unused Streaming sessions with the Close Session endpoint when they are no longer being used; they will automatically close after some minutes.

You can check how many active sessions you have open with the List Sessions endpoint: https://docs.heygen.com/reference/list-sessions

## Installing the Demo

1. Clone the repository.

   ```
   git clone https://github.com/HeyGen-Official/StreamingAvatar.git
   ```

2. Open the `index.js` file and replace `'YourApiKey'` with your API key:

   ```
   "apiKey": "YourApiKey";
   ```
3. (optional) Open the `server.js` file and set your OpenAI API key to use talk mode:
   ```
   const openai = new OpenAI({
     apiKey: "<your openai api key>",
   });
   ```

4. open a terminal in the folder and then install the express and run the server.js:

   ```
   npm install express
   node server.js
   ```

5. you will see `App is listening on port 3000!`.

## Using the Demo

1. Open the web browser and enter the `http://localhost:3000`to start the demo.
2. Click the "New" button to create a new session. The status updates will be displayed on the screen.
3. After the session is created successfully, click the "Start" button to start streaming the avatar.
4. To send a task to the avatar, type the text in the provided input field and click the "Repeat Text" button.
5. In order to use Talk mode, set your **OpenAI** key in **server.js** before starting the server and click "Talk" button
6. Once done, click the "Close Connection" button to close the session.

Remember, this is a demo and should be modified according to your needs and preferences. Happy coding!

## Troubleshooting

In case you face any issues while running the demo or have any questions, feel free to raise an issue in this repository or contact our support team.

Please note, if you encounter a "Server Error", it could be due to the server being offline. In such cases, please contact the service provider.
```

---

## package.json

```json
{
  "name": "streamingavatar",
  "version": "1.0.0",
  "description": "## Pre-Requisites",
  "main": "index.js",
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.2",
    "openai": "^4.28.0"
  },
  "devDependencies": {
    "prettier": "^3.2.4"
  }
}
```

---

## index.html

```html
<!doctype html>
<html>
  <head>
    <link rel="stylesheet" href="index.css" />
    <link rel="icon" href="./favicon.ico" />
  </head>

  <body>
    <div class="main">
      <div class="actionRowsWrap">
        <div class="actionRow">
          <label>
            AvatarID
            <input id="avatarID" type="text" />
          </label>

          <label>
            VoiceID
            <input id="voiceID" type="text" />
          </label>

          <button id="newBtn">New</button>
          <button id="startBtn">Start</button>
          <button id="closeBtn">Close</button>
        </div>

        <div class="actionRow">
          <label>
            Message
            <input id="taskInput" type="text" />
          </label>
          <button id="repeatBtn">Repeat</button>
          <button id="talkBtn">Talk</button>

        </div>
      </div>

      <p id="status"></p>

      <div class="videoSectionWrap">
        <div class="videoWrap">
          <video id="mediaElement" class="videoEle show" autoplay></video>
          <canvas id="canvasElement" class="videoEle hide"></canvas>
        </div>

        <div class="actionRow switchRow hide" id="bgCheckboxWrap">
          <div class="switchWrap">
            <span>Remove background</span>
            <label class="switch">
              <input type="checkbox" id="removeBGCheckbox" />
              <span class="slider round"></span>
            </label>
          </div>

          <label>
            Background (CSS)
            <input
              type="text"
              id="bgInput"
              value='url("https://app.heygen.com/icons/heygen/logo_hori_text_light_bg.svg") center / contain no-repeat'
            />
          </label>
        </div>
      </div>
    </div>

    <script type="module" src="index.js"></script>
  </body>
</html>
```

---

## index.css

```css
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  background-color: #f5f5f5;
  padding: 24px;
}

.main {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.actionRowsWrap {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.actionRow {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: 12px;
}

.actionRow label {
  display: flex;
  align-items: center;
  gap: 8px;
}

button {
  display: inline-block;
  padding: 0 16px;
  border-radius: 8px;
  height: 40px;

  background-color: #4caf50; /* Green */
  border: none;

  color: #fff;
  text-align: center;
  font-size: 16px;

  cursor: pointer;
  transition-duration: 0.4s;
}

button:hover {
  background-color: #45a049;
}

input {
  height: 40px;
  padding: 0 12px;

  font-size: 16px;
}

#status {
  overflow: auto;

  background-color: #fff;
  height: 120px;
  padding: 10px 12px;

  border: 1px solid #ccc;
  border-radius: 8px;

  font-size: 14px;
  line-height: 1.6;
}

.videoSectionWrap {
  position: relative;

  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
}

.actionRow.switchRow {
  width: 100%;

  justify-content: center;
}
.switchRow {
  flex-direction: column;
}
.switchRow > label {
  width: 100%;

  display: flex;
  justify-content: center;
}

.switchRow > label input {
  flex: 1;
  max-width: 500px;
}

.videoSectionWrap .videoWrap {
  display: flex;
  justify-content: center;
  align-items: center;

  /*background: linear-gradient(0deg, rgba(0, 0, 0, 0.02) 0%, rgba(0, 0, 0, 0.02) 100%),*/
  /*  radial-gradient(*/
  /*    108.09% 141.42% at 0% 100%,*/
  /*    rgba(219, 255, 213, 0.3) 0%,*/
  /*    rgba(255, 255, 255, 0) 100%*/
  /*  ),*/
  /*  linear-gradient(135deg, #ffeede 5.71%, #ffd9d9 47%, #a3dce7 93.47%);*/
}

.videoWrap .videoEle {
  width: 100%;
  max-height: 400px;
}

/*---------- Switch START ----------*/
.switchWrap {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 8px;
}

.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

.slider:before {
  position: absolute;
  content: '';
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

input:checked + .slider {
  background-color: #2196f3;
}

input:focus + .slider {
  box-shadow: 0 0 1px #2196f3;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
/*---------- Switch END ----------*/

.videoSectionWrap .hide {
  display: none;
}

.videoSectionWrap .show {
  display: flex;
}

.hide {
  display: none;
}
.show {
  display: flex;
}
```

---

## index.js

```javascript
'use strict';

const heygen_API = {
  apiKey: 'YourApiKey',
  serverUrl: 'https://api.heygen.com',
};

const statusElement = document.querySelector('#status');
const apiKey = heygen_API.apiKey;
const SERVER_URL = heygen_API.serverUrl;

if (apiKey === 'YourApiKey' || SERVER_URL === '') {
  alert('Please enter your API key and server URL in the api.json file');
}

let sessionInfo = null;
let peerConnection = null;

function updateStatus(statusElement, message) {
  statusElement.innerHTML += message + '<br>';
  statusElement.scrollTop = statusElement.scrollHeight;
}

updateStatus(statusElement, 'Please click the new button to create the stream first.');

function onMessage(event) {
  const message = event.data;
  console.log('Received message:', message);
}

// Create a new WebRTC session when clicking the "New" button
async function createNewSession() {
  updateStatus(statusElement, 'Creating new session... please wait');

  const avatar = avatarID.value;
  const voice = voiceID.value;

  // call the new interface to get the server's offer SDP and ICE server to create a new RTCPeerConnection
  sessionInfo = await newSession('low', avatar, voice);
  const { sdp: serverSdp, ice_servers2: iceServers } = sessionInfo;

  // Create a new RTCPeerConnection
  peerConnection = new RTCPeerConnection({ iceServers: iceServers });

  // When audio and video streams are received, display them in the video element
  peerConnection.ontrack = (event) => {
    console.log('Received the track');
    if (event.track.kind === 'audio' || event.track.kind === 'video') {
      mediaElement.srcObject = event.streams[0];
    }
  };

  // When receiving a message, display it in the status element
  peerConnection.ondatachannel = (event) => {
    const dataChannel = event.channel;
    dataChannel.onmessage = onMessage;
  };

  // Set server's SDP as remote description
  const remoteDescription = new RTCSessionDescription(serverSdp);
  await peerConnection.setRemoteDescription(remoteDescription);

  updateStatus(statusElement, 'Session creation completed');
  updateStatus(statusElement, 'Now.You can click the start button to start the stream');
}

// Start session and display audio and video when clicking the "Start" button
async function startAndDisplaySession() {
  if (!sessionInfo) {
    updateStatus(statusElement, 'Please create a connection first');
    return;
  }

  updateStatus(statusElement, 'Starting session... please wait');

  // Create and set local SDP description
  const localDescription = await peerConnection.createAnswer();
  await peerConnection.setLocalDescription(localDescription);

 // When ICE candidate is available, send to the server
  peerConnection.onicecandidate = ({ candidate }) => {
    console.log('Received ICE candidate:', candidate);
    if (candidate) {
      handleICE(sessionInfo.session_id, candidate.toJSON());
    }
  };

  // When ICE connection state changes, display the new state
  peerConnection.oniceconnectionstatechange = (event) => {
    updateStatus(
      statusElement,
      `ICE connection state changed to: ${peerConnection.iceConnectionState}`,
    );
  };



  // Start session
  await startSession(sessionInfo.session_id, localDescription);

  var receivers = peerConnection.getReceivers();
  
  receivers.forEach((receiver) => {
    receiver.jitterBufferTarget = 500
  });

   updateStatus(statusElement, 'Session started successfully');
}

const taskInput = document.querySelector('#taskInput');

// When clicking the "Send Task" button, get the content from the input field, then send the tas
async function repeatHandler() {
  if (!sessionInfo) {
    updateStatus(statusElement, 'Please create a connection first');

    return;
  }
  updateStatus(statusElement, 'Sending task... please wait');
  const text = taskInput.value;
  if (text.trim() === '') {
    alert('Please enter a task');
    return;
  }

  const resp = await repeat(sessionInfo.session_id, text);

  updateStatus(statusElement, 'Task sent successfully');
}

async function talkHandler() {
  if (!sessionInfo) {
    updateStatus(statusElement, 'Please create a connection first');
    return;
  }
  const prompt = taskInput.value; // Using the same input for simplicity
  if (prompt.trim() === '') {
    alert('Please enter a prompt for the LLM');
    return;
  }

  updateStatus(statusElement, 'Talking to LLM... please wait');

  try {
    const text = await talkToOpenAI(prompt)

    if (text) {
      // Send the AI's response to Heygen's streaming.task API
      const resp = await repeat(sessionInfo.session_id, text);
      updateStatus(statusElement, 'LLM response sent successfully');
    } else {
      updateStatus(statusElement, 'Failed to get a response from AI');
    }
  } catch (error) {
    console.error('Error talking to AI:', error);
    updateStatus(statusElement, 'Error talking to AI');
  }
}


// when clicking the "Close" button, close the connection
async function closeConnectionHandler() {
  if (!sessionInfo) {
    updateStatus(statusElement, 'Please create a connection first');
    return;
  }

  renderID++;
  hideElement(canvasElement);
  hideElement(bgCheckboxWrap);
  mediaCanPlay = false;

  updateStatus(statusElement, 'Closing connection... please wait');
  try {
    // Close local connection
    peerConnection.close();
    // Call the close interface
    const resp = await stopSession(sessionInfo.session_id);

    console.log(resp);
  } catch (err) {
    console.error('Failed to close the connection:', err);
  }
  updateStatus(statusElement, 'Connection closed successfully');
}

document.querySelector('#newBtn').addEventListener('click', createNewSession);
document.querySelector('#startBtn').addEventListener('click', startAndDisplaySession);
document.querySelector('#repeatBtn').addEventListener('click', repeatHandler);
document.querySelector('#closeBtn').addEventListener('click', closeConnectionHandler);
document.querySelector('#talkBtn').addEventListener('click', talkHandler);


// new session
async function newSession(quality, avatar_name, voice_id) {
  const response = await fetch(`${SERVER_URL}/v1/streaming.new`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'X-Api-Key': apiKey,
    },
    body: JSON.stringify({
      quality,
      avatar_name,
      voice: {
        voice_id: voice_id,
      },
    }),
  });
  if (response.status === 500) {
    console.error('Server error');
    updateStatus(
      statusElement,
      'Server Error. Please ask the staff if the service has been turned on',
    );

    throw new Error('Server error');
  } else {
    const data = await response.json();
    console.log(data.data);
    return data.data;
  }
}

// start the session
async function startSession(session_id, sdp) {
  const response = await fetch(`${SERVER_URL}/v1/streaming.start`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'X-Api-Key': apiKey,
    },
    body: JSON.stringify({ session_id, sdp }),
  });
  if (response.status === 500) {
    console.error('Server error');
    updateStatus(
      statusElement,
      'Server Error. Please ask the staff if the service has been turned on',
    );
    throw new Error('Server error');
  } else {
    const data = await response.json();
    return data.data;
  }
}

// submit the ICE candidate
async function handleICE(session_id, candidate) {
  const response = await fetch(`${SERVER_URL}/v1/streaming.ice`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'X-Api-Key': apiKey,
    },
    body: JSON.stringify({ session_id, candidate }),
  });
  if (response.status === 500) {
    console.error('Server error');
    updateStatus(
      statusElement,
      'Server Error. Please ask the staff if the service has been turned on',
    );
    throw new Error('Server error');
  } else {
    const data = await response.json();
    return data;
  }
}

async function talkToOpenAI(prompt) {
  const response = await fetch(`http://localhost:3000/openai/complete`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({ prompt }),
  });
  if (response.status === 500) {
    console.error('Server error');
    updateStatus(
      statusElement,
      'Server Error. Please make sure to set the openai api key',
    );
    throw new Error('Server error');
  } else {
    const data = await response.json();
    return data.text;
  }
}

// repeat the text
async function repeat(session_id, text) {
  const response = await fetch(`${SERVER_URL}/v1/streaming.task`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'X-Api-Key': apiKey,
    },
    body: JSON.stringify({ session_id, text }),
  });
  if (response.status === 500) {
    console.error('Server error');
    updateStatus(
      statusElement,
      'Server Error. Please ask the staff if the service has been turned on',
    );
    throw new Error('Server error');
  } else {
    const data = await response.json();
    return data.data;
  }
}

// stop session
async function stopSession(session_id) {
  const response = await fetch(`${SERVER_URL}/v1/streaming.stop`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'X-Api-Key': apiKey,
    },
    body: JSON.stringify({ session_id }),
  });
  if (response.status === 500) {
    console.error('Server error');
    updateStatus(statusElement, 'Server Error. Please ask the staff for help');
    throw new Error('Server error');
  } else {
    const data = await response.json();
    return data.data;
  }
}

const removeBGCheckbox = document.querySelector('#removeBGCheckbox');
removeBGCheckbox.addEventListener('click', () => {
  const isChecked = removeBGCheckbox.checked; // status after click

  if (isChecked && !sessionInfo) {
    updateStatus(statusElement, 'Please create a connection first');
    removeBGCheckbox.checked = false;
    return;
  }

  if (isChecked && !mediaCanPlay) {
    updateStatus(statusElement, 'Please wait for the video to load');
    removeBGCheckbox.checked = false;
    return;
  }

  if (isChecked) {
    hideElement(mediaElement);
    showElement(canvasElement);

    renderCanvas();
  } else {
    hideElement(canvasElement);
    showElement(mediaElement);

    renderID++;
  }
});

let renderID = 0;
function renderCanvas() {
  if (!removeBGCheckbox.checked) return;
  hideElement(mediaElement);
  showElement(canvasElement);

  canvasElement.classList.add('show');

  const curRenderID = Math.trunc(Math.random() * 1000000000);
  renderID = curRenderID;

  const ctx = canvasElement.getContext('2d', { willReadFrequently: true });

  if (bgInput.value) {
    canvasElement.parentElement.style.background = bgInput.value?.trim();
  }

  function processFrame() {
    if (!removeBGCheckbox.checked) return;
    if (curRenderID !== renderID) return;

    canvasElement.width = mediaElement.videoWidth;
    canvasElement.height = mediaElement.videoHeight;

    ctx.drawImage(mediaElement, 0, 0, canvasElement.width, canvasElement.height);
    ctx.getContextAttributes().willReadFrequently = true;
    const imageData = ctx.getImageData(0, 0, canvasElement.width, canvasElement.height);
    const data = imageData.data;

    for (let i = 0; i < data.length; i += 4) {
      const red = data[i];
      const green = data[i + 1];
      const blue = data[i + 2];

      // You can implement your own logic here
      if (isCloseToGreen([red, green, blue])) {
        // if (isCloseToGray([red, green, blue])) {
        data[i + 3] = 0;
      }
    }

    ctx.putImageData(imageData, 0, 0);

    requestAnimationFrame(processFrame);
  }

  processFrame();
}

function isCloseToGreen(color) {
  const [red, green, blue] = color;
  return green > 90 && red < 90 && blue < 90;
}

function hideElement(element) {
  element.classList.add('hide');
  element.classList.remove('show');
}
function showElement(element) {
  element.classList.add('show');
  element.classList.remove('hide');
}

const mediaElement = document.querySelector('#mediaElement');
let mediaCanPlay = false;
mediaElement.onloadedmetadata = () => {
  mediaCanPlay = true;
  mediaElement.play();

  showElement(bgCheckboxWrap);
};
const canvasElement = document.querySelector('#canvasElement');

const bgCheckboxWrap = document.querySelector('#bgCheckboxWrap');
const bgInput = document.querySelector('#bgInput');
bgInput.addEventListener('keydown', (e) => {
  if (e.key === 'Enter') {
    renderCanvas();
  }
});
```

---

## server.js

```javascript
const express = require('express');
const OpenAI = require('openai')
const path = require('path');
const app = express();
app.use(express.json());


const openai = new OpenAI({
  apiKey: "<your openai api key>",
});

const systemSetup = "you are a demo streaming avatar from HeyGen, an industry-leading AI generation product that specialize in AI avatars and videos.\nYou are here to showcase how a HeyGen streaming avatar looks and talks.\nPlease note you are not equipped with any specific expertise or industry knowledge yet, which is to be provided when deployed to a real customer's use case.\nAudience will try to have a conversation with you, please try answer the questions or respond their comments naturally, and concisely. - please try your best to response with short answers, limit to one sentence per response, and only answer the last question."

app.use(express.static(path.join(__dirname, '.')));

app.post('/openai/complete', async (req, res) => {
  try {
    const prompt = req.body.prompt;
    const chatCompletion = await openai.chat.completions.create({
      messages: [
        { role: 'system', content: systemSetup},
        { role: 'user', content: prompt }
      ],
      model: 'gpt-3.5-turbo',
    });
    res.json({ text: chatCompletion.choices[0].message.content });
  } catch (error) {
    console.error('Error calling OpenAI:', error);
    res.status(500).send('Error processing your request');
  }
});

app.listen(3000, function () {
  console.log('App is listening on port 3000!');
});
```

---

## Binary Files

The following binary files are present in the repository:

### favicon.ico
- **Type:** MS Windows icon resource - 1 icon, 64x64 with PNG image data
- **Format:** 64 x 64, 8-bit/color RGBA, non-interlaced, 32 bits/pixel
- **Note:** Binary files cannot be represented as text in this snapshot

---

## Repository Statistics

- **Total Text Files:** 8
- **Total Binary Files:** 1
- **Languages:** JavaScript, HTML, CSS, JSON
- **Main Components:**
  - Frontend: HTML/CSS/JavaScript streaming avatar interface
  - Backend: Express.js server with OpenAI integration
  - Configuration: Prettier, Git ignore rules

---

**End of Repository Snapshot**
