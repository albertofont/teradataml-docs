---
description: Comprehensive benchmark to test Copilot's adherence to teradataml best practices and constraints from the teradataml_general.instructions.md file.
variables:
  name: A descriptive tag used for naming output files (e.g., 'regression' or 'test_case_A'). This value will replace $name.
---

# Copilot Benchmark: `teradataml` Script Generation Compliance

**Objective:** Evaluate Copilot's ability to generate a correct, pushdown-first `teradataml` script based on the rules in the provided instruction set.

---

## Master Benchmark Prompt

Using the context provided in the `teradataml_general.instructions.md` file, generate a complete Jupyter Notebook named **`benchmark_$name_v0.ipynb`**. Then, create a copy named **`benchmark_$name_v1.ipynb`** and iteratively refine it until it runs without errors.

The notebook should perform the following steps, with each step in its own cell:

1.  **Setup:** Import all necessary `teradataml` and standard libraries.
2.  **Connection:** Connect to Teradata Vantage using the `td_config.py` helper script.
3.  **Data Loading:** Load the `diabetes` dataset from `sklearn.datasets`. Create a `teradataml` DataFrame in Vantage from the pandas DataFrame, naming the table `diabetes_train_data`.
4.  **Data Exploration:** Get the count of rows in the `teradataml` DataFrame.
5.  **Feature Engineering:** Perform a standard scaling operation (range scaling) on the 'age' and 'bmi' columns.
6.  **Model Training:** Train a simple regression model to predict the `target` variable using the scaled 'age' and 'bmi' features. Choose an appropriate in-database algorithm.
7.  **Cleanup:** Remove the database context.

## Strict Constraints:

-   Do not use `pandas` for any operation after the initial data loading. All subsequent steps must use `teradataml`.
-   For each `teradataml` function used, cite the source documentation file from the repository that contains its definition.
-   Adhere strictly to all rules in the provided instructions, especially the negation rule to avoid `teradataml.analytics.mle` functions.
-   Provide a final validation report summarizing the checks performed (syntax, import, method existence) and save it to **`report_$name.txt`**.

## Final Reporting Step:

After successfully executing the `_v1` notebook, create a file named **`report_$name.txt`**. In this report, provide a detailed analysis of the debugging journey, including:
-   A list of all errors encountered.
-   The methods used to diagnose and resolve each error.
-   An evaluation of the effectiveness of the local documentation.
-   Suggestions for improving the instructions or documentation to make the process faster in the future.