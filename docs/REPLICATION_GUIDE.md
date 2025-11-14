# Complete Step-by-Step Replication Guide - Open Lovable

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Quick Start](#quick-start)
3. [Detailed Setup](#detailed-setup)
4. [Configuration Guide](#configuration-guide)
5. [Building from Scratch](#building-from-scratch)
6. [Development Workflow](#development-workflow)
7. [Deployment Guide](#deployment-guide)
8. [Troubleshooting](#troubleshooting)
9. [Advanced Topics](#advanced-topics)

---

## Prerequisites

### Required Software

#### 1. Node.js (Version 22+ LTS)

**Install Node.js**:

**macOS** (using Homebrew):
```bash
brew install node@22
```

**Linux** (using nvm):
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install 22
nvm use 22
```

**Windows** (using installer):
- Download from [nodejs.org](https://nodejs.org/)
- Install LTS version (22.x)

**Verify Installation**:
```bash
node --version  # Should show v22.x.x
npm --version   # Should show 10.x.x
```

#### 2. Package Manager

**Option A: npm** (comes with Node.js)
```bash
npm --version
```

**Option B: pnpm** (recommended - faster, space-efficient)
```bash
npm install -g pnpm
pnpm --version
```

**Option C: yarn**
```bash
npm install -g yarn
yarn --version
```

#### 3. Git

**Verify Installation**:
```bash
git --version
```

**Install if needed**:
- **macOS**: `brew install git`
- **Linux**: `sudo apt-get install git`
- **Windows**: Download from [git-scm.com](https://git-scm.com/)

#### 4. Code Editor

**Recommended**: Visual Studio Code

**Install Extensions**:
- ESLint
- Prettier
- Tailwind CSS IntelliSense
- TypeScript and JavaScript Language Features

### Required Accounts & API Keys

#### 1. Firecrawl API (Web Scraping)

**Sign Up**: [firecrawl.dev](https://firecrawl.dev)

**Get API Key**:
1. Create account
2. Navigate to API keys section
3. Copy API key (starts with `fc-`)

**Cost**: Free tier available

#### 2. AI Provider (at least one required)

**Option A: Anthropic (Claude)**
- **Sign Up**: [console.anthropic.com](https://console.anthropic.com)
- **Get Key**: API Keys section (starts with `sk-ant-`)
- **Cost**: Pay-per-use, free trial credits

**Option B: OpenAI (GPT)**
- **Sign Up**: [platform.openai.com](https://platform.openai.com)
- **Get Key**: API Keys section (starts with `sk-`)
- **Cost**: Pay-per-use, free trial credits

**Option C: Google (Gemini)**
- **Sign Up**: [ai.google.dev](https://ai.google.dev)
- **Get Key**: API Keys section
- **Cost**: Free tier available

**Option D: Groq (Kimi)**
- **Sign Up**: [console.groq.com](https://console.groq.com)
- **Get Key**: API Keys section (starts with `gsk_`)
- **Cost**: Free tier available

#### 3. Sandbox Provider (choose one)

**Option A: Vercel (Recommended for beginners)**

No explicit sign-up needed for development. For production:
- **Sign Up**: [vercel.com](https://vercel.com)
- **Install CLI**: `npm i -g vercel`
- **Login**: `vercel login`
- **Link Project**: `vercel link`
- **Pull Env**: `vercel env pull .env.local`

**Option B: E2B (More powerful)**
- **Sign Up**: [e2b.dev](https://e2b.dev)
- **Get Key**: Dashboard → API Keys
- **Cost**: Pay-per-use, free tier available

---

## Quick Start

### Clone and Run (5 minutes)

```bash
# 1. Clone repository
git clone https://github.com/firecrawl/open-lovable.git
cd open-lovable

# 2. Install dependencies
npm install
# or
pnpm install

# 3. Copy environment template
cp .env.example .env.local

# 4. Edit .env.local with your API keys
# Open in your editor and fill in the keys

# 5. Start development server
npm run dev

# 6. Open browser
# Navigate to http://localhost:3000
```

**That's it!** You should see the landing page.

---

## Detailed Setup

### Step 1: Clone Repository

```bash
# HTTPS
git clone https://github.com/firecrawl/open-lovable.git

# SSH (if you have SSH keys set up)
git clone git@github.com:firecrawl/open-lovable.git

# Navigate to project
cd open-lovable
```

### Step 2: Install Dependencies

```bash
# Using npm
npm install

# Using pnpm (faster)
pnpm install

# Using yarn
yarn install
```

**Expected Output**:
```
added 89 packages in 30s
```

**Common Issues**:
- **Error: EACCES**: Run with sudo or fix npm permissions
- **Error: ERESOLVE**: Use `npm install --legacy-peer-deps`
- **Slow install**: Try pnpm for faster installs

### Step 3: Environment Configuration

#### Create Environment File

```bash
cp .env.example .env.local
```

#### Configure Environment Variables

Open `.env.local` in your editor:

```bash
# .env.local

#########################################
# REQUIRED - Core Services
#########################################

# Firecrawl API (web scraping)
FIRECRAWL_API_KEY=fc-your-key-here

# Sandbox Provider (choose one)
SANDBOX_PROVIDER=vercel  # or 'e2b'

#########################################
# REQUIRED - At least one AI provider
#########################################

# Anthropic (Claude) - Recommended
ANTHROPIC_API_KEY=sk-ant-your-key-here

# OpenAI (GPT)
# OPENAI_API_KEY=sk-your-key-here

# Google (Gemini)
# GEMINI_API_KEY=your-key-here

# Groq (Kimi)
# GROQ_API_KEY=gsk_your-key-here

#########################################
# Vercel Sandbox Configuration
#########################################

# Option 1: OIDC Token (auto-generated in Vercel)
# VERCEL_OIDC_TOKEN=<auto-generated>

# Option 2: Manual tokens
# VERCEL_TOKEN=your-vercel-token
# VERCEL_TEAM_ID=team_xxx
# VERCEL_PROJECT_ID=prj_xxx

#########################################
# E2B Sandbox Configuration (alternative)
#########################################

# E2B_API_KEY=your-e2b-key-here

#########################################
# OPTIONAL Services
#########################################

# Morph API (fast code application)
# MORPH_API_KEY=your-morph-key

# Vercel AI Gateway (caching)
# AI_GATEWAY_API_KEY=your-gateway-key
```

#### Environment Variables Explained

**Required Variables**:

1. **`FIRECRAWL_API_KEY`**
   - Purpose: Web scraping and screenshot capture
   - Get from: [firecrawl.dev](https://firecrawl.dev)
   - Format: `fc-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`

2. **`SANDBOX_PROVIDER`**
   - Purpose: Choose sandbox runtime
   - Options: `vercel` or `e2b`
   - Recommendation: Start with `vercel`

3. **At least one AI provider key**:
   - `ANTHROPIC_API_KEY` (Claude) - Recommended
   - `OPENAI_API_KEY` (GPT)
   - `GEMINI_API_KEY` (Gemini)
   - `GROQ_API_KEY` (Kimi/Groq)

**Vercel Sandbox Setup**:

If using `SANDBOX_PROVIDER=vercel`:

**For Local Development**:
```bash
# Install Vercel CLI
npm i -g vercel

# Login to Vercel
vercel login

# Link your project
vercel link

# Pull environment variables
vercel env pull .env.local
```

This will auto-generate `VERCEL_OIDC_TOKEN` in `.env.local`.

**For Production**:
Set these in Vercel dashboard:
- `VERCEL_TOKEN` - Personal access token
- `VERCEL_TEAM_ID` - Team ID (from dashboard URL)
- `VERCEL_PROJECT_ID` - Project ID (from settings)

**E2B Sandbox Setup**:

If using `SANDBOX_PROVIDER=e2b`:

```bash
# Just add to .env.local
E2B_API_KEY=your-e2b-api-key
```

### Step 4: Verify Configuration

```bash
# Check environment variables are loaded
npm run dev
```

**Expected Output**:
```
▲ Next.js 15.4.3
- Local:        http://localhost:3000
- Environments: .env.local

✓ Ready in 2.5s
```

**Check for Errors**:
- ❌ **Missing API key errors**: Check `.env.local`
- ❌ **Port 3000 already in use**: Kill existing process or use different port
- ❌ **TypeScript errors**: Run `npm run type-check`

### Step 5: First Run

1. **Open browser**: http://localhost:3000
2. **You should see**: Landing page with hero section
3. **Test scraping**: Enter a URL (e.g., `https://example.com`)
4. **Test generation**: Should redirect to `/generation` and start creating sandbox

---

## Configuration Guide

### Application Configuration

#### Edit `config/app.config.ts`

```typescript
export const appConfig = {
  // Vercel Sandbox Settings
  vercelSandbox: {
    timeoutMinutes: 15,      // Sandbox timeout
    devPort: 3000,           // Default dev port
    runtime: 'node22'        // Node.js version
  },

  // E2B Sandbox Settings
  e2b: {
    timeoutMinutes: 30,      // Longer timeout
    vitePort: 5173           // Vite dev server port
  },

  // AI Configuration
  ai: {
    // Default model to use
    defaultModel: 'moonshotai/kimi-k2-instruct-0905',

    // Available models
    availableModels: [
      'moonshotai/kimi-k2-instruct-0905',    // Groq/Kimi
      'openai/gpt-5-preview',                 // OpenAI
      'anthropic/claude-sonnet-4',            // Anthropic
      'google/gemini-2.0-flash-exp'          // Google
    ],

    // Generation settings
    maxTokens: 8000,         // Max tokens per request
    temperature: 0.7         // Creativity (0-1)
  },

  // Code Application Settings
  codeApplication: {
    defaultRefreshDelay: 2000,           // Delay before refresh (ms)
    enableTruncationRecovery: false      // Truncation recovery
  },

  // File System Settings
  files: {
    excludePatterns: [
      '**/node_modules/**',
      '**/.git/**',
      '**/dist/**',
      '**/.next/**',
      '**/build/**'
    ],
    maxFileSize: 1024 * 1024  // 1MB max file size
  }
};
```

**Customization Tips**:

1. **Change default AI model**:
   ```typescript
   defaultModel: 'anthropic/claude-sonnet-4'
   ```

2. **Adjust sandbox timeout**:
   ```typescript
   vercelSandbox: {
     timeoutMinutes: 30  // Increase for longer sessions
   }
   ```

3. **Modify token limit**:
   ```typescript
   ai: {
     maxTokens: 16000  // Increase for larger responses
   }
   ```

### Tailwind Configuration

#### Edit `tailwind.config.ts`

**Add Custom Colors**:
```typescript
theme: {
  extend: {
    colors: {
      'brand-primary': '#your-color',
      'brand-secondary': '#your-color'
    }
  }
}
```

**Add Custom Spacing**:
```typescript
theme: {
  extend: {
    spacing: {
      '128': '32rem',
      '144': '36rem'
    }
  }
}
```

**Add Custom Fonts**:
```typescript
theme: {
  extend: {
    fontFamily: {
      'custom': ['Your Font', 'sans-serif']
    }
  }
}
```

### Next.js Configuration

#### Edit `next.config.ts`

**Add Image Domains**:
```typescript
const nextConfig: NextConfig = {
  images: {
    domains: ['your-image-domain.com']
  }
};
```

**Add Redirects**:
```typescript
const nextConfig: NextConfig = {
  async redirects() {
    return [
      {
        source: '/old-path',
        destination: '/new-path',
        permanent: true
      }
    ];
  }
};
```

**Configure API Timeouts**:
```typescript
const nextConfig: NextConfig = {
  api: {
    responseLimit: '50mb',
    bodyParser: {
      sizeLimit: '10mb'
    }
  }
};
```

---

## Building from Scratch

If you want to create a similar project from scratch:

### Step 1: Initialize Next.js Project

```bash
npx create-next-app@latest open-lovable-clone
cd open-lovable-clone
```

**Options**:
- ✅ TypeScript: Yes
- ✅ ESLint: Yes
- ✅ Tailwind CSS: Yes
- ✅ App Router: Yes
- ✅ Turbopack: Yes
- ❌ Customize default import alias: No

### Step 2: Install Core Dependencies

```bash
npm install next@15.4.3 react@19.1.0 react-dom@19.1.0
npm install typescript@^5
```

### Step 3: Install AI SDK

```bash
npm install ai@5.0.0
npm install @ai-sdk/anthropic@2.0.1
npm install @ai-sdk/openai@2.0.4
npm install @ai-sdk/google@2.0.4
npm install @ai-sdk/groq@2.0.0
```

### Step 4: Install Sandbox Providers

```bash
npm install @vercel/sandbox@0.0.17
npm install @e2b/code-interpreter@2.0.0
```

### Step 5: Install UI Dependencies

```bash
# Styling
npm install tailwindcss@3.4.17
npm install @tailwindcss/typography@0.5.16
npm install framer-motion@12.23.12
npm install class-variance-authority@0.7.1
npm install tailwind-merge@3.3.1

# UI Components (Radix)
npm install @radix-ui/react-dialog
npm install @radix-ui/react-dropdown-menu
npm install @radix-ui/react-popover
npm install @radix-ui/react-tabs
npm install @radix-ui/react-toast
# ... add more as needed

# Icons
npm install lucide-react@0.532.0
npm install @tabler/icons-react@3.34.1

# Graphics
npm install pixi.js@8.13.1
```

### Step 6: Install Utilities

```bash
npm install jotai@2.14.0
npm install lodash-es@4.17.21
npm install nanoid@5.1.5
npm install zod@3.25.76
npm install react-hook-form@7.62.0
npm install sonner@2.0.7
npm install next-themes@0.4.6
```

### Step 7: Install Web Scraping

```bash
npm install @mendable/firecrawl-js@4.3.3
```

### Step 8: Create Directory Structure

```bash
mkdir -p app/api
mkdir -p components/{app,shared,ui}
mkdir -p lib/sandbox/providers
mkdir -p hooks
mkdir -p types
mkdir -p styles
mkdir -p config
mkdir -p atoms
```

### Step 9: Copy Core Files

From the open-lovable repository, copy:

1. **Configuration Files**:
   - `tailwind.config.ts`
   - `postcss.config.mjs`
   - `tsconfig.json`
   - `eslint.config.mjs`
   - `colors.json`

2. **Application Config**:
   - `config/app.config.ts`

3. **Sandbox System**:
   - `lib/sandbox/` (entire directory)

4. **Type Definitions**:
   - `types/` (entire directory)

5. **Styles**:
   - `styles/` (entire directory)

### Step 10: Implement Core Features

**Create API Routes**:
1. Sandbox management (`/api/create-ai-sandbox-v2/route.ts`)
2. Code generation (`/api/generate-ai-code-stream/route.ts`)
3. Code application (`/api/apply-ai-code-stream/route.ts`)
4. Web scraping (`/api/scrape-website/route.ts`)

**Create Pages**:
1. Landing page (`app/page.tsx`)
2. Generation interface (`app/generation/page.tsx`)

**Create Components**:
1. HeroInput (`components/HeroInput.tsx`)
2. SandboxPreview (`components/SandboxPreview.tsx`)
3. CodeApplicationProgress (`components/CodeApplicationProgress.tsx`)

### Step 11: Implement Sandbox Factory

```typescript
// lib/sandbox/factory.ts
import { E2BProvider } from './providers/e2b-provider';
import { VercelProvider } from './providers/vercel-provider';

export class SandboxFactory {
  static createSandbox(provider: 'vercel' | 'e2b') {
    if (provider === 'e2b') {
      return new E2BProvider();
    }
    return new VercelProvider();
  }
}
```

### Step 12: Test & Iterate

```bash
npm run dev
```

Test each feature:
- [ ] URL scraping works
- [ ] Sandbox creation succeeds
- [ ] Code generation streams properly
- [ ] Code application works
- [ ] Live preview shows generated app
- [ ] Error handling works

---

## Development Workflow

### Daily Development

#### Start Development Server

```bash
# With Turbopack (faster)
npm run dev

# Without Turbopack
npm run dev -- --no-turbopack

# On different port
npm run dev -- -p 3001
```

#### Code Quality Checks

```bash
# Run linter
npm run lint

# Fix auto-fixable issues
npm run lint -- --fix

# Type check
npx tsc --noEmit

# Format code (if Prettier installed)
npx prettier --write .
```

### Making Changes

#### 1. Create Feature Branch

```bash
git checkout -b feature/your-feature-name
```

#### 2. Make Changes

Edit files in your code editor.

#### 3. Test Changes

```bash
# Start dev server
npm run dev

# Test in browser
# Navigate to http://localhost:3000
```

#### 4. Commit Changes

```bash
git add .
git commit -m "feat: add your feature description"
```

**Commit Convention**:
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation
- `style:` - Formatting
- `refactor:` - Code refactoring
- `test:` - Tests
- `chore:` - Maintenance

#### 5. Push Changes

```bash
git push origin feature/your-feature-name
```

### Adding New API Routes

#### Create API Route File

```bash
mkdir app/api/your-endpoint
touch app/api/your-endpoint/route.ts
```

#### Implement Handler

```typescript
// app/api/your-endpoint/route.ts
import { NextResponse } from 'next/server';

export async function POST(request: Request) {
  try {
    const body = await request.json();

    // Your logic here
    const result = await doSomething(body);

    return NextResponse.json({
      success: true,
      data: result
    });

  } catch (error) {
    console.error('API Error:', error);
    return NextResponse.json(
      { error: error instanceof Error ? error.message : 'Unknown error' },
      { status: 500 }
    );
  }
}
```

#### Test API Route

```bash
curl -X POST http://localhost:3000/api/your-endpoint \
  -H "Content-Type: application/json" \
  -d '{"test": "data"}'
```

### Adding New Components

#### Create Component File

```bash
touch components/shared/YourComponent.tsx
```

#### Implement Component

```typescript
// components/shared/YourComponent.tsx
'use client'; // If needs interactivity

import { useState } from 'react';

interface YourComponentProps {
  title: string;
  onAction?: () => void;
}

export function YourComponent({ title, onAction }: YourComponentProps) {
  const [state, setState] = useState();

  return (
    <div>
      <h1>{title}</h1>
      <button onClick={onAction}>Action</button>
    </div>
  );
}
```

#### Use Component

```typescript
import { YourComponent } from '@/components/shared/YourComponent';

<YourComponent title="Hello" onAction={() => console.log('clicked')} />
```

---

## Deployment Guide

### Deploy to Vercel (Recommended)

#### Method 1: CLI Deployment

```bash
# Install Vercel CLI
npm i -g vercel

# Login
vercel login

# Link project (first time)
vercel link

# Deploy to preview
vercel

# Deploy to production
vercel --prod
```

#### Method 2: Git Integration

1. **Push to GitHub**:
   ```bash
   git push origin main
   ```

2. **Import to Vercel**:
   - Go to [vercel.com/new](https://vercel.com/new)
   - Import your GitHub repository
   - Configure project:
     - Framework Preset: Next.js
     - Root Directory: `./`
     - Build Command: `npm run build`
     - Output Directory: `.next`

3. **Add Environment Variables**:
   - Go to Project Settings → Environment Variables
   - Add all variables from `.env.local`:
     - `FIRECRAWL_API_KEY`
     - `ANTHROPIC_API_KEY`
     - `SANDBOX_PROVIDER`
     - `VERCEL_TOKEN` (if using Vercel sandboxes)
     - etc.

4. **Deploy**:
   - Click "Deploy"
   - Wait for build to complete
   - Visit your deployment URL

#### Automatic Deployments

Once set up, every push to `main` will trigger a production deployment, and every push to other branches will create a preview deployment.

### Deploy to Docker

#### Create Dockerfile

```dockerfile
# Dockerfile
FROM node:22-alpine AS base

# Install dependencies only when needed
FROM base AS deps
RUN apk add --no-cache libc6-compat
WORKDIR /app

COPY package*.json ./
RUN npm ci

# Rebuild the source code only when needed
FROM base AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .

ENV NEXT_TELEMETRY_DISABLED=1

RUN npm run build

# Production image, copy all the files and run next
FROM base AS runner
WORKDIR /app

ENV NODE_ENV=production
ENV NEXT_TELEMETRY_DISABLED=1

RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

COPY --from=builder /app/public ./public
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static

USER nextjs

EXPOSE 3000

ENV PORT=3000
ENV HOSTNAME="0.0.0.0"

CMD ["node", "server.js"]
```

#### Build and Run

```bash
# Build image
docker build -t open-lovable .

# Run container
docker run -p 3000:3000 \
  -e FIRECRAWL_API_KEY=$FIRECRAWL_API_KEY \
  -e ANTHROPIC_API_KEY=$ANTHROPIC_API_KEY \
  open-lovable
```

#### Use Docker Compose

```yaml
# docker-compose.yml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - FIRECRAWL_API_KEY=${FIRECRAWL_API_KEY}
      - ANTHROPIC_API_KEY=${ANTHROPIC_API_KEY}
      - SANDBOX_PROVIDER=e2b
      - E2B_API_KEY=${E2B_API_KEY}
    env_file:
      - .env.production
    restart: unless-stopped
```

```bash
docker-compose up -d
```

### Deploy to Traditional VPS

#### Prerequisites

- Ubuntu 22.04+ server
- Node.js 22+ installed
- Nginx installed
- Domain pointed to server

#### Steps

1. **SSH into server**:
   ```bash
   ssh user@your-server.com
   ```

2. **Install Node.js**:
   ```bash
   curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
   sudo apt-get install -y nodejs
   ```

3. **Clone repository**:
   ```bash
   cd /var/www
   git clone https://github.com/firecrawl/open-lovable.git
   cd open-lovable
   ```

4. **Install dependencies**:
   ```bash
   npm ci --only=production
   ```

5. **Create `.env.production`**:
   ```bash
   nano .env.production
   # Add all environment variables
   ```

6. **Build application**:
   ```bash
   npm run build
   ```

7. **Install PM2**:
   ```bash
   sudo npm install -g pm2
   ```

8. **Start application**:
   ```bash
   pm2 start npm --name "open-lovable" -- start
   pm2 save
   pm2 startup
   ```

9. **Configure Nginx**:
   ```bash
   sudo nano /etc/nginx/sites-available/open-lovable
   ```

   ```nginx
   server {
       listen 80;
       server_name your-domain.com;

       location / {
           proxy_pass http://localhost:3000;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_set_header Host $host;
           proxy_cache_bypass $http_upgrade;
       }
   }
   ```

   ```bash
   sudo ln -s /etc/nginx/sites-available/open-lovable /etc/nginx/sites-enabled/
   sudo nginx -t
   sudo systemctl reload nginx
   ```

10. **Setup SSL** (optional but recommended):
    ```bash
    sudo apt install certbot python3-certbot-nginx
    sudo certbot --nginx -d your-domain.com
    ```

---

## Troubleshooting

### Common Issues

#### 1. Port 3000 Already in Use

**Error**:
```
Error: listen EADDRINUSE: address already in use :::3000
```

**Solution**:
```bash
# Find process using port 3000
lsof -ti:3000

# Kill the process
kill -9 $(lsof -ti:3000)

# Or use different port
npm run dev -- -p 3001
```

#### 2. Missing API Keys

**Error**:
```
Error: Missing FIRECRAWL_API_KEY
```

**Solution**:
1. Check `.env.local` file exists
2. Verify API keys are correct
3. Restart dev server after adding keys

#### 3. Sandbox Creation Fails

**Error**:
```
Failed to create sandbox: Unauthorized
```

**Solution for Vercel Sandbox**:
```bash
# Re-authenticate
vercel login

# Pull environment variables
vercel env pull .env.local

# Restart dev server
npm run dev
```

**Solution for E2B Sandbox**:
1. Verify `E2B_API_KEY` in `.env.local`
2. Check API key is valid at [e2b.dev](https://e2b.dev)
3. Ensure you have credits remaining

#### 4. TypeScript Errors

**Error**:
```
Type error: Cannot find module '@/...'
```

**Solution**:
```bash
# Restart TypeScript server in VSCode
# Press: Cmd+Shift+P (Mac) or Ctrl+Shift+P (Windows)
# Type: "TypeScript: Restart TS Server"

# Or clear Next.js cache
rm -rf .next
npm run dev
```

#### 5. Tailwind Styles Not Working

**Error**: Styles not applying

**Solution**:
```bash
# Rebuild Tailwind
npm run dev

# Check tailwind.config.ts has correct content paths
# Should include: "./app/**/*.{js,ts,jsx,tsx}"
```

#### 6. Node Modules Issues

**Error**: Various dependency errors

**Solution**:
```bash
# Delete node_modules and lock file
rm -rf node_modules package-lock.json

# Reinstall
npm install

# Or clear npm cache
npm cache clean --force
npm install
```

#### 7. Build Failures

**Error**: Build fails in production

**Solution**:
```bash
# Check for TypeScript errors
npx tsc --noEmit

# Check for ESLint errors
npm run lint

# Try building locally
npm run build

# Check logs
npm run build 2>&1 | tee build.log
```

### Debug Mode

#### Enable Verbose Logging

```bash
# Set environment variable
export DEBUG=*

# Or add to .env.local
DEBUG=*

# Run dev server
npm run dev
```

#### Check API Responses

```bash
# Test API endpoint
curl -X POST http://localhost:3000/api/sandbox-status \
  -H "Content-Type: application/json" \
  -d '{}'
```

#### Browser DevTools

1. Open DevTools (F12)
2. Check Console for errors
3. Check Network tab for failed requests
4. Check Application tab for session storage

---

## Advanced Topics

### Custom AI Model Integration

#### Add New Provider

1. **Install SDK**:
   ```bash
   npm install @ai-sdk/custom-provider
   ```

2. **Create Provider Instance**:
   ```typescript
   // lib/ai-providers.ts
   import { createCustom } from '@ai-sdk/custom-provider';

   export const customProvider = createCustom({
     apiKey: process.env.CUSTOM_API_KEY
   });
   ```

3. **Add to Config**:
   ```typescript
   // config/app.config.ts
   ai: {
     availableModels: [
       // ... existing models
       'custom/model-name'
     ]
   }
   ```

4. **Use in API**:
   ```typescript
   import { customProvider } from '@/lib/ai-providers';

   const result = await streamText({
     model: customProvider('model-name'),
     prompt: 'Generate code'
   });
   ```

### Custom Sandbox Provider

#### Create New Provider

1. **Create Provider Class**:
   ```typescript
   // lib/sandbox/providers/custom-provider.ts
   import { SandboxProvider } from '../types';

   export class CustomProvider implements SandboxProvider {
     async createSandbox() {
       // Implementation
     }

     async runCommand(command: string) {
       // Implementation
     }

     // ... implement all methods
   }
   ```

2. **Add to Factory**:
   ```typescript
   // lib/sandbox/factory.ts
   import { CustomProvider } from './providers/custom-provider';

   export class SandboxFactory {
     static createSandbox(provider: string) {
       if (provider === 'custom') {
         return new CustomProvider();
       }
       // ... existing providers
     }
   }
   ```

3. **Update Config**:
   ```typescript
   // .env.local
   SANDBOX_PROVIDER=custom
   CUSTOM_SANDBOX_API_KEY=your-key
   ```

### Performance Optimization

#### Enable Caching

```typescript
// next.config.ts
const nextConfig: NextConfig = {
  experimental: {
    staleTimes: {
      dynamic: 30,
      static: 180
    }
  }
};
```

#### Image Optimization

```typescript
// next.config.ts
const nextConfig: NextConfig = {
  images: {
    formats: ['image/avif', 'image/webp'],
    deviceSizes: [640, 750, 828, 1080, 1200, 1920, 2048, 3840],
    imageSizes: [16, 32, 48, 64, 96, 128, 256, 384]
  }
};
```

#### Bundle Analysis

```bash
# Install analyzer
npm install @next/bundle-analyzer

# Configure
# next.config.ts
const withBundleAnalyzer = require('@next/bundle-analyzer')({
  enabled: process.env.ANALYZE === 'true'
});

module.exports = withBundleAnalyzer(nextConfig);

# Analyze
ANALYZE=true npm run build
```

### Monitoring & Logging

#### Add Sentry

```bash
npm install @sentry/nextjs
```

```typescript
// sentry.client.config.ts
import * as Sentry from '@sentry/nextjs';

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  tracesSampleRate: 1.0,
});
```

#### Add Vercel Analytics

```bash
npm install @vercel/analytics
```

```typescript
// app/layout.tsx
import { Analytics } from '@vercel/analytics/react';

export default function Layout({ children }) {
  return (
    <html>
      <body>
        {children}
        <Analytics />
      </body>
    </html>
  );
}
```

### Testing

#### Setup Jest

```bash
npm install -D jest @testing-library/react @testing-library/jest-dom
```

```javascript
// jest.config.js
module.exports = {
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: ['<rootDir>/jest.setup.js'],
  moduleNameMapper: {
    '^@/(.*)$': '<rootDir>/$1'
  }
};
```

#### Write Tests

```typescript
// components/__tests__/HeroInput.test.tsx
import { render, screen } from '@testing-library/react';
import { HeroInput } from '../HeroInput';

describe('HeroInput', () => {
  it('renders input field', () => {
    render(<HeroInput />);
    expect(screen.getByPlaceholderText(/enter url/i)).toBeInTheDocument();
  });
});
```

---

## Summary

### Quick Reference

**Start Development**:
```bash
npm run dev
```

**Build for Production**:
```bash
npm run build
npm start
```

**Deploy to Vercel**:
```bash
vercel --prod
```

**Common Commands**:
```bash
npm run lint        # Lint code
npm run type-check  # Check types
npm run clean       # Clean build
```

### Essential Files

- **Configuration**: `.env.local`, `config/app.config.ts`
- **Styling**: `tailwind.config.ts`, `styles/main.css`
- **Types**: `types/*.ts`
- **Core Logic**: `lib/sandbox/`, `lib/*.ts`
- **API**: `app/api/*/route.ts`
- **Pages**: `app/page.tsx`, `app/generation/page.tsx`

### Getting Help

- **Documentation**: Check `/docs` folder
- **Issues**: [GitHub Issues](https://github.com/firecrawl/open-lovable/issues)
- **Community**: Discussions tab on GitHub

### Next Steps

1. ✅ Get basic setup working
2. ✅ Test all features
3. 🔧 Customize styling and branding
4. 🔧 Add your own features
5. 🚀 Deploy to production
6. 📊 Add monitoring and analytics
7. 🧪 Add tests
8. 📈 Scale as needed

---

**Document Version**: 1.0
**Last Updated**: 2025-11-14
**Difficulty**: Intermediate
**Time to Complete**: 30-60 minutes
**Maintained By**: Development Team
