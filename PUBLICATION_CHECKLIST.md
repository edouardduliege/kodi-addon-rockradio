# Kodi publication checklist — RockRadio 1.0.2

This checklist belongs at the **source repository root**, outside `plugin.audio.rockradio/`.

## Implemented in the 1.0.2 candidate

- [x] Native Kodi player; English/French localization; account-based Premium streams
- [x] Cached sessions, renewal after HTTP 401/403, atomic session storage
- [x] Portable Windows/Unix playback-state locking and atomic writes
- [x] Playback instance checks to avoid stale track metadata
- [x] Authenticated redirects disabled and playlist URL validation strengthened
- [x] Controlled retry of transient track detail failures
- [x] Favorite channel IDs normalized as strings
- [x] Non-blocking playback startup, ~15 s metadata polling and stream fallback
- [x] No bundled Python bytecode or source-repository development files in Kodi addon folder
- [x] `addon.xml` and `.po` headers set to 1.0.2

## Before submitting 1.0.2 to Kodi

- [ ] Confirm clean upgrade from the currently installed RockRadio release
- [ ] Play multiple stations and verify metadata/artwork updates
- [ ] Test add/remove favorites and context-menu status
- [ ] Test stop/restart and switching quickly between streams
- [ ] Check `kodi.log` for unexpected errors
- [ ] Run the official Kodi addon-checker on the final add-on folder
- [ ] Confirm Kodi package contains `LICENSE.txt` but no `PUBLICATION_CHECKLIST.md`
- [ ] Submit a new PR from an up-to-date `upstream/omega` branch

## Deferred

- [ ] Validate JSON authentication payload types (`null`, `[]`, `{}`) and raise `AudioAddictError` when invalid
- [ ] End-to-end stream redirect / `getPlayingFile()` tests
- [ ] Temporary network outage/recovery tests
- [ ] Dynamic changes of credentials while service is active (not necessary for V1)
- [ ] Optional V2 features: interactive controls, sleep timer, track-level progress
