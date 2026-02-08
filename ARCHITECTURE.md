# 🏗️ Edu-Assistant Architecture

This document describes the high-level architecture of the Edu-Assistant application, highlighting how user inputs are processed, sanitized, and sent to the Google Gemini API.

## System Overview

The application is built using **Streamlit** for the frontend and **Google Gemini 2.5 Flash** as the reasoning engine.

### Data Flow Diagram

```mermaid
graph TD
    %% Styling
    classDef user fill:#f9f,stroke:#333,stroke-width:2px;
    classDef ui fill:#bbf,stroke:#333,stroke-width:2px;
    classDef logic fill:#bfb,stroke:#333,stroke-width:2px;
    classDef api fill:#ff9,stroke:#333,stroke-width:2px;

    User([👤 Student]) -->|Interacts| UI[💻 Streamlit Interface]
    class User user
    class UI ui

    subgraph Application Logic
        UI -->|Selects Mode| Router{⚙️ Mode Handler}
        class Router logic
        
        Router -->|Uploads File| FileProc[📄 File Processor]
        FileProc -->|Extracts Text| Context[📝 Context Manager]
        class FileProc,Context logic
        
        Router -->|Type: Question| Type1[❓ Q&A Prompt]
        Router -->|Type: Quiz| Type2[📝 Quiz Prompt]
        Router -->|Type: Assignment| Type3[📚 Assignment Prompt]
        Router -->|Type: Dictionary| Type4[📖 Dictionary Prompt]
        
        Context --> Type1
        
        Type1 & Type2 & Type3 & Type4 --> Builder[🔨 Prompt Builder]
        class Builder logic
        
        Builder -->|Wraps Input| XML[Safe Input Wrapper <tag>]
        XML -->|Injects| System[🛡️ System Prompt & Safety Rules]
        class XML,System logic
    end

    System -->|Sends Request| Gemini[🤖 Google Gemini API]
    class Gemini api
    
    Gemini -->|Returns Response| UI
```

### Key Components

1.  **Streamlit Interface**: Handles user interaction, file uploads, and displaying chat history.
2.  **Mode Handler**: Routes the request based on the selected mode (Tutor, Quiz, Assignment, etc.).
3.  **File Processor**: Extracts text from uploaded PDF or TXT files using `pypdf`.
4.  **Prompt Builder**: dynamically constructs the prompt based on the mode and user input.
5.  **Safety Layer**: 
    - Wraps user input in XML-like tags (e.g., `<question>...</question>`) to prevent prompt injection.
    - Injects specific safety instructions into the system prompt.
6.  **Google Gemini API**: Processes the final sanitized prompt and returns the educational content.
