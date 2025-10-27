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

Extract the downloaded ZIP file to both directories:

```bash
# Extract to public directory (for development)
unzip TFRdata.zip -d public/data

# If files are nested, move them to correct location:
# The structure should be: public/data/index.json, public/data/ideology/, etc.
```

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

**Images not loading in production?**
- Verify `dist/data/` directory exists after build
- Check web server configuration serves static files correctly
- Ensure the build process completed successfully

## License

Community project for TFR game.
