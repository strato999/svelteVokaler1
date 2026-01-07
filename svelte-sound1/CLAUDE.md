# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a simple Svelte 3 SPA (Single Page Application) that plays audio files. The app displays 10 buttons (A1-A10) that trigger corresponding MP3 files from the `public/mySounds/` directory when clicked.

## Build System & Development

This project uses Rollup as its build tool (configured in `rollup.config.js`).

### Common Commands

```bash
# Install dependencies
npm install

# Start development server with watch mode (requires two terminal windows)
npm run dev          # Terminal 1: Starts Rollup in watch mode
npm run start        # Terminal 2: Starts sirv server on localhost:8080

# Build for production
npm run build        # Creates optimized bundle in public/build/

# Run production build
npm run start        # Serves the public/ directory
```

**Important**: Development requires running two separate commands in two terminal windows - `npm run dev` for the build watcher and `npm run start` for the local server.

## Architecture

### Build Pipeline

- **Entry Point**: `src/main.js` imports and instantiates the main `App.svelte` component
- **Output**: Rollup bundles everything into `public/build/bundle.js` (IIFE format)
- **CSS Extraction**: Component styles are extracted to `public/build/bundle.css` via `rollup-plugin-css-only`
- **Production Optimization**: Uses `@rollup/plugin-terser` for minification in production mode

### Application Structure

**Single Component Architecture**: The entire app is contained in one component (`src/App.svelte`):
- Generates 10 buttons dynamically using Svelte's `{#each}` block
- Each button plays a corresponding MP3 file using the Web Audio API
- All styles are scoped within the component
- No routing or state management - intentionally simple

### Static Assets

- **HTML Entry**: `public/index.html` loads the bundled JavaScript
- **Audio Files**: Located in `public/mySounds/` directory, named `A1.mp3` through `A10.mp3`
- **Global Styles**: `public/global.css` (mostly unused, styles are in component)

## Technical Notes

- This project is based on the legacy Svelte template (`sveltejs/template`) which is no longer maintained
- The template itself suggests using `npm init vite` or SvelteKit for new projects
- Uses Svelte 3.x (not Svelte 5)
- Module format: ES modules (`"type": "module"` in package.json)
- No TypeScript setup by default (though `scripts/setupTypeScript.js` exists for migration)
