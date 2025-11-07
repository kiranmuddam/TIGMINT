# TIGMINT - Upgrade Notes (2025)

This document describes the major updates made to modernize the TIGMINT codebase.

## Summary of Changes

The entire codebase has been updated to use modern dependencies and resolve security vulnerabilities. The application has been tested and confirmed to start successfully.

## Updated Dependencies

### Node.js Dependencies
All Node.js packages have been updated to their latest stable versions:
- `body-parser`: 1.19.0 → 1.20.3
- `ejs`: 3.1.3 → 3.1.10 (fixes security vulnerabilities)
- `express`: 4.17.1 → 4.21.2
- `nodemon`: 2.0.4 → 3.1.9
- `python-shell`: 2.0.1 → 5.0.0
- `csvtojson`: 2.0.10 (unchanged, already current)

### Python Dependencies
Python dependencies have been updated with proper version pinning:
- Updated all packages to latest stable versions compatible with Python 3.11
- Added version constraints to ensure reproducible builds
- Replaced deprecated `bs4` with `beautifulsoup4`
- Added `snscrape` as an alternative to deprecated `twint`

### Docker
The Dockerfile has been completely rewritten:
- Updated from Python 3.7 (EOL) to Python 3.11
- Updated from Node.js 12.19.0 (EOL) to Node.js 22
- Simplified build process
- Reduced image size
- Added exiftool for metadata analysis

## Environment Requirements

### Current Versions
- **Node.js**: v22.x or higher
- **Python**: 3.11 or higher
- **npm**: 10.x or higher

### System Requirements (Linux)
```bash
sudo apt install exiftool
```

## Important Notes

### Twitter/Twint Deprecation
⚠️ **CRITICAL**: The original project used `twint` for Twitter scraping, which is now **completely broken** due to Twitter/X API changes.

**Twitter functionality will not work without updates:**
- The twint library has been removed from dependencies
- Twitter scraping features in the app will fail
- To restore Twitter functionality, you need to:
  1. Obtain Twitter API keys from developer.twitter.com
  2. Use `tweepy` library instead of `twint`
  3. Update the Python scripts in `Python_Scripts/twitter/` directory

**Affected modules:**
- `Python_Scripts/twitter/base_func.py`
- `Python_Scripts/twitter/func_call.py`
- `Python_Scripts/twitter/similar_hashtags/`
- `Python_Scripts/twitter/top_mentions_hashtags/`
- `Python_Scripts/geolocation_analysis/top_mentions_hashtags_geo.py`

### Instagram Scraping
⚠️ Instagram's website structure has changed significantly since 2020. The scraping functionality in `Python_Scripts/instagram/` may not work correctly and might need updates.

### Alternative: Use Official APIs
For production use, consider migrating to official APIs:
- **Twitter**: Use `tweepy` with Twitter API v2
- **Instagram**: Use Instagram Graph API (requires Facebook Developer account)

## Installation

### Standard Installation
```bash
# Clone repository
git clone https://github.com/TIGMINT/TIGMINT
cd TIGMINT

# Install Node.js dependencies
npm install

# Install Python dependencies
pip3 install -r requirements.txt

# Start the server
npm start
# OR
node Api/server.js
```

### Docker Installation
```bash
# Build the image
docker build -t tigmint .

# Run the container
docker run --name tigmint -p 3000:3000 tigmint

# Access the application
# Open browser to http://localhost:3000
```

## Security Improvements

1. ✅ Updated EJS from 3.1.3 to 3.1.10 (fixes CVE vulnerabilities)
2. ✅ Updated all Node.js dependencies to patch known vulnerabilities
3. ✅ Updated Python dependencies to latest secure versions
4. ✅ Removed deprecated and insecure libraries

## Build Status

✅ **Application starts successfully**
- Express server runs on port 3000
- No npm audit vulnerabilities
- All dependencies installed correctly

## Known Limitations

1. **Twitter functionality is broken** - requires API key implementation
2. **Instagram scraping may be unreliable** - depends on Instagram's website structure
3. **Username checker** - social media platforms may block scraping attempts
4. **Geo-tagging features** - depend on Twitter, which is currently non-functional

## Migration Path for Twitter Features

To restore Twitter functionality:

1. Register for Twitter API access at https://developer.twitter.com/
2. Install tweepy:
   ```bash
   pip3 install tweepy
   ```
3. Update `Python_Scripts/twitter/base_func.py` to use tweepy instead of twint
4. Add API key configuration to the application
5. Update all Twitter-related scripts

## Testing

After upgrade, the following has been verified:
- ✅ Node.js server starts without errors
- ✅ Python dependencies install successfully
- ✅ No security vulnerabilities in npm packages
- ⚠️ Twitter functionality requires API implementation
- ⚠️ Instagram functionality requires testing with live accounts

## Support

If you encounter issues:
1. Ensure you're using the correct Node.js and Python versions
2. Delete `node_modules` and reinstall: `rm -rf node_modules && npm install`
3. Clear Python cache: `pip3 cache purge`
4. Check the GitHub issues page for similar problems

## Contributors

This upgrade was performed to modernize the codebase and address security concerns. Original functionality has been preserved where possible, but some features (particularly Twitter scraping) require additional work to restore functionality due to external API changes beyond our control.
