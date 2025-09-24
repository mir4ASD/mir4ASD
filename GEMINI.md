# Project: miRNA Genes Associated with ASD

This project aims to create a simple frontend to display information about miRNA genes associated with Autism Spectrum Disorder (ASD). The data is provided in an Excel file and will be displayed in two interactive JavaScript tables on a static HTML page.

## 1. Objectives

*   Develop a static HTML frontend to display miRNA gene data.
*   Use the DataTables.js library to create two interactive tables.
*   Process data from an Excel file (`Tabelas_resumo_para_Hugo.xlsx`).
*   Create a Python script to read the Excel file and convert the data into JSON format.
*   The frontend will load the JSON data to populate the tables.
*   The existing code in the directory is for reference only and will be adapted.

## 2. Data Source

*   **Excel File:** `Tabelas_resumo_para_Hugo.xlsx`
*   **Sheets (after processing):**
    *   **`miRNA_expression_studies`:**
        *   `miRNA ID`
        *   `miRNA mature ID`
        *   `Alteration`
        *   `Tissue`
        *   `Reported`
        *   `# studies upregulation`
        *   `# studies downregulation`
        *   `Total studies`
        *   `StudyDetails` (nested object)
    *   **`miRNA_other_studies`:**
        *   `miRNA ID`
        *   `miRNA mature ID`
        *   `Alteration`
        *   `Study description`
        *   `StudyDetails` (nested object)
    *   **`miRNA_study_details`:**
        *   `Study`
        *   `Title`
        *   `DOI`
        *   `Study Type`
        *   `Tissue type`
        *   `Tissue - subtype`
        *   `# ASD Samples`
        *   `# Control Samples`
        *   `Others`

## 3. Data Processing

A Python script (`process_data.py`) will be created to perform the following actions:

*   Use the `pandas` library to read the three sheets from the Excel file.
*   In the `miRNA_expression_studies` sheet, split rows that contain multiple studies into individual rows for each study.
*   Convert the processed data from each sheet into three separate JSON files:
    *   `expression_studies.json`
    *   `other_studies.json`
    *   `study_details.json`

## 4. Frontend

*   A new `index.html` file will be created.
*   The page will use the DataTables.js library for creating interactive tables.
*   Two tables will be displayed: one for expression studies and one for other studies.
*   The tables will have search and filter functionality for all columns, with a focus on: `miRNA hairpin`, `miRNA mature`, `Alteration`, and `Reported`.
*   The main tables will link to the study details, which will be displayed on the same page.

## 5. Plan

1.  **Phase 1: Data Processing (Python)**
    *   Create the `process_data.py` script.
    *   Implement the logic to read, process, and convert the Excel data to JSON files.
2.  **Phase 2: Frontend Development (HTML/JavaScript)**
    *   Create the `index.html` file.
    *   Include DataTables.js and create the table placeholders.
    *   Write the JavaScript code to load the JSON data and initialize the DataTables.
    *   Implement the search, filter, and linking functionalities.
