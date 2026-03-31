# AI Agent

A CLI coding agent powered by Google Gemini. Give it a task in plain English and it will autonomously read, write, and execute files in the `calculator/` working directory to complete it.

## Prerequisites

- Python 3.13+
- [uv](https://docs.astral.sh/uv/)
- A [Google Gemini API key](https://aistudio.google.com/app/apikey)

## Setup

1. Clone the repo and navigate to the project directory.

2. Install dependencies:
   ```bash
   uv sync
   ```

3. Create a `.env` file in the project root with your Gemini API key:
   ```
   GEMINI_API_KEY=your_api_key_here
   ```

## Usage

```bash
uv run main.py "your prompt here"
```

Add `--verbose` to see token counts and detailed function call output:

```bash
uv run main.py "your prompt here" --verbose
```

### Examples

```bash
uv run main.py "What files are in the project?"
uv run main.py "Run the tests and tell me if they pass" --verbose
uv run main.py "Fix any failing tests in tests.py"
```

## How it works

The agent uses Gemini 2.5 Flash with a tool-use loop (up to 20 iterations) and has access to four tools, all sandboxed to the `calculator/` directory:

| Tool | Description |
|---|---|
| `get_files_info` | List files and directories with sizes |
| `get_file_content` | Read file contents (truncated at 10,000 chars) |
| `run_python_file` | Execute a Python file and capture output |
| `write_file` | Write or overwrite a file |

The agent keeps calling tools until it produces a final text response, or hits the 20-iteration limit.

## Project structure

```
ai_agent/
├── main.py           # Entry point and agent loop
├── prompts.py        # System prompt
├── call_function.py  # Tool dispatcher
├── functions/        # Tool implementations
│   ├── config.py
│   ├── get_files_info.py
│   ├── get_file_content.py
│   ├── run_python_file.py
│   └── write_file.py
└── calculator/       # Sandboxed working directory the agent operates in
```
