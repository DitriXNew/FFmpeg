# Corresponding source — FFmpeg build for AI Video Translator

This repository is a fork of [FFmpeg/FFmpeg](https://github.com/FFmpeg/FFmpeg)
that builds the **macOS and Windows** FFmpeg binaries used by the *AI Video
Translator* application, together with the public build scripts used to produce
them.

Per the GNU LGPL, this is the **corresponding source** for the binaries produced
here: the FFmpeg source (by exact commit), the source of every statically linked
library (by exact commit/version), and the build recipe.

## Build recipes

Both targets are LGPL v2.1 (no GPL, no nonfree, version3 disabled) and link the
same BSD-licensed external libraries pinned below. Run either via
*Actions → Run workflow*.

### macOS arm64

Workflow: [`.github/workflows/ffmpeg-macos.yml`](.github/workflows/ffmpeg-macos.yml)

```
--disable-gpl --disable-nonfree --disable-version3 --enable-static --disable-shared
--enable-pic --enable-videotoolbox --arch=arm64 --target-os=darwin
--enable-libaom --enable-libsvtav1 --enable-encoder=libaom_av1 --enable-encoder=libsvtav1
--enable-libopus --enable-encoder=libopus --enable-encoder=flac --enable-encoder=alac
--enable-encoder=pcm_s16le --enable-encoder=pcm_s24le --enable-encoder=pcm_f32le
--enable-decoder=opus --enable-decoder=flac --enable-decoder=alac
--enable-decoder=pcm_s16le --enable-decoder=pcm_s24le --enable-decoder=pcm_f32le
```

macOS offloads input decoding to the OS (CoreAudio/VideoToolbox), so the bundled
binary needs only the audio encoders/decoders above.

### Windows x86_64

Workflow: [`.github/workflows/ffmpeg-windows.yml`](.github/workflows/ffmpeg-windows.yml)
— mingw-w64 cross-compile on Ubuntu, fully static `ffmpeg.exe` / `ffprobe.exe`
(no sibling DLLs).

```
--disable-gpl --disable-nonfree --disable-version3 --enable-static --disable-shared
--enable-w32threads --arch=x86_64 --target-os=mingw32 --cross-prefix=x86_64-w64-mingw32- --enable-cross-compile
--enable-libaom --enable-libsvtav1 --enable-encoder=libaom_av1 --enable-encoder=libsvtav1
--enable-libopus --enable-encoder=libopus --enable-encoder=aac --enable-encoder=flac --enable-encoder=alac
--enable-encoder=pcm_s16le --enable-encoder=pcm_s24le --enable-encoder=pcm_f32le
--enable-decoder=h264 --enable-decoder=hevc --enable-decoder=vp8 --enable-decoder=vp9 --enable-decoder=av1
--enable-decoder=mpeg4 --enable-decoder=mpeg2video --enable-decoder=mjpeg
--enable-decoder=aac --enable-decoder=mp3 --enable-decoder=ac3 --enable-decoder=eac3
--enable-decoder=opus --enable-decoder=vorbis --enable-decoder=flac --enable-decoder=alac
--enable-decoder=pcm_s16le --enable-decoder=pcm_s24le --enable-decoder=pcm_f32le
```

Windows has no FFmpeg-visible OS decoder (MediaFoundation exposes encoders only),
so the app feeds user media straight to this binary — hence the native software
decoders and the native `aac` encoder (the Windows MP4 dub muxes `-c:a aac`).
These are native libavcodec components under LGPL v2.1 and add no external
dependency, so the licence posture is identical to macOS.

## Pinned sources (by commit hash — hashes are immutable, tags are not)

| Component | Version | Commit | Repository |
| --- | --- | --- | --- |
| FFmpeg  | n8.1.2  | `38b88335f99e76ed89ff3c93f877fdefce736c13` | https://github.com/FFmpeg/FFmpeg |
| libaom  | v3.14.1 | `03087864cf4bea6abb0d28f95cf7843511413d8f` | https://aomedia.googlesource.com/aom |
| SVT-AV1 | v4.1.0  | `c04f951541ad600e0d9c10836f2ab7b9bc69816d` | https://gitlab.com/AOMediaCodec/SVT-AV1 |
| opus    | 1.5.2   | (release tarball) | https://downloads.xiph.org/releases/opus/opus-1.5.2.tar.gz |

Build targets: **macOS arm64** (Apple Silicon) and **Windows x86_64**. Both
targets use the pins above.

## Library licenses (statically linked)

- **libaom** — BSD 2-Clause + Alliance for Open Media Patent License 1.0
- **SVT-AV1** — BSD 3-Clause Clear + Alliance for Open Media Patent License 1.0
- **opus** — BSD 3-Clause

## Distribution status

- **macOS** — the binary produced by the workflow above is bundled inside the
  application and distributed with it. This document is its corresponding source.
- **Windows** — the workflow above now produces the corresponding Windows
  binary. The app is being switched to bundling this build; until that ships, the
  released app still downloads a general LGPL v3 FFmpeg build from
  [BtbN/FFmpeg-Builds](https://github.com/BtbN/FFmpeg-Builds) at runtime (release
  tag `autobuild-2026-06-19-23-17`, FFmpeg upstream commit `4bbb7d9b99`), for
  which BtbN provides the corresponding source. The app's `FFMPEG_NOTICE.txt`
  records which build a given release actually ships.

## Written offer

If any link above ever becomes unavailable, the corresponding source is also
available on request at **ai.video.translators@gmail.com**.
