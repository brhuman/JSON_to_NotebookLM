# Large JSON → NotebookLM

Split large JSON files (e.g. Telegram exports) into Markdown chunks for NotebookLM upload.

- **src/** — put JSON here  
- **dist/** — output: `name_1.md`, `name_2.md`, …

## Quick start

```bash
python3 -m venv .venv && .venv/bin/pip install -r requirements.txt
.venv/bin/python split_json.py
```

Config: **config.json** (`max_file_size_mb`, `max_objects_per_file`, `array_path`, `output_format`). Uses ijson for streaming.

## Commands

| Action | Command |
|--------|--------|
| All `.json` in src → dist | `python3 split_json.py` |
| Single file | `python3 split_json.py src/foo.json` |
| Set limits | `python3 split_json.py config --max-size-mb 150 --max-objects 400000` |
| Show config | `python3 split_json.py config --show` |
| Clean dist | `python3 split_json.py clean dist` |

**NotebookLM limits:** 200 MB and ~500k words per source; 50 sources per notebook. Use `output_format: "md"` and lower `max_objects_per_file` (e.g. 15k–25k) if uploads fail.
