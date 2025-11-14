# File Structure Analysis - Open Lovable

## Table of Contents
1. [Project Overview](#project-overview)
2. [Complete Directory Tree](#complete-directory-tree)
3. [Directory Analysis](#directory-analysis)
4. [File Categories](#file-categories)
5. [Key Files Reference](#key-files-reference)
6. [Code Organization Patterns](#code-organization-patterns)
7. [Import Paths and Aliases](#import-paths-and-aliases)

---

## Project Overview

### Project Statistics

- **Total Directories**: ~119
- **Total Files**: ~300+
- **TypeScript Files**: ~269
- **Configuration Files**: 8
- **Documentation Files**: 5+
- **API Routes**: 28
- **React Components**: 100+

### Technology Distribution

```
Language/Type         | Files | Percentage
---------------------|-------|------------
TypeScript (.ts/.tsx)| 269   | 85%
JavaScript (.js)     | 15    | 5%
CSS (.css)           | 20    | 6%
JSON (.json)         | 8     | 2%
Markdown (.md)       | 6     | 2%
```

---

## Complete Directory Tree

```
/home/user/open-lovable/
├── .github/                          # GitHub configuration
│   └── workflows/                    # GitHub Actions (if any)
│
├── .next/                            # Next.js build output (generated)
│   ├── cache/                        # Build cache
│   ├── server/                       # Server bundles
│   ├── static/                       # Static assets
│   └── types/                        # Generated TypeScript types
│
├── app/                              # Next.js App Router (main application)
│   ├── api/                          # API Routes (28 endpoints)
│   │   ├── analyze-edit-intent/
│   │   │   └── route.ts              # Analyze AI edit requests
│   │   ├── apply-ai-code/
│   │   │   └── route.ts              # Apply generated code (legacy)
│   │   ├── apply-ai-code-stream/
│   │   │   └── route.ts              # Stream code application
│   │   ├── check-vite-errors/
│   │   │   └── route.ts              # Check Vite error status
│   │   ├── clear-vite-errors-cache/
│   │   │   └── route.ts              # Clear error cache
│   │   ├── conversation-state/
│   │   │   └── route.ts              # Manage conversation context
│   │   ├── create-ai-sandbox/
│   │   │   └── route.ts              # Create sandbox (legacy)
│   │   ├── create-ai-sandbox-v2/
│   │   │   └── route.ts              # Create sandbox (current)
│   │   ├── create-zip/
│   │   │   └── route.ts              # Export project as ZIP
│   │   ├── detect-and-install-packages/
│   │   │   └── route.ts              # Auto-detect npm packages
│   │   ├── generate-ai-code-stream/
│   │   │   └── route.ts              # Stream AI code generation
│   │   ├── get-sandbox-files/
│   │   │   └── route.ts              # List sandbox files
│   │   ├── install-packages/
│   │   │   └── route.ts              # Install packages (legacy)
│   │   ├── install-packages-v2/
│   │   │   └── route.ts              # Install packages (current)
│   │   ├── kill-sandbox/
│   │   │   └── route.ts              # Terminate sandbox
│   │   ├── monitor-vite-logs/
│   │   │   └── route.ts              # Monitor Vite server logs
│   │   ├── report-vite-error/
│   │   │   └── route.ts              # Report Vite errors
│   │   ├── restart-vite/
│   │   │   └── route.ts              # Restart Vite dev server
│   │   ├── run-command/
│   │   │   └── route.ts              # Run sandbox command (legacy)
│   │   ├── run-command-v2/
│   │   │   └── route.ts              # Run sandbox command (current)
│   │   ├── sandbox-logs/
│   │   │   └── route.ts              # Retrieve sandbox logs
│   │   ├── sandbox-status/
│   │   │   └── route.ts              # Check sandbox health
│   │   ├── scrape-screenshot/
│   │   │   └── route.ts              # Capture website screenshot
│   │   ├── scrape-url-enhanced/
│   │   │   └── route.ts              # Enhanced URL scraping
│   │   ├── scrape-website/
│   │   │   └── route.ts              # Scrape website content
│   │   └── search/
│   │       └── route.ts              # Web search functionality
│   │
│   ├── builder/                      # Builder page route (alternate UI)
│   │   └── page.tsx                  # Builder interface
│   │
│   ├── generation/                   # Main generation interface
│   │   └── page.tsx                  # Generation page component
│   │
│   ├── fonts/                        # Font files
│   │   ├── GeistVF.woff              # Geist Variable Font
│   │   └── GeistMonoVF.woff          # Geist Mono Variable Font
│   │
│   ├── favicon.ico                   # Site favicon
│   ├── globals.css                   # Global styles (imports main.css)
│   ├── landing.tsx                   # Landing page component
│   ├── layout.tsx                    # Root layout (fonts, metadata)
│   └── page.tsx                      # Home page route
│
├── atoms/                            # Jotai state management
│   └── sheets.ts                     # Sheet/modal state atoms
│
├── components/                       # React components
│   ├── app/                          # Application-specific components
│   │   ├── (home)/                   # Home page components (route group)
│   │   │   └── sections/
│   │   │       ├── CarouselSectionExtension.tsx  # Search carousel
│   │   │       ├── HeroSection.tsx               # Hero section
│   │   │       ├── HeroSectionInput.tsx          # Input section
│   │   │       └── SearchCarousel.tsx            # Search results
│   │   │
│   │   └── generation/               # Generation page components
│   │       ├── ChatInterface.tsx     # AI chat interface
│   │       ├── CodeViewer.tsx        # Code display
│   │       ├── FileTree.tsx          # File browser
│   │       ├── GenerationControls.tsx # Control buttons
│   │       ├── ProgressTracker.tsx   # Generation progress
│   │       └── SandboxControls.tsx   # Sandbox management
│   │
│   ├── shared/                       # Shared/reusable components
│   │   ├── Playground/               # Code playground
│   │   │   ├── index.tsx
│   │   │   ├── Editor.tsx
│   │   │   └── Preview.tsx
│   │   │
│   │   ├── button/                   # Custom button components
│   │   │   ├── GradientButton.tsx
│   │   │   ├── IconButton.tsx
│   │   │   └── PrimaryButton.tsx
│   │   │
│   │   ├── effects/                  # Visual effects
│   │   │   ├── AsciiExplosion.tsx    # ASCII art animation
│   │   │   ├── FlameEffect.tsx       # Flame effect
│   │   │   └── ParticleBackground.tsx
│   │   │
│   │   ├── header/                   # Header components
│   │   │   ├── Header.tsx
│   │   │   ├── HeaderBrandKit.tsx    # Logo/branding
│   │   │   ├── HeaderDropdown.tsx    # Navigation dropdown
│   │   │   └── HeaderWrapper.tsx     # Header container
│   │   │
│   │   ├── icons/                    # Custom icon components
│   │   │   ├── AnimatedIcon.tsx
│   │   │   └── CustomIcons.tsx
│   │   │
│   │   ├── layout/                   # Layout components
│   │   │   ├── Container.tsx
│   │   │   ├── Grid.tsx
│   │   │   ├── Sidebar.tsx
│   │   │   └── SplitPane.tsx
│   │   │
│   │   ├── loading/                  # Loading states
│   │   │   ├── LoadingSpinner.tsx
│   │   │   ├── SkeletonLoader.tsx
│   │   │   └── ProgressBar.tsx
│   │   │
│   │   ├── notifications/            # Toast/notification system
│   │   │   ├── Toast.tsx
│   │   │   ├── ToastContainer.tsx
│   │   │   └── useToast.ts
│   │   │
│   │   ├── pixi/                     # PixiJS graphics
│   │   │   ├── PixiCanvas.tsx
│   │   │   ├── PixiParticles.tsx
│   │   │   └── HomeHeroPixi.tsx
│   │   │
│   │   ├── preview/                  # Preview components
│   │   │   ├── LivePreview.tsx
│   │   │   └── PreviewFrame.tsx
│   │   │
│   │   ├── tabs/                     # Tab components
│   │   │   ├── TabBar.tsx
│   │   │   ├── TabContent.tsx
│   │   │   └── TabPanel.tsx
│   │   │
│   │   └── ui/                       # Basic UI primitives
│   │       ├── Badge.tsx
│   │       ├── Card.tsx
│   │       ├── Divider.tsx
│   │       └── Typography.tsx
│   │
│   ├── ui/                           # UI component library
│   │   ├── motion/                   # Framer Motion wrappers
│   │   │   ├── FadeIn.tsx
│   │   │   ├── SlideIn.tsx
│   │   │   └── AnimatedPresence.tsx
│   │   │
│   │   └── shadcn/                   # Shadcn UI components
│   │       ├── accordion.tsx
│   │       ├── alert-dialog.tsx
│   │       ├── avatar.tsx
│   │       ├── badge.tsx
│   │       ├── button.tsx
│   │       ├── card.tsx
│   │       ├── checkbox.tsx
│   │       ├── dialog.tsx
│   │       ├── dropdown-menu.tsx
│   │       ├── input.tsx
│   │       ├── label.tsx
│   │       ├── popover.tsx
│   │       ├── progress.tsx
│   │       ├── radio-group.tsx
│   │       ├── select.tsx
│   │       ├── separator.tsx
│   │       ├── sheet.tsx
│   │       ├── skeleton.tsx
│   │       ├── slider.tsx
│   │       ├── switch.tsx
│   │       ├── tabs.tsx
│   │       ├── textarea.tsx
│   │       ├── toast.tsx
│   │       ├── toaster.tsx
│   │       └── tooltip.tsx
│   │
│   ├── CodeApplicationProgress.tsx   # Code application progress UI
│   ├── HeroInput.tsx                 # Main search/input component
│   ├── SandboxPreview.tsx            # Sandbox iframe preview
│   └── HMRErrorDetector.tsx          # Hot reload error detection
│
├── config/                           # Configuration files
│   └── app.config.ts                 # Application configuration
│
├── docs/                             # Documentation
│   ├── PACKAGE_DETECTION_GUIDE.md    # Package detection documentation
│   ├── STREAMING_FIXES_SUMMARY.md    # Streaming fixes
│   ├── TOOL_CALL_FIX_SUMMARY.md      # Tool call fixes
│   ├── UI_FEEDBACK_DEMO.md           # UI feedback documentation
│   ├── TECHNICAL_ARCHITECTURE.md     # Architecture documentation
│   └── FILE_STRUCTURE_ANALYSIS.md    # This file
│
├── hooks/                            # Custom React hooks
│   ├── useDebouncedCallback.ts       # Debounced callback hook
│   ├── useDebouncedEffect.ts         # Debounced effect hook
│   └── useSwitchingCode.ts           # Code switching hook
│
├── lib/                              # Library code and utilities
│   ├── sandbox/                      # Sandbox abstraction layer
│   │   ├── providers/                # Sandbox provider implementations
│   │   │   ├── e2b-provider.ts       # E2B sandbox provider
│   │   │   └── vercel-provider.ts    # Vercel sandbox provider
│   │   ├── factory.ts                # Sandbox factory pattern
│   │   ├── sandbox-manager.ts        # Sandbox lifecycle management
│   │   └── types.ts                  # Sandbox type definitions
│   │
│   ├── context-selector.ts           # File context selection for AI
│   ├── edit-examples.ts              # Edit pattern examples
│   ├── edit-intent-analyzer.ts       # AI edit intent analysis
│   ├── file-parser.ts                # Code file parsing utilities
│   ├── file-search-executor.ts       # File search utilities
│   ├── icons.ts                      # Centralized icon exports
│   ├── morph-fast-apply.ts           # Fast code application (Morph API)
│   └── utils.ts                      # General utility functions
│
├── node_modules/                     # Dependencies (generated)
│
├── packages/                         # Monorepo packages
│   └── create-open-lovable/          # CLI scaffolding tool
│       ├── templates/                # Project templates
│       │   ├── e2b/                  # E2B sandbox template
│       │   │   ├── .env.example
│       │   │   ├── package.json
│       │   │   └── README.md
│       │   └── vercel/               # Vercel sandbox template
│       │       ├── .env.example
│       │       ├── package.json
│       │       └── README.md
│       ├── lib/                      # CLI utilities
│       │   ├── create-project.js     # Project creation logic
│       │   └── prompts.js            # CLI prompts
│       ├── index.js                  # CLI entry point
│       ├── package.json              # CLI package config
│       └── README.md
│
├── public/                           # Static assets
│   ├── firecrawl.svg                 # Firecrawl logo
│   ├── globe.svg                     # Globe icon
│   ├── hero-bg.png                   # Hero background
│   ├── logo.svg                      # App logo
│   ├── og-image.png                  # Open Graph image
│   └── ...                           # Other static assets
│
├── styles/                           # CSS styling
│   ├── additional-styles/            # Additional style utilities
│   │   ├── custom-fonts.css          # Font imports
│   │   ├── theme.css                 # Theme variables
│   │   └── utility-patterns.css      # Utility classes
│   │
│   ├── components/                   # Component-specific styles
│   │   ├── button.css                # Button styles
│   │   ├── code.css                  # Code block styles
│   │   ├── input.css                 # Input styles
│   │   └── ...
│   │
│   ├── design-system/                # Design system foundation
│   │   ├── base/                     # Base styles
│   │   │   ├── reset.css             # CSS reset
│   │   │   └── layout.css            # Layout utilities
│   │   ├── animations.css            # Animation keyframes
│   │   ├── colors.css                # Color system
│   │   ├── fonts.css                 # Font definitions
│   │   ├── typography.css            # Typography scale
│   │   └── utilities.css             # Utility classes
│   │
│   ├── fire.css                      # Flame effect styles
│   ├── inside-border-fix.css         # Border utility fixes
│   └── main.css                      # Main stylesheet (imports all)
│
├── types/                            # TypeScript type definitions
│   ├── conversation.ts               # Conversation state types
│   ├── file-manifest.ts              # File manifest types
│   └── sandbox.ts                    # Sandbox-related types
│
├── utils/                            # Utility functions
│   ├── api.ts                        # API utilities
│   ├── date.ts                       # Date utilities
│   ├── format.ts                     # Formatting utilities
│   ├── parse.ts                      # Parsing utilities
│   └── validation.ts                 # Validation utilities
│
├── .env.example                      # Environment variable template
├── .eslintrc.json                    # ESLint configuration
├── .gitignore                        # Git ignore rules
├── colors.json                       # Design system colors
├── eslint.config.mjs                 # ESLint config (modern)
├── next-env.d.ts                     # Next.js TypeScript declarations
├── next.config.ts                    # Next.js configuration
├── package-lock.json                 # Locked dependencies
├── package.json                      # Project dependencies & scripts
├── postcss.config.mjs                # PostCSS configuration
├── README.md                         # Project documentation
├── tailwind.config.ts                # Tailwind CSS configuration
└── tsconfig.json                     # TypeScript configuration
```

---

## Directory Analysis

### `/app` - Next.js App Router

**Purpose**: Main application structure using Next.js 15 App Router

**Key Characteristics**:
- File-based routing
- Server Components by default
- API routes in `/app/api`
- Layouts and nested routing

**Important Files**:
- `layout.tsx` - Root layout with fonts and providers
- `page.tsx` - Home page (landing/search)
- `globals.css` - Global styles
- `generation/page.tsx` - Main generation interface

### `/app/api` - API Routes

**Purpose**: Backend API endpoints (28 routes)

**Organization**:
```
API Routes by Category:
├── Sandbox Management (8 routes)
├── Code Generation (4 routes)
├── Package Management (3 routes)
├── Web Scraping (4 routes)
├── Development Server (5 routes)
└── Utilities (4 routes)
```

**Naming Convention**:
- `route.ts` - API handler file
- Folder name = endpoint name
- Versioning: `-v2` suffix for updates

**Pattern**:
```typescript
// app/api/[endpoint]/route.ts
export async function POST(request: Request) {
  // Handler logic
  return NextResponse.json(result);
}
```

### `/components` - React Components

**Purpose**: All React components organized by scope

**Organization Strategy**:
```
components/
├── app/          # Feature-specific components
├── shared/       # Reusable components
└── ui/           # UI primitives (Shadcn)
```

**Component Types**:
1. **Page Components** (`app/`): Specific to pages
2. **Shared Components** (`shared/`): Reusable across features
3. **UI Components** (`ui/`): Design system primitives

**Naming Convention**:
- PascalCase for files: `HeroSection.tsx`
- Same name as export: `export function HeroSection()`

### `/lib` - Library Code

**Purpose**: Business logic and utilities

**Key Modules**:
- `sandbox/` - Sandbox provider abstraction
- `context-selector.ts` - AI context selection
- `edit-intent-analyzer.ts` - Edit analysis
- `file-parser.ts` - Code parsing
- `utils.ts` - General utilities

**Pattern**: Pure functions, no React dependencies

### `/hooks` - Custom React Hooks

**Purpose**: Reusable React hooks

**Examples**:
- `useDebouncedCallback` - Debounce function calls
- `useDebouncedEffect` - Debounce effects
- `useSwitchingCode` - Code switching logic

**Convention**: `use` prefix, returns values/functions

### `/types` - TypeScript Types

**Purpose**: Shared type definitions

**Files**:
- `conversation.ts` - Chat/conversation types
- `file-manifest.ts` - File structure types
- `sandbox.ts` - Sandbox-related types

**Usage**:
```typescript
import type { ConversationMessage } from '@/types/conversation';
```

### `/atoms` - State Management

**Purpose**: Jotai atomic state

**Files**:
- `sheets.ts` - Sheet/modal state

**Pattern**:
```typescript
export const sheetsAtom = atom<SheetState>({...});
```

### `/config` - Configuration

**Purpose**: Application configuration

**Files**:
- `app.config.ts` - Centralized app config

**Contents**:
- Sandbox settings
- AI model configuration
- Feature flags
- Constants

### `/styles` - Styling

**Purpose**: CSS styling and design system

**Structure**:
```
styles/
├── design-system/    # Foundation (colors, typography)
├── components/       # Component-specific styles
├── additional-styles/# Utilities and themes
└── main.css         # Main entry (imports all)
```

**Strategy**: Tailwind CSS + Custom CSS for complex styles

### `/public` - Static Assets

**Purpose**: Publicly accessible files

**Contents**:
- Images (PNG, SVG)
- Icons
- Fonts (if not using next/font)
- Static resources

**Access**: `/filename.ext` in browser

### `/packages` - Monorepo Packages

**Purpose**: Separate packages within monorepo

**Current Packages**:
- `create-open-lovable/` - CLI scaffolding tool

**Pattern**: Each package has own `package.json`

### `/docs` - Documentation

**Purpose**: Project documentation

**Files**:
- Technical guides
- API documentation
- Architecture docs
- Setup instructions

---

## File Categories

### Configuration Files (Root Level)

| File | Purpose | Language |
|------|---------|----------|
| `package.json` | Dependencies & scripts | JSON |
| `tsconfig.json` | TypeScript configuration | JSON |
| `next.config.ts` | Next.js configuration | TypeScript |
| `tailwind.config.ts` | Tailwind CSS config | TypeScript |
| `postcss.config.mjs` | PostCSS plugins | JavaScript |
| `eslint.config.mjs` | ESLint rules | JavaScript |
| `.env.example` | Environment template | ENV |
| `colors.json` | Design system colors | JSON |

### Entry Points

| File | Purpose |
|------|---------|
| `app/layout.tsx` | Root layout |
| `app/page.tsx` | Home page |
| `app/generation/page.tsx` | Generation interface |
| `app/api/*/route.ts` | API endpoints |
| `styles/main.css` | CSS entry |

### Core Application Files

#### Layouts
- `app/layout.tsx` - Root layout
- `app/generation/layout.tsx` - Generation layout (if exists)

#### Pages
- `app/page.tsx` - Home/landing page
- `app/landing.tsx` - Landing component
- `app/generation/page.tsx` - Main generation interface
- `app/builder/page.tsx` - Alternative builder UI

#### Critical Components
- `components/HeroInput.tsx` - Search/URL input
- `components/SandboxPreview.tsx` - Live preview
- `components/CodeApplicationProgress.tsx` - Progress UI
- `components/HMRErrorDetector.tsx` - Error detection

### API Route Files

All API routes follow this pattern:
```
app/api/[endpoint-name]/route.ts
```

Example:
```typescript
// app/api/create-ai-sandbox-v2/route.ts
export async function POST(request: Request) {
  const body = await request.json();
  // Create sandbox logic
  return NextResponse.json({ sandboxId, url });
}
```

### Type Definition Files

| File | Exports |
|------|---------|
| `types/conversation.ts` | `ConversationMessage`, `ConversationState`, `ConversationEdit` |
| `types/file-manifest.ts` | `FileManifest`, `FileNode`, `FileInfo` |
| `types/sandbox.ts` | `SandboxData`, `SandboxProvider`, `SandboxStatus` |

### Utility Files

| File | Purpose |
|------|---------|
| `lib/utils.ts` | General utilities (cn, formatDate, etc.) |
| `lib/file-parser.ts` | Parse code files |
| `lib/context-selector.ts` | Select AI context |
| `lib/edit-intent-analyzer.ts` | Analyze edits |
| `utils/api.ts` | API helpers |
| `utils/validation.ts` | Validation functions |

---

## Key Files Reference

### Most Important Files (Top 20)

1. **`app/layout.tsx`** - Root layout, fonts, metadata
2. **`app/page.tsx`** - Home page entry point
3. **`app/generation/page.tsx`** - Main generation interface
4. **`app/api/generate-ai-code-stream/route.ts`** - Code generation API
5. **`app/api/create-ai-sandbox-v2/route.ts`** - Sandbox creation API
6. **`app/api/apply-ai-code-stream/route.ts`** - Code application API
7. **`app/api/scrape-website/route.ts`** - Web scraping API
8. **`lib/sandbox/factory.ts`** - Sandbox factory
9. **`lib/sandbox/providers/e2b-provider.ts`** - E2B implementation
10. **`lib/sandbox/providers/vercel-provider.ts`** - Vercel implementation
11. **`lib/sandbox/sandbox-manager.ts`** - Sandbox lifecycle
12. **`components/HeroInput.tsx`** - Main input component
13. **`components/SandboxPreview.tsx`** - Preview component
14. **`config/app.config.ts`** - App configuration
15. **`styles/main.css`** - Main stylesheet
16. **`tailwind.config.ts`** - Tailwind configuration
17. **`next.config.ts`** - Next.js configuration
18. **`package.json`** - Dependencies
19. **`tsconfig.json`** - TypeScript config
20. **`lib/utils.ts`** - Utility functions

### Configuration Deep Dive

#### `package.json`

```json
{
  "name": "open-lovable",
  "version": "0.1.0",
  "scripts": {
    "dev": "next dev --turbopack",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "next": "15.4.3",
    "react": "19.1.0",
    "react-dom": "19.1.0",
    "typescript": "^5",
    // ... 80+ more packages
  }
}
```

**Key Scripts**:
- `dev` - Development with Turbopack
- `build` - Production build
- `start` - Production server
- `lint` - Code linting

#### `tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "ES2017",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [{ "name": "next" }],
    "paths": {
      "@/*": ["./*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

**Key Features**:
- Strict mode enabled
- Path aliases: `@/*` → `./*`
- JSX preserved (Next.js handles it)
- Modern ES features

#### `next.config.ts`

```typescript
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  /* config options here */
};

export default nextConfig;
```

**Default Configuration**:
- Minimal setup (relies on Next.js defaults)
- TypeScript configuration
- Can be extended for:
  - Custom webpack config
  - Image domains
  - Redirects/rewrites
  - Environment variables

#### `tailwind.config.ts`

**Key Customizations**:

```typescript
export default {
  content: [
    "./pages/**/*.{js,ts,jsx,tsx,mdx}",
    "./components/**/*.{js,ts,jsx,tsx,mdx}",
    "./app/**/*.{js,ts,jsx,tsx,mdx}",
  ],
  theme: {
    extend: {
      colors: {
        // Custom colors from colors.json
      },
      fontSize: {
        'title-h1': ['60px', '64px'],
        'title-h2': ['48px', '52px'],
        'title-h3': ['36px', '40px'],
        'title-h4': ['30px', '34px'],
        'title-h5': ['24px', '28px'],
        // ... more sizes
      },
      spacing: {
        // 0-1000px custom spacing
      }
    }
  },
  plugins: [
    require('@tailwindcss/typography'),
    require('tailwindcss-animate'),
    // ... more plugins
  ]
}
```

**Custom Utilities**:
- `.center`, `.center-x`, `.center-y`
- `.flex-center`
- `.overlay`
- Custom sizing: `cw-*`, `ch-*`, `cs-*`

#### `app.config.ts`

```typescript
export const appConfig = {
  vercelSandbox: {
    timeoutMinutes: 15,
    devPort: 3000,
    runtime: 'node22'
  },
  e2b: {
    timeoutMinutes: 30,
    vitePort: 5173
  },
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
  },
  codeApplication: {
    defaultRefreshDelay: 2000,
    enableTruncationRecovery: false
  },
  files: {
    excludePatterns: [
      '**/node_modules/**',
      '**/.git/**',
      '**/dist/**',
      '**/.next/**'
    ],
    maxFileSize: 1024 * 1024 // 1MB
  }
};
```

---

## Code Organization Patterns

### 1. Feature-Based Organization (App Directory)

```
app/
├── (auth)/              # Route group: authentication
│   ├── login/
│   └── register/
├── (dashboard)/         # Route group: dashboard
│   ├── settings/
│   └── profile/
└── generation/          # Public route: generation
```

**Benefits**:
- Clear feature boundaries
- Easy to find related files
- Scalable structure

### 2. Component Co-location

```
components/
├── HeroInput/
│   ├── HeroInput.tsx         # Component
│   ├── HeroInput.test.tsx    # Tests (if any)
│   ├── HeroInput.module.css  # Styles (if needed)
│   ├── hooks.ts              # Component-specific hooks
│   └── utils.ts              # Component-specific utils
```

**Benefits**:
- Related code stays together
- Easy to refactor/move
- Clear dependencies

### 3. Barrel Exports

```typescript
// components/shared/index.ts
export { HeroInput } from './HeroInput';
export { Button } from './Button';
export { Card } from './Card';

// Usage
import { HeroInput, Button, Card } from '@/components/shared';
```

**Benefits**:
- Clean imports
- Easier refactoring
- Centralized exports

### 4. Type Co-location

```
lib/sandbox/
├── factory.ts
├── sandbox-manager.ts
├── types.ts              # All sandbox types
└── providers/
    ├── e2b-provider.ts
    └── vercel-provider.ts
```

**Pattern**: Types in same directory as implementation

### 5. Configuration Co-location

```
app/api/
└── [endpoint]/
    ├── route.ts          # API handler
    ├── schema.ts         # Validation schema
    └── types.ts          # Endpoint-specific types
```

### 6. Utility Organization

```
lib/
├── utils.ts              # General utilities
├── api.ts                # API utilities
└── [feature]/
    └── utils.ts          # Feature-specific utilities
```

**Rule**: General utils in root, specific utils in feature folders

---

## Import Paths and Aliases

### Path Alias Configuration

**`tsconfig.json`**:
```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./*"]
    }
  }
}
```

**Usage**:
```typescript
// Instead of: import { ... } from '../../../lib/utils'
import { ... } from '@/lib/utils';

// Instead of: import { ... } from '../../components/shared/Button'
import { Button } from '@/components/shared/Button';
```

### Import Patterns

**Absolute imports (preferred)**:
```typescript
import { appConfig } from '@/config/app.config';
import { SandboxFactory } from '@/lib/sandbox/factory';
import { Button } from '@/components/ui/shadcn/button';
import type { ConversationState } from '@/types/conversation';
```

**Relative imports (when nearby)**:
```typescript
import { helper } from './helper';
import { useLocalHook } from './hooks';
```

### Import Order Convention

**Recommended**:
```typescript
// 1. External dependencies
import React, { useState, useEffect } from 'react';
import { NextResponse } from 'next/server';
import { z } from 'zod';

// 2. Internal absolute imports
import { appConfig } from '@/config/app.config';
import { SandboxFactory } from '@/lib/sandbox/factory';
import { Button } from '@/components/ui/button';

// 3. Type imports
import type { SandboxData } from '@/types/sandbox';
import type { ConversationState } from '@/types/conversation';

// 4. Relative imports
import { localHelper } from './helper';

// 5. Styles
import './styles.css';
```

### Module Resolution

**Next.js Resolution Order**:
1. TypeScript path aliases (`@/*`)
2. `node_modules`
3. Relative paths
4. File extensions (`.ts`, `.tsx`, `.js`, `.jsx`)

**Example**:
```typescript
// All resolve correctly
import { Button } from '@/components/ui/button';
import { Button } from '@/components/ui/button.tsx';
import { Button } from '@/components/ui/button/index.tsx';
```

---

## File Naming Conventions

### Components

- **PascalCase**: `HeroSection.tsx`, `CodeViewer.tsx`
- **Index files**: `index.tsx` for barrel exports
- **Test files**: `ComponentName.test.tsx` (if tests exist)

### Utilities & Libraries

- **camelCase**: `utils.ts`, `fileParser.ts`
- **Hyphenated**: `context-selector.ts`, `edit-intent-analyzer.ts`

### API Routes

- **Hyphenated**: `create-ai-sandbox-v2/route.ts`
- **Always**: `route.ts` for handler

### Types

- **camelCase**: `conversation.ts`, `sandbox.ts`
- **Interface files**: Match feature name

### Styles

- **Hyphenated**: `design-system.css`, `utility-patterns.css`
- **Component styles**: Match component name

### Configuration

- **Descriptive**: `app.config.ts`, `tailwind.config.ts`
- **Dot files**: `.env.example`, `.gitignore`

---

## Special Directories

### `(route-groups)` - Grouped Routes

**Pattern**: Parentheses `(name)` create layout groups without affecting URL

**Example**:
```
app/
├── (home)/
│   └── sections/
│       └── HeroSection.tsx
└── (dashboard)/
    └── settings/
        └── page.tsx
```

**URL**: `/settings` (not `/(dashboard)/settings`)

### `[dynamic]` - Dynamic Routes

**Pattern**: Square brackets for dynamic segments

**Example**:
```
app/
└── posts/
    └── [id]/
        └── page.tsx
```

**URL**: `/posts/123`, `/posts/abc`

**Access**:
```typescript
export default function Page({ params }: { params: { id: string } }) {
  return <div>Post ID: {params.id}</div>;
}
```

---

## Summary

### Project Structure Strengths

✅ **Clear Organization**: Logical directory structure
✅ **Feature-Based**: Related code stays together
✅ **Modular**: Easy to find and modify code
✅ **TypeScript**: Strong typing throughout
✅ **Scalable**: Can grow without restructuring

### Recommended Practices

1. **Use path aliases**: `@/*` for all imports
2. **Co-locate**: Keep related files together
3. **Consistent naming**: Follow established conventions
4. **Type safety**: Define types near implementations
5. **Barrel exports**: Use index files for clean imports

### File Organization Checklist

- [ ] Components in appropriate directory (app/shared/ui)
- [ ] Types co-located with features
- [ ] Utilities organized by scope
- [ ] API routes follow naming convention
- [ ] Styles in design system structure
- [ ] Documentation kept up-to-date

---

**Document Version**: 1.0
**Last Updated**: 2025-11-14
**Total Files Analyzed**: 300+
**Maintained By**: Development Team
