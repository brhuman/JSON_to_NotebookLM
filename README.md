# Large JSON → NotebookLM

Split large JSON files (e.g. Telegram exports) into Markdown chunks for NotebookLM upload.

- **src/** — put JSON here  
- **dist/** — output: `name_1.md`, `name_2.md`, …

## Quick start

```bash
python3 -m venv .venv && .venv/bin/pip install -r requirements.txt
.venv/bin/python split_json.py
```

Config: **config.json** (`max_file_size_mb`, `max_objects_per_file`, `max_words_per_file` for NotebookLM ~500k limit, `array_path`, `output_format`). Uses ijson for streaming.

## Commands

| Action | Command |
|--------|--------|
| All `.json` in src → dist | `python3 split_json.py` |
| **One specific file** | `python3 split_json.py src/filename.json` |
| Set limits | `python3 split_json.py config --max-size-mb 150 --max-objects 400000` |
| Show config | `python3 split_json.py config --show` |
| Clean dist | `python3 split_json.py clean dist` |

**Run one file:** pass the path to the JSON (from project root or absolute). Output goes to `dist/filename_1.md`, `dist/filename_2.md`, …

```bash
python3 split_json.py src/your_export.json
```

**NotebookLM limits:** 200 MB and ~500k words per source; 50 sources per notebook. Default `max_words_per_file: 450000` keeps each .md under the limit; adjust in config if needed.
