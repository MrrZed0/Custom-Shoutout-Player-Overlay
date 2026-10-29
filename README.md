# 🎬 Streamer.bot Custom Shoutout Player & Studio Overlay

![Platform](https://img.shields.io/badge/Platform-Twitch%20%7C%20Kick%20%7C%20YouTube-9146ff?style=for-the-badge)
![Integration](https://img.shields.io/badge/Powered%20By-Streamer.bot-00f0ff?style=for-the-badge)
![OBS Studio](https://img.shields.io/badge/OBS-Browser%20Source-302e31?style=for-the-badge)
![Borders](https://img.shields.io/badge/Custom%20Borders-120%2B%20Presets-ff007f?style=for-the-badge)

A two-phase **Shoutout Alert & Auto-Playing Twitch Clip Overlay** built specifically for **Streamer.bot** and **OBS Studio**. Designed with a visual **Customizer Studio** featuring **120+ procedural animated borders**, real-time previewing, and zero manual API token configuration.

---

## ✨ Features

* **🔒 100% Streamer.bot Native (Zero Manual Tokens):** Automatically pulls user avatars, display names, last played game, and latest Twitch clips directly through Streamer.bot's built-in methods and WebSocket server.
* **⚡ Strict Action Activation:** The overlay remains 100% hidden and idle until triggered by your Streamer.bot C# action (`CustomShoutoutTrigger`).
* **🎭 Two-Phase Shoutout Sequence:**
  1. **Phase 1 (Shoutout Alert Pill):** Displays an animated alert card featuring the user's profile picture (supports **Twitch, Kick, and YouTube**), platform badge, and custom shoutout text for a configurable duration (default: `10s`).
  2. **Phase 2 (Custom Border + Auto-Playing Clip):** As soon as the alert exits, your chosen custom frame smoothly transitions in and auto-plays the user's **latest Twitch clip** with a streamer info tag and countdown bar.
* **🎨 120+ Custom Borders in `customizer.html`:** Choose from 120 built-in border combinations across 6 style categories (*Neon & Cyber, Gaming & HUD, Fire & Elemental, Luxury & Glass, Retro & Pattern*).
* **🛠️ Live Visual Customizer Studio:** Adjust alert duration, clip duration, audio volume, player size, screen placement, fonts, glow intensity, and colors with a 1-click **OBS Browser Source URL Generator**.
* **🛡️ Built-In Multi-Layer Clip Resolver:** Uses both Streamer.bot's native clip methods (`login` and numeric `userId`) and a browser-side Twitch persisted GraphQL fallback so clips play reliably for any channel.

---

## 📂 Project Structure

* `overlay.html` — The OBS Browser Source overlay (Phase 1 Alert + Phase 2 Clip Player)
* `customizer.html` — Visual Customizer Studio (120+ borders, timers, live preview, URL generator)
* `ShoutoutAction.cs` — Zero-dependency C# script for Streamer.bot's "Execute C# Code" sub-action
* `README.md` — Setup and documentation

---

## 🚀 Quick Setup Guide

### 1. Enable the Streamer.bot WebSocket Server
1. Open **Streamer.bot**.
2. Navigate to **Servers/Clients** → **WebSocket Server**.
3. Check **Auto Start**, set **Address** to `127.0.0.1` and **Port** to `8080`, then click **Start Server**.

### 2. Add the C# Action in Streamer.bot
1. In Streamer.bot, go to **Actions** and create a new Action (e.g., `Custom Shoutout Overlay`).
2. Add a Trigger of your choice:
   * **Chat Command** (e.g., `!so @username`)
   * **Twitch Raid** / **Channel Point Reward** / **Alert Trigger**
3. Under **Sub-Actions**, right-click and select **Core** → **C#** → **Execute C# Code**.
4. Clear the editor (`Ctrl + A`, `Delete`), paste the contents of `ShoutoutAction.cs`, and click **Compile** → **Save and Compile**.
   * *Note: This script is built with zero external assembly dependencies—no manual `.dll` references required.*

### 3. Customize Your Overlay (`customizer.html`)
1. Open **`customizer.html`** in any modern web browser (Chrome, Edge, Brave, Firefox).
2. Configure your settings in the left sidebar:
   * **Alert Display Duration:** How long the initial Shoutout Alert displays (`2s` – `30s`, default `10s`).
   * **Twitch Clip Time Limit:** Maximum time the Twitch clip plays (`5s` – `90s`, default `30s`).
   * **Clip Audio Volume:** Set playback volume (`0%` – `100%`).
   * **Alert Text & Layout:** Customize the header text, subtitle, font family, and screen position (*Center, Top/Bottom Left, Top/Bottom Right, Top/Bottom Center*).
   * **120+ Border Library:** Click any border card in the grid to apply it immediately to the live preview.
3. Click **"Preview Full Alert → Clip Sequence"** in the top toolbar to test the transition.
4. Click **"Copy OBS Browser Source URL"** at the bottom of the sidebar.

### 4. Add to OBS Studio
1. In **OBS Studio**, add a new **Browser Source** to your scene.
2. Paste the URL you copied from `customizer.html` into the **URL** field.
3. Set the dimensions to:
   * **Width:** `1920`
   * **Height:** `1080`
4. *(Optional)* Check **Control audio via OBS** if you want to manage the clip's audio track in your OBS Audio Mixer.
5. Click **OK**.

---

## 🎮 How to Trigger Shoutouts

The C# script automatically detects the target user in the following priority order:

| Trigger Method | How It Works |
| :--- | :--- |
| **Chat Command (`!so @user`)** | Automatically strips the command (`!so`) and `@` symbol, grabs the target streamer's info, and plays their latest clip. |
| **Target Info Sub-Action** | Reads `%targetUserName%` and `%targetUser%` if you run `Twitch -> User -> Get Target Info` beforehand. |
| **Raids / Subs / Rewards** | Uses the user who triggered the event (`%user%` / `%userName%`) and detects their platform (`twitch`, `kick`, or `youtube`). |
| **Streamer.bot "Test" Button** | Safely falls back to your broadcaster profile (`%broadcastUserName%`) so you can test anytime. |

---

## ⚙️ URL Parameters Reference

When you copy the URL from `customizer.html`, all settings are passed cleanly via URL query parameters so OBS always remembers your exact configuration even after restarting:

| Parameter | Default | Description |
| :--- | :--- | :--- |
| `wsHost` | `127.0.0.1` | Streamer.bot WebSocket Server IP address |
| `wsPort` | `8080` | Streamer.bot WebSocket Server port |
| `alertDuration` | `10` | Duration (in seconds) to show the Phase 1 User Alert |
| `clipDuration` | `30` | Maximum duration (in seconds) to play the Phase 2 Twitch Clip |
| `borderId` | `1` | Selected preset border ID (`1` to `120`) |
| `playerWidth` | `720` | Width of the video player in pixels (16:9 aspect ratio) |
| `borderThickness`| `6` | Border frame thickness in pixels |
| `borderRadius` | `18` | Corner roundness in pixels |
| `glowSize` | `22` | Outer neon glow radius in pixels |
| `position` | `pos-center` | Screen placement (`pos-center`, `pos-bottom-left`, `pos-top-right`, etc.) |
| `volume` | `80` | Clip playback volume (`0` to `100`) |

---

## 🛠️ Troubleshooting

* **Overlay doesn't appear when triggered:**
  * Verify that Streamer.bot's **WebSocket Server** is running on `127.0.0.1:8080` (or matches the host/port configured in `customizer.html`).
  * In OBS, double-click the Browser Source and click **"Refresh cache of current page"**.
* **Clip plays muted:**
  * If OBS blocks unmuted autoplay on initial load, the player automatically falls back to muted playback so the video never stalls. Ensure your OBS Browser Source audio monitoring is set to **Monitor and Output** if using *Control audio via OBS*.
* **User has no Twitch clips:**
  * If the target streamer has `0` clips on their Twitch channel, Phase 2 still displays your custom border along with a fallback channel card showing their avatar, username, and last played category.

---

## 📜 License

Released under the [MIT License](LICENSE). Free to use, modify, and share for content creators and stream developers.
