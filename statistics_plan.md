### 1. Understanding the Goal

The primary objective is to analyze the existing miRNA gene data, calculate relevant summary statistics, and display these statistics prominently on the project's main web page. This will provide users with a high-level overview of the dataset's key metrics at a glance.

### 2. Investigation & Analysis

To formulate a robust strategy, a thorough investigation of the existing project structure and data is required. The following steps will be taken:

*   **Analyze Data Sources:**
    *   Read the contents of `expression_studies.json`, `other_studies.json`, and `study_details.json` to understand the structure, fields, and data types of the processed data that feeds the frontend. This is critical for identifying which fields can be used for statistical calculations.
*   **Examine Data Processing Logic:**
    *   Read the `process_data.py` script to understand how the original Excel file (`Tabelas_resumo_para_Hugo.xlsx`) is read, processed, and transformed into the final JSON files. This will reveal the ideal place to inject the statistics calculation logic.
*   **Review Frontend Implementation:**
    *   Read `index.html` to identify the existing layout, the libraries used (e.g., DataTables.js), and the current data loading mechanism. This will determine where the new statistics display should be placed and how it should be technically integrated.
    *   Read `ui.md` to see if there are any mockups or descriptions of the intended user interface that might guide the placement and style of the statistics display.
*   **Consult Project Documentation:**
    *   Review `GEMINI.md` and `README.md` for any high-level project goals, constraints, or technical decisions that might influence the implementation.

**Critical Questions to Answer:**

1.  What are the most meaningful statistics to derive from the data? (e.g., total number of unique miRNAs, count of up-regulated vs. down-regulated studies, distribution of studies by tissue type).
2.  Should the statistics be calculated and stored in a new, separate JSON file, or should they be embedded within one of the existing ones?
3.  Where in the `index.html` layout should the statistics be displayed to be most effective? (e.g., above the tables, in a sidebar).
4.  How will the frontend fetch and render the statistics? Will it require a new JavaScript function or can it be integrated into the existing DataTables initialization logic?

### 3. Proposed Strategic Approach

The strategy is divided into two distinct phases: backend data processing to calculate the statistics and frontend development to display them.

**Phase 1: Backend - Statistics Calculation (Python)**

1.  **Modify `process_data.py`:**
    *   Extend the existing Python script to add a new function for calculating statistics *after* the main data processing is complete.
    *   This function will read the generated `expression_studies.json` and `other_studies.json` files.
    *   It will calculate the following metrics:
        *   Total number of expression studies.
        *   Total number of "other" studies.
        *   Count of unique miRNA IDs across all studies.
        *   A breakdown of alterations (e.g., counts of "Upregulation", "Downregulation").
        *   A count of studies per tissue type.
2.  **Generate `statistics.json`:**
    *   The script will save the calculated statistics into a new, dedicated file named `statistics.json`. This keeps the statistics data separate from the raw table data, making it clean and easy to consume for the frontend. The structure would be a simple key-value object, for example:
        ```json
        {
          "total_expression_studies": 150,
          "unique_mirnas": 85,
          "alteration_counts": {
            "Upregulation": 70,
            "Downregulation": 80
          },
          "tissue_counts": {
            "Brain": 50,
            "Blood": 100
          }
        }
        ```

**Phase 2: Frontend - Statistics Display (HTML/JavaScript)**

1.  **Update `index.html`:**
    *   Add a new HTML `div` container with a unique ID (e.g., `id="statistics-container"`) above the existing DataTables containers.
    *   This container will have placeholders for each statistic that will be displayed.
2.  **Create a JavaScript Loading Function:**
    *   In a `<script>` tag within `index.html`, create a new JavaScript function that uses the `fetch()` API to load the `statistics.json` file.
    *   Upon successful loading, this function will parse the JSON and use `document.getElementById()` or similar methods to populate the placeholder elements with the calculated statistics.
    *   This script will be called when the page loads, ensuring the statistics are displayed immediately.

### 4. Verification Strategy

*   **Backend Verification:**
    *   The correctness of the numbers in `statistics.json` must be manually verified against the content of `expression_studies.json` and `other_studies.json`. A separate, temporary Python script could be written to perform an independent count to double-check the logic in `process_data.py`.
*   **Frontend Verification:**
    *   Load the `index.html` page in a web browser and confirm that the statistics are displayed correctly and match the values in `statistics.json`.
    *   Check the browser's developer console for any errors related to fetching or parsing the JSON file.
    *   Ensure the new statistics display does not negatively impact the rendering or functionality of the existing DataTables.

### 5. Anticipated Challenges & Considerations

*   **Data Quality:** The accuracy of the statistics is entirely dependent on the quality and consistency of the data in the JSON files. Inconsistent naming (e.g., "Brain" vs. "brain") in the tissue field could lead to incorrect counts. The Python script should include logic to normalize or clean this data before counting.
*   **Asynchronous Loading:** The statistics and the main tables will be loaded asynchronously. Care must be taken to ensure that a failure in loading the statistics does not prevent the main tables from loading and vice-versa.
*   **UI/UX Design:** The statistics should be presented in a clear, visually appealing, and easily understandable manner. Simply displaying numbers might not be as effective as adding labels or simple charts. The final design should be considered to ensure it enhances the user experience.
*   **Maintainability:** By separating the statistics calculation (Python) from the display (JavaScript) and using a dedicated JSON file, the solution remains modular and easy to maintain. Future changes to the statistics will only require modifying the Python script, without touching the frontend code.