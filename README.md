# 🎓 AI Study Companion (Edu-Assistant)

A powerful AI-powered educational assistant built with **Streamlit** and **Google Gemini 2.5**. This tool helps students understand concepts, solve assignments, quiz themselves, and even translate terms.

## 🚀 Features

- **Normal Tutor**: Ask general questions and get simple, step-by-step explanations.
- **Ask from my Notes**: Upload your study material (PDF/TXT) and ask questions specifically from your notes.
- **Take Quiz From Internet**: Generate quizzes on any topic to test your knowledge.
- **Assignment Helper**: Get help with assignments, including code solutions and explanations.
- **Dictionary / Translation**: Look up meanings of words and get translations in multiple languages.
- **🛡️ Safety Guardrails**: Built-in protection against prompt injection and non-educational content.

## 🏗️ Architecture

The application follows a streamlined architecture connecting the user interface to the Gemini API with safety layers:

```mermaid
graph TD
    User([User]) -->|Interacts| UI[Streamlit UI]
    UI -->|Selects Mode| ModeHandler{Mode Handler}
    
    ModeHandler -->|Normal Tutor| Prompt[Construct Prompt]
    ModeHandler -->|Notes| FileProc[File Processor (PDF/TXT)]
    FileProc -->|Extract Text| Prompt
    ModeHandler -->|Quiz| QuizPrompt[Construct Quiz Prompt]
    ModeHandler -->|Assignment| AssignPrompt[Construct Assignment Prompt]
    ModeHandler -->|Dictionary| DictPrompt[Construct Dictionary Prompt]
    
    QuizPrompt --> Prompt
    AssignPrompt --> Prompt
    DictPrompt --> Prompt
    
    Prompt -->|Applies| Safety[Safety Guardrails & System Instructions]
    Safety -->|Sends Request| Gemini[Google Gemini API]
    
    Gemini -->|Returns Response| UI
```

## 🛠️ Setup & Installation

Follow these steps to set up the project locally on your machine.

### 1. Prerequisites

- Python 3.8 or higher installed.
- A Google Gemini API Key (Get it from [Google AI Studio](https://aistudio.google.com/)).

### 2. Create a Virtual Environment

It is recommended to use a virtual environment to keep dependencies isolated.

**Windows:**
```powershell
# Open your terminal/command prompt in the project folder
python -m venv venv

# Activate the virtual environment
.\venv\Scripts\activate
```

**macOS / Linux:**
```bash
# Open your terminal in the project folder
python3 -m venv venv

# Activate the virtual environment
source venv/bin/activate
```

### 3. Install Dependencies

Once the virtual environment is active, install the required packages:

```bash
pip install -r requirements.txt
```

### 4. Set Up Environment Variables

1. Create a file named `.env` in the root directory (same level as `app.py`).
2. Add your Google Gemini API Key inside it:

```env
GEMINI_API_KEY=your_actual_api_key_here
```

### 5. Run the Application

Start the Streamlit app with the following command:

```bash
streamlit run app.py
```

The app should open automatically in your default browser at `http://localhost:8501`.

## 📂 Project Structure

```
edu-assistant/
├── app.py              # Main application code (Streamlit + Gemini logic)
├── requirements.txt    # Python dependencies (streamlit, google-generativeai, etc.)
├── .env                # API Key configuration (kept local, not pushed to git)
└── README.md           # Project documentation
```

---
**Note:** This project is designed for educational purposes. The AI follows strict safety guidelines to ensure safe and relevant interactions.
