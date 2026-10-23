# ONCF Learning Assistant

A simple educational chatbot built with Python, Streamlit and the Groq API. It answers questions using the supplied `knowledge.json` file and remembers the conversation for the current Streamlit session.

> **Important:** This is an unofficial educational prototype. It is not affiliated with ONCF. It does not provide live train information and cannot book, buy, cancel or refund tickets.

## Project files

```text
ONCF_Chatbot/
├── app.py
├── knowledge.json
├── .env
├── .gitignore
├── requirements.txt
└── README.md
```

## Requirements

- Windows 10/11
- Python 3.10 or newer
- Internet connection for the Groq API
- A Groq API key

## 1. Install Python

Download Python from [https://www.python.org/downloads/](https://www.python.org/downloads/).

During installation on Windows, select **Add Python to PATH**.

Confirm installation in Command Prompt:

```bat
python --version
```

## 2. Create a virtual environment

Open Command Prompt and move into the project directory:

```bat
cd path\to\ONCF_Chatbot
```

Create and activate a virtual environment:

```bat
python -m venv .venv
.venv\Scripts\activate
```

If PowerShell is used instead of Command Prompt:

```powershell
.\.venv\Scripts\Activate.ps1
```

## 3. Install the dependencies

```bat
python -m pip install --upgrade pip
pip install -r requirements.txt
```

The project uses only Streamlit, the official Groq Python SDK and python-dotenv. Pandas is not needed for this chatbot.

## 4. Get a Groq API key

1. Open [https://console.groq.com/keys](https://console.groq.com/keys).
2. Sign in or create a Groq account.
3. Create a new API key.
4. Copy the key. Treat it like a password and do not share it.

Open the `.env` file in a text editor and replace the placeholder:

```env
GROQ_API_KEY=your_groq_api_key_here
GROQ_MODEL=openai/gpt-oss-20b
```

For example, paste your own key after the equals sign on the first line. Do not add it to `app.py`, screenshots, public repositories or messages.

The default model is `openai/gpt-oss-20b`. Groq model availability and free-plan limits can change; if this model is unavailable to your account, select another currently available text-generation model in the Groq Console and update `GROQ_MODEL`.

## 5. Run the application

From the project folder, with the virtual environment activated:

```bat
streamlit run app.py
```

Streamlit normally opens the application in your browser at:

```text
http://localhost:8501
```

To stop the application, return to Command Prompt and press `Ctrl+C`.

## How it works

- `knowledge.json` is loaded dynamically with Python's `json` module.
- The original JSON structure and records are preserved.
- FAQ `fact_ids` are used to retrieve related knowledge records.
- A lightweight keyword matcher adds other potentially relevant records.
- Related official source references and all limitation records are included in the model instructions.
- Record statuses are explicitly explained to the AI: `sourced_general`, `dated_snapshot` and `indexed_reference`.
- The last messages are kept in `st.session_state` so follow-up questions can be understood during the current session.
- The **Clear conversation** button removes the current session chat history.
- The API key is read from `.env` and is never hardcoded in the Python file.

## Accuracy and limitations

This application uses a static research file, not a live ONCF connection. It must not be used to guarantee current train schedules, delays, platforms, seat availability, fares, promotions or operational conditions.

For any missing or time-sensitive information, check the official ONCF website:

[https://www.oncf.ma/](https://www.oncf.ma/)

The chatbot cannot access customer accounts or perform transactions. It must not ask for passwords, API keys, banking information or identity-document uploads.

The AI model can still misunderstand a question. Check important information with ONCF before travelling or making a purchase.

## Troubleshooting

### `GROQ_API_KEY` is missing

- Confirm that `.env` is in the same folder as `app.py`.
- Replace the placeholder with your actual Groq key.
- Save the file and restart Streamlit.

### Invalid API key / authentication error

Create or copy a valid key from [Groq Console](https://console.groq.com/keys). Make sure there are no extra spaces or quotation marks around the key.

### Model not found or unavailable

The model catalog can change. Check the current model IDs and your account's access in the Groq Console, then update `GROQ_MODEL` in `.env`.

### Rate limit reached

Wait before sending another request, or review the limits shown in your Groq Console.

### `knowledge.json` not found

Keep `knowledge.json` in the same directory as `app.py`. Do not rename it unless you also update the filename in `app.py`.

## Security

- `.env` is listed in `.gitignore`.
- Never commit your real API key to Git.
- Do not publish the application with a real key embedded in source code.
- If a key is exposed, revoke it in Groq Console and create a replacement.


## Questions fréquentes repliables

Les questions suggérées sont dans un panneau repliable avec une flèche,
fermé par défaut. Cliquez sur « Questions fréquentes » pour l'ouvrir,
choisissez une question dans la liste, puis cliquez sur « Poser cette question ».

Le chatbot affiche uniquement le dernier échange. Chaque nouvelle question,
qu'elle soit saisie ou choisie dans la liste, remplace la question et la réponse
précédentes.


## Correction : un seul échange visible

Le traitement de la nouvelle question est maintenant exécuté avant l'affichage
de l'historique Streamlit. À chaque nouvelle question, la liste de messages est
remplacée par la question courante et sa réponse. L'ancien échange n'est donc
plus rendu pendant le même rafraîchissement et il n'est plus nécessaire de
cliquer sur « Clear conversation » entre deux questions.
