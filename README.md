# Theses & Dissertations Search

A simple, self-contained HTML page for searching a collection of theses and dissertations. It features client-side fuzzy search capabilities and allows users to export search results as a PDF.

This tool is designed to work without a traditional backend database. Instead, it fetches raw text data from Google Drive, parses it, and builds a searchable index in the user's browser.

---

## Features

* **Fuzzy Search:** Uses **Fuse.js** to provide powerful, typo-tolerant search across titles, authors, departments, years, and abstracts.
* **PDF Export:** Uses **html2pdf.js** to allow users to download their search results as a formatted PDF document.
* **Dynamic Data Fetching:** Pulls data directly from text files hosted on Google Drive via a simple proxy.
* **Client-Side Parsing:** Includes a parser to convert unstructured text blocks into structured JSON objects (title, author, year, etc.).
* **Zero-Backend:** The entire application (HTML, CSS, JS) runs in the browser.

---

## How It Works

* **Data Fetching:** On page load, the script fetches data from multiple Google Drive files specified in the `DRIVE_FILE_IDS` array.
* **Proxy:** It uses a **Google Apps Script proxy** (defined in `SCRIPT_PROXY_URL`) to access the raw text content of these files.
* **Parsing:** The raw text from each file is split into individual thesis entries using the `splitIntoTheses` function. Each entry is then parsed using regular expressions (`parseOneThesis`) to extract fields like `thesis_title`, `full_name`, `year`, and `abstract`.
* **Indexing:** All parsed thesis objects are compiled into a single array and indexed by **Fuse.js** to create a searchable database in memory.
* **Search:** When a user submits the form, the query is run against the **Fuse.js** index, which returns a list of matching results.
* **Display:** The results are formatted as HTML and injected into the `#results` container.
* **Export:** Clicking the "Download Results as PDF" button uses **html2pdf.js** to capture the contents of the `#results` div and save it as a PDF.

---

## Usage

1.  Clone this repository or download the `index.html` file.
2.  Open the `index.html` file in any modern web browser.
3.  Wait for the status message to change from "Loading data…" to "Loaded [X] theses."
4.  Type your search query into the input box and press Enter or click "Search".
5.  To save the results, click the "Download Results as PDF" button.

---

## Configuration & Dependencies

This file relies on external scripts and a specific data setup. To adapt this for your own use, you will need to modify the following:

### 1. Dependencies (CDN)

The tool loads two external libraries from CDNs:

* **Fuse.js (v7.0.0):** For fuzzy search.
* **html2pdf.js (v0.10.1):** For PDF generation.

### 2. JavaScript Configuration

All configuration is at the top of the `<script>` tag.

**`DRIVE_FILE_IDS`**: This array contains the Google Drive File IDs for your source text files. You must replace these with the IDs of your own files.

```javascript
const DRIVE_FILE_IDS = [
  "YOUR_FILE_ID_1",
  "YOUR_FILE_ID_2"
];
