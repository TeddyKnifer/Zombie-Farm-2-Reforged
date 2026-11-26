# iPhone Safari install and release test plan

**Status: test plan, not a completed device test.** The project already contains a Vite PWA manifest, iOS Home Screen metadata, an Apple touch icon reference, and a GitHub Pages deploy workflow. This checklist records what must be verified before calling the Expanded Edition ready for an iPhone.

## Install the current published build

1. Open the project's published GitHub Pages URL in Safari on the iPhone. Use the exact Pages URL shown in the repository's Settings → Pages; do not assume the repository name or deployment URL.
2. Wait for the game to finish loading, then tap the game start screen.
3. In Safari, tap Share → Add to Home Screen.
4. Keep the suggested title or use Zombie Farm 2; tap Add.
5. Launch the new Home Screen icon and confirm it opens in standalone mode without Safari's address bar.
6. This installs the currently published build only. A feature on an unmerged development branch is not included.

## Device acceptance matrix

Record device model, iOS version, deployed commit, browser, date, and result for each run.

| Area | Required checks |
|---|---|
| Install / launch | Home Screen icon is correct; standalone launch; cold launch after force-quit |
| Safe areas | HUD and dialogs clear the Dynamic Island/notch and bottom home indicator |
| Portrait | Status rail, tool dock, side actions, Market and modal layouts fit without clipping |
| Landscape | Canvas and HUD reflow; controls remain reachable; no accidental page scroll |
| Touch | Tap, long-press, drag, pinch zoom, pan and modal dismissal work without browser gestures stealing input |
| Persistence | Create a Local Farm save, close the app, relaunch, and confirm the save is intact |
| Offline | After a successful first load, enable Airplane Mode and relaunch; record which cached screens and assets remain available |
| Network recovery | Restore connectivity; confirm asset loading and any optional online status recover cleanly |
| Update | Publish a newer build, relaunch the installed PWA, and confirm the service worker eventually serves the new version |
| Performance | Check first load, farm entry, zoom/pan, long play session, heat and memory pressure |
| Accessibility | Reduced-motion preference, readable text, visible focus for any keyboard-accessible controls |

## Release gates

- [ ] Client checks pass: `npm test` and `npm run build`.
- [ ] Full CI succeeds on the commit intended for release.
- [ ] GitHub Pages deploy publishes that exact validated commit.
- [ ] Confirm the deployed URL and commit in the browser; don't treat a development branch or open PR as a release.
- [ ] Complete the physical iPhone checks above in Safari and from the Home Screen icon.
- [ ] Re-test the update path after changing the service worker or cached asset strategy.
- [ ] Do not announce iPhone compatibility as tested until a real device run is recorded.

## Known boundaries

The app's manifest requests standalone display and the HTML includes iOS Home Screen metadata, but that alone does not prove a successful install or full offline behavior on every iOS version. Service-worker cache policy, browser storage eviction, and platform behavior must be tested on-device. An iPhone Air is the target device in this project conversation; no physical iPhone Air test has been performed as part of this change.