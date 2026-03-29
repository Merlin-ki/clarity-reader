# Clarity

### [https://clarity.rahul.gs/](https://clarity.rahul.gs/)

Clarity is a reading app that lets you explore texts in depth. Instead of reading from top to bottom, Clarity presents content in layers. Start with a summary, then tap to explore details as you like. It's a simpler way to read, giving you control over your focus.

## Demo

![Clarity Demo](/public/demo.gif)

### Built with [modal.com](https://modal.com)

## Windows 11 + VS Code setup in `C:\dev\clarity-reader-stack`

This repository can be developed locally on Windows 11 without deploying to Modal.
The frontend is a Next.js app and the backend is a FastAPI app. The current backend
implementation is wired to the OpenAI SDK, so OpenAI works immediately. Gemini and
DeepSeek would require a small backend provider refactor before they can be used.

### Recommended install order

1. Install **Git for Windows**
2. Install **VS Code**
3. Install **Node.js 18 LTS** (includes `npm`)
4. Install **uv**
5. Install **Python 3.10** with `uv`
6. Install **Poetry** with `uv tool install poetry`

See `WINDOWS_SETUP_REQUIREMENTS.txt` for a concise checklist of required tools,
optional accounts, and environment variables.

If you want to onboard a German-speaking GPT-5.4 coding agent directly inside
VS Code, use the copy-paste prompt in `VSCODE_GPT54_ONBOARDING_PROMPT_DE.txt`.

### 1. Create the workspace

Open PowerShell and run:

```powershell
New-Item -ItemType Directory -Force C:\dev\clarity-reader-stack | Out-Null
Set-Location C:\dev\clarity-reader-stack
```

### 2. Fork and clone the repository

1. Fork `Merlin-ki/clarity-reader` in GitHub.
2. Clone your fork into the workspace:

```powershell
git clone https://github.com/<your-user>/clarity-reader.git
Set-Location C:\dev\clarity-reader-stack\clarity-reader
code .
```

If VS Code is already open in the existing folder `C:\dev\clarity-reader-stack`,
clone the repository into the `clarity-reader` subfolder and then open that
repository folder in the Explorer/workspace.

### 3. Install frontend dependencies

In the VS Code terminal at
`C:\dev\clarity-reader-stack\clarity-reader`:

```powershell
npm install
```

### 4. Install Python and backend tooling with uv

In a second VS Code terminal:

```powershell
uv python install 3.10
uv tool install poetry
Set-Location C:\dev\clarity-reader-stack\clarity-reader\backend
poetry env use 3.10
poetry install
```

Why both **uv** and **Poetry**? This repository already stores its backend
dependencies in `backend/pyproject.toml` and `backend/poetry.lock`. Using `uv`
to install Python and Poetry gives a fast Windows setup while keeping dependency
resolution aligned with the existing project files.

### 5. Configure local environment variables

Create `C:\dev\clarity-reader-stack\clarity-reader\.env.local`:

```env
NEXT_PUBLIC_SERVER_ORIGIN=http://127.0.0.1:8000/
NEXT_PUBLIC_COHERE_API_KEY=placeholder-not-required-for-local-dev
```

Create `C:\dev\clarity-reader-stack\clarity-reader\backend\.env`:

```env
OPENAI_API_KEY=your_openai_api_key
```

Notes:

- `NEXT_PUBLIC_SERVER_ORIGIN` points the Next.js frontend to your local FastAPI backend.
- `NEXT_PUBLIC_COHERE_API_KEY` is only present because `_app.tsx` currently initializes
  the Cohere client on startup. Cohere is not part of the local summarization path.
- Modal is only needed for hosted deployment. It is not required for local development.
- If you want to use Gemini or DeepSeek locally, the backend must first be updated
  because the current implementation calls the OpenAI SDK directly.

### 6. Run backend and frontend locally

Backend terminal:

```powershell
Set-Location C:\dev\clarity-reader-stack\clarity-reader\backend
poetry run uvicorn pkg.main:app --reload --host 127.0.0.1 --port 8000
```

Frontend terminal:

```powershell
Set-Location C:\dev\clarity-reader-stack\clarity-reader
npm run dev
```

Then open:

- Frontend: `http://127.0.0.1:3000`
- Backend health check: `http://127.0.0.1:8000/ping`

### 7. Recommended VS Code extensions

- GitHub Pull Requests and Issues
- ESLint
- Prettier
- Python
- Pylance
- npm Intellisense

### 8. How the local app works

- The **frontend** fetches article content and asks the backend for summaries.
- The **backend** splits long text into token-limited chunks.
- The **LLM** summarizes each chunk.
- The backend recursively summarizes those intermediate summaries until only one
  top-level summary remains.
- The frontend renders that summary tree as layered reading cards.

## Key Features

- Recursive Summarization: Begin with summaries, dive into details.
- Depth-First Exploration: Customize your reading path.
- Minimalist Design: Clear interface, no distractions.

## Why Clarity?

Traditional reading can feel overwhelming. Clarity makes it easier to understand complex topics and discover what matters to you. Dive deeper, read smarter.

## License

MIT License
