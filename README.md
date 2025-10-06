# INFOMDWR – Assignment 2: Data Integration & Preparation

## Introduction

In this assignment, you will work on different tasks related to data preparation, including data profiling, finding records that refer to the same entity, and computing the correlation between different attributes. The datasets you need are available online (links provided).

> **Note:**  
> Do not use AI tools such as ChatGPT for any part of this assignment.

---

## Task 1: Profiling Relational Data

- Download and read the paper on [profiling relational data](https://link.springer.com/article/10.1007/s00778-015-0389-y).
- Select a set of summary statistics about the data (minimum of 10 different values).
- Write Python code to compute these quantities for a dataset of your choice. Preferably, use one of the `.csv` files from the [road safety dataset](https://www.data.gov.uk/dataset/cb7ae6f0-4be6-4935-9277-47e5ce24a11f/road-safety-data).
- **Explain the importance of each summary statistic** you selected for understanding the characteristics of the dataset.

> *Note: Computing the same statistical quantity on multiple columns will be counted only once.*

---

## Task 2: Entity Resolution

Use the [DBLP-ACM dataset](https://dbs.uni-leipzig.de/research/projects/object_matching/benchmark_datasets_for_entity_resolution).

### Part 1

1. **Compare Records:**
   - Write Python code to compare every record in `ACM.csv` with all records in `DBLP2.csv` to find similar records (same publication).
   - To compare:
     - Ignore the `pub_id`.
     - Change all letters to lowercase.
     - Convert multiple spaces to one.
     - Use **Levenshtein similarity** on `title` and compute the score \( \text{sim}_{\text{title}} = 1 - \frac{MED}{|s|} \).
       - (*MED = minimum edit distance; \(|s|\) = number of characters*)
     - Use **Jaro similarity** for `authors` and compute \( \text{sim}_{\text{authors}} \).
     - Use a **modified affine similarity** (scaled to [0, 1]) for `venue` \( \text{sim}_{\text{venue}} \).
     - Use **Match (1) / Mismatch (0)** for `year` \( \text{sim}_{\text{year}} \).
     - Combine the scores using the formula:  
       \( \text{rec\_sim} = \alpha \cdot \text{sim}_{\text{title}} + \beta \cdot \text{sim}_{\text{authors}} + \gamma \cdot \text{sim}_{\text{venue}} + \delta \cdot \text{sim}_{\text{year}} \),
       where \( \alpha + \beta + \gamma + \delta = 1 \).
   - Report all record pairs with \( \text{rec\_sim} > 0.7 \) as duplicates, storing both IDs in a list.

2. **Evaluate Results:**
   - Use `DBLP-ACM_perfectMapping.csv` (actual mappings) to compute **precision**:  
     > Among all reported similar records, how many pairs actually exist in the perfect mapping file?
   - **Record the running time** of the method.
   - **Discussion:**  
     - Why does the program take a long time?
     - What strategies could reduce running time? (No need to implement; just discuss.)

### Part 2

1. **Approximate Matching with LSH:**
   - Concatenate the values in each record into one string.
   - Change all letters to lowercase.
   - Convert multiple spaces to one.
   - Combine records from both tables into one list (as in the lab).
   - Use lab 5 tutorial functions to compute shingles, minhash signature, and similarity.
   - Extract the top \( n \) candidates from LSH.  
   - Compare them to the mappings in `DBLP-ACM_perfectMapping.csv` and compute precision.
   - **Record the running time**.

2. **Compare Approaches:**
   - Compare **precision** and **running time** between Parts 1 and 2.

---

## Task 3: Data Preparation

Use the [Pima Indians Diabetes Database](https://www.kaggle.com/uciml/pima-indians-diabetes-database).

1. **Compute Initial Correlation:**
   - Calculate correlation between different columns (excluding the `outcome` column).

2. **Remove Disguised Values:**
   - In these columns: `BloodPressure`, `SkinThickness`, and `BMI` — replace all `0` values with `null` (these represent missing values).
   - Do **not** remove the record; only change the value.

3. **Impute Missing Values:**
   - For each `null`, fill it using the mean of records with the same class label.

4. **Post-Imputation Correlation:**
   - Compute the correlation again between columns.

5. **Compare and Comment:**
   - Contrast the correlation matrices from before and after imputation.
   - Mention the most important changes (if any) and comment on the findings.

---
