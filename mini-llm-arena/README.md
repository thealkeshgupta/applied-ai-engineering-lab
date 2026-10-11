# LLM Arena: Dual-Model Evaluation

## Overview

This project is a learning-focused, single-script application designed to evaluate Large Language Models side-by-side. It allows users to send an identical prompt to two distinct models and compare their responses in a real-time web interface. 

## Technical Architecture

The application is entirely contained within `app.py`. It utilizes the OpenAI Python SDK to interface with OpenAI-compatible endpoints from different model providers. 

The current configuration pits two models against each other:

- **Model A:** `gemini-3.8-flash`, accessed via the Google Generative AI compatible endpoint.
- **Model B:** `openai/gpt-oss-20b`, accessed via the Groq high-speed compatible endpoint.


## System Architecture

The application handles both the user interface and the API orchestration within a unified execution context:

```text
[ User Input: Evaluation Prompt ]
                 │
                 ▼
┌──────────────────────────────────────────────┐
│ app.py (Application Core)                  │
│ • Renders Gradio side-by-side interface      │
│ • Dispatches parallel requests to Model A & B│
│ • Captures execution outputs                 │
│ • Handles human preference voting callbacks  │
└──────────────────────┬───────────────────────┘
                       │
         ┌─────────────┴─────────────┐
         ▼                           ▼
┌──────────────────┐       ┌──────────────────┐
│ Model A Endpoint │       │ Model B Endpoint │
│ ( e.g., Gemini ) │       │ ( e.g., OpenAI ) │
└────────┬─────────┘       └────────┬─────────┘
         │                          │
         └─────────────┬────────────┘
                       ▼
┌──────────────────────────────────────────────┐
│ Presentation & Evaluation UI                 │
│ • Dual Markdown response panes               │
│ • Preference Voting Buttons (Dummy Logic)    │
└──────────────────────────────────────────────┘
```

## Tech Stack
The project relies on a minimal stack defined in `requirements.txt`:

* **Language:** Python 3.x
* **UI Framework:** Gradio
* **Model Orchestration:** OpenAI Python SDK (configured via compatibility layer for Gemini)
* **Configuration:** python-dotenv


## Setup Instructions
### 1. Configure the Virtual Environment
Navigate to the project directory and isolate your dependencies by creating a virtual environment:
```bash
python3 -m venv .venv
source .v
```

### 2. Install Dependencies
Install the required packages using the requirements manifest:
```bash
pip install -r requirements.txt
```
*(Dependencies generally include `openai`, `gradio`, and `python-dotenv`)*

### 3. Configure Environment Variables
The application uses `python-dotenv` to load credentials. Create a `.env` file in the project directory with the following keys:

GEMINI_API_KEY=your_google_api_key_here
GROQ_API_KEY=your_groq_api_key_here

*Note: The script automatically handles the* `base_url` *routing for both the Gemini and Groq clients internally.*

### 4. Launch the Application

Start the application by running the main interface script:
```bash
python app.py
```

This will start the local server and generate a public, shareable Gradio link because the launch configuration is set to `share=True`.

## Usage Notes

- Enter a prompt in the provided text box and click "Battle" to dispatch the request to both models.
- The application features a UI voting system with thumbs-up and thumbs-down buttons for each model. 
- This voting system is a dummy mechanism for learning purposes; clicking a vote simply updates the UI verdict label with your choice and does not store the vote persistently.

