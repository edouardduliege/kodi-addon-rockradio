# RockRadio for Kodi — v1.0.0

Unofficial community RockRadio / AudioAddict music add-on for Kodi.

## V1 scope

The V1 deliberately provides a simple linear-radio experience using Kodi's
native player:

- AudioAddict account login with cached/reused sessions
- current RockRadio channel list and style filters
- Popular and New views
- favourites synchronised with the AudioAddict account
- Premium stream quality selection
- multiple stream-server fallback
- dynamic title, artist and track-cover updates
- channel artwork exposed as Kodi fanart
- English and French localisation
- no custom player UI

The RockRadio channel directory is fetched from AudioAddict when its Kodi view
is opened; the add-on does not maintain a local channel catalogue cache.

## Architecture

`default.py`
: Kodi plugin entry point. Builds directories and resolves a selected station
  to a linear Premium stream.

`service.py`
: Lightweight background service. While a RockRadio stream is playing, it polls
  Now Playing metadata and updates Kodi's actual playing `ListItem`, allowing
  title/artist/cover changes to propagate to Estuary, the Kodi web interface
  and remote controls such as Kore.

`resources/lib/client.py`
: AudioAddict network client. Handles authentication/session renewal, channel
  metadata, favourites and Premium PLS stream resolution.

`resources/lib/helpers.py`
: Image URL normalisation and Kodi `ListItem` helpers.

`resources/lib/state.py`
: Small state file stored only inside this add-on's Kodi profile directory.

## Authentication and session handling

The user's AudioAddict email/password remain Kodi settings. A successful
AudioAddict session is cached in the add-on profile and reused across restarts
only while it matches the currently configured credentials.

The add-on requires configured credentials before opening its root menu. If the
server rejects a cached session with HTTP 401/403, the add-on deletes the cached
session and performs one fresh login automatically.

## Premium quality mapping

- Medium → `premium_medium` → AAC-HE 64 kb/s
- High → `premium` → AAC 128 kb/s
- Ultra → `premium_high` → MP3 320 kb/s

Changing this setting affects the next time a stream is opened. V1 does not
interrupt/restart an already playing stream when the quality setting changes.

## Stream-server fallback

RockRadio returns several candidate streaming servers in its PLS playlist.
The add-on probes a bounded number of candidates using lightweight streamed
`GET` requests rather than relying on `HEAD`, because some streaming nodes may
reject `HEAD` while still being playable.

Explicit HTTP rejection is treated as a negative result, while connection
errors and timeouts are treated as inconclusive so Kodi can still attempt the
candidate directly. Stream URLs written to debug logs are redacted.

## V2 candidates

- per-track progress using a track-by-track playback engine
- apply a quality change without manually stopping/restarting playback
- optional interactive RockRadio functions only if there is enough user value to
  justify the additional Kodi UI complexity
- sleep timer
- improved behaviour/recovery after a temporary network loss

## Publication

- Maintainer: Édouard Duliège
- Source: https://github.com/edouardduliege/kodi-addon-rockradio
- License: GPL-3.0-or-later
- Kodi Omega compatibility validated with `kodi-addon-checker`

This is an unofficial community add-on. It is not affiliated with, endorsed by,
or supported by RockRadio or AudioAddict.

The add-on uses original community artwork and does not redistribute the
official RockRadio logo.

The add-on relies on AudioAddict endpoints that are not supported as a public
API and may therefore change without notice.

## Changelog

### 1.0.0

Initial public release.

### 0.9.0

Initial local test build.

- Linear RockRadio playback through Kodi's native player
- AudioAddict account authentication with cached session reuse
- channel browsing, style filters, Popular and New views
- favourites synchronisation
- Premium stream quality selection
- bounded multiple stream-server fallback
- dynamic Now Playing title, artist and artwork updates
- English and French localisation
- session renewal on HTTP 401/403
- cached-session fingerprint tied to the configured credentials
- atomic session-cache writes with restrictive permissions
- Linux `flock()` playback-state locking
- redacted stream URLs in debug logs
- reduced unnecessary Now Playing API calls
- original community artwork clearly distinguishing the add-on from an official release
