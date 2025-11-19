---
applyTo: '**'
---
# Copilot Instructions — teradataml-docs

**Goal:** Provide accurate, pushdown-first, runnable Teradata-centric Python code using only the APIs documented in this repository.

---

## Core Rules & Constraints

-   **Pushdown-First:** Prefer `teradataml` DataFrame and analytic methods that execute directly in **Teradata Vantage**.
-   **Local-First, Not Local-Only:** Your primary source of truth is the documentation and assets in this workspace. If you cannot find an answer locally, you may consult the official Teradata documentation as a secondary source, but you must state when you are doing so.
    -   Approved URL 1: `https://docs.teradata.com/r/Enterprise_IntelliFlex_VMware/Teradata-Package-for-Python-Function-Reference-20.00`
    -   Approved URL 2: `https://docs.teradata.com/r/Enterprise_IntelliFlex_VMware/Teradata-Package-for-Python-User-Guide`
-   **No Client-Side Shortcuts:** **AVOID** recommending client-side libraries (like Pandas for manipulation) for large Vantage datasets unless the user explicitly requests data export.
-   **Negation Rule: Avoid MLE and VCL:** **NEVER** suggest code, functions, or concepts related to the **Machine Learning Engine (MLE)** or **Vantage Cloud Lake (VCL)**. These are unsupported workflows.
-   **SQL as Fallback Only:** Use raw SQL only when **no documented `teradataml` wrapper exists**. If used, cite the local doc you used to confirm the fallback was necessary.

---

## Internal Reference & Discovery Priority

Before answering any question or writing any code, you MUST follow this strict search and verification process in order:

1.  **High-Level Documentation Review (Mandatory First Step):**
    *   Consult the `README.md` file to understand the high-level structure of the documentation.
    *   Pay special attention to the User Guide and Function Reference tables to locate relevant chapters for the user's task.

2.  **Function Discovery (Primary):**
    *   You are prohibited from using a `teradataml` function without first locating it in the master index: **`teradataml_function_table.md`**.
    *   Use the `Name`, `Description`, and `Synonyms/Keywords` columns to find the exact `teradataml` function or class that matches the user's conceptual goal (e.g., search for "deploy model", "split data").

3.  **Syntax & Import Retrieval (Mandatory Second Step):**
    *   From the function table row, identify the documentation file in the first column (e.g., `22_teradataml_Vantage_Analytics_Library_Functions.md`).
    *   **Immediately open this file.** Do not proceed without consulting it.
    *   Use this file to determine the correct module for import (e.g., `from teradataml.analytics import valib`) and the precise function arguments.

4.  **Code Pattern Retrieval (Local):**
    *   Once the function and its syntax are known, find a runnable example in the workspace.
    *   **Highest Priority:** Search **`teradataml_sample_notebooks/`** for a notebook that uses this function.
    *   **Secondary Source:** Search the **`teradataml_user_guide/`** for conceptual explanations and multi-step workflow examples.

---

## Fallback & Escalation Path

Use this path ONLY if the "Internal Reference & Discovery Priority" process fails to yield a result.

1.  **Filesystem Inspection (First Fallback):**
    *   If the exact import path or class name is still unclear, inspect the installed package files directly within the user's environment.
    *   Use terminal commands like `ls` and `cat` to explore the `teradataml` directory structure (e.g., `ls /home/alberto/anaconda3/envs/py311gcopilot/lib/python3.11/site-packages/teradataml/analytics/`).

2.  **External Search (Final Fallback):**
    *   If and only if all local methods are exhausted, search the approved URLs listed in the Core Rules. You must cite the URL and state that you are using an external source.

---

## Pylance-Based Code Validation Workflow

Before generating a final code block, you **must** perform these validation steps using the available Pylance tools:

1.  **Pre-Execution Syntax & Import Check:** Before attempting to run any generated code for the first time, validate the entire script for syntax errors and missing imports. Fix all identified issues.

2.  **Verify Function Existence (Pre-flight Check):** Before writing the full implementation for a function, you **MUST** first write and execute a single-line Pylance snippet to validate the import and function existence.
    *   **Method:** Generate and run a simple `assert hasattr(module, 'function_name')` snippet using the `mcp_pylance_mcp_s_pylanceRunCodeSnippet` tool.
    *   **Example:**
        ```python
        from teradataml import DataFrame
        assert hasattr(DataFrame, 'select'), "DataFrame.select method not found."
        ```
    *   If the function does not exist, return to the Discovery process and state why you are switching. Only after validation succeeds, proceed to write the full code cell.

3.  **Import Cleanup:** Use the appropriate tool to remove unused imports from the final script.

4.  **Connection Handling:**
    *   Do **NOT** generate code with hardcoded credentials.
    *   All Vantage connections **MUST** be established using the `get_vantage_config` function from the `td_config.py` script.
    *   **Required Code Snippet:**
        ```python
        from td_config import get_vantage_config
        from teradataml import create_context

        # Note: Assumes you have a .env file with TD_HOST, TD_USERNAME, and TD_PASSWORD
        config = get_vantage_config()
        con = create_context(**config)
        print(con)
        ```

---

## Common Pitfalls & Agent Learnings

This section documents common errors and their solutions to improve future performance. As an agent, you are **required to append new findings** to this table whenever you overcome a non-trivial challenge.

| Pitfall | Solution | Model |
| --- | --- | --- |
| `ImportError` for a function like `train_test_split`. | `teradataml` often uses PascalCase for analytic function classes. The correct import was `from teradataml.analytics.sqle import TrainTestSplit`. The error was discovered by searching the function table and validated with a Pylance pre-flight check. | Gemini 2.5 Pro |
| `PMMLPredict` fails with a generic syntax error. | The `PMMLPredict` function requires the `configure.byom_install_location` to be set to the database where BYOM functions are installed. A missing configuration leads to a misleading `Syntax error, expected something like ';'` message. | Gemini 2.5 Pro |
| `TrainTestSplit` fails with "Unsupported data type". | The `id_column` argument for `TrainTestSplit` must be a column with a unique integer for each row. The output column for splitting is `TD_IsTrainRow`, not `sampleid`. | Gemini 2.5 Pro |
| `len(df.index)` fails on a `teradataml.DataFrame`. | To get the number of rows in a `teradataml.DataFrame`, use the `.shape[0]` attribute. The `.index` attribute does not behave like its pandas counterpart. | Gemini 2.5 Pro |
| Using `copy_to_sql` to save a PMML model for `PMMLPredict`. | The `PMMLPredict` function requires the model table to have a specific schema (`model_id`, `model` as `BLOB`). The correct, purpose-built function is `save_byom`. If using `copy_to_sql`, the DataFrame must have a 'model' column and the type must be explicitly set to `BLOB` from `teradatasqlalchemy.types`. | Gemini 2.5 Pro |
| `drop()` method on a DataFrame fails to drop the table. | The `DataFrame.drop()` method is for dropping *columns*. To drop a temporary table and clean up the session, use `remove_context()`. | Gemini 2.5 Pro |

---

## Deliverables & Style

-   Keep responses concise, clear, and focused on **runnable code**. Include only the minimal code needed to solve the user's problem.
-   When returning code, provide:
    *   A one-line contract.
    *   The minimal code snippet that maps to the cited document.
    *   A short note on assumptions (data shape, required connection/permissions).
    *   A **Validation Report** showing syntax check and import verification results.

### Validation Report Example
When you validate code, report the results as follows:
\`\`\`
✓ Syntax validated (no errors)
✓ Imports verified: teradataml.DataFrame available
✓ Method exists: DataFrame.select() confirmed
⚠ Cannot test execution (requires database connection)
\`\`\`
