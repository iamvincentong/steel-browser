# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Steel Browser is an open-source browser API that enables AI agents and applications to interact with web browsers programmatically. It provides a REST API for browser automation, session management, and web scraping with anti-detection capabilities.

## Architecture

### Monorepo Structure
This is a Node.js monorepo with three main workspaces:
- `api/` - Fastify-based REST API server (main backend)
- `ui/` - React-based web dashboard for managing sessions  
- `repl/` - Interactive REPL for testing browser automation

### Core Components

**API Server (`api/`):**
- **Entry Point**: `src/index.ts` - Fastify server setup
- **Plugin System**: `src/steel-browser-plugin.ts` - Main plugin that registers all functionality
- **Services**: `src/services/` - Core business logic
  - `session.service.ts` - Session lifecycle management
  - `cdp/cdp.service.ts` - Chrome DevTools Protocol integration
  - `selenium.service.ts` - Selenium WebDriver compatibility
- **Routes**: `src/modules/` - API endpoints organized by feature
  - `sessions/` - Session management endpoints
  - `actions/` - Quick actions (scrape, screenshot, PDF)
  - `selenium/` - Selenium-compatible endpoints
  - `cdp/` - Chrome DevTools Protocol endpoints
- **Browser Management**: `src/services/cdp/` - Puppeteer integration and browser lifecycle
- **Proxy Support**: `src/utils/proxy.ts` - Proxy chain management
- **Anti-Detection**: `src/scripts/fingerprint.js` - Browser fingerprinting

**UI Dashboard (`ui/`):**
- React + TypeScript frontend
- Session management interface
- Live session viewer with Chrome DevTools integration
- Built with Vite, uses Tailwind CSS and Radix UI components

## Development Commands

### Root Level
```bash
# Install dependencies for all workspaces
npm install

# Build API and UI
npm run build

# Start both API and UI in development mode
npm run dev

# Prepare husky hooks
npm run prepare
```

### API Development (`api/`)
```bash
# Development with hot reload
npm run dev

# Build TypeScript
npm run build

# Start production server
npm run start

# Format code
npm run pretty

# Generate OpenAPI schema
npm run generate:openapi
```

### UI Development (`ui/`)
```bash
# Development server
npm run dev

# Build for production
npm run build

# Lint TypeScript/React
npm run lint

# Generate API client from OpenAPI
npm run generate-api
```

### REPL Testing (`repl/`)
```bash
# Run interactive REPL
npm run start
```

## Browser Requirements

Chrome must be installed at these locations:
- **Linux**: `/usr/bin/google-chrome`
- **macOS**: `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`
- **Windows**: `C:\Program Files\Google\Chrome\Application\chrome.exe`

Use `CHROME_EXECUTABLE_PATH` environment variable for custom locations.

## Docker Development

```bash
# Development with hot reload
docker compose -f docker-compose.dev.yml up --build

# Production deployment
docker compose up
```

## Key Technologies

- **Backend**: Fastify, Puppeteer, Chrome DevTools Protocol
- **Frontend**: React, TypeScript, Vite, Tailwind CSS
- **Database**: LevelDB for session storage
- **Proxy**: HTTP/HTTPS/SOCKS proxy support via proxy-chain
- **Anti-Detection**: fingerprint-generator, stealth plugins

## Environment Variables

Key environment variables (see `api/src/env.ts`):
- `PORT` - API server port (default: 3000)
- `HOST` - Server host (default: 0.0.0.0)
- `CHROME_EXECUTABLE_PATH` - Custom Chrome path
- `NODE_ENV` - Environment (development/production)
- `LOG_LEVEL` - Logging level
- `ENABLE_VERBOSE_LOGGING` - Enable detailed logging

## Session Management

Sessions are the core abstraction - each session represents a browser instance with:
- Unique session ID
- Browser context (cookies, localStorage, etc.)
- WebSocket connection for real-time communication
- Optional proxy configuration
- Configurable timeout and resource limits

## API Endpoints

**Sessions**: `/v1/sessions` - Create, manage, and control browser sessions
**Actions**: `/v1/scrape`, `/v1/screenshot`, `/v1/pdf` - Quick one-off actions
**Selenium**: `/v1/selenium` - Selenium WebDriver compatibility
**CDP**: `/v1/cdp` - Chrome DevTools Protocol proxy
**Files**: `/v1/files` - File upload/download for sessions

## Testing

No automated tests are currently configured. The REPL in `/repl` can be used for manual testing and development.