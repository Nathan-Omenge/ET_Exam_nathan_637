# Data Mining and Warehousing Mid Semester Project – Airline Tweets Analysis

## 1. Project Overview
This project demonstrates the **Extract (E)** and **Transform (T)** phases of the ETL (Extract, Transform, Load) process using the **Airline Tweets dataset** pulled from Kaggle, which contains customer feedback directed at major U.S. airlines on Twitter.  

The goal of this project is to extract, inspect, validate, and transform the dataset into a clean and structured format ready for analysis. Through these steps, the dataset becomes suitable for downstream analytical workflows such as sentiment modeling and service quality trend analysis.

---

## 2. Data Source
- **Dataset:** Airline Tweets Dataset pulled from Kaggle 
- **Description:** Contains over 14,000 tweets directed at six major U.S. airlines. Each record includes details such as the tweet text, sentiment label, confidence scores, tweet location, user timezone, and timestamp.  
- **Fields Include:**
  - `tweet_id`, `airline_sentiment`, `airline_sentiment_confidence`
  - `negative_reason`, `negative_reason_confidence`
  - `airline`, `name`, `text`, `tweet_created`, `tweet_location`, and others  
- **Purpose:** To analyze customer sentiment and complaints directed toward airlines.

---

## 3. ET Phases

### **Extract Phase**
Performed in `etl_extract.ipynb`

**Steps completed:**
1. Loaded both the **raw dataset** (`raw_data.csv`) and **incremental dataset** (`incremental_data.csv`) from the `data/` directory.  
2. Displayed `.head()`, `.info()`, and `.describe()` to understand structure and key statistics.  
3. Identified and discussed data quality issues such as:
   - Missing values in `negative_reason`, `tweet_location`, and `user_timezone`.
   - Duplicate records (36 rows).
   - Consistent data types across datasets.  
4. Created an **incremental subset** from the raw dataset (most recent 2,000 tweets) to simulate new data ingestion.  
5. Confirmed schema and structure alignment between raw and incremental datasets.  
6. Saved the cleaned datasets for the transformation phase.

**Outcome:**  
Both datasets (`raw_data.csv` and `incremental_data.csv`) were validated and prepared for transformation.

---

### **Transform Phase**
Performed in `etl_transform.ipynb`

**Transformations applied:**
1. **Data Cleaning**
   - Removed irrelevant or redundant fields (where applicable).
   - Standardized date and string formats.
2. **Feature Engineering**
   - Created a new column `tweet_length` to represent the number of characters in each tweet.
3. **Categorical Transformation**
   - Converted the `airline_sentiment` column into one-hot encoded columns:
     - `sentiment_positive`
     - `sentiment_neutral`
     - `sentiment_negative`
4. **Validation and Export**
   - Verified transformations by comparing before/after samples.
   - Exported final cleaned files:
     - `transformed_full.csv`
     - `transformed_incremental.csv`  
     stored under the `transformed/` directory.

**Outcome:**  
The transformed datasets are fully cleaned, feature-enriched, and structured for analysis or database loading.

---

## 4. Tools Used
- **Python 3.x**
- **Pandas** – for data manipulation and cleaning  
- **NumPy** – for numerical operations  
- **Jupyter Notebook** – for interactive development and documentation  
- **Git & GitHub** – for version control  
- *requirements.txt* for reproducibility

---

### 5. Steps to Run the Project

To replicate this ETL workflow on your local environment, follow these steps:

```bash
# 1. Clone the Repository
git clone <your-repo-link>
cd ET_Exam_<YourName>_<YourID>

# 2. Set Up the Python Environment
# (Recommended: use a virtual environment)
python -m venv etl_exam_env
source etl_exam_env/bin/activate  # On Windows use: etl_exam_env\Scripts\activate

# 3. Install Dependencies
pip install -r requirements.txt

# 4. Launch the Jupyter Notebook Interface
jupyter notebook

# 5. Run the Extraction Notebook
# This step loads, inspects, and validates the datasets.
open and execute: etl_extract.ipynb

# 6. Run the Transformation Notebook
# This step cleans, enriches, and transforms the datasets for analysis.
open and execute: etl_transform.ipynb

```

# 7. Verify Outputs
 Ensure the following files are generated and saved correctly:

```bash

 ├── data/
 │   ├── raw_data.csv
 │   └── incremental_data.csv
 ├── transformed/
 │   ├── transformed_full.csv
 │   └── transformed_incremental.csv

```

## 6. Sample Outputs 

Below are representative outputs captured during the **Extract (E)** and **Transform (T)** phases.

---

### **Extract Phase**

#### **1. Raw Dataset Structure**
### **Extract Phase**

```bash
--- RAW DATASET STRUCTURE ---
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 14640 entries, 0 to 14639
Data columns (total 15 columns):
 #   Column                      Non-Null Count  Dtype
---  ------                      --------------  -----
 0   tweet_id                    14640 non-null  int64
 1   airline_sentiment           14640 non-null  object
 2   airline_sentiment_confidence 14640 non-null  float64
 3   negativereason              9178 non-null   object
 4   negativereason_confidence   10522 non-null  float64
 5   airline                     14640 non-null  object
 6   airline_sentiment_gold      40 non-null     object
 7   name                        14640 non-null  object
 8   negativereason_gold         32 non-null     object
 9   retweet_count               14640 non-null  int64
 10  text                        14640 non-null  object
 11  tweet_coord                 1019 non-null   object
 12  tweet_created               14640 non-null  datetime64[ns, UTC-08:00]
 13  tweet_location              9907 non-null   object
 14  user_timezone               9820 non-null   object
dtypes: datetime64 , float64(2), int64(2), object(10)
memory usage: 1.7+ MB


--- MISSING VALUES (RAW DATASET) ---
negativereason               5462
negativereason_confidence    4118
airline_sentiment_gold      14600
negativereason_gold         14608
tweet_coord                 13621
tweet_location               4733
user_timezone                4820
dtype: int64


--- DUPLICATE RECORDS ---
Raw dataset duplicate records: 36
Incremental dataset duplicate records: 36

```


### **Transform Phase**

```bash
--- TEXT ENRICHMENT ---

Before transformation:
  text
0  @VirginAmerica What @dhepburn said.
1  @VirginAmerica plus you've added commercials t...
2  @VirginAmerica I didn't today... Must mean I n...

After transformation:
  text                                              tweet_length
0  @VirginAmerica What @dhepburn said.              35
1  @VirginAmerica plus you've added commercials t... 72
2  @VirginAmerica I didn't today... Must mean I n... 71


--- SENTIMENT ENCODING ---

Before transformation:
  airline_sentiment
0           neutral
1          positive
2           neutral

After transformation:
  airline_sentiment  sentiment_positive  sentiment_neutral  sentiment_negative
0           neutral                   0                  1                   0
1          positive                   1                  0                   0
2           neutral                   0                  1                   0


--- OUTPUT VERIFICATION ---

Transformed full dataset saved to: transformed/transformed_full.csv
Transformed incremental dataset saved to: transformed/transformed_incremental.csv

File Summary:
File                           Rows   Columns
transformed_full.csv           14640  17
transformed_incremental.csv    2000   17

All transformations applied successfully.
Both datasets are cleaned, enriched, encoded, and ready for analysis.
```