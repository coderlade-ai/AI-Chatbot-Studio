# Claude Studio — Single-File AI Chat & Artifacts Workspace

> A standalone, single-file web chat client built with an editorial cream aesthetic, persistent threads, live Artifact previews, and optional Anthropic API integration.

_Assumptions: Built as a zero-dependency, single-file HTML application running directly in modern web browsers with optional Anthropic API key support._

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Installation & Quickstart](#installation--quickstart)
- [Configuration & API Setup](#configuration--api-setup)
- [Design Tokens & Theme System](#design-tokens--theme-system)
- [Usage Workflow](#usage-workflow)
- [Common Issues & Troubleshooting](#common-issues--troubleshooting)
- [Project Structure](#project-structure)
- [License](#license)

---

## Overview

Claude Studio is an implementation of Anthropic's editorial design system packaged into a single, self-contained HTML file. It provides an AI chat workspace featuring conversation memory, side-by-side live execution of code artifacts, collapsible model reasoning disclosures, voice dictation, and speech synthesis.

It runs locally with zero installation overhead—usable either offline via an embedded typewriter simulation engine or online by connecting directly to Anthropic's Claude API.

---

## Features

- **Editorial Design System**: Implements the warm cream canvas (`#faf9f5`), serif display headlines (Cormorant Garamond 400 with negative tracking), coral accents (`#cc785c`), and dark product chrome (`#181715`).
- **Interactive Artifacts Sandbox**: Split-screen execution panel for HTML, SVG, and Canvas snippets inside an isolated `iframe`, with tabbed toggling between live preview and raw code.
- **Extended Reasoning / Thinking Disclosures**: Collapsible reflection cards with status indicators modeled after hybrid thinking systems.
- **Local Persistence**: Stores conversation threads, system prompts, active models, and configurations in browser `localStorage`.
- **Thread Management**: Supports multi-session management, pinned conversations, title editing, real-time search, and thread deletion.
- **Voice Integration**: Hands-free dictation using the Web Speech Recognition API and text-to-speech audio readout via the SpeechSynthesis API.
- **Markdown & Syntax Highlighting**: Renders GitHub-flavored Markdown and syntax-highlighted code blocks with language indicators and one-click copy buttons.
- **Hybrid Inference Engine**: Works out-of-the-box offline with realistic simulated streaming, or connects directly to the Anthropic Messages API (`claude-3-7-sonnet`, `claude-3-5-opus`, `claude-3-5-haiku`).
- **One-Click Export**: Downloads conversation threads as formatted Markdown files.

---

## Tech Stack

| Layer | Technology | Delivery |
|---|---|---|
| Structure | HTML5 (Semantic Elements) | Single-file architecture |
| Styling | Pure CSS3 (Custom Properties, Flexbox, Grid) | Inline `<style>` block |
| Runtime Logic | Vanilla JavaScript (ES2020+) | Inline `<script>` block |
| Storage | Browser `localStorage` API | Client-side persistence |
| Voice & Audio | Web Speech API (`SpeechRecognition`, `SpeechSynthesis`) | Browser native |
| Markdown Parser | `marked.js` (v12+) | CDN (`cdn.jsdelivr.net`) |
| Syntax Highlighter | `highlight.js` (v11.9) | CDN (`cdnjs.cloudflare.com`) |
| Typography | Google Fonts (`Cormorant Garamond`, `Inter`, `JetBrains Mono`) | External web fonts |

---

## Prerequisites

- Modern desktop web browser: Google Chrome, Brave, Microsoft Edge, Firefox, or Safari.
  - *Note: Speech-to-text dictation requires Chromium-based browsers that support `webkitSpeechRecognition`.*
- (Optional) An Anthropic API key starting with `sk-ant-` if connecting to live Claude models.

---

## Installation & Quickstart

Because the entire application is contained in a single `index.html` file, no build tools, compilers, or package managers are required.

### Method 1: Direct File Execution

Open the file directly from your local filesystem:

```bash
# Clone the repository
git clone [https://github.com/your-username/claude-studio.git](https://github.com/your-username/claude-studio.git)
cd claude-studio

# Open index.html in your default browser:
# macOS:
open index.html

# Linux:
xdg-open index.html

# Windows:
start index.html
```

### Method 2: Local Static Web Server

Serving the file over a local HTTP server is recommended to prevent local file protocol (`file:///`) security restrictions on clipboard and iframe rendering:

```bash
# Using Python 3
python3 -m http.server 8000

# Or using Node.js / npx
npx serve .
```

Then visit `http://localhost:8000` in your browser.

---

## Configuration & API Setup

### Offline Simulation Mode (Default)

The studio operates by default with no API key. Prompts requesting code, system architecture, or literary prose trigger the built-in streaming simulation engine and render interactive Artifacts automatically.

### Live Anthropic API Connection

To connect directly to official Anthropic models:

1. Click **Model & Keys** in the lower-left sidebar footer.
2. In the modal:
   - **System Directive / Persona**: Set the global system instructions.
   - **Model Selection**: Choose between `Claude 3.7 Sonnet`, `Claude 3.5 Opus`, or `Claude 3.5 Haiku`.
   - **Anthropic API Key**: Enter your key in the format `sk-ant-api03-...`.
3. Click **Save Preferences**. Keys are retained only in your local browser storage (`localStorage`).

```
[Browser Client] ---> [Anthropic API: [https://api.anthropic.com/v1/messages](https://api.anthropic.com/v1/messages)]
  Headers:
    x-api-key: sk-ant-...
    anthropic-version: 2023-06-01
    anthropic-dangerous-direct-browser-access: true
```

---

## Design Tokens & Theme System

The interface adheres to the following core design tokens:

| Token | Hex / Value | Purpose |
|---|---|---|
| `--colors-canvas` | `#faf9f5` | Main viewport and floor background (warm cream) |
| `--colors-primary` | `#cc785c` | Signature warm coral accent and primary CTAs |
| `--colors-primary-active` | `#a9583e` | Hover / active pressed button state |
| `--colors-surface-card` | `#efe9de` | Light cream cards and user bubbles |
| `--colors-surface-dark` | `#181715` | Dark navy product chrome, code blocks, artifacts |
| `--colors-surface-dark-soft` | `#1f1e1b` | Code block inner background |
| `--colors-ink` | `#141413` | Headings and primary text |
| `--colors-muted` | `#6c6a64` | Subheadings and secondary labels |
| `--font-display` | `Cormorant Garamond` | Weight 400 headlines with negative tracking (`-0.025em`) |
| `--font-sans` | `Inter` | Humanist body text, UI labels, buttons |
| `--font-mono` | `JetBrains Mono` | Code blocks and terminal text |

---

## Usage Workflow

### 1. Working with Artifacts

When Claude generates code snippets containing complete HTML, SVG, or Canvas markup:
1. An **Open Artifact** button appears in the dark code card header.
2. Clicking it opens the 480px side drawer.
3. Use the **Preview** tab to interact with the rendered application inside the sandbox.
4. Use the **Code** tab to review the raw syntax.
5. Click the copy icon in the artifact bar to copy the full artifact code.

### 2. Dictation & Audio Readout

- **Speech-to-Text**: Click the microphone icon inside the input bar. Speak your prompt, and speech will automatically transcribe into the input area. Click again to stop.
- **Text-to-Speech**: Click the **Read** button below any assistant message to trigger browser speech synthesis.

### 3. Thread Organization & Export

- **New Chat**: Click **Start new chat** at the top of the sidebar.
- **Search**: Type into the search input to filter threads by title.
- **Pinning**: Click the pin icon on any thread to lock it to the top "Pinned" section.
- **Exporting**: Click **Export** in the sidebar footer to download a clean `.md` transcript of the active chat.

---

## Common Issues & Troubleshooting

### 1. Voice dictation button does not activate
- **Cause**: Speech recognition relies on `window.SpeechRecognition` or `window.webkitSpeechRecognition`, which is unavailable in standard desktop Firefox or when microphone access is blocked.
- **Fix**: Open the application in Google Chrome, Brave, or Microsoft Edge. Ensure microphone permission is granted in browser site settings (`chrome://settings/content/microphone`).

### 2. Direct Anthropic API requests fail with Network Error / CORS
- **Cause**: Opening the file via `file://` causes browser security policies to block outbound cross-origin `fetch` requests.
- **Fix**: Serve the application via local HTTP:
  ```bash
  python3 -m http.server 8000
  ```
  Ensure your API key has active credits and is entered without leading/trailing whitespace.

### 3. Artifact sandbox content fails to render or throws errors
- **Cause**: The artifact frame enforces `sandbox="allow-scripts allow-modals"`. Any script attempting to access parent frames (`window.parent`, `window.top`) or load unencrypted HTTP assets is blocked.
- **Fix**: Use self-contained HTML/CSS/JS or load external dependencies exclusively over secure HTTPS CDN links.

### 4. Conversations reset after closing the browser
- **Cause**: Chat history is persisted in `localStorage` under the key `claude_studio_state_v2`. Incognito/Private browsing windows clear this data upon closing.
- **Fix**: Use a standard browsing session for persistent storage, and use the **Export** button to create Markdown backups of important sessions.

---

## Project Structure

```
claude-studio/
├── index.html        # Unified single-file application (HTML5, CSS3, ES6 JS)
└── README.md         # Technical documentation and setup instructions
```

---

## License

This project is licensed under the MIT License. See `LICENSE` for details.
