# n8n Content Pipeline

> Watches a Google Drive folder for new video files, uses Gemini AI to analyze the content and auto-generate optimized metadata, then publishes simultaneously to YouTube Shorts and TikTok.

## What It Does

- **Trigger**: Google Drive folder watch — fires whenever a new video is uploaded
- **AI Metadata Generation**: Sends the video to Gemini 2.5 Flash for content analysis
- **YouTube Shorts**: Auto-generates title (≤100 chars with `#shorts`), description, and tags
- **TikTok**: Auto-generates caption (≤2200 chars with ASMR hashtags)
- **Dual Publish**: Uploads to both platforms in a single workflow run

## Tech Stack

- **n8n** (self-hosted or cloud)
- Google Drive API (OAuth2) — folder watch trigger + file download
- Google Gemini API (`gemini-2.5-flash`) — video analysis
- YouTube Data API v3 (OAuth2)
- TikTok Content Posting API (OAuth2)

## Setup

1. **Import the workflow** (`gdrive-to-youtube-tiktok-shorts.json`)
2. **Set your Google Drive folder ID** — replace `REPLACE_WITH_GOOGLE_DRIVE_FOLDER_ID` in the trigger node
3. **Configure credentials** in n8n:
   - Google Drive OAuth2
   - Google Gemini API key
   - YouTube OAuth2
   - TikTok OAuth2
4. **Activate** the workflow

## Workflow Overview

```
Google Drive Trigger (new file in folder)
  → Filter: video files only
  → Prepare metadata context
  → Download video file
  → Gemini 2.5 Flash: analyze video + generate metadata JSON
  → Parse AI metadata
  → [parallel]
       → Upload to YouTube Shorts
       → Upload to TikTok
```

## Gemini Prompt

The workflow asks Gemini for a single minified JSON object with:
- `youtube_title` — ≤100 chars, includes `#shorts` and `#asmr`
- `youtube_description` — concise with hashtags
- `youtube_tags` — array of 6–12 strings
- `tiktok_caption` — ≤2200 chars with ASMR hashtags
- `hook` — attention-grabbing opening line
- `vibe` — mood descriptor

## Customization

Swap the Gemini prompt for any niche (cooking, gaming, nature, etc.) — the publish pipeline stays the same.
