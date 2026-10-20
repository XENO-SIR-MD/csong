# XENO EXE — Csong Plugin

<p align="center">
  <img src="https://img.shields.io/badge/Developer-XENO%20EXE-00D9FF?style=for-the-badge" alt="Developer"/>
  <img src="https://img.shields.io/badge/Platform-WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white" alt="WhatsApp"/>
  <img src="https://img.shields.io/badge/Runtime-Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="Node.js"/>
  <img src="https://img.shields.io/badge/Plugin-CSong-blueviolet?style=for-the-badge" alt="CSong"/>
</p>

<p align="center">
  <b> Send songs directly to WhatsApp Channels as voice notes!</b>
</p>

---

##  Overview

**XENO EXE Csong** is a WhatsApp bot plugin that lets you search for songs, download audio, convert it into voice-note format, and forward it directly to a WhatsApp Channel.

It also supports forwarding audio messages by replying to them with the `.csong` command.

##  Features

- Search songs by name or YouTube URL.
- Download song audio using a configured download API.
- Convert audio into Opus voice-note format using FFmpeg.
- Generate waveform data for voice-note messages.
- Send audio directly to WhatsApp Channels.
- Post a stylish music-player text card in the channel.
- Forward audio messages by replying to them.
- Display song title, artist, duration, and source URL when available.
- Display reaction updates while processing.
- Automatically clean up temporary audio files.
- Prevent duplicate processing of the same message for a short period.

##  Requirements

Before installing, make sure you have:

- [Node.js](https://nodejs.org/) installed.
- [FFmpeg](https://ffmpeg.org/) installed and available in your system PATH.
- A working WhatsApp bot using Baileys.
- The project's existing `config.js` file.
- The required npm packages.

## Installation

### 1. Install dependencies

Run this command in your bot's project directory:

```bash
npm install axios fs-extra
```

The plugin also uses Node.js built-in modules, so those do not need separate installation.

### 2. Install FFmpeg

Download FFmpeg from its official website:

https://ffmpeg.org/download.html

Make sure the `ffmpeg` command works in your terminal:

```bash
ffmpeg -version
```

### 3. Add the plugin

Save the plugin code as:

```text
plugins/csong.js
```

Adjust the file path if your bot uses a different plugin directory structure.

### 4. Check the configuration

The plugin imports your bot configuration:

```javascript
const config = require("../config");
```

Ensure the path is correct and that your configuration exports the expected settings, such as `botName`.

### 5. Restart your bot

Restart your WhatsApp bot using the start command configured in your project, for example:

```bash
npm start
```

##  Usage

### Method 1: Download a song by name

```text
.csong CHANNEL_LINK , SONG_NAME
```

Example:

```text
.csong https://whatsapp.com/channel/YOUR_CHANNEL_CODE , Faded
```

### Method 2: Download using a YouTube URL

```text
.csong CHANNEL_LINK , YOUTUBE_URL
```

Example:

```text
.csong https://whatsapp.com/channel/YOUR_CHANNEL_CODE , https://www.youtube.com/watch?v=60ItHLz5WEA
```

### Method 3: Forward an existing audio message

1. Reply to an audio message.
2. Enter the command with your WhatsApp Channel link.

```text
.csong https://whatsapp.com/channel/YOUR_CHANNEL_CODE
```

The plugin attempts to convert the replied audio into a voice note and send it to the channel.

> **Note:** Replace `YOUR_CHANNEL_CODE` with your actual channel invitation code.

##  How It Works

1. Receives the `.csong` command.
2. Extracts the WhatsApp Channel link and song query.
3. Resolves the channel identifier through Baileys newsletter metadata.
4. Searches for the song or downloads the replied audio.
5. Downloads the audio into a temporary directory.
6. Uses FFmpeg to convert the audio into Opus format.
7. Generates waveform data for the voice note.
8. Sends the audio to the WhatsApp Channel.
9. Replies to the audio with a styled song-information card.
10. Removes temporary files after processing.

## API Dependencies

The current plugin uses external APIs for song searching and audio downloading.

| Service | Purpose |
|---|---|
| YouTube Search API | Finds songs by name or URL |
| YouTube Download API | Retrieves downloadable MP3 audio |
| FFmpeg | Converts audio into Opus voice-note format |
| Baileys | Connects the bot to WhatsApp and sends channel messages |

**Important:** External APIs may change, become unavailable, impose limits, or return different response formats. Update the API handling when necessary.

## Suggested Project Structure

```text
XENO-BOT/
├── plugins/
│   └── csong.js
├── config.js
├── package.json
├── index.js
└── tmp/
```

The `tmp` directory is created automatically by the plugin when required.

##  Troubleshooting

### FFmpeg not found

Install FFmpeg and add its executable directory to your PATH.

### Invalid WhatsApp Channel link

Check that the channel invitation link is correct and that your Baileys version supports the newsletter metadata methods used by the plugin.

### Song not found

Try a different song title or a valid YouTube URL. The search API must return a compatible `results` array.

### Audio download failed

Check the external download API and confirm that it returns a valid downloadable URL.

### Voice note does not display correctly

Verify that FFmpeg is installed and that the output is valid Opus audio. WhatsApp and Baileys may handle waveform metadata differently depending on the version.

### Bot cannot send to a channel

Confirm that the bot connection supports newsletter messages and has the permissions needed to post to the selected channel.

## Security & Best Practices

- Never publish WhatsApp session credentials or private API keys.
- Keep your Baileys session files private.
- Validate input links and API responses.
- Use temporary-file cleanup and appropriate
