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

---

## Почему «Error uploading source, try again» в NotebookLM

Сообщение **"Error uploading source, try again"** чаще всего появляется, когда **один источник превышает лимит 500 000 слов** или 200 МБ. Официальные лимиты: [NotebookLM FAQ](https://support.google.com/notebooklm/answer/16269187).

---

## Почему файлы из dist/ могут не приниматься в NotebookLM

По [официальной справке NotebookLM](https://support.google.com/notebooklm/answer/16269187):

1. **Формат**  
   Принимаются только: **PDF, Word, Text (.txt), Markdown (.md)**.  
   Файлы **.json** из dist/ NotebookLM **не поддерживает** — используйте `output_format: "md"` в config (по умолчанию так и есть).

2. **Лимит по словам**  
   На один источник: **до 500 000 слов**.  
   Скрипт ограничивает только размер в МБ и число объектов, **лимита по словам нет**. Один большой `*_1.md` может содержать миллионы слов (например, 400k коротких сообщений × несколько слов = перебор), и такой файл NotebookLM отклонит.

3. **Лимит по размеру**  
   До **200 МБ** на файл. Дефолт скрипта 150 МБ — по размеру всё ок, проблема чаще в пункте 2.

4. **Число источников**  
   В один ноутбук — не более **50 источников**. Если частей больше 50, загрузить все не получится.

**Что сделать:**  
- Уменьшить `max_objects_per_file` в config (например, до **15 000–25 000**), чтобы один .md содержал не больше ~500k слов.  
- Или задать `max_file_size_mb` меньше (например, 20–30 МБ) — тогда слов в части тоже будет меньше.  
- Для загрузки в NotebookLM использовать только файлы **.md** (или .txt из `to-txt`), не .json.
