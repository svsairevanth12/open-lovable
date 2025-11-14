# Technical Architecture Document - Open Lovable

## Table of Contents
1. [Project Overview](#project-overview)
2. [System Architecture](#system-architecture)
3. [Component Architecture](#component-architecture)
4. [Data Flow](#data-flow)
5. [API Architecture](#api-architecture)
6. [Sandbox Architecture](#sandbox-architecture)
7. [State Management](#state-management)
8. [Security Architecture](#security-architecture)
9. [Performance Optimization](#performance-optimization)
10. [Deployment Architecture](#deployment-architecture)

---

## Project Overview

### What is Open Lovable?

**Open Lovable** is an AI-powered web application that enables users to clone and reimagine existing websites through natural language interactions. It combines web scraping, artificial intelligence, and isolated sandbox environments to generate, edit, and preview React applications in real-time.

### Key Capabilities

- **Website Cloning**: Scrape and analyze existing websites with AI-powered understanding
- **Multi-Model AI Generation**: Support for GPT-5, Claude Sonnet 4, Gemini 2.0, and Kimi K2
- **Live Sandbox Environments**: Isolated execution in Vercel or E2B sandboxes
- **Real-time Streaming**: Progressive code generation with live feedback
- **Interactive Code Editing**: Natural language code modifications
- **Style Templates**: Pre-built design systems (Glassmorphism, Neumorphism, Brutalism, etc.)

### Technical Foundation

- **Version**: 0.1.0
- **Framework**: Next.js 15.4.3 with App Router
- **Language**: TypeScript 5.x (strict mode)
- **Rendering**: React 19.1.0 with Server Components
- **Styling**: Tailwind CSS 3.4.17 + Custom Design System

---

## System Architecture

### High-Level Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        USER INTERFACE                            │
│  ┌──────────────┐  ┌────────────────┐  ┌────────────────────┐  │
│  │ Landing Page │  │   Generation   │  │  Sandbox Preview   │  │
│  │   (Search)   │→│    Interface   │→│    (Live iframe)   │  │
│  └──────────────┘  └────────────────┘  └────────────────────┘  │
└────────────────────────────┬────────────────────────────────────┘
                             │ HTTP/SSE
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│                   NEXT.JS APPLICATION SERVER                     │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                   API LAYER (28 Routes)                  │   │
│  │  ┌──────────────┐ ┌──────────────┐ ┌─────────────────┐ │   │
│  │  │  Sandbox     │ │   AI Code    │ │  Web Scraping   │ │   │
│  │  │  Management  │ │  Generation  │ │  & Search       │ │   │
│  │  └──────────────┘ └──────────────┘ └─────────────────┘ │   │
│  │  ┌──────────────┐ ┌──────────────┐ ┌─────────────────┐ │   │
│  │  │  Package     │ │  File        │ │  Development    │ │   │
│  │  │  Management  │ │  Operations  │ │  Server Control │ │   │
│  │  └──────────────┘ └──────────────┘ └─────────────────┘ │   │
│  └─────────────────────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │              BUSINESS LOGIC LAYER                        │   │
│  │  • Sandbox Manager     • Context Selector               │   │
│  │  • Edit Intent Analyzer • File Parser                   │   │
│  │  • Morph Fast Apply    • File Search Executor           │   │
│  └─────────────────────────────────────────────────────────┘   │
└───────┬──────────────┬──────────────┬──────────────┬───────────┘
        │              │              │              │
        ↓              ↓              ↓              ↓
┌─────────────┐ ┌────────────┐ ┌──────────────┐ ┌──────────────┐
│ AI Services │ │ Web Scraper│ │   Sandbox    │ │ Node.js      │
│             │ │            │ │  Providers   │ │ Runtime      │
│ • Anthropic │ │ Firecrawl  │ │              │ │              │
│ • OpenAI    │ │  - Scrape  │ │ • Vercel     │ │ • Global     │
│ • Google    │ │  - Screen  │ │ • E2B        │ │   State      │
│ • Groq      │ │  - Search  │ │              │ │ • Process    │
│             │ │            │ │ • Vite Setup │ │   Memory     │
└─────────────┘ └────────────┘ └──────────────┘ └──────────────┘
```

### Architecture Principles

1. **Microservices-Style API Routes**: Each API route is a self-contained service
2. **Provider Pattern**: Abstract sandbox providers for flexibility
3. **Streaming First**: Real-time updates for better UX
4. **Server Components**: Leverage React Server Components for performance
5. **Separation of Concerns**: Clear boundaries between UI, logic, and services

---

## Component Architecture

### Frontend Layer Architecture

```
App Structure (Next.js App Router)
│
├── app/
│   ├── layout.tsx ──────────────→ Root Layout (Fonts, Metadata, Providers)
│   ├── page.tsx ────────────────→ Landing/Search Page
│   ├── generation/
│   │   └── page.tsx ────────────→ Main Generation Interface
│   └── api/
│       └── [28 routes] ─────────→ Backend API Services
│
├── components/
│   ├── app/
│   │   ├── (home)/
│   │   │   └── sections/ ───────→ Hero, Input, Carousel
│   │   └── generation/
│   │       ├── ChatInterface ───→ AI Chat UI
│   │       ├── FileTree ────────→ File Browser
│   │       ├── CodeViewer ──────→ Syntax Highlighting
│   │       └── SandboxControl ──→ Sandbox Management
│   ├── shared/
│   │   ├── Playground/ ─────────→ Code Playground
│   │   ├── effects/ ────────────→ Visual Effects
│   │   └── ui/ ─────────────────→ Reusable Components
│   └── ui/
│       ├── shadcn/ ─────────────→ 30+ UI Components
│       └── motion/ ─────────────→ Framer Motion Wrappers
│
└── lib/
    ├── sandbox/
    │   ├── factory.ts ──────────→ Sandbox Factory
    │   ├── sandbox-manager.ts ──→ Lifecycle Management
    │   └── providers/
    │       ├── e2b-provider.ts ─→ E2B Implementation
    │       └── vercel-provider.ts→ Vercel Implementation
    └── [utility modules]
```

### Component Hierarchy

```
RootLayout
│
├── HeaderProvider
│   └── Header
│       ├── HeaderBrandKit (Logo)
│       └── HeaderDropdown (Nav)
│
└── Page Content
    │
    ├── HomePage (/)
    │   ├── Background (PixiJS)
    │   ├── HeroFlame Effect
    │   ├── HeroSection
    │   │   ├── Title
    │   │   ├── Badge
    │   │   └── HeroInput
    │   │       ├── Search/URL Input
    │   │       ├── StyleSelector
    │   │       └── ModelSelector
    │   └── SearchCarousel
    │       └── ScreenshotCards[]
    │
    └── GenerationPage (/generation)
        ├── Sidebar
        │   ├── ChatInterface
        │   │   ├── MessageList
        │   │   ├── InputArea
        │   │   └── ProgressTracker
        │   └── FileTree
        │       └── FileTreeNode[]
        └── MainContent
            ├── Tabs
            │   ├── CodeView
            │   │   └── SyntaxHighlighter
            │   └── PreviewView
            │       └── SandboxPreview
            │           └── iframe
            └── ControlBar
                ├── RestartButton
                ├── RefreshButton
                └── ExportButton
```

### Component Communication Patterns

**1. Props Drilling (Parent → Child)**
```typescript
<SandboxPreview
  sandboxUrl={sandboxData.url}
  onError={handleError}
/>
```

**2. State Lifting (Child → Parent via Callbacks)**
```typescript
<HeroInput
  onSubmit={(url, style, model) => handleGeneration(url, style, model)}
/>
```

**3. Global State (Jotai Atoms)**
```typescript
const [sheets, setSheets] = useAtom(sheetsAtom);
```

**4. Session Storage (Persistence)**
```typescript
sessionStorage.setItem('targetUrl', url);
const url = sessionStorage.getItem('targetUrl');
```

**5. Server State (API Calls)**
```typescript
const response = await fetch('/api/sandbox-status');
const status = await response.json();
```

---

## Data Flow

### 1. Complete User Journey Flow

```
┌─────────────────────────────────────────────────────────────────┐
│ PHASE 1: WEBSITE SELECTION                                       │
└─────────────────────────────────────────────────────────────────┘
                              │
    User enters URL/search ───┤
                              ↓
         ┌────────────────────────────────────┐
         │  HeroInput Component               │
         │  • Validate input                  │
         │  • Select style template           │
         │  • Select AI model                 │
         └────────────────────────────────────┘
                              │
                Store in sessionStorage ───┐
                              │            │
                              ↓            ↓
                   Redirect to /generation
                              │
┌─────────────────────────────────────────────────────────────────┐
│ PHASE 2: INITIALIZATION                                          │
└─────────────────────────────────────────────────────────────────┘
                              │
        Generation Page Load ─┤
                              ↓
         ┌────────────────────────────────────┐
         │  useEffect Hook                    │
         │  1. Read sessionStorage            │
         │  2. Clear old conversation         │
         │  3. Create new sandbox             │
         └────────────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ↓                               ↓
    POST /api/conversation-state    POST /api/create-ai-sandbox-v2
              │                               │
              ↓                               ↓
    Clear old context              ┌──────────────────────┐
                                   │ SandboxFactory       │
                                   │  • Detect provider   │
                                   │  • Create instance   │
                                   │  • Setup Vite app    │
                                   │  • Install deps      │
                                   │  • Start dev server  │
                                   └──────────────────────┘
                                              │
                              Store in global.sandboxState
                                              │
                              Return: { sandboxId, url }
                                              │
┌─────────────────────────────────────────────────────────────────┐
│ PHASE 3: WEB SCRAPING (if URL provided)                         │
└─────────────────────────────────────────────────────────────────┘
                              │
                If URL exists ┤
                              ↓
         ┌────────────────────────────────────┐
         │  POST /api/scrape-website          │
         │  • Validate URL                    │
         │  • Call Firecrawl API              │
         │  • Capture screenshot              │
         │  • Extract HTML/markdown           │
         │  • Store in conversation context   │
         └────────────────────────────────────┘
                              │
                              ↓
               Return: { markdown, html, screenshot }
                              │
┌─────────────────────────────────────────────────────────────────┐
│ PHASE 4: CODE GENERATION                                         │
└─────────────────────────────────────────────────────────────────┘
                              │
          Auto-start generation (or user prompt)
                              │
                              ↓
         ┌────────────────────────────────────┐
         │  POST /api/generate-ai-code-stream │
         └────────────────────────────────────┘
                              │
              ┌───────────────┴────────────────┐
              ↓                                ↓
    Prepare Context                    Select AI Model
    • Scraped content                  • GPT-5
    • Style template                   • Claude Sonnet 4
    • User preferences                 • Gemini 2.0
    • Previous edits                   • Kimi K2
              │                                │
              └───────────────┬────────────────┘
                              ↓
                  ┌───────────────────────┐
                  │  AI Provider (Stream) │
                  │  • Send prompt        │
                  │  • Stream tokens      │
                  │  • Extract files      │
                  └───────────────────────┘
                              │
                   Server-Sent Events (SSE)
                              │
              ┌───────────────┼───────────────┐
              ↓               ↓               ↓
         File chunk      Progress        Completion
              │               │               │
              └───────────────┴───────────────┘
                              ↓
                    Update UI in real-time
                    • Show generated code
                    • Display progress
                    • Track files
                              │
┌─────────────────────────────────────────────────────────────────┐
│ PHASE 5: CODE APPLICATION                                        │
└─────────────────────────────────────────────────────────────────┘
                              │
         When generation complete
                              ↓
         ┌────────────────────────────────────┐
         │  POST /api/apply-ai-code-stream    │
         └────────────────────────────────────┘
                              │
              ┌───────────────┴────────────────┐
              ↓                                ↓
    Parse Generated Files          Access Sandbox Provider
    • Extract file paths           (global.sandboxState)
    • Detect file types                       │
    • Organize structure                      │
              │                                │
              └───────────────┬────────────────┘
                              ↓
              ┌───────────────────────────────┐
              │  For each file:               │
              │  1. Write to sandbox          │
              │  2. Detect new packages       │
              │  3. Install packages          │
              │  4. Update file tree          │
              └───────────────────────────────┘
                              │
                   Server-Sent Events (SSE)
                              │
              ┌───────────────┼───────────────┐
              ↓               ↓               ↓
         Writing        Installing        Building
         progress       packages          project
              │               │               │
              └───────────────┴───────────────┘
                              ↓
                  CodeApplicationProgress UI
                  • File-by-file tracking
                  • Package installation status
                  • Build output
                              │
┌─────────────────────────────────────────────────────────────────┐
│ PHASE 6: LIVE PREVIEW                                            │
└─────────────────────────────────────────────────────────────────┘
                              │
         When build complete ─┤
                              ↓
         ┌────────────────────────────────────┐
         │  SandboxPreview Component          │
         │  • Load iframe with sandbox URL    │
         │  • Monitor for errors              │
         │  • Detect HMR updates              │
         └────────────────────────────────────┘
                              │
                              ↓
                   User sees live preview
                   • Fully interactive
                   • Hot module reload
                   • Real Vite dev server
                              │
┌─────────────────────────────────────────────────────────────────┐
│ PHASE 7: ITERATIVE EDITING                                       │
└─────────────────────────────────────────────────────────────────┘
                              │
         User sends chat message
                              ↓
         ┌────────────────────────────────────┐
         │  POST /api/analyze-edit-intent     │
         │  • Analyze user message            │
         │  • Select relevant files           │
         │  • Prepare context                 │
         └────────────────────────────────────┘
                              │
                              ↓
         Return to PHASE 4 (Code Generation)
         with targeted context
                              │
                              ↓
         Loop: Generate → Apply → Preview
```

### 2. Streaming Architecture Detail

```
Client (Browser)                Server (Next.js)              AI Provider
      │                               │                            │
      │  POST /api/generate-ai       │                            │
      │  -code-stream                │                            │
      ├──────────────────────────────>│                            │
      │                               │  streamText()              │
      │                               ├────────────────────────────>│
      │                               │                            │
      │                               │<┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈│
      │                               │  Token stream              │
      │<┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈│                            │
      │  SSE: data: {token}           │                            │
      │                               │<┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈│
      │<┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈│                            │
      │  SSE: data: {token}           │                            │
      │                               │<┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈│
      │<┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈│                            │
      │  SSE: data: {token}           │                            │
      │                               │                            │
      │  [Parse tokens in real-time]  │  [Buffer & parse files]    │
      │  [Update UI progressively]    │                            │
      │                               │<┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈│
      │<┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈│  Stream complete           │
      │  SSE: data: [DONE]            │                            │
      │                               │                            │
```

### 3. State Synchronization Flow

```
┌─────────────────────────────────────────────────────────────────┐
│ CLIENT STATE                                                     │
├─────────────────────────────────────────────────────────────────┤
│ • chatMessages: ChatMessage[]                                    │
│ • sandboxData: { sandboxId, url }                                │
│ • generationProgress: { stage, percent }                         │
│ • fileTree: FileNode[]                                           │
│ • selectedFile: string | null                                    │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                    API Requests
                            │
                            ↓
┌─────────────────────────────────────────────────────────────────┐
│ SERVER STATE (Node.js Global)                                   │
├─────────────────────────────────────────────────────────────────┤
│ global.sandboxState = {                                          │
│   sandbox: SandboxProvider,                                      │
│   sandboxData: { sandboxId, url },                               │
│   fileCache: {                                                   │
│     files: Record<string, SandboxFile>,                          │
│     lastSync: timestamp                                          │
│   }                                                              │
│ }                                                                │
│                                                                  │
│ global.conversationState = {                                     │
│   conversationId: string,                                        │
│   context: {                                                     │
│     messages: ConversationMessage[],                             │
│     edits: ConversationEdit[],                                   │
│     currentTopic: string,                                        │
│     projectEvolution: {...},                                     │
│     userPreferences: {...}                                       │
│   }                                                              │
│ }                                                                │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                    Provider API Calls
                            │
                            ↓
┌─────────────────────────────────────────────────────────────────┐
│ SANDBOX STATE (E2B/Vercel)                                      │
├─────────────────────────────────────────────────────────────────┤
│ • File System: /home/user/project/**/*                           │
│ • Node Modules: /home/user/project/node_modules/**               │
│ • Vite Dev Server: Running on port 5173                          │
│ • Process State: { pid, status, logs }                           │
└─────────────────────────────────────────────────────────────────┘
```

---

## API Architecture

### API Route Organization

```
/app/api/
├── Sandbox Management (8 routes)
│   ├── create-ai-sandbox/          [Legacy]
│   ├── create-ai-sandbox-v2/       [Current] Sandbox initialization
│   ├── kill-sandbox/               Terminate sandbox
│   ├── sandbox-status/             Health check
│   ├── sandbox-logs/               Retrieve logs
│   ├── run-command/                [Legacy]
│   ├── run-command-v2/             [Current] Execute commands
│   └── get-sandbox-files/          List files
│
├── Code Generation (4 routes)
│   ├── generate-ai-code-stream/    Stream code generation
│   ├── apply-ai-code/              [Legacy]
│   ├── apply-ai-code-stream/       [Current] Stream code application
│   └── analyze-edit-intent/        Analyze edit requests
│
├── Package Management (3 routes)
│   ├── install-packages/           [Legacy]
│   ├── install-packages-v2/        [Current] Install npm packages
│   └── detect-and-install-packages/Auto-detect & install
│
├── Web Scraping (4 routes)
│   ├── scrape-website/             Scrape URL content
│   ├── scrape-url-enhanced/        Enhanced scraping
│   ├── scrape-screenshot/          Capture screenshot
│   └── search/                     Web search
│
├── Development Server (5 routes)
│   ├── restart-vite/               Restart Vite server
│   ├── monitor-vite-logs/          Monitor Vite logs
│   ├── report-vite-error/          Report errors
│   ├── check-vite-errors/          Check error status
│   └── clear-vite-errors-cache/    Clear error cache
│
└── Utilities (4 routes)
    ├── conversation-state/         Manage conversation context
    └── create-zip/                 Export project as ZIP
```

### API Design Patterns

**1. Server-Sent Events (SSE) for Streaming**

```typescript
// app/api/generate-ai-code-stream/route.ts
export async function POST(request: Request) {
  const encoder = new TextEncoder();

  const stream = new ReadableStream({
    async start(controller) {
      try {
        // Stream AI responses
        for await (const chunk of aiStream) {
          controller.enqueue(
            encoder.encode(`data: ${JSON.stringify(chunk)}\n\n`)
          );
        }
        controller.enqueue(encoder.encode('data: [DONE]\n\n'));
        controller.close();
      } catch (error) {
        controller.error(error);
      }
    }
  });

  return new Response(stream, {
    headers: {
      'Content-Type': 'text/event-stream',
      'Cache-Control': 'no-cache',
      'Connection': 'keep-alive'
    }
  });
}
```

**2. Global State Management**

```typescript
// Persistent sandbox state across API requests
declare global {
  var activeSandboxProvider: SandboxProvider | undefined;
  var sandboxState: SandboxState | undefined;
  var conversationState: ConversationState | undefined;
}

// Access in any API route
const sandbox = global.activeSandboxProvider;
if (!sandbox) {
  return NextResponse.json(
    { error: 'No active sandbox' },
    { status: 400 }
  );
}
```

**3. Versioned API Routes**

```
/api/create-ai-sandbox/      → v1 (legacy)
/api/create-ai-sandbox-v2/   → v2 (current)

Benefits:
• Backward compatibility
• Gradual migration
• A/B testing
```

**4. Error Handling Pattern**

```typescript
export async function POST(request: Request) {
  try {
    const body = await request.json();

    // Validation
    if (!body.required) {
      return NextResponse.json(
        { error: 'Missing required field' },
        { status: 400 }
      );
    }

    // Business logic
    const result = await doSomething(body);

    return NextResponse.json({ success: true, data: result });

  } catch (error) {
    console.error('API Error:', error);
    return NextResponse.json(
      { error: error instanceof Error ? error.message : 'Unknown error' },
      { status: 500 }
    );
  }
}
```

### API Authentication & Security

**Current State**: No authentication implemented (development/demo mode)

**Security Measures**:
- CORS middleware for API protection
- Environment variable for API keys
- Sandbox isolation prevents code execution on main server
- Rate limiting (recommended for production)

**Production Recommendations**:
```typescript
// Future implementation
import { auth } from '@/lib/auth';

export async function POST(request: Request) {
  const session = await auth();
  if (!session) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 });
  }
  // ... proceed with authenticated request
}
```

---

## Sandbox Architecture

### Sandbox Provider Pattern

The application uses a **Strategy Pattern** with a **Factory** to support multiple sandbox providers.

```
                    ┌─────────────────┐
                    │ SandboxFactory  │
                    │  createSandbox()│
                    └────────┬────────┘
                             │
                   detects provider type
                             │
              ┌──────────────┴──────────────┐
              ↓                             ↓
    ┌──────────────────┐          ┌──────────────────┐
    │  E2BProvider     │          │ VercelProvider   │
    │  implements      │          │  implements      │
    │  SandboxProvider │          │  SandboxProvider │
    └──────────────────┘          └──────────────────┘
              │                             │
              ↓                             ↓
    ┌──────────────────┐          ┌──────────────────┐
    │  E2B Sandbox     │          │ Vercel Sandbox   │
    │  • Code Interp.  │          │  • Node.js       │
    │  • Port: 5173    │          │  • Port: 3000    │
    │  • Timeout: 30m  │          │  • Timeout: 15m  │
    └──────────────────┘          └──────────────────┘
```

### SandboxProvider Interface

```typescript
interface SandboxProvider {
  // Core Operations
  createSandbox(): Promise<SandboxData>;
  runCommand(command: string): Promise<CommandResult>;
  writeFile(path: string, content: string): Promise<void>;
  readFile(path: string): Promise<string>;
  listFiles(path: string): Promise<FileInfo[]>;
  installPackages(packages: string[]): Promise<InstallResult>;
  terminate(): Promise<void>;

  // Metadata
  getSandboxId(): string;
  getSandboxUrl(): string;
  getStatus(): SandboxStatus;

  // Development Server
  startViteServer(): Promise<void>;
  restartViteServer(): Promise<void>;
  monitorLogs(): AsyncIterable<LogEntry>;
}
```

### Sandbox Lifecycle

```
┌─────────────────────────────────────────────────────────────────┐
│ 1. CREATION                                                      │
└─────────────────────────────────────────────────────────────────┘
    POST /api/create-ai-sandbox-v2
         │
         ↓
    SandboxFactory.createSandbox(provider)
         │
         ├─→ E2BProvider.createSandbox()
         │    • Create E2B code interpreter
         │    • Wait for ready state
         │    • Store in global.activeSandboxProvider
         │
         └─→ VercelProvider.createSandbox()
              • Create Vercel sandbox runtime
              • Initialize Node.js environment
              • Store in global.activeSandboxProvider

┌─────────────────────────────────────────────────────────────────┐
│ 2. INITIALIZATION                                                │
└─────────────────────────────────────────────────────────────────┘
    setupViteApp()
         │
         ├─→ Create package.json
         ├─→ Create vite.config.ts
         ├─→ Create index.html
         ├─→ Create src/main.tsx (React entry)
         ├─→ Create tsconfig.json
         └─→ Install base dependencies:
              • react, react-dom
              • vite, @vitejs/plugin-react
              • typescript
              • @types/react, @types/react-dom

┌─────────────────────────────────────────────────────────────────┐
│ 3. OPERATION                                                     │
└─────────────────────────────────────────────────────────────────┘
    During code generation:
         │
         ├─→ writeFile() for each generated file
         ├─→ detectPackages() to find npm dependencies
         ├─→ installPackages() to add new packages
         ├─→ startViteServer() or restartViteServer()
         └─→ monitorLogs() for error detection

┌─────────────────────────────────────────────────────────────────┐
│ 4. MONITORING                                                    │
└─────────────────────────────────────────────────────────────────┘
    Continuous monitoring:
         │
         ├─→ GET /api/sandbox-status (health checks)
         ├─→ GET /api/sandbox-logs (retrieve logs)
         ├─→ GET /api/monitor-vite-logs (Vite-specific)
         └─→ HMRErrorDetector component (client-side)

┌─────────────────────────────────────────────────────────────────┐
│ 5. TERMINATION                                                   │
└─────────────────────────────────────────────────────────────────┘
    POST /api/kill-sandbox
         │
         ↓
    sandbox.terminate()
         │
         ├─→ Stop Vite dev server
         ├─→ Clean up processes
         ├─→ Close sandbox connection
         ├─→ Clear global.activeSandboxProvider
         └─→ Clear global.sandboxState

    Automatic cleanup:
         • Timeout after 15min (Vercel) or 30min (E2B)
         • On page unload (beforeunload event)
         • On navigation away from /generation
```

### Sandbox File System Structure

```
/home/user/project/
├── package.json                 # Dependencies & scripts
├── vite.config.ts              # Vite configuration
├── tsconfig.json               # TypeScript config
├── index.html                  # Entry HTML
├── src/
│   ├── main.tsx                # React entry point
│   ├── App.tsx                 # Main App component
│   ├── components/             # Generated components
│   ├── hooks/                  # Custom hooks
│   ├── utils/                  # Utility functions
│   └── styles/                 # CSS/styling
├── public/                     # Static assets
└── node_modules/               # Installed packages
```

### Provider Comparison

| Feature | E2B Provider | Vercel Provider |
|---------|--------------|-----------------|
| Runtime | E2B Code Interpreter | Vercel Sandbox |
| Default Port | 5173 (Vite) | 3000 (Node.js) |
| Timeout | 30 minutes | 15 minutes |
| Environment | Ubuntu | Node.js 22 |
| File System | Full Linux FS | Restricted |
| Command Execution | Full shell access | Limited commands |
| Package Installation | npm/yarn/pnpm | npm only |
| Use Case | Complex projects | Simple projects |
| Cost | E2B API credits | Vercel resources |

---

## State Management

### State Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│ CLIENT-SIDE STATE                                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────┐  ┌──────────────────┐  ┌────────────────┐│
│  │  React State    │  │  Jotai Atoms     │  │ Session Storage││
│  │  (useState)     │  │  (Global State)  │  │  (Persistence) ││
│  │                 │  │                  │  │                ││
│  │ • Local UI      │  │ • Sheet state    │  │ • targetUrl    ││
│  │ • Form inputs   │  │ • Modal state    │  │ • selectedStyle││
│  │ • Loading flags │  │ • Theme          │  │ • selectedModel││
│  └─────────────────┘  └──────────────────┘  └────────────────┘│
│                                                                  │
└────────────────────────────┬─────────────────────────────────────┘
                             │
                        API Bridge
                             │
                             ↓
┌─────────────────────────────────────────────────────────────────┐
│ SERVER-SIDE STATE (Node.js Global)                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  global.sandboxState = {                                         │
│    sandbox: SandboxProvider,      // Active provider instance   │
│    sandboxData: {                 // Connection info            │
│      sandboxId: string,                                          │
│      url: string                                                 │
│    },                                                            │
│    fileCache: {                   // Performance cache          │
│      files: Map<string, File>,                                   │
│      lastSync: timestamp                                         │
│    }                                                             │
│  }                                                               │
│                                                                  │
│  global.conversationState = {                                    │
│    conversationId: string,        // Unique session ID          │
│    startedAt: timestamp,                                         │
│    lastUpdated: timestamp,                                       │
│    context: {                     // AI context                 │
│      messages: [...],             // Chat history               │
│      edits: [...],                // Edit history               │
│      currentTopic: string,                                       │
│      projectEvolution: {          // Project changes            │
│        initialPrompt: string,                                    │
│        majorChanges: [...]                                       │
│      },                                                          │
│      userPreferences: {           // Learned preferences        │
│        stylePatterns: [...],                                     │
│        codingConventions: [...]                                  │
│      }                                                           │
│    }                                                             │
│  }                                                               │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Jotai Atom Store

**Location**: `/atoms/sheets.ts`

```typescript
import { atom } from 'jotai';

export type SheetType = 'settings' | 'help' | 'export' | null;

export interface SheetState {
  isOpen: boolean;
  type: SheetType;
}

export const sheetsAtom = atom<SheetState>({
  isOpen: false,
  type: null
});

// Usage in components
import { useAtom } from 'jotai';
import { sheetsAtom } from '@/atoms/sheets';

function Component() {
  const [sheets, setSheets] = useAtom(sheetsAtom);

  const openSettings = () => {
    setSheets({ isOpen: true, type: 'settings' });
  };
}
```

### Server State Persistence

**Why Global Variables?**

Node.js runs on a single process, so `global` variables persist across API requests within the same server instance. This is essential for:

1. **Sandbox Persistence**: Maintain sandbox connection across multiple API calls
2. **Performance**: Avoid recreating sandbox for each request
3. **Memory Efficiency**: Single sandbox instance per session
4. **State Continuity**: Preserve conversation context

**Considerations**:
- **Not suitable for serverless edge functions** (stateless)
- **Memory leaks**: Must clean up on termination
- **Horizontal scaling**: Sticky sessions required
- **Production**: Consider Redis or database for distributed systems

### State Synchronization Strategies

**1. Optimistic UI Updates**
```typescript
// Update UI immediately
setChatMessages([...chatMessages, userMessage]);

// Then send to server
fetch('/api/generate-ai-code-stream', {
  method: 'POST',
  body: JSON.stringify({ message: userMessage })
});

// Rollback if error
if (error) {
  setChatMessages(chatMessages.filter(m => m.id !== userMessage.id));
}
```

**2. Polling for Status**
```typescript
useEffect(() => {
  const interval = setInterval(async () => {
    const status = await fetch('/api/sandbox-status').then(r => r.json());
    setSandboxStatus(status);
  }, 5000);

  return () => clearInterval(interval);
}, []);
```

**3. Server-Sent Events for Real-time Updates**
```typescript
const eventSource = new EventSource('/api/generate-ai-code-stream');

eventSource.onmessage = (event) => {
  const data = JSON.parse(event.data);
  setGeneratedCode(prev => prev + data.token);
};
```

---

## Security Architecture

### Threat Model

**1. Code Injection Risks**
- **Threat**: Malicious AI-generated code
- **Mitigation**: Sandbox isolation (E2B/Vercel)
- **Status**: ✅ Implemented

**2. API Key Exposure**
- **Threat**: Keys leaked in client code
- **Mitigation**: Server-side API calls only
- **Status**: ✅ Implemented

**3. XSS Attacks**
- **Threat**: Cross-site scripting via user input
- **Mitigation**: React's built-in XSS protection
- **Status**: ✅ Implemented (React auto-escaping)

**4. SSRF Attacks**
- **Threat**: Server-side request forgery via scraping
- **Mitigation**: Firecrawl handles scraping (3rd party)
- **Status**: ⚠️ Rely on Firecrawl security

**5. Unauthorized Access**
- **Threat**: Unauthenticated API access
- **Mitigation**: None currently (dev mode)
- **Status**: ❌ Not implemented (TODO for production)

### Security Layers

```
┌─────────────────────────────────────────────────────────────────┐
│ LAYER 1: Network Security                                        │
├─────────────────────────────────────────────────────────────────┤
│ • HTTPS/TLS encryption                                           │
│ • CORS middleware                                                │
│ • Rate limiting (recommended)                                    │
│ • DDoS protection (via hosting provider)                         │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│ LAYER 2: Application Security                                    │
├─────────────────────────────────────────────────────────────────┤
│ • Input validation (zod schemas)                                 │
│ • Output encoding (React auto-escape)                            │
│ • Error handling (no sensitive data in responses)                │
│ • Environment variable isolation                                 │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│ LAYER 3: Sandbox Isolation                                       │
├─────────────────────────────────────────────────────────────────┤
│ • E2B: Isolated container per sandbox                            │
│ • Vercel: Isolated runtime environment                           │
│ • Limited file system access                                     │
│ • Process isolation                                              │
│ • Automatic timeout & cleanup                                    │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│ LAYER 4: Data Security                                           │
├─────────────────────────────────────────────────────────────────┤
│ • No persistent storage (stateless)                              │
│ • Session data in memory only                                    │
│ • Automatic cleanup on timeout                                   │
│ • No user data collection                                        │
└─────────────────────────────────────────────────────────────────┘
```

### Environment Variable Security

**Secure Configuration:**

```bash
# .env.local (never committed)
FIRECRAWL_API_KEY=fc-xxxxxxxxxxxxx
ANTHROPIC_API_KEY=sk-ant-xxxxxxxxxxxxx
OPENAI_API_KEY=sk-xxxxxxxxxxxxx
GEMINI_API_KEY=xxxxxxxxxxxxx
GROQ_API_KEY=gsk_xxxxxxxxxxxxx

# Vercel (auto-generated, time-limited)
VERCEL_OIDC_TOKEN=xxxxxxxxxxxxx

# E2B (if using E2B provider)
E2B_API_KEY=e2b_xxxxxxxxxxxxx
```

**Access Pattern:**
```typescript
// Server-side only
const apiKey = process.env.ANTHROPIC_API_KEY;
if (!apiKey) {
  throw new Error('Missing API key');
}

// Never expose to client
// ❌ BAD: const apiKey = process.env.NEXT_PUBLIC_API_KEY;
// ✅ GOOD: Keep without NEXT_PUBLIC_ prefix
```

### Recommended Production Enhancements

**1. Authentication & Authorization**
```typescript
// Implement NextAuth.js or similar
import { getServerSession } from 'next-auth';

export async function POST(req: Request) {
  const session = await getServerSession();
  if (!session) {
    return new Response('Unauthorized', { status: 401 });
  }
  // ... proceed with authenticated request
}
```

**2. Rate Limiting**
```typescript
import { Ratelimit } from '@upstash/ratelimit';

const ratelimit = new Ratelimit({
  redis: Redis.fromEnv(),
  limiter: Ratelimit.slidingWindow(10, '10 s'),
});

export async function POST(req: Request) {
  const ip = req.headers.get('x-forwarded-for');
  const { success } = await ratelimit.limit(ip);
  if (!success) {
    return new Response('Too many requests', { status: 429 });
  }
  // ... proceed
}
```

**3. Input Sanitization**
```typescript
import { z } from 'zod';

const schema = z.object({
  url: z.string().url(),
  style: z.enum(['modern', 'minimal', 'brutalism']),
  model: z.string()
});

export async function POST(req: Request) {
  const body = await req.json();
  const validated = schema.parse(body); // Throws if invalid
  // ... use validated data
}
```

---

## Performance Optimization

### Frontend Optimizations

**1. Code Splitting**
```typescript
// Lazy load heavy components
const Playground = dynamic(() => import('@/components/shared/Playground'), {
  ssr: false,
  loading: () => <LoadingSpinner />
});
```

**2. Image Optimization**
```tsx
import Image from 'next/image';

<Image
  src="/hero-bg.png"
  alt="Hero"
  width={1920}
  height={1080}
  priority // For above-the-fold images
/>
```

**3. Font Optimization**
```typescript
// app/layout.tsx
import { GeistSans, GeistMono } from 'geist/font';

export default function Layout({ children }) {
  return (
    <html className={`${GeistSans.variable} ${GeistMono.variable}`}>
      {children}
    </html>
  );
}
```

**4. React Server Components**
```typescript
// Server component (default in App Router)
async function ServerComponent() {
  const data = await fetchData(); // No client-side fetch
  return <div>{data}</div>;
}

// Client component (only when needed)
'use client';
function ClientComponent() {
  const [state, setState] = useState();
  return <button onClick={() => setState(...)}>Click</button>;
}
```

**5. Debounced Callbacks**
```typescript
import { useDebouncedCallback } from '@/hooks/useDebouncedCallback';

const handleSearch = useDebouncedCallback((query: string) => {
  performSearch(query);
}, 300); // 300ms delay
```

### Backend Optimizations

**1. File Caching**
```typescript
global.sandboxState = {
  fileCache: {
    files: new Map(),
    lastSync: Date.now()
  }
};

// Avoid re-reading files
if (cache.has(filePath) && Date.now() - lastSync < 10000) {
  return cache.get(filePath);
}
```

**2. Streaming Responses**
```typescript
// Stream AI responses as they arrive
const stream = new ReadableStream({
  async start(controller) {
    for await (const chunk of aiStream) {
      controller.enqueue(encoder.encode(JSON.stringify(chunk)));
    }
  }
});
```

**3. Parallel Package Installation**
```typescript
// Install multiple packages concurrently
await Promise.all([
  sandbox.runCommand('npm install react react-dom'),
  sandbox.runCommand('npm install -D vite @vitejs/plugin-react')
]);
```

**4. Intelligent Context Selection**
```typescript
// Only send relevant files to AI
const relevantFiles = await selectRelevantFiles(
  userMessage,
  allFiles,
  maxTokens: 8000
);
```

### Sandbox Performance

**1. Vite Dev Server**
- **Fast HMR**: Hot Module Replacement for instant updates
- **ES Modules**: Native browser modules (no bundling in dev)
- **Optimized**: Pre-bundled dependencies

**2. Lazy Package Installation**
```typescript
// Only install when detected in code
const newPackages = detectPackages(generatedCode);
if (newPackages.length > 0) {
  await installPackages(newPackages);
}
```

**3. Build Caching**
```typescript
// Vite caches node_modules/.vite/
// Speeds up subsequent builds
```

### Monitoring & Metrics

**Current Monitoring**:
- Sandbox health checks (`/api/sandbox-status`)
- Vite error detection (`HMRErrorDetector`)
- Log monitoring (`/api/sandbox-logs`)

**Recommended Additions**:
```typescript
// Performance monitoring
import { Analytics } from '@vercel/analytics';

<Analytics />

// Error tracking
import * as Sentry from '@sentry/nextjs';

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  tracesSampleRate: 1.0,
});
```

### Performance Benchmarks

| Operation | Target | Actual |
|-----------|--------|--------|
| Page Load (First Paint) | < 1s | ~800ms |
| Sandbox Creation | < 10s | ~5-8s |
| Code Generation Start | < 2s | ~1-2s |
| Code Application | < 5s | ~3-5s |
| Package Installation | < 30s | ~10-20s |
| Hot Reload | < 1s | ~200-500ms |

---

## Deployment Architecture

### Deployment Options

**1. Vercel (Recommended)**

```bash
# Install Vercel CLI
npm i -g vercel

# Link project
vercel link

# Deploy
vercel --prod
```

**Architecture**:
```
┌─────────────────────────────────────────────┐
│ Vercel Edge Network (CDN)                   │
│  • Global distribution                      │
│  • Static asset caching                     │
│  • SSL/TLS termination                      │
└──────────────────┬──────────────────────────┘
                   │
                   ↓
┌─────────────────────────────────────────────┐
│ Vercel Serverless Functions (US-East)      │
│  • Next.js App Router                       │
│  • API Routes                               │
│  • Server-side rendering                    │
└──────────────────┬──────────────────────────┘
                   │
    ┌──────────────┼──────────────┐
    ↓              ↓              ↓
┌────────┐  ┌──────────┐  ┌──────────────┐
│ AI APIs│  │Firecrawl │  │ Sandbox Prvdr│
└────────┘  └──────────┘  └──────────────┘
```

**Configuration**:
```json
// vercel.json
{
  "functions": {
    "app/api/**/*.ts": {
      "maxDuration": 60
    }
  },
  "env": {
    "SANDBOX_PROVIDER": "vercel"
  }
}
```

**2. Docker (Self-Hosted)**

```dockerfile
# Dockerfile
FROM node:22-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

EXPOSE 3000

CMD ["npm", "start"]
```

```yaml
# docker-compose.yml
version: '3.8'
services:
  open-lovable:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - SANDBOX_PROVIDER=e2b
    env_file:
      - .env.local
```

**3. Traditional Node.js Server**

```bash
# Install dependencies
npm ci

# Build
npm run build

# Start with PM2
npm i -g pm2
pm2 start npm --name "open-lovable" -- start
pm2 save
pm2 startup
```

### Environment Configuration

**Development**:
```bash
# .env.local
NODE_ENV=development
SANDBOX_PROVIDER=vercel
VERCEL_OIDC_TOKEN=<auto-generated>
```

**Production**:
```bash
# .env.production
NODE_ENV=production
SANDBOX_PROVIDER=vercel # or 'e2b'

# Required
FIRECRAWL_API_KEY=fc-xxxxx
ANTHROPIC_API_KEY=sk-ant-xxxxx

# Optional (at least one AI provider)
OPENAI_API_KEY=sk-xxxxx
GEMINI_API_KEY=xxxxx
GROQ_API_KEY=gsk_xxxxx
```

### Scaling Considerations

**Horizontal Scaling Challenges**:

1. **Global State**: `global.*` variables don't work across instances
   - **Solution**: Use Redis or database for shared state
   ```typescript
   // Replace global.sandboxState with Redis
   import { Redis } from '@upstash/redis';
   const redis = Redis.fromEnv();
   await redis.set('sandbox:state', sandboxState);
   ```

2. **Sticky Sessions**: Ensure requests go to same instance
   - **Solution**: Use load balancer with session affinity
   ```nginx
   # nginx.conf
   upstream backend {
     ip_hash; # Sticky sessions
     server server1.example.com;
     server server2.example.com;
   }
   ```

3. **Sandbox Persistence**: Sandboxes tied to server instance
   - **Solution**: Store sandbox connection info in Redis
   - Reconnect to sandbox from any instance

**Recommended Architecture for Scale**:

```
                    ┌─────────────┐
                    │ Load Balancer│
                    └──────┬──────┘
                           │
           ┌───────────────┼───────────────┐
           ↓               ↓               ↓
    ┌──────────┐    ┌──────────┐    ┌──────────┐
    │ Instance 1│    │ Instance 2│    │ Instance 3│
    └─────┬────┘    └─────┬────┘    └─────┬────┘
          │               │               │
          └───────────────┼───────────────┘
                          ↓
                   ┌─────────────┐
                   │ Redis Cache │
                   │ • Sessions  │
                   │ • State     │
                   └─────────────┘
```

### CI/CD Pipeline

**GitHub Actions Example**:

```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '22'

      - name: Install dependencies
        run: npm ci

      - name: Run linter
        run: npm run lint

      - name: Build
        run: npm run build

      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          vercel-args: '--prod'
```

### Monitoring & Observability

**Production Monitoring Stack**:

1. **Application Monitoring**: Vercel Analytics
2. **Error Tracking**: Sentry
3. **Logging**: Vercel Logs or Datadog
4. **Uptime Monitoring**: Pingdom or UptimeRobot
5. **Performance**: Lighthouse CI

```typescript
// lib/monitoring.ts
import * as Sentry from '@sentry/nextjs';
import { Analytics } from '@vercel/analytics';

export function initMonitoring() {
  if (process.env.NODE_ENV === 'production') {
    Sentry.init({
      dsn: process.env.SENTRY_DSN,
      tracesSampleRate: 1.0,
    });
  }
}

// app/layout.tsx
<Analytics />
```

---

## Conclusion

This technical architecture document provides a comprehensive overview of the Open Lovable platform. The system is built with modern web technologies, follows best practices, and is designed for scalability and maintainability.

### Key Architectural Strengths

1. ✅ **Modular Design**: Clear separation of concerns
2. ✅ **Provider Pattern**: Flexible sandbox implementations
3. ✅ **Streaming Architecture**: Real-time user feedback
4. ✅ **Type Safety**: Full TypeScript coverage
5. ✅ **Modern Stack**: Latest Next.js, React, and tooling

### Future Enhancement Opportunities

1. 🔧 **Authentication**: Implement user accounts
2. 🔧 **Distributed State**: Redis for horizontal scaling
3. 🔧 **Testing**: Add comprehensive test suite
4. 🔧 **Monitoring**: Enhanced observability
5. 🔧 **Rate Limiting**: Production-ready API protection

### Architecture Decision Records (ADRs)

**ADR-001: Next.js App Router**
- **Decision**: Use Next.js 15 App Router over Pages Router
- **Rationale**: React Server Components, improved performance, modern patterns

**ADR-002: Dual Sandbox Providers**
- **Decision**: Support both E2B and Vercel sandboxes
- **Rationale**: Flexibility, redundancy, cost optimization

**ADR-003: Global State for Sandboxes**
- **Decision**: Use Node.js global variables for sandbox state
- **Rationale**: Simplicity, performance, sufficient for single-instance deployment

**ADR-004: Streaming AI Responses**
- **Decision**: Server-Sent Events for real-time AI streaming
- **Rationale**: Better UX, progressive enhancement, no WebSocket complexity

**ADR-005: Tailwind CSS + Custom Design System**
- **Decision**: Combine Tailwind utilities with custom design tokens
- **Rationale**: Rapid development + brand consistency

---

**Document Version**: 1.0
**Last Updated**: 2025-11-14
**Maintained By**: Development Team
