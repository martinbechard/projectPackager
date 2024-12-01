/\*\*

- Copyright (c) 2024 Martin Bechard <martin.bechard@DevConsult.ca>
- This software is licensed under the MIT License.
- File: scripts/README-ProjectPackager.md
- This document was generated by Claude Sonnet 3.5, with the assistance of my human mentor
- A comprehensive guide to the Project Packager utility - because packaging code is like gift wrapping for developers!
  \*/

# Project Packager Utility

## Overview

The Project Packager is a utility designed to package your project files for easier sharing and analysis. It traverses your project directory, concatenates files of similar types, and saves them in a specified output folder. This tool is perfect for when you need to bundle your code for review, archiving, or just to marvel at your project's complexity in one place.

Key features include:

- File concatenation and organization
- Unpackaging of previously packaged files
- Built-in testing feature with watch mode
- Proper file documentation with headers and comments
- Support for multiple file types and comment styles
- File tree generation for project structure visualization

## Installation

1. Place the `projectPackager.js` and `projectPackager.json` files in a `scripts` folder in your project root.
2. Install the required dependencies:

```bash
pnpm add yargs
pnpm add -D jest @types/jest
```

## Usage

Run the script from your project root directory:

```bash
node scripts/projectPackager.js [options]
```

## Command-line Options

- `--verbose`, `-v`: Run with verbose logging (for when you want to know EVERYTHING)
- `--unpackage`, `-u`: Unpackage previously packaged files (for when you need to spread your wings again)
- `--expand`, `-e`: Expand the files being packaged into the output directory (default: false)
- `--test`, `-t`: Run unit tests (keep your code in check!)
- `--watch`: Run tests in watch mode (default: true) (for the vigilant developer)
- `--configPath`: Path to the configuration file (default: scripts/projectPackager.json)
- `--dumpFileMap`: Dump all files as a file map (experimental)
- `--generateFileTree`: Generate a file tree suitable for README
- `--help`, `-h`: Show help (when all else fails, read the manual)

Examples:

```bash
node scripts/projectPackager.js --verbose
node scripts/projectPackager.js --unpackage
node scripts/projectPackager.js --expand
node scripts/projectPackager.js --test
node scripts/projectPackager.js --test --watch=false
node scripts/projectPackager.js --configPath=./custom-config.json
node scripts/projectPackager.js --generateFileTree
```

## Configuration

The `projectPackager.json` file contains the following configuration options:

```json
{
  "outputFolder": "project",
  "skipFolders": [
    "node_modules",
    "project",
    ".next",
    ".git",
    "logs",
    "data",
    "test_runs"
  ],
  "fileTypes": {
    "js": {
      "extensions": [".js", ".ts", ".tsx", ".jsx"],
      "style": "block",
      "prefix": "/**",
      "linePrefix": " *",
      "suffix": " */"
    },
    "json": {
      "extensions": [".json"],
      "style": "json",
      "attributes": ["_copyright", "_license", "_file"],
      "exclude": ["package.json", "package-lock.json", "tsconfig.json"]
    },
    "md": {
      "extensions": [".md", ".mdx"],
      "style": "markdown",
      "prefix": "<!--",
      "linePrefix": "",
      "suffix": "-->"
    },
    "css": {
      "extensions": [".css"],
      "style": "block",
      "prefix": "/*",
      "linePrefix": " *",
      "suffix": "*/"
    },
    "html": {
      "extensions": [".html"],
      "style": "html",
      "prefix": "<!--",
      "linePrefix": "",
      "suffix": "-->"
    },
    "py": {
      "extensions": [".py"],
      "style": "block",
      "prefix": "\"\"\"",
      "linePrefix": "",
      "suffix": "\"\"\""
    }
  },
  "copyright": "Copyright (c) 2024 Martin Bechard <martin.bechard@DevConsult.ca>",
  "backupFolder": "backups",
  "maxNestingLevel": 1
}
```

- `outputFolder`: The folder where packaged files will be saved (default: "project")
- `skipFolders`: An array of folder names to skip during processing
- `fileTypes`: An object defining how different file types should be processed:
  - `extensions`: Array of file extensions to process
  - `style`: Comment style ("block", "json", "markdown", "html")
  - `prefix`: Comment opening syntax
  - `linePrefix`: Prefix for each comment line
  - `suffix`: Comment closing syntax
  - `attributes`: (For JSON files) Attributes to include in JSON metadata
  - `exclude`: Files to exclude from processing
- `copyright`: The copyright notice to be added to files missing it
- `backupFolder`: The folder name for storing backups of modified files
- `maxNestingLevel`: Maximum directory nesting level for output organization

## Features

1. **File Concatenation**: Combines files of similar types from your project into markdown files, organized by directory structure up to the configured nesting level.
2. **Progress Tracking**: Displays progress information during processing.
3. **File Headers**: Adds or updates file headers with:
   - Copyright notice
   - License information
   - File path
   - AI attribution (if configured)
4. **Multiple Comment Styles**: Supports different comment styles for various file types:
   - Block comments for JS/CSS (/\* \*/)
   - JSDoc style for JavaScript (/\*\* \*/)
   - JSON metadata (\_copyright, \_license, \_file)
   - HTML/Markdown comments (<!-- -->)
   - Python docstrings (""")
5. **Backup Creation**: Creates backups of files before modifying them.
6. **File Tree Generation**: Creates a markdown-friendly visualization of your project's structure.
7. **Unpackaging**: Can unpackage previously packaged files back into their original structure.
8. **Built-in Testing**: Includes comprehensive unit tests with watch mode support.
9. **Flexible Output**: Can either create concatenated markdown files or expand files in the output directory.

## Output

The utility creates a folder (default: `project`) in your project root, containing:

1. Markdown files for each directory (up to maxNestingLevel), containing the concatenated contents of files in that directory
2. A `backups` folder with original versions of modified files (if applicable)
3. A `fileTree.md` file containing the project structure (if --generateFileTree is used)
4. If run with `--expand`, individual processed files in their original structure

## Running Tests

To run the built-in unit tests, use the `--test` or `-t` option:

```bash
node scripts/projectPackager.js --test
```

By default, this will run the tests in watch mode. To run tests without watch mode, use:

```bash
node scripts/projectPackager.js --test --watch=false
```

This will execute the Jest tests defined in `projectPackager.test.js`.

## Best Practices

1. Always run the script with the `--verbose` option first to see detailed logs
2. Review the `projectPackager.json` configuration before running to ensure correct settings
3. Use the `--expand` option when you need to preserve the original file structure in the output
4. When unpackaging, ensure you have backups of any files that might be overwritten
5. Run the tests regularly, especially after making changes to the utility
6. Use the `--generateFileTree` option to maintain up-to-date documentation of your project structure

## Troubleshooting

If you encounter issues:

1. Check the console output for error messages and warnings
2. Verify that the `projectPackager.json` configuration is correct
3. Ensure you have the necessary permissions to read/write files in your project directory
4. If files are being modified, check the `backups` folder for original versions
5. Run the tests using the `--test` option to check for any functional issues
6. If the output structure seems incorrect, check the `maxNestingLevel` configuration

For further assistance, please refer to the project's issue tracker or contact the maintainer.

Happy packaging, and may your code always compile on the first try!
