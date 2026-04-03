---
name: youtube
description: Download YouTube transcripts, metadata, and videos using yt-dlp. Use when user shares YouTube URLs or asks about video content.
allowed-tools:
  - Bash
---

# YouTube Video Tool (yt-dlp)

Use `yt-dlp` for all YouTube operations. Install it via `pip install yt-dlp` or `brew install yt-dlp`.

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

## Processing VTT Transcripts

VTT files contain timestamps. To extract plain text:
```bash
# Remove timestamps and duplicates
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
