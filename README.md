# Medicare Anomaly Detection: Identifying Opioid Over-Prescription

### Executive Summary
This project implements a scalable **Anomaly Detection Pipeline** to identify healthcare providers who statistically over-prescribe opioids compared to their peers.

Leveraging **Snowflake** for compute-heavy statistical processing and **Python** for orchestration, the system analyzed over **200,000 Medicare Part D prescription records**. It successfully identified high-risk providers with prescription rates exceeding **12 standard deviations (Z-Score > 12)** above the mean for their specialty.

---

### Architecture & Workflow
The project follows a Modern Data Stack **ELT (Extract, Load, Transform)** pattern:

1. **Ingestion (Extract & Load):**
   - Raw government data (CMS.gov) is ingested into a **Snowflake Data Warehouse**.
   - Data is staged in a raw schema (`RAW_PRESCRIBER_DATA`) without pre-processing.

2. **Feature Engineering (Transform in SQL):**
   - *Problem:* The raw data lacked classification flags for opioid drugs.
   - *Solution:* Engineered a custom classification system using **SQL pattern matching (`ILIKE`)** to identify opioid compounds (Oxycodone, Fentanyl, Morphine, etc.).

3. **Statistical Modeling (Snowflake Window Functions):**
   - Cohorted physicians by **Specialty** (e.g., comparing Dentists only to other Dentists).
   - Calculated **Z-Scores** directly within the database to handle scale:
     $$Z = \frac{(X - \mu)}{\sigma}$$

4. **Visualization (Python):**
   - Extracted high-risk outliers to Jupyter for visualization using **Seaborn**.

---

### Key Findings
* **The "Unicorn" Outliers:** While most outliers fell within the expected range ($Z \approx 3$), the model detected extreme anomalies with Z-Scores as high as **12**.
* **Specialty Bias Removed:** By normalizing against specialty averages, the model correctly ignored Pain Management specialists and focused on family practitioners with unexplained high volumes.

---
