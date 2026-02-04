# Large JSON → NotebookLM

Script to **split and convert** large JSON files (e.g. Telegram exports, up to ~1 GB) into **Markdown chunks** sized for **NotebookLM** (and similar tools with upload limits).

- **src/** — drop your JSON files here  
- **dist/** — output: `name_1.md`, `name_2.md`, … (ready to upload to NotebookLM)

## Quick start

```bash
python3 -m venv .venv && .venv/bin/pip install -r requirements.txt
.venv/bin/python split_json.py
```

Uses **config.json** for limits (default: 150 MB and 400k objects per file). Put JSON in `src/`, run the script, then upload the `.md` files from `dist/` to NotebookLM.

## Commands

| Action | Command |
|--------|--------|
| Convert all `.json` in `src/` → `dist/*.md` | `python3 split_json.py` |
| One file | `python3 split_json.py src/foo.json` |
| Set limits | `python3 split_json.py config --max-size-mb 150 --max-objects 400000` |
| Show config | `python3 split_json.py config --show` |
| Clean output | `python3 split_json.py clean dist` |

**config.json**: `max_file_size_mb`, `max_objects_per_file`, `array_path` (e.g. `messages` for Telegram), `output_format` (`md` / `json`). Streams with **ijson** so big files stay out of memory.
