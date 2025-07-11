# Complete Steel Browser Local Setup Guide

## 📋 Prerequisites

### System Requirements
- **Node.js**: Version 22 or higher (required by `engines.node`)
- **Chrome Browser**: Latest version installed
- **Git**: For cloning the repository  
- **Docker**: Optional but recommended for easier setup

### Chrome Executable Paths
Ensure Chrome is installed in one of these standard locations:
- **Linux**: `/usr/bin/google-chrome`
- **macOS**: `/Applications/Google Chrome.app/Contents/MacOS/Google Chrome`
- **Windows**: 
  - `C:\Program Files\Google\Chrome\Application\chrome.exe`
  - `C:\Program Files (x86)\Google\Chrome\Application\chrome.exe`

## 🚀 Setup Options

### Option 1: Docker Setup (Recommended)

**For Production/Demo:**
```bash
# Clone repository
git clone https://github.com/steel-dev/steel-browser
cd steel-browser

# Start with pre-built images
docker compose up
```

**For Development with Live Reloading:**
```bash
# Use development compose file
docker compose -f docker-compose.dev.yml up --build
```

### Option 2: Node.js Direct Setup

```bash
# 1. Clone and enter directory
git clone https://github.com/steel-dev/steel-browser
cd steel-browser

# 2. Install dependencies (workspace-aware)
npm install

# 3. Start development servers
npm run dev
```

## ⚙️ Configuration

### Environment Setup
```bash
# Copy environment template
cp .env.example .env

# Edit .env file with your settings
# Default content:
# VITE_API_URL=http://HOST:3000
# VITE_WS_URL=ws://HOST:3000
```

### Custom Chrome Path (if needed)
```bash
# For non-standard Chrome installations
export CHROME_EXECUTABLE_PATH=/path/to/your/chrome
npm run dev
```

### Host Configuration
For custom host setups, modify the `.env` file:
```env
VITE_API_URL=http://your-host:3000
VITE_WS_URL=ws://your-host:3000
```

## 🎯 Port Configuration

**Default Ports:**
- **API Server**: `3000` (`http://localhost:3000`)
- **UI Dashboard**: `5173` (`http://localhost:5173`) 
- **CDP Protocol**: `9223` (Chrome DevTools Protocol)

**Access Points:**
- Main UI: `http://localhost:5173`
- API Documentation: `http://localhost:3000/documentation`
- API Endpoint: `http://localhost:3000`

## 🧪 Verification Steps

1. **Check API Health:**
```bash
curl http://localhost:3000/health
```

2. **Access UI Dashboard:**
Open `http://localhost:5173` in your browser

3. **Test API Documentation:**
Visit `http://localhost:3000/documentation` for Swagger UI

4. **Quick API Test:**
```bash
# Test scraping endpoint
curl -X POST http://localhost:3000/v1/scrape \
  -H "Content-Type: application/json" \
  -d '{"url": "https://example.com"}'
```

## 🛠️ Development Workflow

### Workspace Structure
```
steel-browser/
├── api/          # Backend API (Fastify + Puppeteer)
├── ui/           # Frontend Dashboard (Vite/React)
├── repl/         # Interactive REPL tool
└── docker-compose.dev.yml
```

### Development Commands
```bash
# Build all workspaces
npm run build

# Start development servers (API + UI)
npm run dev

# Work on specific workspace
npm run dev -w api    # API only
npm run dev -w ui     # UI only
```

### Testing REPL
```bash
cd repl
npm run start
```

## 🔧 Troubleshooting

### Common Issues

**Port Conflicts:**
- Change ports in `docker-compose.yml` if 3000, 5173, or 9223 are taken
- Update `.env` file accordingly

**Chrome Not Found:**
- Verify Chrome installation path
- Set `CHROME_EXECUTABLE_PATH` environment variable
- Check file at `api/src/utils/browser.ts` for detection logic

**Docker Build Issues:**
- Use `--build` flag: `docker compose up --build`
- Clear Docker cache if needed

**Node.js Version Issues:**
```bash
# Check Node.js version
node --version

# Should be 22 or higher
# If not, update Node.js or use nvm/nvs
```

**Permission Issues (Linux/macOS):**
```bash
# Fix Docker permissions
sudo usermod -aG docker $USER
# Log out and back in

# Fix npm permissions
npm config set prefix ~/.npm-global
export PATH=~/.npm-global/bin:$PATH
```

### Port Conflict Resolution

**Check what's using your ports:**
```bash
# Check port usage
lsof -i -P -n | grep LISTEN

# Kill process on specific port
lsof -ti:3000 | xargs kill -9
```

**Alternative port configuration:**
```yaml
# docker-compose.yml
services:
  api:
    ports:
      - "3001:3000"  # Use 3001 instead of 3000
      - "9224:9223"  # Use 9224 instead of 9223
  ui:
    ports:
      - "5174:80"    # Use 5174 instead of 5173
```

## 📁 Project Structure Understanding

### Key Files
- `package.json`: Root workspace configuration
- `docker-compose.yml`: Production Docker setup
- `docker-compose.dev.yml`: Development Docker setup
- `.env.example`: Environment template
- `api/`: Backend API server
- `ui/`: Frontend dashboard
- `docs/`: Documentation (this file!)

### API Structure
```
api/src/
├── modules/          # Feature modules
│   ├── actions/      # Quick action endpoints (/scrape, /screenshot, /pdf)
│   ├── sessions/     # Browser session management
│   ├── cdp/          # Chrome DevTools Protocol
│   └── selenium/     # Selenium WebDriver compatibility
├── services/         # Business logic
├── plugins/          # Fastify plugins
└── utils/            # Utility functions
```

### UI Structure
```
ui/src/
├── components/       # React components
├── containers/       # Page containers
├── hooks/           # Custom React hooks
├── steel-client/    # Auto-generated API client
└── styles/          # Styling and themes
```

## 🚀 Next Steps

1. **Explore the API**: Visit `/documentation` for full REST API reference
2. **SDK Integration**: Install `steel-sdk` for Node.js or Python projects
3. **Examples**: Check the [Cookbook](https://github.com/steel-dev/steel-cookbook)
4. **Connect Puppeteer/Playwright**: Use session endpoints for browser automation
5. **Join Community**: Discord at [discord.gg/steel-dev](https://discord.gg/steel-dev)

## 🎯 Quick Start Checklist

- [ ] Node.js 22+ installed
- [ ] Chrome browser installed
- [ ] Repository cloned
- [ ] Dependencies installed (`npm install`)
- [ ] Environment configured (`.env` file)
- [ ] Services started (`npm run dev` or `docker compose up`)
- [ ] API accessible at `http://localhost:3000`
- [ ] UI accessible at `http://localhost:5173`
- [ ] Documentation available at `http://localhost:3000/documentation`

**Ready to build!** 🎉 Steel Browser should now be running locally with full browser automation capabilities.

---

*Last updated: $(date)*
*For issues or questions, check the [GitHub Issues](https://github.com/steel-dev/steel-browser/issues)*