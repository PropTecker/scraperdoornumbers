# Rightmove Property Address Scraper

A Google Colab notebook that scrapes property addresses from Rightmove search results using Chrome extension authentication.

## Features

- 🔐 Chrome extension integration for session management
- 📍 Multi-outcode support via JSON configuration
- 📄 Automatic pagination (increments by 24 per page)
- 🎯 Smart filtering (excludes "only postcode found" entries)
- 📊 Excel export of all collected addresses
- 👤 User-friendly interface for non-technical users

## Quick Start

### 1. Open in Google Colab

1. Upload the `rightmove_scraper.ipynb` file to Google Colab, or
2. Open directly from GitHub in Colab

### 2. Prepare Your Files

You'll need two files:

#### Chrome Extension File
- Your Chrome extension (.zip or .crx format)
- This extension handles authentication/session management for Rightmove
- Keep this file ready on your local machine

#### Outcodes JSON File
- A JSON file containing the outcodes you want to search
- Format: `[{"code":1,"outcode":"AB10"},{"code":2,"outcode":"AB11"}, ...]`
- **code**: The numeric location identifier used by Rightmove
- **outcode**: The postcode outcode (e.g., "SW16", "AB10")
- See `sample_outcodes.json` for an example

### 3. Run the Notebook

Simply run each cell in order:

1. **Cell 1-2**: Install dependencies and import libraries
2. **Cell 3**: Upload your Chrome extension when prompted
3. **Cell 4**: Upload your outcodes JSON file when prompted
4. **Cell 5-6**: Setup functions (runs automatically)
5. **Cell 7**: The scraper runs and processes all outcodes
6. **Cell 8**: Export results to Excel and download

## How It Works

### URL Structure
The scraper builds Rightmove URLs with the complete format:
```
https://www.rightmove.co.uk/property-for-sale/find.html?useLocationIdentifier=true&locationIdentifier=OUTCODE%5E{code}&radius=0.0&_includeSSTC=on&index={index}&sortType=2&channel=BUY&transactionType=BUY&displayLocationIdentifier={outcode}.html&includeSSTC=true
```

Where:
- `{code}` is the numeric location identifier (e.g., 2502)
- `{outcode}` is the postcode outcode (e.g., SW16)
- `{index}` is the pagination offset (0, 24, 48, etc.)

Example URLs:
- **Page 1**: index=0
- **Page 2**: index=24
- **Page 3**: index=48

### Address Extraction
For each property listing, the scraper:
1. Finds links with `target="_blank" rel="noopener noreferrer"`
2. Extracts the address text from the link
3. Checks for "Only postcode found" indicator
4. If found, skips the entry
5. Otherwise, adds the address to results

### Output Format
The Excel file contains:
- **Code**: The numeric location identifier
- **Outcode**: The postcode outcode searched
- **Address**: The full property address

## Example Outcodes JSON

```json
[
  {"code": 1, "outcode": "AB10"},
  {"code": 2, "outcode": "AB11"},
  {"code": 3, "outcode": "AB12"},
  {"code": 4, "outcode": "AB15"},
  {"code": 5, "outcode": "AB16"}
]
```

## Troubleshooting

### WebDriverException or Browser Initialization Errors
- **Most common issue**: This typically happens when ChromeDriver can't start
- **First steps**:
  1. Ensure the installation cell (Step 1) ran completely without errors
  2. Check for any error messages during installation
  3. Restart the Colab runtime: `Runtime → Restart runtime`
  4. Re-run ALL cells from the beginning in order
- **Binary location**: The notebook is configured to use `/usr/bin/chromium-browser`
- **ChromeDriver path**: Should be at `/usr/bin/chromedriver` after installation
- **If error persists**:
  - Try running the installation cell again
  - Check if there are any system updates that need to be applied
  - The notebook includes detailed error messages to help diagnose the issue
- **Extension issues**: If you see warnings about extension loading, the scraper will continue without it
  - This is normal and expected in some cases
  - You may need to handle authentication manually or use a different approach

### No addresses found
- **Check extension**: Make sure your Chrome extension is properly authenticated
- **Try manual login**: You may need to manually log in through the extension before scraping
- **Check outcodes**: Verify your outcodes are valid and exist on Rightmove
- **Extension warnings**: If you see a warning about extension extraction, the scraper will continue but may not be authenticated

### Scraper is slow
- This is normal! Each page needs time to load
- The scraper includes delays to avoid being blocked
- Processing time depends on the number of outcodes and properties

### Browser errors
- Make sure you're running in Google Colab (not locally)
- Re-run the installation cell if you see Chrome-related errors
- Check that your extension file is not corrupted
- The browser runs in headless mode (no visible window) - this is normal for Colab

### Excel file issues
- If download fails, check your browser's pop-up blocker
- The file is also saved in the Colab environment (left sidebar, Files section)
- You can manually download it from there

## Technical Details

### Dependencies
- `selenium==4.15.2` - Web automation
- `openpyxl==3.1.2` - Excel file creation
- `pandas==2.1.3` - Data handling

### Browser Configuration
- Runs in headless mode (no visible browser window)
- Configured for Colab environment
- Extension loaded automatically

### Safety Features
- Page limit of 100 pages per outcode (prevents infinite loops)
- Timeouts on page loads
- Error handling for missing elements
- Automatic browser cleanup

## Notes

- **Google Colab Only**: This notebook is designed specifically for Google Colab
- **Extension Required**: You must provide a working Chrome extension
- **Rate Limiting**: The scraper includes delays to be respectful to Rightmove's servers
- **Data Privacy**: All processing happens in your Colab session; no data is shared externally

## Support

If you encounter issues:
1. Re-read the instructions carefully
2. Check the troubleshooting section
3. Verify your input files are correctly formatted
4. Try with a smaller set of outcodes first to test

## License

This project is provided as-is for educational and research purposes.
