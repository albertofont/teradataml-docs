---
description: >
  Comprehensive benchmark to test Copilot's ability to execute a "Bring Your Own Model" (BYOM) workflow. 
  This includes local model training with scikit-learn, PMML export, and in-database scoring with teradataml.
variables:
  name: byom
---

# Copilot Benchmark: `teradataml` BYOM Workflow Compliance

**Objective:** Evaluate Copilot's ability to generate a correct, pushdown-first `teradataml` script for a complete BYOM lifecycle, based on the rules in the provided instruction set.

---

## Master Benchmark Prompt

Using the context provided in the `teradataml_general.instructions.md` file, generate a complete Jupyter Notebook named **`benchmark_$name_v0.ipynb`**. Then, create a copy named **`benchmark_$name_v1.ipynb`** and iteratively refine it until it runs without errors.

The notebook should perform the following steps, with each step in its own cell:

1.  **Setup:** Import all necessary libraries, including `teradataml`, `pandas`, `sklearn`, and `sklearn2pmml`. If `sklearn2pmml` is not installed, include the command to install it.
2.  **Connection:** Connect to Teradata Vantage using the `td_config.py` helper script.
3.  **Data Loading:** Load the `iris` dataset from `sklearn.datasets`. Create a `teradataml` DataFrame in Vantage from the pandas DataFrame, naming the table `iris_data`.
4.  **Data Preparation:** Perform an 80/20 train/test split on the `iris_data` DataFrame *within Vantage*. The output should be two `teradataml` DataFrames: one for training and one for testing.
5.  **Local Model Training:**
    *   Download the training data from the `teradataml` training DataFrame into a local pandas DataFrame.
    *   Create a `scikit-learn` pipeline that includes a `StandardScaler` and a `LogisticRegression` classifier.
    *   Train this pipeline on the local training data.
6.  **PMML Export:**
    *   Export the trained `scikit-learn` pipeline to a local PMML file named `iris_pipeline.pmml`.
7.  **Deploy Model to Vantage:**
    *   Load the contents of the `iris_pipeline.pmml` file into a new table in Vantage named `pmml_models`. The table should have columns for a model ID and the PMML content.
8.  **In-Database Scoring:**
    *   Use the appropriate `teradataml` function to score the *test* DataFrame (which has remained in Vantage) using the PMML model stored in the `pmml_models` table.
9.  **Cleanup:** Remove the `iris_data` and `pmml_models` tables from the database.

## Strict Constraints:

-   Do not use `pandas` for any operation other than the initial data loading and the local model training step. All other data manipulation and scoring must use `teradataml`.
-   For each `teradataml` function used, cite the source documentation file from the repository that contains its definition.
-   Adhere strictly to all rules in the provided instructions.

## Final Reporting Step:

After successfully executing the `_v1` notebook, update the file named **`report_$name.txt`**. In this report, provide a detailed analysis of the debugging journey. The report should be a narrative that explains your reasoning and decision-making process at each step. Include:
-   A list of all errors encountered.
-   For each error, describe your "thought process":
    -   What was your initial hypothesis for the cause of the error?
    -   Which tools did you use to investigate (e.g., `read_file`, `grep_search`, `copilot_getNotebookSummary`)?
    -   How did the results of those tools lead you to the solution?
    -   Why did you choose a particular solution (e.g., why `copy_to_sql` instead of `save_byom`)?
-   An evaluation of the effectiveness of the local documentation and instructions.
-   Suggestions for improving the instructions or documentation to make the process faster in the future.
-   A "What Went Wrong" section detailing any incorrect assumptions or actions you took, like using a function that did not exist.

**CRITICAL: If you discover a new, non-obvious pitfall during your debugging process, you MUST append it to the "Common Pitfalls & Agent Learnings" table in the `teradataml_general.instructions.md` file.**
