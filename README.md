# Google Drive Lyrics Embedding Script

A Google Colab–ready Python script that scans a music folder in Google Drive, detects whether audio files already contain embedded lyrics, and—when missing—automatically fetches and embeds lyrics using free public APIs.

The script is designed for stability, resumability, and Drive-safe operation, making it suitable for large libraries processed over long sessions.

---

## Features

- Supports multiple audio formats:
  - MP3 (ID3 USLT frames)
  - FLAC (Vorbis comments)
  - OGG / OGA (Vorbis)
  - OPUS
  - M4A / MP4 (AAC in MP4 containers)
- Automatically skips:
  - Files that already contain lyrics
  - Files already processed in previous runs
  - Raw .aac files (no reliable tag container)
- Uses free lyric sources (no API keys required)
- Safe for Google Drive:
  - Writes are performed on a local copy, then atomically copied back
  - Avoids common Drive corruption and disconnect issues
- Persistent progress log stored on Google Drive
- Resume-safe: re-running the script continues where it left off
- Console progress tracking with heartbeat output for long operations

---

## How It Works

1. Mounts Google Drive in Google Colab
2. Recursively scans a target music folder
3. For each supported audio file:
   - Checks if lyrics are already embedded
   - Extracts artist/title metadata (with filename fallback)
   - Fetches lyrics from public APIs
   - Embeds lyrics into the file using the correct tagging format
4. Records results in a persistent log file on Drive

---

## Supported Formats & Tagging

| Format | Tagging Method |
|------|---------------|
| MP3 | ID3 USLT frame |
| FLAC | Vorbis comment LYRICS |
| OGG / OGA | Vorbis comments |
| OPUS | Vorbis comments |
| M4A / MP4 | MP4 ©lyr atom |
| AAC (raw) | Skipped |

---

## Requirements

- Google Colab
- Python 3.x
- Internet access (for lyric fetching)

Dependencies (installed automatically in Colab):

- mutagen
- requests

---

## Usage

1. Open a new Google Colab notebook
2. Paste the script into a cell
3. Set your music folder path:

   MUSIC_FOLDER = "/content/drive/MyDrive/MyMusic"

4. Run the cell
5. Leave the notebook open while it processes

You can safely stop and restart the script at any time. Already-processed files will be skipped.

---

## Output & Logging

- Progress is printed periodically to the console
- A persistent log file is stored at:

  /content/drive/MyDrive/lyrics_log.json

The log records:
- File status (embedded, already present, skipped, error, etc.)
- File size and modification time
- Timestamp of processing

The log enables fast skipping and safe resumption after disconnects.

---

## Notes on Google Drive Performance

Google Drive is not a local filesystem. For best results:

- Avoid syncing the same folder from another computer while running
- Keep the Colab tab active
- Expect slower performance with very large libraries
- The script prioritizes correctness and safety over raw speed

---

## License

This project is released under the CC0 1.0 Universal (Public Domain Dedication).

You may copy, modify, distribute, and use this script for any purpose, without restriction.

See:
https://creativecommons.org/publicdomain/zero/1.0/

---

## Disclaimer

This script fetches lyrics from public lyric services.
Accuracy, completeness, and copyright status of lyrics depend on the upstream sources.
Use responsibly and in accordance with applicable laws.
