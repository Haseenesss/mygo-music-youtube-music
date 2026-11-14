# YouTube Music for MyGo Music

Plays YouTube Music in [MyGo Music](https://github.com/egoist/mygo-music). It isn't affiliated with or endorsed by YouTube or Google, uses YouTube's private API, and may stop working at any time.

## Install

Install [ffmpeg](https://ffmpeg.org), and [yt-dlp](https://github.com/yt-dlp/yt-dlp) as a fallback. On macOS:

```bash
brew install ffmpeg yt-dlp
```

yt-dlp needs a JavaScript runtime (Deno, Node or Bun) to solve YouTube's challenges; Homebrew includes Deno. Keep yt-dlp up to date, as YouTube changes often.

Install with the deeplink:

[mygo-music://install-plugin?repo=egoist/mygo-music-youtube-music](mygo-music://install-plugin?repo=egoist/mygo-music-youtube-music)

Or paste the GitHub URL in Settings > Plugins:

[https://github.com/egoist/mygo-music-youtube-music](https://github.com/egoist/mygo-music-youtube-music)

In Settings > Plugins, allow YouTube Music to start programs. To get your library, click **Sign In**.

It needs MyGo Music 0.1.8 or later, and only starts ffmpeg and yt-dlp: from the PATH, `/opt/homebrew/bin` or `/usr/local/bin`. If they're somewhere else, link them into one of those. It also starts `/usr/bin/defaults` (macOS) or `xdg-settings` (Linux) to find your default browser, `/usr/bin/open` (macOS) to open the sign-in page in the browser and profile you chose, and `/usr/bin/pbcopy` to copy the sign-in link.

## Signing in

You sign in in your own browser, so a session you already have there is used as it is. Signing in reads YouTube's cookies from your default browser with yt-dlp, so yt-dlp is needed for it. If you're signed in to YouTube Music there already, that's all. Otherwise the sign-in page opens in that browser, its link is copied, and the dialog shows `https://music.youtube.com` to open by hand. Once you're signed in there, the dialog finishes on its own. With a MyGo Music whose sign-in dialog shows links and fields, it shows the whole sign-in link, to copy to another browser or open again, and a field to paste the Cookie of a request to music.youtube.com into, from the browser's developer tools, for a browser yt-dlp can't read.

To use another browser than the default, or one of its profiles, fill in **Browser to sign in with** in the plugin's settings: `chrome`, `safari`, `firefox`, `edge`, `brave`, `chromium`, `opera`, `vivaldi` or `whale`, with a profile as in `chrome:Profile 1`. `app` signs in in a window of the app instead, as before. On Windows, that window is the default, as newer versions of Chrome there keep their cookies from yt-dlp.

- Chrome and other Chromium browsers on macOS ask, the first time, for their key in the keychain: choose **Always Allow**.
- Safari's cookies need MyGo Music to have Full Disk Access, in System Settings > Privacy & Security.
- Arc and other browsers yt-dlp doesn't read can't be used: name one it does.

The session is kept unencrypted in the plugin's data on this machine, and is sent to music.youtube.com, s.youtube.com for your history, and to yt-dlp when it fetches audio. As Google renews the browser's cookies, the plugin reads them again from it when YouTube turns the session down. Signing out forgets it here, but leaves you signed in in your browser. To end the session on Google's side, sign out there.

## What works

- Signed out: playing, Home, charts, new releases, search, albums, artists, playlists and timed lyrics.
- Signed in: your library, playlists, liked songs, recently played, liking songs, a personal Home, listening history and radio.
- Not available: editing playlists, saving albums, following artists, podcasts and comments.

## How it plays

Tracks are fetched the way YouTube's Vision Pro app fetches them, then ffmpeg converts them to MP3 on the fly. If that fails, yt-dlp is used instead; it can take up to 15 seconds to start, and needs you signed in from a VPN or server network. The audio is lossy.

## Lyrics

From [Better Lyrics](https://github.com/better-lyrics/better-lyrics) when available, otherwise from YouTube Music.
