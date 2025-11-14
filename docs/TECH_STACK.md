# Tech Stack Documentation - Open Lovable

## Table of Contents
1. [Overview](#overview)
2. [Core Technologies](#core-technologies)
3. [Frontend Stack](#frontend-stack)
4. [Backend Stack](#backend-stack)
5. [AI & Machine Learning](#ai--machine-learning)
6. [Sandbox Providers](#sandbox-providers)
7. [Development Tools](#development-tools)
8. [Dependencies Deep Dive](#dependencies-deep-dive)
9. [Version Matrix](#version-matrix)
10. [Technology Decisions](#technology-decisions)

---

## Overview

### Technology Philosophy

Open Lovable is built with a **modern, scalable, and performant** technology stack focused on:

- **Developer Experience**: Fast development with hot reload, TypeScript, and modern tooling
- **User Experience**: Streaming AI responses, real-time updates, and smooth interactions
- **Flexibility**: Multi-provider support for AI and sandboxes
- **Type Safety**: Full TypeScript coverage for reliability
- **Performance**: Optimized builds, code splitting, and efficient rendering

### Stack Summary

```
┌─────────────────────────────────────────┐
│           PRESENTATION LAYER            │
│  Next.js 15 + React 19 + TypeScript     │
│  Tailwind CSS + Framer Motion           │
└────────────────┬────────────────────────┘
                 │
┌────────────────┴────────────────────────┐
│          APPLICATION LAYER              │
│  API Routes + Server Components         │
│  Jotai State + Custom Hooks             │
└────────────────┬────────────────────────┘
                 │
┌────────────────┴────────────────────────┐
│          INTEGRATION LAYER              │
│  AI Providers + Sandbox Providers       │
│  Web Scraping + File Operations         │
└────────────────┬────────────────────────┘
                 │
┌────────────────┴────────────────────────┐
│         INFRASTRUCTURE LAYER            │
│  Vercel Hosting + E2B/Vercel Sandboxes  │
│  Firecrawl API + AI APIs                │
└─────────────────────────────────────────┘
```

---

## Core Technologies

### Next.js 15.4.3

**Purpose**: React framework with full-stack capabilities

**Key Features Used**:
- ✅ **App Router** - File-based routing with layouts
- ✅ **React Server Components** - Server-side rendering by default
- ✅ **API Routes** - Backend endpoints in `/app/api`
- ✅ **Streaming** - Progressive rendering and SSE support
- ✅ **Turbopack** - Fast development builds
- ✅ **Image Optimization** - Automatic image optimization
- ✅ **Font Optimization** - Variable font loading
- ✅ **Metadata API** - SEO and social sharing

**Why Next.js?**
- Full-stack framework (frontend + backend)
- Excellent developer experience
- Production-ready optimizations
- Large ecosystem and community
- Vercel deployment integration

**Configuration**:
```typescript
// next.config.ts
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  experimental: {
    turbopack: true // Fast dev builds
  }
};

export default nextConfig;
```

### React 19.1.0

**Purpose**: UI library for building interactive interfaces

**Key Features Used**:
- ✅ **Server Components** - Default in Next.js App Router
- ✅ **Client Components** - Interactive UI with "use client"
- ✅ **Hooks** - useState, useEffect, useRef, custom hooks
- ✅ **Suspense** - Loading states and streaming
- ✅ **Error Boundaries** - Error handling

**Component Patterns**:
```typescript
// Server Component (default)
async function ServerComponent() {
  const data = await fetchData();
  return <div>{data}</div>;
}

// Client Component (interactive)
'use client';
function ClientComponent() {
  const [state, setState] = useState();
  return <button onClick={...}>Click</button>;
}
```

### TypeScript 5.x

**Purpose**: Type-safe JavaScript with static type checking

**Configuration**:
```json
{
  "compilerOptions": {
    "target": "ES2017",
    "lib": ["dom", "dom.iterable", "esnext"],
    "strict": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "jsx": "preserve",
    "paths": {
      "@/*": ["./*"]
    }
  }
}
```

**Benefits**:
- Catch errors at compile time
- Better IDE support (autocomplete, refactoring)
- Self-documenting code
- Safer refactoring

**Usage Coverage**: ~95% of codebase

---

## Frontend Stack

### Styling

#### Tailwind CSS 3.4.17

**Purpose**: Utility-first CSS framework

**Custom Configuration**:
```typescript
// tailwind.config.ts
export default {
  theme: {
    extend: {
      colors: {
        // 50+ custom colors from colors.json
      },
      fontSize: {
        'title-h1': ['60px', '64px'],
        'title-h2': ['48px', '52px'],
        // ... typography scale
      },
      spacing: {
        // Custom spacing 0-1000px
      }
    }
  },
  plugins: [
    require('@tailwindcss/typography'),
    require('tailwindcss-animate'),
    require('tailwind-gradient-mask-image')
  ]
}
```

**Custom Utilities**:
```css
.center          /* position: absolute + center transform */
.center-x        /* horizontal centering */
.center-y        /* vertical centering */
.flex-center     /* display: flex + center alignment */
.overlay         /* absolute fullscreen overlay */
```

**Plugins**:
- `@tailwindcss/typography` - Beautiful typography styles
- `tailwindcss-animate` - Animation utilities
- `tailwind-gradient-mask-image` - Gradient masks
- `class-variance-authority` - Component variants
- `tailwind-merge` - Merge Tailwind classes safely

#### PostCSS

**Purpose**: CSS processing and transformations

**Plugins**:
```javascript
// postcss.config.mjs
export default {
  plugins: {
    'postcss-import': {},      // Import resolution
    'postcss-nesting': {},     // CSS nesting support
    'tailwindcss': {},         // Tailwind processing
    'autoprefixer': {},        // Vendor prefixes
  }
}
```

#### Framer Motion 12.23.12

**Purpose**: Animation library for React

**Features Used**:
- Component animations
- Page transitions
- Gesture animations
- Layout animations

**Example**:
```typescript
import { motion } from 'framer-motion';

<motion.div
  initial={{ opacity: 0, y: 20 }}
  animate={{ opacity: 1, y: 0 }}
  transition={{ duration: 0.5 }}
>
  Content
</motion.div>
```

### UI Components

#### Radix UI (20+ packages)

**Purpose**: Headless, accessible UI primitives

**Components Used**:
```typescript
@radix-ui/react-accordion
@radix-ui/react-alert-dialog
@radix-ui/react-avatar
@radix-ui/react-checkbox
@radix-ui/react-dialog
@radix-ui/react-dropdown-menu
@radix-ui/react-hover-card
@radix-ui/react-label
@radix-ui/react-popover
@radix-ui/react-progress
@radix-ui/react-radio-group
@radix-ui/react-scroll-area
@radix-ui/react-select
@radix-ui/react-separator
@radix-ui/react-slider
@radix-ui/react-switch
@radix-ui/react-tabs
@radix-ui/react-toast
@radix-ui/react-tooltip
```

**Why Radix UI?**
- Fully accessible (ARIA compliant)
- Unstyled (style with Tailwind)
- Composable primitives
- Keyboard navigation
- Focus management

#### Shadcn UI

**Purpose**: Pre-built component library based on Radix UI

**Components** (30+):
- Buttons, Cards, Badges
- Forms (Input, Select, Checkbox, etc.)
- Overlays (Dialog, Sheet, Popover)
- Feedback (Toast, Alert, Progress)
- Navigation (Tabs, Dropdown)

**Philosophy**: Copy-paste components, not npm package

### Icons

**Three Icon Libraries**:

1. **Lucide React (0.532.0)** - Main icon library
   ```typescript
   import { Search, Settings, X } from 'lucide-react';
   ```

2. **Tabler Icons (3.34.1)** - Additional icons
   ```typescript
   import { IconBrandGithub } from '@tabler/icons-react';
   ```

3. **React Icons (5.5.0)** - Icon collection
   ```typescript
   import { FaReact } from 'react-icons/fa';
   ```

### Graphics & Visualization

#### PixiJS 8.13.1

**Purpose**: WebGL rendering engine for high-performance graphics

**Usage**: Hero section background effects

**Features**:
- Hardware-accelerated rendering
- Particle systems
- Interactive graphics
- Smooth animations

**Example**:
```typescript
import * as PIXI from 'pixi.js';

const app = new PIXI.Application({
  background: '#000000',
  resizeTo: window
});

// Create graphics, particles, etc.
```

### Code Display

#### React Syntax Highlighter 15.6.1

**Purpose**: Code syntax highlighting

**Languages Supported**: All major languages

**Themes**: Multiple syntax themes

**Example**:
```typescript
import { Prism as SyntaxHighlighter } from 'react-syntax-highlighter';
import { vscDarkPlus } from 'react-syntax-highlighter/dist/esm/styles/prism';

<SyntaxHighlighter language="typescript" style={vscDarkPlus}>
  {code}
</SyntaxHighlighter>
```

---

## Backend Stack

### API Framework

**Next.js API Routes** - Built-in backend

**Pattern**:
```typescript
// app/api/[endpoint]/route.ts
export async function POST(request: Request) {
  try {
    const body = await request.json();
    // Business logic
    return NextResponse.json({ success: true, data });
  } catch (error) {
    return NextResponse.json(
      { error: error.message },
      { status: 500 }
    );
  }
}
```

**Features**:
- TypeScript support
- Streaming responses (SSE)
- Middleware support
- Edge runtime option

### State Management

#### Jotai 2.14.0

**Purpose**: Atomic state management for React

**Why Jotai?**
- Minimal boilerplate
- TypeScript-first
- Atomic design (vs global store)
- React Suspense support
- Small bundle size (2.4kb)

**Usage**:
```typescript
// Define atom
import { atom } from 'jotai';

export const sheetsAtom = atom<SheetState>({
  isOpen: false,
  type: null
});

// Use in component
import { useAtom } from 'jotai';

function Component() {
  const [sheets, setSheets] = useAtom(sheetsAtom);
  // Use state
}
```

### Utilities

#### Core Utilities

1. **Lodash-ES (4.17.21)** - Utility functions
   ```typescript
   import { debounce, throttle, groupBy } from 'lodash-es';
   ```

2. **Nanoid (5.1.5)** - ID generation
   ```typescript
   import { nanoid } from 'nanoid';
   const id = nanoid(); // "V1StGXR8_Z5jdHi6B-myT"
   ```

3. **Zod (3.25.76)** - Schema validation
   ```typescript
   import { z } from 'zod';
   const schema = z.object({
     url: z.string().url(),
     style: z.enum(['modern', 'minimal'])
   });
   ```

4. **React Hook Form (7.62.0)** - Form management
   ```typescript
   import { useForm } from 'react-hook-form';
   ```

5. **Class Utilities**
   - `clsx` - Conditional classes
   - `classnames` - Class name helper
   - `tailwind-merge` - Merge Tailwind classes

6. **Copy to Clipboard (3.3.3)** - Clipboard API
   ```typescript
   import copy from 'copy-to-clipboard';
   copy('text to copy');
   ```

7. **Next Themes (0.4.6)** - Theme management
   ```typescript
   import { useTheme } from 'next-themes';
   const { theme, setTheme } = useTheme();
   ```

8. **Sonner (2.0.7)** - Toast notifications
   ```typescript
   import { toast } from 'sonner';
   toast.success('Success!');
   ```

---

## AI & Machine Learning

### AI SDK - Vercel AI SDK 5.0.0

**Purpose**: Unified interface for AI providers

**Features**:
- Streaming responses
- Multiple providers
- Type-safe API
- React hooks
- Edge runtime support

**Core Functions**:
```typescript
import { streamText } from 'ai';

const result = streamText({
  model: anthropic('claude-sonnet-4'),
  prompt: 'Generate React code',
  temperature: 0.7,
  maxTokens: 8000
});

for await (const chunk of result.textStream) {
  console.log(chunk);
}
```

### AI Provider SDKs

#### 1. Anthropic (Claude)

**Packages**:
- `@ai-sdk/anthropic@2.0.1` - AI SDK provider
- `@anthropic-ai/sdk@0.57.0` - Direct SDK

**Models**:
- `claude-sonnet-4` - Balanced performance
- `claude-opus-4` - Maximum capability
- `claude-haiku-4` - Fast responses

**Usage**:
```typescript
import { anthropic } from '@ai-sdk/anthropic';

const result = await streamText({
  model: anthropic('claude-sonnet-4'),
  prompt: 'Generate code'
});
```

#### 2. OpenAI (GPT)

**Package**: `@ai-sdk/openai@2.0.4`

**Models**:
- `gpt-5-preview` - Latest model
- `gpt-4-turbo` - Fast GPT-4
- `gpt-4` - Stable GPT-4

**Usage**:
```typescript
import { openai } from '@ai-sdk/openai';

const result = await streamText({
  model: openai('gpt-5-preview'),
  prompt: 'Generate code'
});
```

#### 3. Google (Gemini)

**Package**: `@ai-sdk/google@2.0.4`

**Models**:
- `gemini-2.0-flash-exp` - Fast experimental
- `gemini-pro` - Balanced model

**Usage**:
```typescript
import { google } from '@ai-sdk/google';

const result = await streamText({
  model: google('gemini-2.0-flash-exp'),
  prompt: 'Generate code'
});
```

#### 4. Groq (Kimi)

**Packages**:
- `@ai-sdk/groq@2.0.0` - AI SDK provider
- `groq-sdk@0.29.0` - Direct SDK

**Models**:
- `moonshotai/kimi-k2-instruct-0905` - Default model

**Usage**:
```typescript
import { createGroq } from '@ai-sdk/groq';

const groq = createGroq({ apiKey: process.env.GROQ_API_KEY });

const result = await streamText({
  model: groq('moonshotai/kimi-k2-instruct-0905'),
  prompt: 'Generate code'
});
```

### AI Configuration

**Default Settings** (`config/app.config.ts`):
```typescript
ai: {
  defaultModel: 'moonshotai/kimi-k2-instruct-0905',
  availableModels: [
    'moonshotai/kimi-k2-instruct-0905',
    'openai/gpt-5-preview',
    'anthropic/claude-sonnet-4',
    'google/gemini-2.0-flash-exp'
  ],
  maxTokens: 8000,
  temperature: 0.7
}
```

---

## Sandbox Providers

### 1. Vercel Sandbox 0.0.17

**Purpose**: Isolated Node.js runtime for code execution

**Features**:
- Node.js 22 runtime
- File system access
- Package installation
- Command execution
- 15-minute timeout

**Configuration**:
```typescript
vercelSandbox: {
  timeoutMinutes: 15,
  devPort: 3000,
  runtime: 'node22'
}
```

**Usage**:
```typescript
import { Sandbox } from '@vercel/sandbox';

const sandbox = await Sandbox.create({
  timeout: 15 * 60 * 1000 // 15 minutes
});

await sandbox.writeFile('package.json', packageJson);
await sandbox.runCommand('npm install');
```

**Authentication**:
- `VERCEL_OIDC_TOKEN` (auto-generated in Vercel environment)
- OR `VERCEL_TOKEN` + `VERCEL_TEAM_ID` + `VERCEL_PROJECT_ID`

### 2. E2B Code Interpreter 2.0.0

**Purpose**: Full Linux environment for code execution

**Features**:
- Ubuntu-based container
- Full shell access
- Multiple languages
- 30-minute timeout
- More powerful than Vercel

**Configuration**:
```typescript
e2b: {
  timeoutMinutes: 30,
  vitePort: 5173
}
```

**Usage**:
```typescript
import { CodeInterpreter } from '@e2b/code-interpreter';

const sandbox = await CodeInterpreter.create({
  apiKey: process.env.E2B_API_KEY,
  timeout: 30 * 60 * 1000
});

await sandbox.filesystem.write('/home/user/file.ts', content);
await sandbox.process.start('npm install');
```

**Authentication**: `E2B_API_KEY`

### Provider Comparison

| Feature | Vercel Sandbox | E2B Sandbox |
|---------|---------------|-------------|
| Runtime | Node.js 22 | Ubuntu Linux |
| Timeout | 15 minutes | 30 minutes |
| Port | 3000 | 5173 |
| Shell Access | Limited | Full |
| File System | Restricted | Full |
| Use Case | Simple projects | Complex projects |
| Cost | Free (Vercel) | Pay-per-use |

### Sandbox Architecture

**Provider Pattern**:
```typescript
// lib/sandbox/types.ts
interface SandboxProvider {
  createSandbox(): Promise<SandboxData>;
  runCommand(command: string): Promise<CommandResult>;
  writeFile(path: string, content: string): Promise<void>;
  installPackages(packages: string[]): Promise<void>;
  terminate(): Promise<void>;
}

// lib/sandbox/factory.ts
export class SandboxFactory {
  static createSandbox(provider: 'vercel' | 'e2b'): SandboxProvider {
    if (provider === 'e2b') {
      return new E2BProvider();
    }
    return new VercelProvider();
  }
}
```

---

## Development Tools

### Build Tools

#### Turbopack (Next.js)

**Purpose**: Fast development bundler

**Features**:
- Incremental compilation
- Fast HMR (Hot Module Replacement)
- Built into Next.js 15
- Replaces Webpack in dev mode

**Usage**: Automatic with `next dev --turbopack`

#### Vite (In Sandboxes)

**Purpose**: Frontend build tool for generated projects

**Features**:
- Lightning-fast HMR
- ES modules in dev
- Optimized production builds
- Plugin ecosystem

**Used In**: Generated React applications in sandboxes

### Code Quality

#### ESLint 9.x

**Purpose**: JavaScript/TypeScript linter

**Configuration**:
```javascript
// eslint.config.mjs
export default {
  extends: ['next/core-web-vitals'],
  rules: {
    '@typescript-eslint/no-explicit-any': 'off',
    '@typescript-eslint/no-unused-vars': 'off',
    'react/no-unescaped-entities': 'off'
  }
}
```

**Features**:
- TypeScript support
- React hooks rules
- Next.js optimizations
- Auto-fix capability

### TypeScript

**Purpose**: Static type checking

**Scripts**:
```json
{
  "scripts": {
    "type-check": "tsc --noEmit"
  }
}
```

### Package Management

**Supported**:
- **npm** - Default
- **pnpm** - Recommended (faster, space-efficient)
- **yarn** - Alternative

**Lock Files**:
- `package-lock.json` (npm)
- `pnpm-lock.yaml` (pnpm)
- `yarn.lock` (yarn)

---

## Dependencies Deep Dive

### Production Dependencies (80+ packages)

#### Framework & Core
```json
{
  "next": "15.4.3",
  "react": "19.1.0",
  "react-dom": "19.1.0",
  "typescript": "^5"
}
```

#### AI & ML
```json
{
  "ai": "5.0.0",
  "@ai-sdk/anthropic": "2.0.1",
  "@ai-sdk/openai": "2.0.4",
  "@ai-sdk/google": "2.0.4",
  "@ai-sdk/groq": "2.0.0",
  "@anthropic-ai/sdk": "0.57.0",
  "groq-sdk": "0.29.0"
}
```

#### Sandbox Providers
```json
{
  "@vercel/sandbox": "0.0.17",
  "@e2b/code-interpreter": "2.0.0"
}
```

#### Styling
```json
{
  "tailwindcss": "3.4.17",
  "@tailwindcss/typography": "0.5.16",
  "framer-motion": "12.23.12",
  "class-variance-authority": "0.7.1",
  "tailwind-merge": "3.3.1",
  "tailwindcss-animate": "1.0.7",
  "postcss": "8.5.1",
  "autoprefixer": "10.4.20"
}
```

#### UI Components (Radix UI)
```json
{
  "@radix-ui/react-accordion": "1.2.2",
  "@radix-ui/react-alert-dialog": "1.1.4",
  "@radix-ui/react-avatar": "1.1.2",
  "@radix-ui/react-checkbox": "1.1.3",
  "@radix-ui/react-dialog": "1.1.4",
  "@radix-ui/react-dropdown-menu": "2.1.4",
  "@radix-ui/react-hover-card": "1.1.4",
  "@radix-ui/react-label": "2.1.1",
  "@radix-ui/react-popover": "1.1.4",
  "@radix-ui/react-progress": "1.1.1",
  "@radix-ui/react-radio-group": "1.2.2",
  "@radix-ui/react-scroll-area": "1.2.2",
  "@radix-ui/react-select": "2.1.4",
  "@radix-ui/react-separator": "1.1.1",
  "@radix-ui/react-slider": "1.2.1",
  "@radix-ui/react-slot": "1.1.1",
  "@radix-ui/react-switch": "1.1.2",
  "@radix-ui/react-tabs": "1.1.2",
  "@radix-ui/react-toast": "1.2.4",
  "@radix-ui/react-tooltip": "1.1.6"
}
```

#### Icons
```json
{
  "lucide-react": "0.532.0",
  "@tabler/icons-react": "3.34.1",
  "react-icons": "5.5.0"
}
```

#### State & Forms
```json
{
  "jotai": "2.14.0",
  "react-hook-form": "7.62.0",
  "zod": "3.25.76"
}
```

#### Utilities
```json
{
  "lodash-es": "4.17.21",
  "nanoid": "5.1.5",
  "clsx": "2.1.1",
  "classnames": "2.5.1",
  "copy-to-clipboard": "3.3.3",
  "next-themes": "0.4.6",
  "sonner": "2.0.7",
  "usehooks-ts": "3.1.1",
  "dotenv": "17.2.1",
  "cors": "2.8.5"
}
```

#### Graphics & Visualization
```json
{
  "pixi.js": "8.13.1"
}
```

#### Code Display
```json
{
  "react-syntax-highlighter": "15.6.1"
}
```

#### Web Scraping
```json
{
  "@mendable/firecrawl-js": "4.3.3"
}
```

### Development Dependencies (9 packages)

```json
{
  "@types/node": "^22",
  "@types/react": "^19",
  "@types/react-dom": "^19",
  "@types/lodash-es": "^4",
  "@types/react-syntax-highlighter": "^15",
  "eslint": "^9",
  "eslint-config-next": "15.4.3",
  "postcss": "^8.5.1",
  "tailwindcss": "3.4.17"
}
```

---

## Version Matrix

### Major Version Compatibility

| Package | Version | Compatible With |
|---------|---------|-----------------|
| Next.js | 15.4.3 | React 19+ |
| React | 19.1.0 | Next.js 15+ |
| TypeScript | 5.x | Next.js 15+ |
| Tailwind CSS | 3.4.17 | PostCSS 8+ |
| Framer Motion | 12.23.12 | React 19+ |
| Jotai | 2.14.0 | React 18+ |
| AI SDK | 5.0.0 | Next.js 15+ |

### Node.js Requirements

**Minimum**: Node.js 18.17+
**Recommended**: Node.js 22+ (LTS)
**Used in Production**: Node.js 22

### Browser Support

**Modern Browsers**:
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

**ES Features Used**:
- ES2017 target
- Async/await
- Optional chaining
- Nullish coalescing
- Dynamic imports

---

## Technology Decisions

### Why Next.js over Create React App?

✅ **Next.js Advantages**:
- Built-in API routes (full-stack)
- Server-side rendering
- Automatic code splitting
- Image optimization
- Better performance
- Production-ready

❌ **CRA Limitations**:
- No backend support
- Manual optimization
- Slower builds
- Deprecated by React team

### Why Tailwind CSS over CSS-in-JS?

✅ **Tailwind Advantages**:
- Zero runtime overhead
- Faster development
- Smaller bundle size
- Consistent design system
- Better performance

❌ **CSS-in-JS Limitations**:
- Runtime cost
- Larger bundle size
- Server-side rendering complexity

### Why Jotai over Redux?

✅ **Jotai Advantages**:
- Minimal boilerplate
- Atomic design
- Better TypeScript support
- Smaller bundle size (2.4kb vs 12kb)
- Simpler API

❌ **Redux Limitations**:
- Boilerplate heavy
- Complex setup
- Larger bundle size

### Why Multiple AI Providers?

✅ **Benefits**:
- Flexibility (user choice)
- Redundancy (fallback)
- Cost optimization
- Different model strengths
- Avoid vendor lock-in

### Why Dual Sandbox Support?

✅ **Benefits**:
- Vercel: Free, simple, fast
- E2B: Powerful, flexible, full Linux
- Flexibility for different needs
- Redundancy if one provider down

---

## Future Technology Considerations

### Potential Additions

1. **Database** (if needed for persistence)
   - Options: PostgreSQL, MongoDB, Supabase
   - Current: Stateless (no database)

2. **Caching Layer**
   - Options: Redis, Upstash
   - Use case: Session state, rate limiting

3. **Authentication**
   - Options: NextAuth.js, Clerk, Supabase Auth
   - Current: No auth (demo mode)

4. **Testing**
   - Options: Vitest, Jest, Playwright
   - Current: No tests

5. **Monitoring**
   - Options: Sentry, Datadog, Vercel Analytics
   - Current: Basic logging

6. **Search**
   - Options: Algolia, Meilisearch
   - Current: Basic search via Firecrawl

### Technology Roadmap

**Phase 1** (Current):
- ✅ Core functionality
- ✅ Multi-provider AI
- ✅ Dual sandbox support

**Phase 2** (Next):
- 🔧 Add authentication
- 🔧 Add testing suite
- 🔧 Add monitoring

**Phase 3** (Future):
- 📅 Add database (if needed)
- 📅 Add caching layer
- 📅 Add advanced search

---

## Dependency Management

### Update Strategy

**Regular Updates**:
```bash
# Check outdated packages
npm outdated

# Update non-major versions
npm update

# Update major versions (carefully)
npm install package@latest
```

**Security Updates**:
```bash
# Check vulnerabilities
npm audit

# Fix vulnerabilities
npm audit fix
```

### Lock File

**Importance**: Always commit lock files
- `package-lock.json` (npm)
- `pnpm-lock.yaml` (pnpm)

**Why**: Ensures consistent installs across environments

### Peer Dependencies

**Auto-installed** by npm 7+

**Check**:
```bash
npm ls
```

---

## Summary

### Tech Stack Strengths

✅ **Modern**: Latest versions of all major packages
✅ **Type-Safe**: Full TypeScript coverage
✅ **Performant**: Optimized builds and runtime
✅ **Flexible**: Multiple providers for key services
✅ **Scalable**: Can handle growth
✅ **Developer-Friendly**: Great DX with hot reload, TypeScript, ESLint

### Stack Statistics

- **Total Dependencies**: 89 packages
- **Production**: 80 packages
- **Development**: 9 packages
- **Security Vulnerabilities**: 0 (recommended)
- **Bundle Size**: Optimized with code splitting
- **TypeScript Coverage**: ~95%

### Key Takeaways

1. **Full-Stack Framework**: Next.js provides both frontend and backend
2. **Multi-Provider AI**: Flexibility with 4 AI providers
3. **Dual Sandboxes**: Vercel for simple, E2B for complex
4. **Type Safety**: TypeScript throughout
5. **Modern Styling**: Tailwind CSS + custom design system
6. **Performance-First**: Streaming, code splitting, optimization

---

**Document Version**: 1.0
**Last Updated**: 2025-11-14
**Total Dependencies**: 89 packages
**Maintained By**: Development Team
