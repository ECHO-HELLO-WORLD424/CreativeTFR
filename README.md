# CreativeTFR

A community-driven UI recreation tool for TFR game. Built with Vue3 as a serverless, frontend-only application with IndexedDB for local storage.

[中文文档](./README.zh.md)

## Features

- Vue3-based modern frontend application
- Serverless architecture - runs entirely in the browser
- Local data persistence using IndexedDB
- HTML formatting support in text inputs
- Image management system for game assets

## Prerequisites

- Node.js (v16 or higher) or Bun
- npm, yarn, or bun package manager

## Installation

### 1. Clone the Repository

```bash
git clone <repository-url>
cd CreativeTFR
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Download Game Assets

Download the TFR asset database: [TFRdata.zip](http://997779.xyz/share/TFRdata.zip) (1.5GB after extraction)

### 4. Extract Assets

The ZIP file contains nested directories. Extract and fix the structure:

```bash
# Extract to a temporary location first
unzip TFRdata.zip

# Move files to correct location for development
# Note: The ZIP creates root/CreativeTFR/data/* structure
cp -r root/CreativeTFR/data/* public/data/
rm -rf root

# Verify the structure is correct
ls public/data/
# Should show: index.json, ideology/, event/, flag/, focus/, leader/, news/, etc.

# Also copy to dist for production (if you've already built)
cp -r public/data dist/
```

**Alternative one-liner approach:**

```bash
unzip TFRdata.zip && cp -r root/CreativeTFR/data/* public/data/ && rm -rf root && cp -r public/data dist/
```

### 5. Setup Spirit Images

The Spirit Manager (国家精神管理) requires spirit images to be in the `public/data/spirit/` directory. Copy preset images and regenerate the index:

```bash
# Create spirit directory
mkdir -p public/data/spirit

# Copy spirit images from preset
cp public/preset/*.png public/data/spirit/

# Regenerate index.json to include spirit images
cd public/data
python3 ftojson.py
cd ../..
```

This ensures the built-in spirit images appear correctly in the Spirit Manager dialog.

## Development

Start the development server:

```bash
npm run dev
```

The application will be available at `http://localhost:5173` (or another port if 5173 is occupied).

## Production Build

### Build the Application

```bash
npm run build
```

This creates an optimized production build in the `dist/` directory.

### Preview Production Build Locally

```bash
npm run preview
```

### Deploy to Production

Deploy the `dist/` folder to any static hosting service:

- **Nginx**: Point document root to the `dist/` directory
- **Apache**: Configure virtual host to serve the `dist/` directory
- **Static Hosts**: Upload `dist/` to Netlify, Vercel, GitHub Pages, etc.

Or: you can deploy via http server and do a reverse proxy with nginx:
```bash
npx http-server dist -p 8080
```
Then add reverse proxy configuration to nginx

#### Example Nginx Configuration

```nginx
server {
    listen 80;
    server_name yourdomain.com;
    root /path/to/CreativeTFR/dist;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

## Important Notes

- **Data Storage**: All user data is stored locally in the browser's IndexedDB. No cloud sync or multi-device support.
- **HTML Support**: Input fields support raw HTML formatting.
- **Asset Structure**: Game assets must be in `public/data/` for development and will be copied to `dist/data/` during build.

## Custom Image Requirements

The application supports uploading custom images for various elements. For best results, use the following dimensions:

| Image Type | Recommended Size | Format | Notes |
|------------|-----------------|--------|-------|
| **Ideology** | 66 x 68 px | PNG with transparency | Icon for political ideology |
| **Leader** | 156 x 210 px | PNG | Portrait image |
| **Flag** | 82 x 52 px | PNG with transparency | National flag icon |
| **Faction** | 60 x 60 px | PNG with transparency | Alliance/faction icon |
| **Focus** | 90 x 93 px | PNG with transparency | National focus tree icon |
| **Event** | 500 x 250 px | PNG | Event picture (2:1 ratio) |
| **News** | 400 x 150 px | PNG | News article image |
| **Super Event** | 982 x 594 px | PNG | Large dramatic event image |

**Tips for Custom Images:**
- Use PNG format for transparency support
- Match the recommended dimensions for best visual quality
- Images are resizable in the editor for some types (ideology, faction, focus, event)
- Keep file sizes reasonable for browser performance

## Project Structure

```
CreativeTFR/
├── public/           # Static assets (served at root URL)
│   └── data/        # Game assets (images, index.json)
├── src/             # Vue3 source code
├── dist/            # Production build output
└── package.json     # Project dependencies
```

## Troubleshooting

**Images not loading in development?**
- Ensure assets are extracted to `public/data/`
- Check that `public/data/index.json` exists
- Restart the dev server after adding assets

**Spirit images not appearing in Spirit Manager?**
- Run the setup command: `mkdir -p public/data/spirit && cp public/preset/*.png public/data/spirit/`
- Regenerate index: `cd public/data && python3 ftojson.py && cd ../..`
- Check that `public/data/spirit/` directory contains PNG files

**Images not loading in production?**
- Verify `dist/data/` directory exists after build
- Check web server configuration serves static files correctly
- Ensure the build process completed successfully

## License

Community project for TFR game.
