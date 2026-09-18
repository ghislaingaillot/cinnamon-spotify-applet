# Spotify Control — Cinnamon applet

A [Cinnamon](https://github.com/linuxmint/cinnamon) (Linux Mint) applet that adds Spotify controls to the panel: previous, play/pause, next, and a favorites button — with cover art, title and artist shown on hover.

![Cinnamon](https://img.shields.io/badge/Cinnamon-6.x-2f9e44) ![License](https://img.shields.io/badge/license-GPL--3.0-blue)

## Features

- **Previous / Play-pause / Next** via Spotify's MPRIS interface (D-Bus), no external dependency such as `playerctl`.
- **Hover tooltip**: shows the album cover art, title and artist of the current track.
- **Favorites button (★)**: adds or removes the current track from your Spotify "Liked Songs". This goes through Spotify's official Web API (OAuth), since Spotify does not expose this action over D-Bus. It can be disabled from the applet's settings if you don't need it.
- If Spotify isn't running, clicking any button launches it.
- Compact layout: the favorites button only appears while an actual track is playing.

## Requirements

- Linux Mint / Cinnamon 5.x or later.
- The official Spotify client installed (`/usr/bin/spotify`).
- `curl`, `wget`, `openssl`, `xdg-open` (present by default on most installations).

## Installation

```bash
git clone https://github.com/ghislaingaillot/cinnamon-spotify-applet.git
ln -s "$(pwd)/cinnamon-spotify-applet/spotify-control@ghislaingaillot" ~/.local/share/cinnamon/applets/spotify-control@ghislaingaillot
```

Then, in Cinnamon: **Settings → Applets**, find "Spotify Control" under the *Downloaded* tab and add it to a panel (or enable it directly if it's already listed).

## Setting up the favorites button (optional)

The favorites button needs a one-time OAuth authorization with Spotify (free, a couple of minutes):

1. Go to [developer.spotify.com/dashboard](https://developer.spotify.com/dashboard) and create an application.
2. In the application's settings, add the following redirect URI:
   ```
   http://127.0.0.1:43127/callback
   ```
3. Copy the application's **Client ID**.
4. In Cinnamon, right-click the applet → **Configure**, paste the Client ID.
5. Click **Connect to Spotify**: your browser opens to authorize the application, then closes automatically.

The access token is refreshed automatically afterwards; you can disconnect at any time from the same settings panel, or hide the favorites button entirely with the "Show the favorites (star) button" toggle.

Without this setup, the applet works normally for playback (previous/pause/next/cover art); only the favorites button stays inactive.

## Project structure

```
spotify-control@ghislaingaillot/
├── applet.js             # Applet UI, MPRIS integration, cover art
├── spotifyAuth.js         # OAuth (PKCE) flow and Spotify Web API calls
├── settings-schema.json   # Settings page (Client ID, connect/disconnect)
├── stylesheet.css         # Styles
├── icon.png               # Applet icon
└── metadata.json           # Applet metadata
```

## License

This project is distributed under the [GPL-3.0](LICENSE) license.
