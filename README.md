# paper_clip_ideas

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
(Note: Basic Python libraries like `logging`, `os`, and `csv` are included in the standard library.)

## Setup
1. Clone the repository:
   ```
   git clone https://github.com/your-username/wire-uses-validator.git
   cd wire-uses-validator
   ```

2. Add your API key:
   - Open `usage_check.ipynb`.
   - Replace the placeholder in `API_KEY = 'AIzaSyD5YO3-yDLVQFkaheIewRHm-V45jIpG4Gs'` with your actual Gemini API key.
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
     📊 Uses per category:
     ...
     📝 Sample validated uses:
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
- **Non-Conductive Assumption**: The prompt assumes the wire is non-conductive to avoid electrical uses—edit if needed.
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
