# YouTube Music for MyGo Music

Plays YouTube Music in [MyGo Music](https://github.com/egoist/mygo-music). It isn't affiliated with or endorsed by YouTube or Google.

The catalog comes from the private API that YouTube Music's website calls. A track's audio is found as YouTube's app for the Apple Vision Pro asks for it, and [ffmpeg](https://ffmpeg.org) turns it into the MP3 the app can play. If YouTube turns that down, [yt-dlp](https://github.com/yt-dlp/yt-dlp) finds the audio instead, when it's installed. None of this is an official way in: it may be against YouTube's terms, and could stop working at any time.

## Install

1. Install ffmpeg, and yt-dlp for when the first way doesn't work. On macOS:

   ```bash
   brew install ffmpeg yt-dlp deno
   ```

   yt-dlp uses Deno, or Node or Bun if you have one of those, to work out some of YouTube's addresses. Keep yt-dlp up to date, as YouTube changes often.

2. Copy `plugin.json` and `index.js` into a folder in MyGo Music's `plugins` folder (Settings > Plugins > Show Folder), and reopen the app.
3. In Settings > Plugins, under YouTube Music, allow it to start programs.
4. To have your library, click **Sign In** and sign in to Google in the window that opens.

If the programs aren't in `/opt/homebrew/bin`, `/usr/local/bin` or on the PATH the app was started with, say which folder they're in with **Programs Folder**.

## Signing in

You sign in on Google's own page, in a window of the app that shares nothing with your browser or other plugins, and forgets everything once it closes, but the session's cookies, which the plugin keeps. The page is told it's Safari on macOS, and Edge on Windows, as Google turns down signing in on a page it takes for one inside an app. It may still turn it down.

The session is a sign-in to your Google account, kept unencrypted in the plugin's data on this machine. It goes to music.youtube.com, to s.youtube.com to put what you play in your history, and to yt-dlp, on its standard input, when yt-dlp is the one finding the audio. Finding the audio the first way doesn't take it. Signing out forgets it. To end the session itself, sign out of that device in your Google account's security settings.

## What works

Signed out: playing, Home, charts, new releases, search with suggestions, albums, artists, playlists and timed lyrics.

Signed in: your playlists, liked songs, recently played, liking songs, a personal Home, and what you play going into your YouTube Music history, which its recommendations are made from.

Radio plays songs like the one you played last, and needs you signed in.

Not available: changing playlists, saving albums, following artists, podcasts and comments.

## How it plays

Each track asks YouTube for its audio as the Vision Pro app does, as a visitor, which takes under a second, and then starts `ffmpeg`, which converts it to MP3 as it downloads and serves it on `127.0.0.1` to the one request that comes for it. The app keeps what it plays in its audio cache, as it does for any plugin, within the limit in its settings. Tracks play at the same loudness, as YouTube says how loud each one is, when the app's loudness normalization is on.

From a VPN or a server's network, YouTube asks every visitor to sign in to prove it isn't a bot. There, tracks play through yt-dlp, with your session, which can take 15 seconds for a track to start: signed in, YouTube only gives yt-dlp a track's audio as HLS, along with its video at 360p.

There's no lossless: YouTube's audio is lossy, and converting it loses a little more.

## Lyrics

They come from [Better Lyrics](https://github.com/better-lyrics/better-lyrics), which times each word, for the songs it has, and otherwise from YouTube Music, which times each line.
