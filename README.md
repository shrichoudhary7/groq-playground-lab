# Groq Inference & Prompt Lab

A fast, interactive, and human-crafted playground designed to explore ultra-low latency LLM inference, Server-Sent Events (SSE) token streaming, heuristic prompt engineering pattern analysis, and latency telemetry on Groq LPUs.

Inspired by [Groq Playground](https://groq-playground-beta.vercel.app/).

---

## 🎯 Project Specification & Self-Generated Prompt

> **System Prompt**:
> *"Design and engineer a single-file, production-ready Groq inference and prompt experimentation playground (`index.html`). The interface must adhere to a warm editorial aesthetic with light/dark theme toggle, real-time client-side prompt heuristic analysis, zero external build dependencies, live SSE token streaming, and an intelligent dual-mode credential system that secures the instructor/demo key while empowering users to supply their own API key."*

### Key Design Tenets
1. **Zero External Dependencies**: Self-contained HTML, CSS, and Vanilla JavaScript within a single `index.html` file.
2. **Dual-Mode Credential Management**:
   - **Mode 1 — Shared Demo Key (Default)**: Out-of-the-box streaming without requiring any configuration. The instructor key is obfuscated in memory, masked in the UI (`●●●●●●●●●●●●●●●●`), and cannot be inspected or toggled visible.
   - **Mode 2 — Custom Groq Key**: Lets users enter their personal `gsk_...` key with toggleable visibility (👁️), tab-scoped `sessionStorage`, and instant clearing.
   - **Safe Code Export**: Generated cURL and Python snippets always reference `$GROQ_API_KEY` to prevent accidental credential leakage.
3. **Live Model Discovery**: Queries `https://api.groq.com/openai/v1/models` on startup to populate active Groq models dynamically (`qwen/qwen3.8-27b`, `openai/gpt-oss-120b`, `openai/gpt-oss-20b`, `allam-2-7b`, etc.), with offline fallback protection.
4. **Prompt Heuristic Analyzer**: Real-time pattern detection examining in-context examples, few-shot pairs (`Input:`/`Output:`, `Q:`/`A:`), chain-of-thought cues, negative constraints, and output schema formatting.
5. **Real-Time Telemetry & Token Accounting**:
   - **TTFT (Time to First Token)** in milliseconds.
   - **Total Latency** in ms / s.
   - **Generation Speed** in Tokens Per Second (TPS).
   - Token accounting breakdown: Input, Reasoning, Output, Total tokens, and Rate Limit remaining headroom.

---

## 🚀 Quick Start

No installation or build tools required!

### Option 1: Open Directly in Browser
Double-click `index.html` or open it with your browser:
```bash
# macOS
open index.html

# Linux
xdg-open index.html

# Windows
start index.html
```

### Option 2: Run with a Local Static Server
```bash
# Python 3
python3 -m http.server 8000

# Node.js (npx)
npx serve .
```
Then navigate to `http://localhost:8000`.

---

## 🧩 Architectural Breakdown

```
├── index.html       # The complete, standalone application (HTML5 + CSS3 + Vanilla JS)
└── README.md        # Documentation and architectural guide
```

### Interface Sections

1. **Header & Global Controls**:
   - Editorial serif branding with LPU Accelerated status tag.
   - Light / Dark mode toggle (persisted in `localStorage`).
   - "Export Code" modal button.
   - Live Groq API connection indicator.

2. **Section 1: Model & Credentials**:
   - **Segmented Credential Toggle**: `🔒 Shared Demo Key` vs `🔑 Custom Key`.
   - **Model Selector**: Automatically populated with active Groq models.
   - **Hyperparameter Sliders**: Temperature (`0.0` - `2.0`) & Max Tokens (`50` - `4096`).
   - **Feature Gating**: Web search toggle & Internal reasoning tokens toggle.

3. **Section 2: Prompt Workshop**:
   - **Interactive Presets**: Few-Shot Classification, Chain-of-Thought, JSON Schema, Python Code.
   - **Live Heuristic Analyzer**: Real-time verdict badge (`ZERO-SHOT DIRECT`, `ONE-SHOT PATTERN`, `FEW-SHOT`), pattern chips, and diagnostic checklist.
   - **Submission**: `Cmd+Enter` / `Ctrl+Enter` or click "Submit to Groq".

4. **Section 3: Live Stream & Interactive Transcript**:
   - Streaming SSE token-by-token rendering.
   - Native Markdown formatting (headings, bullet points, numbered lists, blockquotes).
   - Code blocks with syntax wrappers and 1-click "Copy" buttons.
   - Collapsible reasoning process container for models with thinking output.
   - Web search source citation cards (for web-search enabled models).
   - Abort / Stop Generation support.

5. **Section 4: Latency Telemetry & Token Accounting**:
   - Real-time performance benchmark cards (TTFT, Total Latency, TPS).
   - Exact token breakdown (prompt tokens, reasoning tokens, completion tokens, total).
   - Rate limit headroom monitor based on `x-ratelimit-remaining-tokens`.

---

## 🔒 Security & Privacy Notice

- **Demo Key Masking**: When using the Shared Demo Key, the plaintext key is kept in obfuscated application memory and never populated into the accessible input `value` or DOM attributes.
- **Custom Key Isolation**: Custom keys are strictly kept in the user's browser `sessionStorage` and sent directly to `https://api.groq.com/openai/v1`. No third-party servers or telemetry proxies are involved.
- **Code Export Sanitization**: Exported scripts never leak actual keys and strictly use `$GROQ_API_KEY`.
