# StudyBuddy

Turn your notes, or **a photo of your notes**, into a **quiz, flashcards or summary**, in **English or Urdu**.

StudyBuddy is a small web app that runs entirely on your own computer. The AI is the open-weight **Gemma 4** model, run locally through **Ollama**. Your notes never leave your machine, and no API key or account is needed.

Built for Hacktoberfest Hack Day Lahore (theme: open-source AI).

## What it does

- **Quiz:** multiple-choice questions you can answer in the page, with instant feedback, a score and short explanations
- **Flashcards:** tap a card to flip between question and answer
- **Summary:** short bullet points plus a key takeaway
- **Photo input:** take a picture of handwritten or printed notes, and Gemma 4 reads the image itself (it is a multimodal model)
- **English or Urdu** output (Urdu is shown right-to-left)

## How open-source AI is used

Everything StudyBuddy does comes from the model, so the open-weight AI is the core of the project, not an add-on.

- The model is **Gemma 4** (`gemma4:e2b`, an open-weight model from Google DeepMind), served locally by **Ollama**.
- The page talks to Ollama's local HTTP API (`POST /api/generate`) from the browser.
- For quizzes and flashcards it asks Ollama for **structured JSON output** (`format: "json"`), then validates and cleans the answer before showing it. If the model slips, the page retries once automatically.
- For photos it sends the image to the model's vision input, after shrinking it so answers come back faster.
- Model output is always rendered as plain text, never as HTML.

## Run it (about 5 minutes)

1. **Install Ollama** from https://ollama.com and start it.
2. **Download the model** (one time):
   ```
   ollama pull gemma4:e2b
   ```
3. **Serve this folder** so the page is opened from `http://localhost`. Pick one:
   - **VS Code:** install the "Live Server" extension, then right-click `index.html` and choose "Open with Live Server".
   - **Python:** run `python -m http.server 8000` in this folder, then open http://localhost:8000
4. Paste some notes (or click **Try an example**), choose Quiz, and press **Generate**.

The first answer can take 10 to 60 seconds while the model loads. After that it is faster.

## Troubleshooting

| Problem | Fix |
|---|---|
| "Ollama not reachable" | Start the Ollama app, and open the page from `http://localhost`, not by double-clicking the file. |
| Says the model is not installed | Run `ollama pull gemma4:e2b`. |
| 403 error | Set the environment variable `OLLAMA_ORIGINS` to `*` and restart Ollama. |
| Answers are slow | Use a smaller photo, fewer questions, or close other heavy apps. |

## Tech

HTML, CSS and vanilla JavaScript (a single `index.html`, no build step) + Gemma 4 via Ollama.

## License

MIT, see [LICENSE](LICENSE).
