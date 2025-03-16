# AI-Powered Chatbot for Quick Access to Jenkins Resources

This is a prototype for my Google Summer of Code (GSoC) 2025 project with the Jenkins community, titled "AI-Powered Chatbot for Quick Access to Jenkins Resources." The chatbot leverages a fine-tuned DistilBERT model for natural language processing (NLP), WebSockets for real-time interaction, and a SQLite database for caching Jenkins resources (plugins, documentation, and snippets). It aims to provide intuitive access to Jenkins resources via natural language queries within the Jenkins UI.

- **Author**: Bamla Varun Singh
- **Email**: vaarunsingghh@gmail.com
- **GitHub**: [https://github.com/singghh](https://github.com/singghh)
- **Demo**: Run locally at `http://localhost:8000` after setup.

## Features

- **NLP-Powered Queries**: Uses fine-tuned DistilBERT to recognize intents (e.g., plugin info, doc requests, snippets) with >70% accuracy on a 30-query dataset.
- **Real-Time Responses**: WebSocket-based communication for instant replies.
- **Resource Access**: Supports 10 plugins (e.g., `maven-plugin`, `docker-workflow`), 5 documentation topics, and 3 code snippets.
- **Fuzzy Matching**: Suggests similar plugins/docs/snippets if exact matches are missing.
- **Caching**: SQLite stores pre-indexed data for fast responses (<50ms for cache hits).

## Installation

Follow these steps to set up and run the chatbot locally from scratch. All dependencies are managed via Python.

### Prerequisites
- **Operating System**: Windows, macOS, or Linux (tested on Windows with Git Bash).
- **Python**: Version 3.9 or higher (recommended: 3.11).
- **Git**: For cloning the repository.
- **Internet Connection**: Required to download dependencies and models.

### Step-by-Step Installation

1. **Clone the Repository**
    - git clone https://github.com/singghh/jenkins-2025.git
    - cd jenkins-2025
2. **Install Python (if not already installed)**
    Download Python from python.org (e.g., Python 3.11).
    During installation, check "Add Python to PATH."
    verify installation through:
      - python --version
4.  **Set Up a Virtual Environment**
     i) Create a virtual environment:
      - python -m venv chatbot_env
     ii) Avtivate it
      - windows: source chatbot_env/Scripts/activate
      - mac: source chatbot_env/bin/activate
5.  **Install Dependencies**
      - pip install fastapi uvicorn transformers torch sqlite3 pandas scikit-learn fuzzywuzzy[speedup] accelerate
6.  **Prepare the Dataset and Model**
      -  Ensure jenkins_queries.csv (provided in the repo) is in the root directory.
      -  The fine-tuned DistilBERT model is included in the ./distilbert_finetuned folder. If missing, run finetune_distilbert.py (see below).
7.  **Run the Fine-Tuning Script (Optional)**
     -  If you want to retrain the model
     -  python finetune_distilbert.py
     -  This generates ./distilbert_finetuned and label_encoder.pkl. Expect warnings about uninitialized weights (normal) and a training log.
8.  **Start server**
   -  uvicorn main:app --host 0.0.0.0 --port 8000
   -  Open your browser at http://localhost:8000 to interact with the chatbot.

**Troubleshooting**

Dependency Errors: Ensure all packages install correctly. If accelerate fails, use pip install accelerate --upgrade.
Model Not Found: If ./distilbert_finetuned is missing, rerun finetune_distilbert.py after verifying jenkins_queries.csv.
Port Conflict: If port 8000 is in use, change it with --port 8001


**Usage**

After starting the server (Step 7 above), open http://localhost:8000 in your browser.
Enter queries in the input field
e.g.:
"Show me maven plugin"
"I need docs for pipelines"
"Give me a pipeline example"

The chatbot will respond with descriptions, links, or snippets based on the intent.
Test fuzzy matching with typos (e.g., "show me mavne plugin") to see suggestions.

**Sample Responses:**

"Maven plugin for building Java projects (Learn more: https://updates.jenkins.io/download/plugins/maven-plugin/)"
"Here’s the documentation for pipelines: https://www.jenkins.io/doc/book/pipeline/"
"Here’s a code snippet for pipeline:\ngroovy\npipeline { agent any stages { stage('Build') { steps { echo 'Building...' } } } }\n"
