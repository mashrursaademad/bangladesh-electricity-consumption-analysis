README.txt
-------------------------------------------------------------------------------
Structured Dataset of Daily Electricity Demand, Generation, Load Shedding,
and Supply Constraints in Bangladesh (2019–2024)
-------------------------------------------------------------------------------

This README outlines the end-to-end methodology for constructing a structured,
multi-version dataset from the Bangladesh Power Development Board (BPDB)'s
daily electricity reports. The dataset spans from November 21, 2019 to 
December 30, 2024 and is released as five sequential Excel files named:

  - version 1.xlsx
  - version 2.xlsx
  - version 3.xlsx
  - version 4.xlsx
  - version 5.xlsx

Each version represents a distinct stage in data cleaning, enrichment, or
preparation for modeling. A separate file, `column_descriptions.xlsx`,
describes each variable in detail.

-------------------------------------------------------------------------------
1. DATA SOURCE
-------------------------------------------------------------------------------
All reports were collected from the BPDB’s public archive:
https://misc.bpdb.gov.bd/daily-generation-archive

Each report corresponds to one calendar day and is formatted as a PDF. The
target table is typically found on the second page and includes daily demand,
generation, load shedding, and supply constraint metrics.

-------------------------------------------------------------------------------
2. PDF DOWNLOADING
-------------------------------------------------------------------------------
A Python script was used to automate the download of 1,867 PDFs.

- HTTP requests were issued using the `requests` library.
- PDF links were extracted from archive pages using `BeautifulSoup`.
- Only files matching `summary_*.pdf` or `####report.pdf` were retained.
- Files were saved locally with original filenames.

-------------------------------------------------------------------------------
3. TEXT EXTRACTION AND PARSING
-------------------------------------------------------------------------------
Each PDF was processed using `pdfplumber` to extract text from the second page.

- Regular expressions (`re`) were used to parse specific numeric values.
- Metrics such as daily demand, supply, generation, limitations, and zone-wise
  values were extracted into dictionaries.
- Inconsistent field names were standardized using a mapping function.
- All outputs were appended into a master `pandas` DataFrame.

-------------------------------------------------------------------------------
4. DATA CONSOLIDATION
-------------------------------------------------------------------------------
Parsed values were written to a cumulative Excel file. If a record for a day
already existed, it was replaced or merged to avoid duplication. Parsing
errors (e.g., unreadable layout) were logged separately.

Missing fields for certain records were filled with `None` to maintain schema
consistency across days.

-------------------------------------------------------------------------------
5. DATA CLEANING AND FEATURE ENGINEERING
-------------------------------------------------------------------------------
The final dataset versions were produced through the following steps:

- version 1.xlsx: Raw parsed data with inconsistencies retained.
- version 2.xlsx: Duplicates removed; manually recovered missing rows added.
- version 3.xlsx: Holiday name and category columns added using a manually
  curated list of national and religious holidays.
- version 4.xlsx: Imputation of missing and invalid values using forward/
  backward interpolation and conditional logic; additional columns for Year
  and Month added.
- version 5.xlsx: Outliers in key numeric fields were smoothed using a
  rolling median. Continuous fields were normalized for modeling readiness.

-------------------------------------------------------------------------------
6. DEPENDENCIES
-------------------------------------------------------------------------------
- Python 3.8+
- pandas
- pdfplumber
- requests
- BeautifulSoup4
- re
- openpyxl (for Excel export)

-------------------------------------------------------------------------------
7. DIRECTORY STRUCTURE
-------------------------------------------------------------------------------
Main Folder Structure:

/version 1/ → Contains version 1.xlsx (Raw extracted dataset)  
/version 2/ → Contains version 2.xlsx (Cleaned and verified dataset)  
/version 3/ → Contains version 3.xlsx (With holiday annotations)  
/version 4/ → Contains version 4.xlsx (With imputed and structured fields)  
/version 5/ → Contains version 5.xlsx (Final normalized dataset)  

Files in main folder:
- column_descriptions.xlsx → Descriptions of each variable  
- README.txt → This file


-------------------------------------------------------------------------------
CONTACT
-------------------------------------------------------------------------------
For questions or clarification, please contact:

Md. Ikrama Hossain  
Email: ikramahossain@iut-dhaka.edu

Tasnia Nafs  
Email: tasnianafs@iut-dhaka.edu

Sakif Yeaser  
Email: sakifyeaser@iut-dhaka.edu
