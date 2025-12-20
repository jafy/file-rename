![File Renamer Assistant logo](file_renamer_assistant_logo.png)

# File Renaming Assistant

An OpenAI Assistant (API) for renaming files base on their contents, using Python, Bash, and Linux CLI.

[File Renamer Helper app](https://chat.openai.com/g/g-O1sujw5iD-file-renamer) available on OpenAI's GPT store, to help install, use and understand this repository.

## Initial setup

1. clone [File Renamer Assistant](https://github.com/toadlyBroodle/asst-file-renamer) repository:
   `git clone https://github.com/toadlyBroodle/asst-file-renamer.git`
2. install dependencies
   `pip3 install python-docx openpyxl PyPDF2 pillow pytesseract`

3. Save new _credentials.json_ file to working directory, replacing with your API key, using format:
   ```
   {
       "openai_api_key": "sk-####",
   }
   ```
4. Create new assistant:
   `python3 file_renamer_asst.py --asst_create`

## Usage, overview

File types currently supported: .txt, .csv, .pdf, .docx, .xlsx, .jpg, .jpeg, .png  
Please submit requests for additional file types.

Renames now happen **in place**. Use `--dry_run` to preview without writing.

EXTRACTION_PERCENT variable may need to be adjusted to achieving accurate new file names, while still preserving file privacy.

Disclaimer: This assistant does **not** upload files directly to OpenAI, but rather parses files locally to extract small percentage of beginning text contexts. This text summary is then necessarily sent to OpenAI API for analysis to generate new file names. Use with discretion and at your own risk.

All the included functions are not necessarily used for renaming files, but are nonetheless included for user customization purposes, as well as to provide a demonstrative, documented, example of how to create and use OpenAI Assistants API.

```
usage: file_renaming_asst.py [-h] [--files_rename FILES_RENAME] [--dry_run] [--verbose] [--extraction_percent EXTRACTION_PERCENT]
                             [--max_preview_chars MAX_PREVIEW_CHARS] [--disable_pdf_images] [--disable_vision] [--model MODEL] [--vision_model VISION_MODEL]
                             [--price_in PRICE_IN] [--price_out PRICE_OUT] [--openai_log_level {debug,info,warning,error,critical,none}]

Rename files in a directory based on content using OpenAI Responses API.

options:
  -h, --help            show this help message and exit
  --files_rename FILES_RENAME, -fr FILES_RENAME
                        Directory containing files to rename
  --dry_run, -dr        Preview renames without writing
  --verbose, -v         Verbose logging
  --extraction_percent EXTRACTION_PERCENT, -p EXTRACTION_PERCENT
                        Percent of file to sample from the start (1-100)
  --max_preview_chars MAX_PREVIEW_CHARS, -mpc MAX_PREVIEW_CHARS
                        Maximum characters of extracted text to send to the model (omit for no limit)
  --disable_pdf_images  Skip image-based extraction for PDFs
  --disable_vision      Skip vision model extraction for images
  --model MODEL         OpenAI model to use (default: gpt-5.2)
  --vision_model VISION_MODEL
                        OpenAI vision-capable model for OCR fallback (default: gpt-4o)
  --price_in PRICE_IN   Override input cost per 1M tokens for the chosen model
  --price_out PRICE_OUT Override output cost per 1M tokens for the chosen model
  --openai_log_level {debug,info,warning,error,critical,none}
                        OpenAI client log level (default: none to suppress HTTP debug).
```

## Run from anywhere (macOS)

Add a small shell function so you can call the renamer from any directory:

1. Ensure dependencies are installed (ideally in a venv) and note this repo path, e.g. `/Users/jeff/Documents/Personal/Code/asst-file-renamer`.
2. Add to your `~/.zshrc` (or `~/.bash_profile`):
   ```sh
   file-rename() {
     /usr/bin/python3 /Users/jeff/Documents/Personal/Code/asst-file-renamer/file_renaming_asst.py -fr "$1" "${@:2}"
   }
   ```
   Then reload your shell: `source ~/.zshrc`.
3. Usage examples from anywhere:
   - Dry run with verbose logs: `file-rename /path/to/files --dry_run -v`
   - Set preview percent: `file-rename /path/to/files -p 50`

If you prefer a standalone script, create `/usr/local/bin/file-rename` with the same command and `chmod +x /usr/local/bin/file-rename` (ensure `/usr/local/bin` is on your `PATH`). Use your venv’s Python path if needed.
