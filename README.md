# Medical Code Search

A search interface for ICD-10 codes built with vanilla JavaScript.  Can use MiniSearch or Fuse.js for searching. Features smart prefix matching, fuzzy search, and medical term abbreviation handling.

![Medical Code Search Interface](https://img.shields.io/badge/Status-Active-green) ![JavaScript](https://img.shields.io/badge/JavaScript-ES6%2B-yellow) ![License](https://img.shields.io/badge/License-MIT-blue)

## Features

- **Smart Search**: Prefix matching, fuzzy search, and intelligent stemming (MiniSearch only. Fuse.js supports fuzzy matching.)
- **Medical Term Aware**: Handles common medical abbreviations (e.g., "preg" → "pregnancy") (MiniSearch only)
- **Fast Performance**: Real-time search with debouncing and optimized indexing
- **Code Boosting**: Prioritizes exact medical code matches in results

## Demo

Try searching for:
- `diabetes preg` - Finds gestational diabetes codes
- `O24` - Shows all codes starting with O24
- `hypert` - Expands to hypertension-related codes
- `gest diab` - Gestational diabetes with abbreviation handling

## Quick Start

1. Clone the repository
2. Open `minisearch_index.html` or `fuse_index.html` in a modern web browser
3. Start searching medical codes immediately


## File Structure

```
medical-code-search/
├── minisearch_index.html  # Search using MiniSearch
├── fuse_index.html        # Search using Fuse
├── medical-codes.json     # Medical codes database
├── package.json           # Node.js dependencies
├── package-lock.json      # Dependency lock file
└── README.md              # This file
```

## Configuration

### Custom Medical Codes Data

Create a `medical-codes.json` file in the root directory with your medical codes:

```json
[
  {
    "code": "E11.9",
    "description": "Type 2 diabetes mellitus without complications"
  },
  {
    "code": "I10",
    "description": "Essential hypertension"
  }
]
```
### Search Configuration - Fuse
The search engine can be customized in the `setupFuse()` method:
```javascript
const options = {
    includeScore: true,
    threshold: 0.4, // Slightly more permissive for fuzzy matching
    ignoreLocation: false, // Enable location-based scoring
    findAllMatches: true,
    minMatchCharLength: 2,
    keys: [
        {
            name: 'code',
            weight: 0.8 // Higher weight for exact code matches
        },
        {
            name: 'description',
            weight: 0.2
        }
    ]
};
```

### Search Configuration - MiniSearch

The search engine can be customized in the `setupMiniSearch()` method:

```javascript
this.miniSearch = new MiniSearch({
  fields: ['code', 'description'],
  storeFields: ['code', 'description'],
  searchOptions: {
    boost: { code: 3 },        // Boost code matches
    prefix: true,              // Enable prefix search
    fuzzy: 0.2,               // Fuzzy matching threshold
    combineWith: 'AND'        // Search term combination
  }
});
```

### Medical Abbreviations - MiniSearch

Add custom abbreviations in the `processTerm` function:

```javascript
const abbreviations = {
  'preg': 'pregnancy',
  'gest': 'gestational',
  'diab': 'diabetes',
  'hypert': 'hypertension',
  // Add your custom abbreviations here
};
```

## Browser Compatibility

- **Modern Browsers**: Chrome 61+, Firefox 60+, Safari 12+, Edge 79+
- **ES6 Modules**: Required for the current implementation
- **Fetch API**: Used for loading external JSON data

## Dependencies

### Runtime Dependencies
- **MiniSearch**: Loaded from CDN (https://cdn.skypack.dev/minisearch)
- **Fuse.js**: Loaded from CDN (https://cdnjs.cloudflare.com/ajax/libs/fuse.js/6.6.2/fuse.min.js)