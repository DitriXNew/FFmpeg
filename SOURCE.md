# Corresponding source — FFmpeg build for AI Video Translator

This repository is a fork of [FFmpeg/FFmpeg](https://github.com/FFmpeg/FFmpeg)
that builds the **macOS** FFmpeg binaries bundled in the *AI Video Translator*
application, together with the public build scripts used to produce them.

Per the GNU LGPL, this is the **corresponding source** for the bundled binary:
the FFmpeg source (by exact commit), the source of every statically linked
library (by exact commit/version), and the build recipe.

## Build recipe

GitHub Actions workflow:
[`.github/workflows/ffmpeg-macos.yml`](.github/workflows/ffmpeg-macos.yml)
(cross-references the pins below; run via *Actions → Run workflow*).

Configure (LGPL v2.1 — no GPL, no nonfree, version3 disabled):

```
--disable-gpl --disable-nonfree --disable-version3 --enable-static --disable-shared
--enable-pic --enable-videotoolbox --arch=arm64 --target-os=darwin
--enable-libaom --enable-libsvtav1 --enable-encoder=libaom_av1 --enable-encoder=libsvtav1
--enable-libopus --enable-encoder=libopus --enable-encoder=flac --enable-encoder=alac
--enable-encoder=pcm_s16le --enable-encoder=pcm_s24le --enable-encoder=pcm_f32le
--enable-decoder=opus --enable-decoder=flac --enable-decoder=alac
--enable-decoder=pcm_s16le --enable-decoder=pcm_s24le --enable-decoder=pcm_f32le
```

## Pinned sources (by commit hash — hashes are immutable, tags are not)

| Component | Version | Commit | Repository |
| --- | --- | --- | --- |
| FFmpeg  | n8.1.2  | `38b88335f99e76ed89ff3c93f877fdefce736c13` | https://github.com/FFmpeg/FFmpeg |
| libaom  | v3.14.1 | `03087864cf4bea6abb0d28f95cf7843511413d8f` | https://aomedia.googlesource.com/aom |
| SVT-AV1 | v4.1.0  | `c04f951541ad600e0d9c10836f2ab7b9bc69816d` | https://gitlab.com/AOMediaCodec/SVT-AV1 |
| opus    | 1.5.2   | (release tarball) | https://downloads.xiph.org/releases/opus/opus-1.5.2.tar.gz |

The build target is **macOS arm64** (Apple Silicon).

## Library licenses (statically linked)

- **libaom** — BSD 2-Clause + Alliance for Open Media Patent License 1.0
- **SVT-AV1** — BSD 3-Clause Clear + Alliance for Open Media Patent License 1.0
- **opus** — BSD 3-Clause

## Windows

The Windows build is **not** produced here. The application downloads a general
LGPL v3 FFmpeg build from [BtbN/FFmpeg-Builds](https://github.com/BtbN/FFmpeg-Builds)
at runtime (release tag `autobuild-2026-06-19-23-17`, FFmpeg upstream commit
`4bbb7d9b99`); BtbN provides the corresponding source for that build.

## Written offer

If any link above ever becomes unavailable, the corresponding source is also
available on request at **ai.video.translators@gmail.com**.
