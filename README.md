# Paper Clip Ideas

[![Python](https://img.shields.io/badge/Python-3.11%2B-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

This repository contains a Jupyter Notebook script for validating creative uses of paperclip. The script uses Google's Gemini API to evaluate whether each proposed use is valid, feasible, and novel based on the wire's properties (e.g., small size, bendable, conductive).

The validation process reads from a CSV file (`paperclip_uses.csv`), processes unvalidated entries, and updates the file with validation results (Yes/No) and brief explanations. It's designed as a test project to demonstrate API integration, rate limiting, and data processing.

## Features
- **API Integration**: Uses Google Generative AI (Gemini) for natural language validation.
- **Rate Limiting & Retries**: Implements rate limits (15 calls/minute) and exponential backoff retries for robust API handling.
- **Incremental Saving**: Saves progress every 15 validations to avoid data loss.
- **Output Summary**: Prints a summary of validated uses, including counts, categories, and samples.
- **CSV Management**: Handles loading, updating, and saving CSV data with columns: `Use`, `Category`, `Valid`, `Explanation`.

## Prerequisites
- Python 3.11+ (tested with 3.11.13).
- A Google Generative AI API key (free tier available for testing; sign up at [Google AI Studio](https://aistudio.google.com/)).
- Jupyter Notebook or JupyterLab for running the script.

### Required Packages
Install the dependencies using pip:
```
pip install google-generative-ai pandas tenacity ratelimit
```

## Setup
1. Clone the repository:
   ```
   git clone https://github.com/gui-llotiner/paper_clip_ideas.git
   cd paper_clip_ideas
   ```

2. Add your API key:
   - Open `usage_check.ipynb`.
   - Replace the placeholder in `API_KEY = 'YOUR-API-KEY'` with your actual Gemini API key.
   - (Optional) Use environment variables for security: Set `API_KEY` via `os.getenv('API_KEY')`.

3. Prepare the CSV file:
   - The script expects `paperclip_uses.csv` in the same directory.
   - If it doesn't exist, run a generation script (not included here) or create a sample CSV with columns: `Use`, `Category`, `Valid` (empty), `Explanation` (empty).
   - Example row: `"Bypass failed component","Temporary Circuit Bridge","",""`.

## Usage
1. Open the Jupyter Notebook:
   ```
   jupyter notebook usage_check.ipynb
   ```

2. Run all cells:
   - The script will load the CSV, identify unvalidated uses, query the Gemini API for each, parse responses, and update the CSV.
   - It logs progress (e.g., "Validating use: Inkjet Print Head Cleaner") and saves incrementally.
   - At the end, it prints a summary like:
     ```
     ✓ Total uses validated: 1086
     ✓ Valid uses: 500
     ✓ Categories covered: 20
     ...
     ```

3. Check the updated `paperclip_uses.csv` for results.

### Example Output
```
2025-10-02 02:04:58,699 - INFO - Loaded 1086 uses for validation
2025-10-02 02:04:58,702 - INFO - Found 300 uses needing validation
...
Incremental save after 15 validations to paperclip_uses.csv
...
Final save: Updated paperclip_uses.csv with 1086 validated uses

✓ Total uses validated: 1086
✓ Valid uses: 543
✓ Categories covered: 25
...
```

## Configuration Options
- **Rate Limiting**: Set via `CALLS_PER_MINUTE = 15` and `PERIOD = 60` (seconds). Adjust based on your API tier.
- **Save Interval**: Change `SAVE_INTERVAL = 15` to save more/less frequently.
- **Validation Prompt**: Customize `VALIDATION_PROMPT` for different evaluation criteria (e.g., emphasize novelty).
- **Model**: Uses `'gemini-2.0-flash-lite'`; switch to other Gemini models if needed (e.g., `'gemini-pro'`).

## Limitations
- **API Costs**: Free tier has limits; monitor usage to avoid quotas.

paper_clip_ideas.ipynb: Generates unique categories and uses for the wire.
usage_check.ipynb: Validates the feasibility and novelty of the generated uses.

Prerequisites

Python 3.11+ (tested with 3.11.13).
A Google Generative AI API key (available via Google AI Studio).
Jupyter Notebook or JupyterLab.

Required Packages
Install dependencies using pip:
pip install google-generative-ai pandas sentence-transformers tenacity ratelimit numpy

Setup

Clone the Repository:
git clone https://github.com/your-username/wire-uses-generator-validator.git
cd wire-uses-generator-validator

CSV File:

The scripts use paperclip_uses.csv for data storage.
If starting fresh, paper_clip_ideas.ipynb creates it with columns: Use, Category, Valid (empty), Explanation (empty).
Example row: "Miniature Vine Trellis","Tiny Sculptural Element","","".



Usage
1. Generating Uses (paper_clip_ideas.ipynb)

Run:jupyter notebook paper_clip_ideas.ipynb


Process:
Loads existing data from paperclip_uses.csv (if present), filtering out electrical-related categories.
Generates 5 new categories at a time (e.g., "Tiny Sculptural Element") and 5 uses per category (e.g., "Dollhouse Coat Hanger").
Ensures uniqueness using semantic similarity checks.
Saves unique uses to paperclip_uses.csv after each batch.
Continues until reaching 1000 uses (configurable) or exhausting unique categories/uses.


Output:2025-10-02 02:04:58,699 - INFO - Loaded 0 existing uses and 0 categories
2025-10-02 02:04:59,123 - INFO - ✓ Saved 4 new uses from 'Tiny Sculptural Element' (Total: 4)
...
✓ Total unique uses generated: 1086
✓ Categories covered: 25
📊 Uses per category:
Tiny Sculptural Element    150
Emergency Tool Improviser  120
...
📝 Sample uses:
Dollhouse Coat Hanger
SIM Ejector Tool
...



2. Validating Uses (usage_check.ipynb)

Run:jupyter notebook usage_check.ipynb


Process:
Loads paperclip_uses.csv and processes unvalidated rows (empty Valid or Explanation).
Validates each use for feasibility and novelty using Gemini API, ensuring non-electrical applications.
Updates paperclip_uses.csv with Valid (Yes/No) and Explanation columns, saving every 15 validations.


Output:2025-10-02 02:04:58,699 - INFO - Loaded 1086 uses for validation
2025-10-02 02:05:14,532 - INFO - Incremental save after 15 validations
...
✓ Total uses validated: 1086
✓ Valid uses: 543
✓ Categories covered: 25
📊 Uses per category:
Tiny Sculptural Element    150
Emergency Tool Improviser  120
...
📝 Sample validated uses:
Use                      Category                   Valid  Explanation
Dollhouse Coat Hanger   Tiny Sculptural Element    Yes    The wire's size and bendability...
SIM Ejector Tool        Emergency Tool Improviser Yes    The wire's small size and bendability...
...



Configuration

Generation (paper_clip_ideas.ipynb):
target_uses = 1000: Set desired number of uses.
similarity_threshold = 0.75: Adjust for stricter/lenient uniqueness checks.
CALLS_PER_MINUTE = 15, PERIOD = 60: Modify rate limits based on API tier.
Customize CATEGORY_GENERATION_PROMPT and USE_GENERATION_PROMPT for alternative use cases.


Validation (usage_check.ipynb):
SAVE_INTERVAL = 15: Adjust frequency of incremental saves.
CALLS_PER_MINUTE = 15, PERIOD = 60: Modify rate limits.
Edit VALIDATION_PROMPT for different validation criteria.


Models: Uses gemini-2.5-flash-lite (generation) and gemini-2.0-flash-lite (validation). Switch to other Gemini models (e.g., gemini-pro) if needed.

License
This project is licensed under the MIT License - see the LICENSE file for details.es—edit if needed.
- **Error Handling**: Retries on exceptions, but persistent API issues may mark uses as invalid.
- **Test Project**: This is a proof-of-concept; not optimized for production-scale validation.

## Contributing
Feel free to fork and submit pull requests! Suggestions for improvements (e.g., adding more categories, better parsing) are welcome.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments
- Built with [Google Generative AI](https://ai.google.dev/).
- Inspired by creative problem-solving exercises like "101 uses for a paperclip."

If you encounter issues, open a GitHub issue or contact me! 🚀
