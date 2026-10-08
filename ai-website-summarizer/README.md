# AI Website Summarizer

## Overview
The AI Website Summarizer is a Python-based web application that extracts readable text from any given URL and leverages Large Language Models (LLMs) to generate concise, highly readable summaries. It operates as the "Reader's Digest of the internet," allowing users to quickly digest long-form articles, documentation, or news.

This project is built with a modular architecture that cleanly separates the data extraction layer (web scraping), the intelligence layer (LLM interaction), and the presentation layer (Gradio frontend).

## System Architecture

The application follows a straightforward, synchronous data pipeline:

```text
[ User Input: URL ]
        │
        ▼
┌─────────────────────────────────────────┐
│ Web Scraper Layer (scraper.py)          │
│ • Validates URL scheme                  │
│ • Fetches HTML payload via Requests     │
│ • Strips DOM noise (scripts, styles)    │
│ • Extracts raw text via BeautifulSoup   │
└─────────────────────────────────────────┘
        │
        ▼ (Raw Page Text)
┌─────────────────────────────────────────┐
│ Intelligence Layer (summarizer.py)      │
│ • Injects text into prompt template     │
│ • Applies System Prompt (Personality)   │
│ • Transmits payload to LLM endpoint     │
└─────────────────────────────────────────┘
        │
        ▼ (Markdown Summary)
┌─────────────────────────────────────────┐
│ UI / Presentation Layer (app.py)        │
│ • Renders application interface         │
│ • Handles user events & async loading   │
│ • Displays formatted markdown output    │
└─────────────────────────────────────────┘
```

## Tech Stack
* **Language:** Python 3.x
* **Frontend/UI:** Gradio
* **Web Scraping:** Requests, BeautifulSoup4
* **LLM Integration:** OpenAI Python SDK (configured for Gemini compatibility)
* **Environment Management:** python-dotenv

## Repository Structure

* `scraper.py`: Handles the web request and DOM parsing. It isolates the extraction logic so the core app doesn't need to understand HTML parsing.
* `summarizer.py`: Manages the LLM connection. It defines the "system prompt" (how the AI behaves) and constructs the API call.
* `app.py`: The main entry point that wraps the backend logic in a Gradio web interface.
* `.env`: (Ignored in version control) Stores the secure API credentials.
* `requirements.txt`: Locked dependencies for environment replication.

## Installation and Setup

### 1. Clone the repository
Navigate to your desired directory and initialize the project:
```bash
git clone https://github.com/thealkeshgupta/applied-ai-engineering-lab.git
cd applied-ai-engineering-lab/ai-website-summarizer
```

### 2. Configure the Virtual Environment
Isolate the project dependencies by creating and activating a virtual environment:
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies
Install the required packages using the requirements manifest:
```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables
Create a `.env` file in the root of this project directory. Since this build utilizes the Gemini API via OpenAI compatibility, configure your endpoints as follows:

```env
GEMINI_API_KEY=your_gemini_api_key_here
```

The OpenAI-compatible Gemini base URL is set in `summarizer.py`:

`https://generativelanguage.googleapis.com/v1beta/openai/`

## Run

Start the application by running the main interface script:
```bash
python app.py
```

Gradio prints a local URL (usually `http://127.0.0.1:7860`) and a temporary public `*.gradio.live` link.

1. Open either URL in a browser.
2. Enter a website (scheme optional; `example.com` becomes `https://example.com`).
3. Submit to see the markdown summary.

Keep the venv activated so `python` uses the packages in `.venv`.

## Customization

Edit `system_prompt` in `summarizer.py`. Default behavior is a short, friendly markdown summary that ignores navigation. Change the prompt for bullet points, another language, or a different tone.
