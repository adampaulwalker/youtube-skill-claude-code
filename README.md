# YouTube Skill for Claude Code

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that downloads YouTube transcripts, metadata, and videos using [yt-dlp](https://github.com/yt-dlp/yt-dlp).

## What It Does

When you share a YouTube URL or ask about video content, Claude Code will automatically use this skill to:

- **Extract transcripts** - auto-generated or manual subtitles as clean text
- **Get metadata** - title, description, duration, uploader, upload date
- **Download videos** - any format, quality, or audio-only
- **Process playlists** - batch download with organized output

## Installation

### Prerequisites

Install yt-dlp:
```bash
pip install yt-dlp
# or
brew install yt-dlp
```

### Add to Claude Code

Drop this folder into your Claude Code skills directory:

```bash
# Clone into your skills folder
git clone https://github.com/adampaulwalker/youtube-skill-claude-code.git ~/.claude/skills/youtube
```

Or if you manage skills in a custom location:
```bash
git clone https://github.com/adampaulwalker/youtube-skill-claude-code.git /path/to/your/skills/youtube
```

Restart Claude Code. The skill will be auto-discovered.

## Usage

Just use natural language in Claude Code:

- "Get the transcript from this video: https://youtube.com/watch?v=..."
- "Download this YouTube video as mp3"
- "What's this video about? https://youtube.com/watch?v=..."
- `/youtube` to invoke directly

## How It Works

Claude Code auto-discovers skills by scanning `~/.claude/skills/` for folders containing a `SKILL.md` file. This skill gives Claude the `Bash` tool permission to run `yt-dlp` commands for YouTube operations.

See [SKILL.md](SKILL.md) for the full command reference.

## License

MIT
