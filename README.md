# ChromaSense AI

AI-powered colour analysis tool that takes an image, identifies distinct regions, and returns a suggested hex colour palette for each — built as a single-file web app using the GPT-4 Vision API.

## What it does

- Accepts an uploaded image via the browser
- Converts it to base64 and sends it to GPT-4 Vision with a structured prompt
- Parses the model's response to extract hex colour codes and region labels
- Renders a visual colour swatch next to each label in the browser

## How it works

### Image encoding
The browser's `FileReader` API reads the uploaded file and converts it to a base64 data URL. The prefix (`data:image/...;base64,`) is stripped, leaving raw base64 which is embedded directly in the API request — no file upload to a server needed.

### API request
The base64 image is sent to the GPT-4 Vision endpoint alongside a text prompt instructing the model to return colours in a structured format:

```
Color: #HexCode; Part: part_name
```

For example:
```
Color: #2C3E50; Part: Background
Color: #E74C3C; Part: Roof
Color: #F39C12; Part: Door
```

### Response parsing
The response is split line by line. Each line is matched against two regex patterns — one for the hex code, one for the part name. Valid pairs are rendered as a coloured square with a label.

## Setup

This is a pure frontend app — no server or build step required.

1. Get an API key from [platform.openai.com](https://platform.openai.com)
2. Open `chromasense.html` and replace `YOUR_API_KEY_HERE` with your key
3. Open the file in any browser

> **Note:** Never commit a real API key to a public repo. For a production version, the API call should be proxied through a backend server so the key is never exposed client-side.

## Files

| File | Description |
|------|-------------|
| `chromasense.html` | Single-file app — all HTML, CSS, and JavaScript |

## Notes

- Uses GPT-4 Vision (`gpt-4-vision-preview`) — requires an OpenAI account with Vision API access
- Works best on line drawings or illustrations with clearly distinct regions
- `max_tokens: 2000` limits response length — reduce for faster/cheaper responses on simple images
