# Browser-Only Version - Architecture Plan

## Overview

Building a **simplified, browser-only version** of Open Lovable that runs entirely in the client without backend servers, sandboxes, or complex build systems.

## Key Differences from Full Version

### What We're Removing ❌

1. **Backend Server** - No Next.js API routes, no Node.js server
2. **External Sandboxes** - No Vercel Sandbox or E2B
3. **Web Scraping** - No Firecrawl (CORS limitations)
4. **Complex Build System** - No Vite, no npm in sandboxes
5. **Server-Side Rendering** - Pure client-side rendering
6. **Package Installation** - Only use CDN packages

### What We're Keeping ✅

1. **AI Code Generation** - Direct API calls from browser
2. **Live Preview** - Iframe-based code execution
3. **Chat Interface** - User can iterate on code
4. **Real-time Updates** - Streaming AI responses
5. **Code Display** - Syntax highlighting

### What We're Simplifying 🔧

1. **Single-File Apps** - Generate complete HTML files with inline React
2. **CDN Dependencies** - Use React/libraries from CDN (unpkg, esm.sh)
3. **No Build Step** - Code runs directly in iframe
4. **localStorage State** - Store conversation and code in browser

---

## Architecture Comparison

### Current (Full-Stack)

```
Browser → Next.js Server → AI API → Sandbox → Live Preview
           ↓                           ↓
       API Routes               File System + npm
```

### New (Browser-Only)

```
Browser → AI API (direct) → Generate HTML → Iframe Preview
   ↓                              ↓
localStorage                  No files, runs in memory
```

---

## Simplified Tech Stack

### Core Framework
- **Vite + React** (simple SPA)
- **TypeScript** (optional, can use plain JS)
- **Tailwind CSS** (styling)

### AI Integration
- **Direct API Calls** from browser
- **Streaming** via fetch API
- **API Key** stored in browser (localStorage)
- **CORS Proxy** if needed for some providers

### Code Execution
- **Iframe with srcdoc** - Run generated code
- **No bundling** - Plain HTML/JS/CSS
- **CDN imports** - React from unpkg/esm.sh
- **No npm** - Only CDN packages

### State Management
- **React useState** - Local component state
- **localStorage** - Persist conversation
- **sessionStorage** - Temporary data

### No Backend Needed
- Pure static site
- Deploy to GitHub Pages, Netlify, Vercel (static)
- No server costs

---

## Implementation Plan

### Phase 1: Basic Setup

**Create Simple Vite App:**
```bash
npm create vite@latest open-lovable-browser -- --template react-ts
cd open-lovable-browser
npm install
npm install tailwindcss autoprefixer postcss
npm install @anthropic-ai/sdk
```

**Project Structure:**
```
open-lovable-browser/
├── src/
│   ├── App.tsx              # Main app
│   ├── components/
│   │   ├── ChatInterface.tsx    # AI chat
│   │   ├── CodePreview.tsx      # Iframe preview
│   │   ├── ApiKeyInput.tsx      # API key management
│   │   └── CodeEditor.tsx       # Optional code viewer
│   ├── lib/
│   │   ├── ai-client.ts         # Direct AI API calls
│   │   ├── code-generator.ts    # Generate HTML from AI
│   │   └── storage.ts           # localStorage utils
│   └── main.tsx
├── index.html
├── package.json
└── vite.config.ts
```

### Phase 2: AI Integration

**Direct API Calls (Client-Side):**

```typescript
// lib/ai-client.ts
import Anthropic from '@anthropic-ai/sdk';

export async function generateCode(prompt: string, apiKey: string) {
  const client = new Anthropic({
    apiKey,
    dangerouslyAllowBrowser: true // Enable browser usage
  });

  const stream = await client.messages.stream({
    model: 'claude-sonnet-4',
    max_tokens: 4000,
    messages: [{
      role: 'user',
      content: `Generate a complete HTML file with inline React (via CDN) for: ${prompt}

      Requirements:
      - Single HTML file
      - Use React from CDN (https://esm.sh/react, https://esm.sh/react-dom)
      - Include all CSS inline
      - No build step required
      - Should work directly in browser`
    }]
  });

  return stream;
}
```

**Alternative: Use Fetch for Any Provider:**

```typescript
// lib/ai-client.ts
export async function streamAI(prompt: string, apiKey: string) {
  const response = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'x-api-key': apiKey,
      'anthropic-version': '2023-06-01'
    },
    body: JSON.stringify({
      model: 'claude-sonnet-4',
      max_tokens: 4000,
      messages: [{ role: 'user', content: prompt }],
      stream: true
    })
  });

  const reader = response.body?.getReader();
  return reader;
}
```

### Phase 3: In-Browser Code Execution

**Iframe Preview Component:**

```typescript
// components/CodePreview.tsx
import { useEffect, useRef } from 'react';

interface CodePreviewProps {
  html: string;
}

export function CodePreview({ html }: CodePreviewProps) {
  const iframeRef = useRef<HTMLIFrameElement>(null);

  useEffect(() => {
    if (iframeRef.current) {
      // Set iframe content directly
      iframeRef.current.srcdoc = html;
    }
  }, [html]);

  return (
    <iframe
      ref={iframeRef}
      className="w-full h-full border-0"
      sandbox="allow-scripts allow-same-origin"
      title="Preview"
    />
  );
}
```

**Generated HTML Template:**

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Generated App</title>
  <script type="importmap">
    {
      "imports": {
        "react": "https://esm.sh/react@18",
        "react-dom/client": "https://esm.sh/react-dom@18/client"
      }
    }
  </script>
  <style>
    /* Inline CSS here */
  </style>
</head>
<body>
  <div id="root"></div>

  <script type="module">
    import React from 'react';
    import ReactDOM from 'react-dom/client';

    // Generated React component
    function App() {
      return (
        <div>
          {/* Generated UI */}
        </div>
      );
    }

    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(<App />);
  </script>
</body>
</html>
```

### Phase 4: Chat Interface

**Simple Chat UI:**

```typescript
// components/ChatInterface.tsx
import { useState } from 'react';
import { generateCode } from '../lib/ai-client';

export function ChatInterface({ onCodeGenerated }) {
  const [messages, setMessages] = useState([]);
  const [input, setInput] = useState('');
  const [isGenerating, setIsGenerating] = useState(false);
  const [apiKey, setApiKey] = useState(
    localStorage.getItem('anthropic_api_key') || ''
  );

  const handleSubmit = async (e) => {
    e.preventDefault();
    if (!input.trim() || !apiKey) return;

    // Add user message
    setMessages(prev => [...prev, { role: 'user', content: input }]);
    setIsGenerating(true);

    try {
      let generatedCode = '';
      const stream = await generateCode(input, apiKey);

      for await (const chunk of stream) {
        if (chunk.type === 'content_block_delta') {
          generatedCode += chunk.delta.text;
          onCodeGenerated(generatedCode); // Update preview in real-time
        }
      }

      setMessages(prev => [...prev, {
        role: 'assistant',
        content: generatedCode
      }]);
    } catch (error) {
      console.error('Generation error:', error);
    } finally {
      setIsGenerating(false);
      setInput('');
    }
  };

  return (
    <div className="flex flex-col h-full">
      {/* API Key Input */}
      {!apiKey && (
        <div className="p-4 bg-yellow-50 border-b">
          <input
            type="password"
            placeholder="Enter Anthropic API Key"
            className="w-full px-4 py-2 border rounded"
            onChange={(e) => {
              setApiKey(e.target.value);
              localStorage.setItem('anthropic_api_key', e.target.value);
            }}
          />
        </div>
      )}

      {/* Messages */}
      <div className="flex-1 overflow-y-auto p-4 space-y-4">
        {messages.map((msg, i) => (
          <div
            key={i}
            className={`p-4 rounded ${
              msg.role === 'user' ? 'bg-blue-50' : 'bg-gray-50'
            }`}
          >
            <div className="font-semibold mb-2">
              {msg.role === 'user' ? 'You' : 'AI'}
            </div>
            <div className="whitespace-pre-wrap">{msg.content}</div>
          </div>
        ))}
      </div>

      {/* Input */}
      <form onSubmit={handleSubmit} className="p-4 border-t">
        <input
          type="text"
          value={input}
          onChange={(e) => setInput(e.target.value)}
          placeholder="Describe what you want to build..."
          className="w-full px-4 py-2 border rounded"
          disabled={isGenerating || !apiKey}
        />
      </form>
    </div>
  );
}
```

### Phase 5: Main App

**App.tsx:**

```typescript
// App.tsx
import { useState } from 'react';
import { ChatInterface } from './components/ChatInterface';
import { CodePreview } from './components/CodePreview';

export default function App() {
  const [generatedHTML, setGeneratedHTML] = useState('');

  return (
    <div className="flex h-screen">
      {/* Sidebar - Chat */}
      <div className="w-96 border-r flex flex-col">
        <div className="p-4 border-b">
          <h1 className="text-xl font-bold">Open Lovable Browser</h1>
          <p className="text-sm text-gray-600">Generate apps in your browser</p>
        </div>
        <ChatInterface onCodeGenerated={setGeneratedHTML} />
      </div>

      {/* Main - Preview */}
      <div className="flex-1 flex flex-col">
        <div className="p-4 border-b">
          <h2 className="font-semibold">Live Preview</h2>
        </div>
        <div className="flex-1">
          {generatedHTML ? (
            <CodePreview html={generatedHTML} />
          ) : (
            <div className="flex items-center justify-center h-full text-gray-400">
              Generate code to see preview
            </div>
          )}
        </div>
      </div>
    </div>
  );
}
```

---

## Security Considerations

### API Key Storage

**Problem**: API keys in browser are exposed

**Solutions**:
1. **User provides their own key** (like ChatGPT web)
   - Store in localStorage
   - Clear warning about key security
   - Never commit to GitHub

2. **Use CORS Proxy** (advanced)
   - Create simple backend proxy just for API calls
   - Keep keys server-side
   - Free hosting on Cloudflare Workers

3. **Use OpenAI/Anthropic browser SDKs**
   - Some providers support browser usage
   - Add `dangerouslyAllowBrowser: true`

### Iframe Sandboxing

```html
<iframe
  sandbox="allow-scripts allow-same-origin allow-forms"
  <!-- Limits what code can do -->
/>
```

---

## Advantages of Browser-Only Version

✅ **No Server Costs** - Pure static hosting
✅ **Instant Deploy** - Deploy anywhere (GitHub Pages, Netlify)
✅ **Offline Capable** - Can work offline with localStorage
✅ **Fast** - No server round trips
✅ **Simple** - Much easier to understand and modify
✅ **Privacy** - All code stays in browser

---

## Limitations

❌ **API Key Exposure** - Users must provide their own keys
❌ **No Web Scraping** - CORS prevents scraping other sites
❌ **Simple Apps Only** - Can't install npm packages
❌ **No Backend** - Can't save projects to database
❌ **CORS Issues** - Some AI providers may block browser requests

---

## Deployment

### GitHub Pages (Free)

```bash
# Build
npm run build

# Deploy
npm install -D gh-pages
npx gh-pages -d dist
```

### Netlify (Free)

```bash
# Build
npm run build

# Deploy
netlify deploy --prod --dir=dist
```

### Vercel (Free)

```bash
vercel --prod
```

---

## Next Steps

1. ✅ Create Vite React app
2. ✅ Add Tailwind CSS
3. ✅ Implement AI streaming client
4. ✅ Build chat interface
5. ✅ Implement iframe preview
6. ✅ Add localStorage for persistence
7. ✅ Test and refine
8. ✅ Deploy to static hosting

---

## Example Prompts for Testing

**Simple Counter:**
```
Create a counter app with increment and decrement buttons
```

**Todo List:**
```
Create a todo list app with add, delete, and mark complete
```

**Color Picker:**
```
Create a color picker that shows the selected color and hex code
```

**Calculator:**
```
Create a basic calculator with +, -, *, / operations
```

---

## Estimated Complexity

- **Time to Build**: 1-2 days
- **Lines of Code**: ~500-1000 (vs 10,000+ for full version)
- **Dependencies**: ~10 packages (vs 89 for full version)
- **Deployment**: 5 minutes (vs hours for full stack)

---

**This is a much simpler, more accessible version perfect for learning, demos, or quick prototypes!**
