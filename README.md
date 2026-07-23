# 🎬 YVT — Yandex Video Translate

🇬🇧 English | [🇷🇺 Русский](README-ru.md)

A CLI tool that downloads a video, translates it with Yandex's neural translation (via [vot-cli](https://github.com/FOSWLY/vot-cli)), and renders it back out with the original audio, the translated dub, and subtitles muxed in.

![CI](https://github.com/Arroheim/yvt/actions/workflows/ci.yml/badge.svg)
![Bash](https://img.shields.io/badge/bash-4%2B-4EAA25?logo=gnubash&logoColor=white)
![Platform](https://img.shields.io/badge/platform-linux%20%7C%20macos-lightgrey)
![License](https://img.shields.io/badge/license-GPL--3.0-blue.svg)

![example](img/example.png)

## ✨ Features

- 📥 Downloads video from most popular hosting platforms via `yt-dlp`
- 🗣️ Translates audio into Russian, English or Kazakh using Yandex's neural translation
- 🎭 **Lively voices** — natural-sounding dubbing for en→ru translations (requires a Yandex OAuth token)
- 🔊 Mixes original + translated audio into one track, keeps a clean original-only track alongside it
- 📝 Embeds subtitles (11 source languages supported) — skipped gracefully if Yandex can't provide them for the requested language pair
- ⚡ Hardware-accelerated encoding: `h264_nvenc` (Nvidia/Linux), `h264_videotoolbox` (Apple Silicon/Intel Mac), falls back to `libx264`
- 📋 Batch mode — process a whole file of links in one run
- 🌍 UI in 3 languages (en, ru, kk), auto-detected from `$LANG`
- 🐧🍎 Works on Linux and macOS
- 🔁 Automatic retry on download failure, graceful skip on translation failure (batch mode keeps going)

## 📦 Requirements

- `bash` ≥ 4.0 (macOS ships 3.2 by default — install a newer one: `brew install bash`)
- [`yt-dlp`](https://github.com/yt-dlp/yt-dlp)
- [`vot-cli`](https://github.com/FOSWLY/vot-cli)
- `ffmpeg` / `ffprobe`

## 🚀 Installation

```bash
git clone https://github.com/Arroheim/yvt.git
cd yvt
chmod +x yvt
./yvt --help
```

Optionally add it to your `PATH` (e.g. symlink into `~/.local/bin`).

## ⚙️ Configuration (optional)

To use **lively voices** (natural-sounding en→ru dubbing), get a Yandex OAuth API token and put it in a `.env` file next to the script:

```bash
cp .env.example .env
```

```env
YANDEX_API_TOKEN=your_token_here
```

Without a token, translation still works — it just uses the standard synthesized voice.

## 📋 Usage

```
yvt [options] [args] <link>
```

### Options

| Flag | Description | Default |
|------|--------------|---------|
| `-v` | Original volume ratio (`0`–`0.6`) | `0.3` |
| `-b` | Bitrate in Kbit (`50`–`20000`) | `450` |
| `-c` | Passthrough mode, no re-encode if source is already h264/aac (`1`/`0`) | `0` |
| `-f` | File with a list of links (batch mode) | — |
| `-o` | Original video language (`en`, `ru`, `kk`, `zh`, `ko`, `ar`, `fr`, `it`, `es`, `de`, `ja`) | `en` |
| `-l` | Translation (dub) language (`en`, `ru`, `kk`) | `ru` |
| `-s` | Subtitles language (`en`, `ru`, `kk`, `zh`, `ko`, `ar`, `fr`, `it`, `es`, `de`, `ja`) | `en` |
| `--help` | Show help | |
| `--version` | Show version | |

### Examples

```bash
# Translate a single video, default settings
yvt 'https://www.youtube.com/watch?v=example_video'

# Custom volume/bitrate
yvt -v 0.5 -b 800 'https://www.youtube.com/watch?v=example_video'

# Batch mode: process every link in a file
yvt -v .2 -b 500 -f ./linklist.txt

# Translate into English, request Russian subtitles
yvt -l en -s ru 'https://www.youtube.com/watch?v=example_video'
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

GNU General Public License v3.0 — see the [LICENSE](LICENSE) file for details.

Originally created by [s-n-alexeyev](https://github.com/s-n-alexeyev/yvt).
