# LG Camera Overlay: Home Assistant app recipe

Personal Home Assistant OS packaging of [mbsintegrations/LG-Camera-Overlay](https://github.com/mbsintegrations/LG-Camera-Overlay) v1.2.1 (GPL-3.0-only). This repository holds only build instructions and a manifest. The app is built on the Home Assistant host from the pinned upstream commit. It contains no credentials or configuration.

The upstream project marks its Home Assistant OS app as experimental and not tested.

## Local change

`lg_camera_overlay/remember-stream.patch` modifies upstream `source/app.js` so a launch without parameters, such as from the remote's Quick Access, reopens the last Home Assistant launch. The saved address stays in the app's storage on the TV.

`zoom.patch` adds digital zoom to the TV panel: remote wheel or red/green buttons, with arrow keys to pan while zoomed.

`faster-start.patch` shortens FFmpeg's stream analysis in the relay, and retries with default settings if that fails.
