### Runtime assets

These files are copied into every Unity build automatically.

- `Skin/`: note, slide, touch, judge-text, and judgement-effect sprites.
- `SFX/`: gameplay and UI sound effects used by standalone/editor playback.
- `slide_time.json`: slide timing table used by muri checks.
- `ffarguments.txt`: Windows FFmpeg recording argument template.

### Desktop recording

Windows uses `ffmpeg.exe` and `ffarguments.txt` with the `\\.\pipe\majdataRec`
input.

macOS/Linux use stdin (`pipe:0`). Put the platform-matched binary in this folder
before building, or install ffmpeg in a standard system path:

- macOS: `ffmpeg-mac` or `ffmpeg`
- Linux: `ffmpeg-linux` or `ffmpeg`

Finder-launched macOS apps often do not inherit the terminal `PATH`, so the
runtime also checks common locations such as `/opt/homebrew/bin/ffmpeg`,
`/usr/local/bin/ffmpeg`, and `/usr/bin/ffmpeg`. Bundling `ffmpeg-mac` is still
the most reliable release option.

Unity builds filter these binaries by target:

- Windows keeps `ffmpeg.exe`.
- macOS keeps `ffmpeg-mac`.
- Linux keeps `ffmpeg-linux`.
- WebGL/Android/iOS keep no desktop ffmpeg binary.

Do not rely on `ffmpeg.exe` outside Windows.

### macOS launch notes

If the exported `.app` opens from Unity but does not open after being copied or
downloaded, check Gatekeeper/quarantine before debugging Unity code:

```bash
xattr -dr com.apple.quarantine MajdataViewBeta.app
chmod -R +x MajdataViewBeta.app
```

For public distribution, sign and notarize the app. Unsigned builds may require
right-click -> Open on the first launch.

### WebGL / mobile

WebGL, Android, and iOS cannot execute Windows `.exe` / `.dll` files from
`StreamingAssets`. Keep platform-native desktop binaries out of Web/mobile
feature paths. Static assets such as `Skin`, `SFX`, and JSON files are safe.
