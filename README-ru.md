# 🎬 YVT — Yandex Video Translate

[🇬🇧 English](README.md) | 🇷🇺 Русский

CLI-утилита, которая скачивает видео, переводит его нейросетевым переводчиком Яндекса (через [vot-cli](https://github.com/FOSWLY/vot-cli)) и собирает результат обратно: оригинальная аудиодорожка, переведённая озвучка и субтитры — всё в одном файле.

![CI](https://github.com/Arroheim/yvt/actions/workflows/ci.yml/badge.svg)
![Bash](https://img.shields.io/badge/bash-4%2B-4EAA25?logo=gnubash&logoColor=white)
![Platform](https://img.shields.io/badge/platform-linux%20%7C%20macos-lightgrey)
![License](https://img.shields.io/badge/license-GPL--3.0-blue.svg)

![пример](img/example.png)

## ✨ Возможности

- 📥 Скачивание видео с большинства популярных видеохостингов через `yt-dlp`
- 🗣️ Перевод озвучки на русский, английский или казахский с помощью нейросети Яндекса
- 🎭 **Живые голоса** — естественная озвучка для перевода en→ru (нужен OAuth-токен Яндекса)
- 🔊 Смешивание оригинала с переводом в одну дорожку + отдельная чистая дорожка оригинала
- 📝 Встраивание субтитров (11 исходных языков) — если Яндекс не может отдать субтитры для выбранной пары языков, скрипт мягко продолжает без них
- ⚡ Аппаратное ускорение кодирования: `h264_nvenc` (Nvidia/Linux), `h264_videotoolbox` (Apple Silicon/Intel Mac), фолбэк на `libx264`
- 📋 Пакетный режим — обработка целого файла со списком ссылок за один запуск
- 🌍 Интерфейс на 3 языках (en, ru, kk), определяется автоматически по `$LANG`
- 🐧🍎 Работает на Linux и macOS
- 🔁 Автоповтор при неудачном скачивании, мягкий пропуск при неудачном переводе (в пакетном режиме обработка остальных ссылок продолжается)

## 📦 Зависимости

- `bash` ≥ 4.0 (в macOS по умолчанию версия 3.2 — поставьте новую: `brew install bash`)
- [`yt-dlp`](https://github.com/yt-dlp/yt-dlp)
- [`vot-cli`](https://github.com/FOSWLY/vot-cli)
- `ffmpeg` / `ffprobe`

## 🚀 Установка

```bash
git clone https://github.com/Arroheim/yvt.git
cd yvt
chmod +x yvt
./yvt --help
```

По желанию добавьте скрипт в `PATH` (например, символьной ссылкой в `~/.local/bin`).

## ⚙️ Настройка (опционально)

Чтобы включить **живые голоса** (естественная озвучка en→ru), получите OAuth-токен Yandex API и положите его в `.env` рядом со скриптом:

```bash
cp .env.example .env
```

```env
YANDEX_API_TOKEN=ваш_токен
```

Без токена перевод всё равно работает — просто со стандартным синтезированным голосом.

## 📋 Использование

```
yvt [options] [args] <link>
```

### Опции

| Флаг | Описание | По умолчанию |
|------|-----------|--------------|
| `-v` | Отношение громкости оригинала (`0`–`0.6`) | `0.3` |
| `-b` | Битрейт в Kbit (`50`–`20000`) | `450` |
| `-c` | Режим без перекодировки, если исходник уже h264/aac (`1`/`0`) | `0` |
| `-f` | Файл со списком ссылок (пакетный режим) | — |
| `-o` | Исходный язык видео (`en`, `ru`, `kk`, `zh`, `ko`, `ar`, `fr`, `it`, `es`, `de`, `ja`) | `en` |
| `-l` | Язык перевода (озвучки) (`en`, `ru`, `kk`) | `ru` |
| `-s` | Язык субтитров (`en`, `ru`, `kk`, `zh`, `ko`, `ar`, `fr`, `it`, `es`, `de`, `ja`) | `en` |
| `--help` | Показать справку | |
| `--version` | Показать версию | |

### Примеры

```bash
# Перевод одного видео с настройками по умолчанию
yvt 'https://www.youtube.com/watch?v=example_video'

# Свои громкость/битрейт
yvt -v 0.5 -b 800 'https://www.youtube.com/watch?v=example_video'

# Пакетный режим: обработать все ссылки из файла
yvt -v .2 -b 500 -f ./linklist.txt

# Перевод на английский, субтитры на русском
yvt -l en -s ru 'https://www.youtube.com/watch?v=example_video'
```

## 🤝 Contributing

1. Сделайте форк репозитория
2. Создайте ветку с фичей (`git checkout -b feature/amazing-feature`)
3. Закоммитьте изменения (`git commit -m 'Add amazing feature'`)
4. Запушьте ветку (`git push origin feature/amazing-feature`)
5. Откройте Pull Request

## 📝 Лицензия

GNU General Public License v3.0 — подробности в файле [LICENSE](LICENSE).

Изначально создано [s-n-alexeyev](https://github.com/s-n-alexeyev/yvt).
