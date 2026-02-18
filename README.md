# Quick Replies & Images

A lightweight single-page tool for browsing saved reply templates and product images sourced directly from Google Sheets. Designed for one-tap copying of text + imagery, fast search, and mobile-friendly usage.

## Features
- Live sync from two Google Sheets tabs (Saved Replies + Images) with zero backend
- Category chips, instant search, and tabbed layout for replies vs. image gallery
- Two-column reply cards (description + image) optimized for desktop yet responsive on mobile
- One-tap copy that attempts to place both text and image HTML on the clipboard (falls back to text when image copy is blocked)
- Persistent sheet configuration stored in `localStorage` via a settings modal

## Getting Started
1. Clone or download this repository.
2. Rename `index_with_settings.html` to `index.html` if you plan to host it anywhere that expects a default entry file.
3. Serve the file locally (for example `npx serve` or the VS Code Live Server extension) so browser clipboard APIs run in a secure context.

## Configuring Sheet Sources
Open the ⚙️ settings modal inside the app and provide two published CSV URLs or sheet IDs:
- **Saved Replies Sheet** – rows: `title, category, description, imageUrl`
- **Image Gallery Sheet** – rows: `productName, __imageUrl__ pairs`

Notes:
- You can paste either a full `https://docs.google.com/spreadsheets/d/.../pub?...output=csv` link or just the sheet ID. The app converts IDs into CSV endpoints automatically.
- Values are stored in `localStorage` keys `REPLIES_SHEETS_CONFIG` and `IMAGES_SHEETS_CONFIG`.
- The legacy single-config key `SHEETS_CONFIG` is still read once for backward compatibility.

## Clipboard Behavior
- Browsers require a secure context (HTTPS or localhost) for rich clipboard writes.
- When an image URL supports CORS, the app fetches the blob, converts it to a data URL, and writes combined `text/html` + `text/plain` so pasting into rich editors preserves both.
- If the fetch fails (CORS/mixed content) it falls back to text and shows a toast explaining the issue.

## Deploying to Vercel (recommended)
1. Install Vercel CLI: `npm i -g vercel`.
2. Ensure the entry file is `index.html`.
3. Run `vercel` from the project folder and follow the prompts. Drag-and-drop or GitHub imports via the Vercel dashboard work as well.

## Local Development Tips
- Use `Live Server`, `npx serve`, or any static file server: `npx http-server -p 5500`.
- Because data loads from Google Sheets, make sure the sheets are published to the web (File → Share → Publish to web) so anonymous fetches succeed.
- If clipboard image copy fails due to specific hosting/CDN rules, consider proxying the image through your own server with proper `Access-Control-Allow-Origin` headers.

