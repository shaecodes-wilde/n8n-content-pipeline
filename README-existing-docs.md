# Google Drive -> Gemini Metadata -> YouTube Shorts + TikTok (n8n)

Workflow file:
- `workflows/gdrive-to-youtube-tiktok-shorts.json`

## Setup
1. Import the workflow JSON into n8n.
2. In `Google Drive Trigger`, replace `REPLACE_WITH_GOOGLE_DRIVE_FOLDER_ID` with your source folder ID.
3. Attach Google Drive OAuth2 credentials to:
   - `Google Drive Trigger`
   - `Download Video`
4. Attach Google Gemini credentials (`googlePalmApi`) to:
   - `Gemini Analyze Video`
5. Attach YouTube OAuth2 credentials to:
   - `Upload Short to YouTube`
6. Set an environment variable in your n8n instance:
   - `TIKTOK_ACCESS_TOKEN=<your current access token>`

## What changed
- Added `Gemini Analyze Video` (`Video -> Analyze Video`) using binary input `videoFile`.
- Added `Parse Gemini Metadata` to safely parse model output JSON.
- Added `Merge AI + File Context` + `Finalize Metadata` so uploads always have binary + fallback metadata.

## TikTok API prerequisites
- Your TikTok app must have Content Posting API access.
- The token must include the `video.publish` scope.
- The TikTok account must authorize your app.

## Important current limitation
- This workflow uses single-chunk TikTok upload (`total_chunk_count: 1`).
- Keep video files at or under 64 MB for TikTok uploads.

## Customization
- Edit the prompt in `Gemini Analyze Video` to change tone/output style.
- Edit `Finalize Metadata` for fallback hashtag policy and title/caption limits.
- Edit YouTube privacy in `Upload Short to YouTube` (`public`, `unlisted`, or `private`).
