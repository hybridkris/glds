# Automated Gravitational Lensing Detection System – Implementation Plan

## Astronomical Data Query (Google Colab & Public Databases)
Use **Google Colab** as a controlled environment to search for candidate black holes via gravitational lensing data. This notebook will leverage astronomy APIs to find objects of interest.

- **Key Technologies:** The Colab notebook will use the **Astroquery** library (part of Astropy) to retrieve data from public astronomical databases. Potential data sources include **SIMBAD** (general object database), **Gaia DR3** (which has microlensing event catalogs), and the **NASA Exoplanet Archive** (for lensing or microlensing events). 
- **Data Retrieval Design:** Use Astroquery to perform queries based on criteria indicative of gravitational lensing. For example, query the Gaia DR3 **microlensing** catalog for events where the lensing object could be a dark remnant (black hole). Similarly, query SIMBAD for known gravitational lens systems or candidates. Filter the results for relevant fields such as object name/ID, coordinates (RA/Dec), magnitude/brightness, redshift, and any lensing-related parameters (e.g., multiple image count or lensing strength if available). Limit to strong candidates.
- **Output Format:** Structure the Colab code to output a list (e.g., a Pandas DataFrame) of candidate objects with columns like **Name**, **RA**, **Dec**, **Magnitude**, **Lensing Indicator** (yes/no or probability), and any other useful notes. 
- **Integration Notes:** Once the query is executed and filtered, the notebook will **write the results to a Google Sheet**. Using the Google Sheets API (via a Python library like **gspread**) allows pushing the table directly from Colab. Ensure the sheet ID and credentials are configured securely. 

---

## Candidate Data Management (Google Sheets)
The **Google Sheet** will act as a lightweight database for candidate objects and later for results. Two spreadsheets (or two tabs in one spreadsheet) are recommended: one for **Candidates** and one for **Observation Results**.

- **Sheet Structure:** Design the **Candidates sheet** with clear headers: *Object ID/Name*, *Right Ascension (RA)*, *Declination (Dec)*, *Magnitude*, *Lensing Score/Flag*, *Other Notes*. The **Results sheet** will store observations. 
- **Writing Data (Automation):** From Colab, use **gspread** or the Google Sheets API to insert data. 
- **Integration Notes:** This Google Sheet is accessible by both the backend (Colab notebooks) and the frontend (web app). 

---

## Prioritization with ChatGPT (OpenAI API Analysis)
Utilize **ChatGPT via OpenAI API** to analyze the candidate list and suggest the best observation targets. 

- **Key Technologies:** The OpenAI **ChatCompletion API** (e.g., GPT-4 or GPT-3.5). 
- **Design Recommendations:** Read the candidate data from the Google Sheet. Formulate a prompt providing necessary data and asking for recommendations.
- **OpenAI API Usage:** Include your OpenAI API key securely in the Colab environment.
- **Integration Notes:** The analysis guides the user on which target to pick but doesn't automatically constrain the system.

---

## Frontend Web Application (GitHub Pages)
The system includes a **static frontend** hosted on **GitHub Pages** to display candidates and facilitate observations.

- **Data Display and Filtering:** Fetch the candidate list from the Google Sheet and display it in a tabular format. 
- **UI and Filters:** Enhance user experience by adding filters and sorting for the target list. 
- **Target Coordinates & Telescope Integration:** Each target row should have an option to **“Go-to”** that object with the Unistellar telescope. 
- **Design Considerations:** Ensure the Google Sheet URL or API key is not exposing any write access.

---

## Image Upload & Lensing Detection (Teachable Machine Model)
Allow the user to **upload raw images from the Unistellar Odyssey telescope** and analyze them for gravitational lensing.

- **Model Training (Teachable Machine):** Train an image classifier to distinguish between images *with gravitational lensing* and those without. 
- **Web Integration of Model:** Include the model files in the GitHub Pages repository or use a Teachable Machine provided hosted link.
- **File Upload UI:** Create an **upload form** on the web page for selecting an image. 
- **Detection and Feedback:** Display results to the user, indicating the confidence of lensing detection. 
- **Reporting Results to Google Sheets:** Log positive detections to the **Results Google Sheet** using Google Apps Script, SheetMonkey, or a hidden Google Form.
- **Integration Notes:** Ensure proper logging and feedback mechanisms for successful uploads.

---

## Observation Results & Visualization (Second Colab Notebook)
A **Google Colab** notebook will aggregate and analyze the outcomes stored in the Results sheet.

- **Key Technologies:** Use **Pandas**, **Matplotlib**, or **Plotly** for data analysis and visualization. 
- **Data Retrieval:** Load data from the Results Google Sheet.
- **Analysis & Visualization:** Summarize the success rate and key findings using statistical analysis and visualizations.
- **Reporting/Output:** The notebook can output a concise report with visuals.
- **Integration Notes:** This Colab is used for reviewing and summarizing results after observation campaigns.

---

## Integration and Automation Considerations
- **Modularity:** Ensure components communicate through well-defined interfaces (primarily the Google Sheets).
- **Automation:** Consider scheduling the **candidate search Colab** to run at intervals. 
- **Security:** Keep secrets and keys out of public code. 
- **Data Validation:** Include checks in the Colab scripts to ensure data quality. 
- **User Experience:** Ensure the workflow is straightforward for practical use at the telescope.
- **Maintenance:** Host code in a GitHub repository for version control.
- **Scalability and Future Enhancements:** Future extensions can include a database or using Google BigQuery for storing results.

---

## Sources
- **Astroquery** documentation.
- **Google Sheets API** usage.
- **OpenAI API** usage guidelines.
- **Teachable Machine model integration**.

---

By following this implementation plan, you will create a cohesive system where data flows from astronomical databases, through an AI decision-making layer, to a user-friendly interface, and back into analysis – enabling an efficient search for gravitationally lensed black holes using the Unistellar telescope and modern cloud tools. 
