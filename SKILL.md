---
name: youtube
description: Download YouTube transcripts, metadata, and videos using yt-dlp. Use when user shares YouTube URLs or asks about video content.
allowed-tools:
  - Bash
---

# YouTube Video Tool (yt-dlp)

## Before Any Operation

Before running any yt-dlp command, **always check dependencies are installed** by running this silently:

```bash
# Check and install Homebrew if missing
if ! command -v brew &>/dev/null; then
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
fi

# Check and install yt-dlp if missing
if ! command -v yt-dlp &>/dev/null; then
  brew install yt-dlp
fi
```

Do this automatically without asking the user. If brew or yt-dlp need to be installed, just tell the user "Installing required tools..." and proceed.

## Common Operations

### Get Video Title
```bash
yt-dlp --print title "URL"
```

### Get Full Metadata (JSON)
```bash
yt-dlp --dump-json "URL" | jq '{title, description, duration, uploader, upload_date}'
```

### Download Transcript/Subtitles
```bash
# Auto-generated English subtitles
yt-dlp --write-auto-sub --skip-download --sub-lang en -o "output_name" "URL"

# Manual subtitles (if available)
yt-dlp --write-sub --skip-download --sub-lang en -o "output_name" "URL"
```

### List Available Subtitles
```bash
yt-dlp --list-subs "URL"
```

### Download Video
```bash
# Best quality
yt-dlp -f best -o "%(title)s.%(ext)s" "URL"

# Specific format (mp4)
yt-dlp -f "bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]" -o "%(title)s.%(ext)s" "URL"

# Audio only (mp3)
yt-dlp -x --audio-format mp3 -o "%(title)s.%(ext)s" "URL"
```

### Download Playlist
```bash
yt-dlp -o "%(playlist_title)s/%(title)s.%(ext)s" "PLAYLIST_URL"
```

## Troubleshooting

### 403 Errors / SABR Streaming

YouTube frequently changes its streaming format. If you get 403 errors:

1. **Update yt-dlp first**: `brew upgrade yt-dlp`
2. **Try the TV client**: `yt-dlp --extractor-args "youtube:player_client=tv" "URL"`
3. **List formats to diagnose**: `yt-dlp -F "URL"`

### Processing VTT Transcripts

VTT files contain timestamps. To extract plain text:
```bash
grep -v '^[0-9]' file.en.vtt | grep -v '^WEBVTT' | grep -v '^Kind:' | grep -v '^Language:' | grep -v '^$' | sed 's/<[^>]*>//g' | awk '!seen[$0]++' > transcript.txt
```

## Output Patterns
- `%(title)s` - Video title
- `%(ext)s` - File extension
- `%(id)s` - Video ID
- `%(uploader)s` - Channel name
- `%(upload_date)s` - Upload date (YYYYMMDD)
- `%(playlist_title)s` - Playlist name
- `%(playlist_index)s` - Position in playlist
