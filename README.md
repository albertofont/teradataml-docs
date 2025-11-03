# Teradata Package for Python (teradataml) Documentation

This repository hosts the full, converted documentation for the **Teradata Package for Python (`teradataml`)** library. The content is extracted from the official Teradata Vantage documentation, converted into clean, searchable Markdown format, and organized into numbered chapters.

The primary goal of this repository structure is to create highly effective, local, and searchable documentation that is indexed efficiently by search engines, allowing developers to quickly find function syntax, usage examples, and conceptual guidance.

## Repository Structure

The documentation is organized into three main directories: two for structured reference material, and one for runnable examples, reflecting the distinction between conceptual guides, API reference, and practical applications:

1. **`teradataml_user_guide`**: Conceptual overview, installation, tutorials, and high-level feature explanations.

2. **`teradataml_function_reference`**: Detailed documentation for every class, function, and method in the API, including parameters, return types, and code examples.

3. **`teradataml_sample_notebooks`**: **Runnable examples that demonstrate best-practice usage patterns.**

All files are prefixed with a two-digit number or letter (e.g., `01_...`, `A ...`) to maintain the original logical flow and chapter ordering of the source material.

---

## 1. teradataml\_user\_guide (Conceptual & Tutorials)

This folder contains the **User Guide**, focusing on how to set up, use, and manage `teradataml` components for practical application development.

| File | Topics Covered |
| ----- | ----- |
| [**00 Table of Contents**](teradataml_user_guide/00_Table_of_Contents.md) | Navigation map for the entire User Guide. | 
| [**01 Introduction to Teradata Package for Python**](teradataml_user_guide/01_Introduction_to_Teradata_Package_for_Python.md) | Core introduction, architecture, and concepts of the package. | 
| [**02 Installing Uninstalling and Upgrading...**](teradataml_user_guide/02_Installing_Uninstalling_and_Upgrading_Teradata_Package_for_Python.md) | Practical guide to managing the installation lifecycle. | 
| [**03 teradataml Components**](teradataml_user_guide/03_teradataml_Components.md) | Detailed breakdown of the main structural components of the library. | 
| [**04 DataFrames Setup and Basics...**](teradataml_user_guide/04_DataFrames_Setup_and_Basics_(Sources_Non_Default_DB_UAF).md) | Initial setup of DataFrames, non-default database usage, and Unbounded Array Framework (UAF) basics. | 
| [**05 DataFrame Manipulation (Core API)**](teradataml_user_guide/05_DataFrame_Manipulation_(Core_API).md) | Comprehensive guide to all standard DataFrame manipulation and transformation methods. | 
| [**06 DataFrame Metadata Rotation Saving and Export**](teradataml_user_guide/06_DataFrame_Metadata_Rotation_Saving_and_Export.md) | Managing data persistence, rotation, metadata, saving, and export operations. | 
| [**07 Executing Python Functions Inside Database Engine 20**](teradataml_user_guide/07_Executing_Python_Functions_Inside_Database_Engine_20.md) | Running user-defined Python logic directly within the Vantage Analytic Engine. | 
| [**08 teradataml DataFrame Column**](teradataml_user_guide/08_teradataml_DataFrame_Column.md) | Working with the `DataFrameColumn` object for expressions and transformations. | 
| [**09 teradataml Window Aggregates**](teradataml_user_guide/09_teradataml_Window_Aggregates.md) | Advanced data analysis using window functions in Vantage. | 
| [**10 Context to Teradata Vantage**](teradataml_user_guide/10_Context_to_Teradata_Vantage.md) | Managing and configuring the connection and session context with Vantage. | 
| [**11 teradataml Options**](teradataml_user_guide/11_teradataml_Options.md) | Setting and retrieving global configuration options. | 
| [**12 teradataml Utility and General Functions**](teradataml_user_guide/12_teradataml_Utility_and_General_Functions.md) | Collection of miscellaneous helper and general-purpose functions. | 
| [**13 teradataml Open Source Machine Learning Functions**](teradataml_user_guide/13_teradataml_Open_Source_Machine_Learning_Functions.md) | Integrating open-source ML libraries into Vantage workflows. | 
| [**14 Script Methods (SCRIPT Table Operator)**](teradataml_user_guide/14_Script_Methods_(SCRIPT_Table_Operator).md) | Using the `SCRIPT` table operator for parallel processing of custom scripts. | 
| [**15 Series (DataFrame Column Sequence)**](teradataml_user_guide/15_Series_(DataFrame_Column_Sequence).md) | Reference for the `Series` object and its sequence-based operations. | 
| [**16 BYOM (Bring Your Own Model) Management**](teradataml_user_guide/16_BYOM_(Bring_Your_Own_Model)_Management.md) | Procedures for managing and deploying custom machine learning models (BYOM). | 
| [**17 Working with Geospatial Data**](teradataml_user_guide/17_Working_with_Geospatial_Data.md) | Guide to handling and analyzing geospatial data types in `teradataml`. | 
| [**18 Exploratory Data Analysis (EDA UI)**](teradataml_user_guide/18_Exploratory_Data_Analysis_(EDA_UI).md) | Using built-in tools for initial data exploration and profiling. | 
| [**19 Plotting in teradataml**](teradataml_user_guide/19_Plotting_in_teradataml.md) | Generating visualizations and charts directly from DataFrame objects. | 
| [**20 Hyperparameter Tuning in teradataml**](teradataml_user_guide/20_Hyperparameter_Tuning_in_teradataml.md) | Methods for optimizing machine learning model parameters. | 
| [**21 AutoML Overview and Methods**](teradataml_user_guide/21_AutoML_Overview_and_Methods.md) | Introduction to the Automated Machine Learning methods. | 
| [**22 AutoML Examples**](teradataml_user_guide/22_AutoML_Examples.md) | Practical, working examples demonstrating AutoML usage. | 
| [**23 AutoDataPrep**](teradataml_user_guide/23_AutoDataPrep.md) | Automated data preparation and feature engineering tools. | 
| [**24 Feature Store in teradataml**](teradataml_user_guide/24_Feature_Store_in_teradataml.md) | Utilizing the Feature Store for centralized feature management. | 
| [**25 Using Teradata Vantage Analytic Functions...**](teradataml_user_guide/25_Using_Teradata_Vantage_Analytic_Functions_with_Teradata_Package_for_Python.md) | High-level guide to accessing and using built-in Vantage analytic functions. | 
| [**A Teradata Package for Python Limitations and Considerations**](teradataml_user_guide/A_Teradata_Package_for_Python_Limitations_and_Considerations.md) | Appendix covering known constraints and best practices. | 
| [**B Using teradataml with Native Object Store**](teradataml_user_guide/B_Using_teradataml_with_Native_Object_Store.md) | Appendix detailing integration with external data storage (NOS). | 
| [**C teradataml Extension with SQLAlchemy**](teradataml_user_guide/C_teradataml_Extension_with_SQLAlchemy.md) | Appendix on the SQLAlchemy extension for lower-level database interaction. | 
| [**D Data Type Mapping**](teradataml_user_guide/D_Data_Type_Mapping.md) | Appendix providing a reference for data type conversions between Python and Vantage. | 
| [**E Additional Information**](teradataml_user_guide/E_Additional_Information.md) | Final appendix containing supplementary reference material. | 

---

## 2. teradataml\_function\_reference (API Reference)

This folder contains the **Function Reference**, which provides exhaustive, alphabetized details on every callable object within the `teradataml` library.

| File | Topics Covered | 
| ----- | ----- | 
| [**01 Introduction and Reference Front Matter**](teradataml_function_reference/01_Introduction_and_Reference_Front_Matter.md) | Introductory content and high-level structure of the reference manual. | 
| [**02 teradataml Context Manager**](teradataml_function_reference/02_teradataml_Context_Manager.md) | Detailed API reference for connection and session management functions. | 
| [**03 teradataml DataFrame Object and Methods**](teradataml_function_reference/03_teradataml_DataFrame_Object_and_Methods.md) | Comprehensive reference for all methods and properties of the DataFrame class. | 
| [**04 teradataml Time Series Methods**](teradataml_function_reference/04_teradataml_Time_Series_Methods.md) | Reference for functions tailored for time series data analysis. | 
| [**05 teradataml DataFrameColumn Expressions**](teradataml_function_reference/05_teradataml_DataFrameColumn_Expressions.md) | Detailed syntax and usage for `DataFrameColumn` expressions. | 
| [**06 teradataml Geospatial Types and DataFrames**](teradataml_function_reference/06_teradataml_Geospatial_Types_and_DataFrames.md) | Reference for all geospatial data types and associated functions. | 
| [**07 teradataml Window Aggregates**](teradataml_function_reference/07_teradataml_Window_Aggregates.md) | Full syntax and examples for all supported window aggregate functions. | 
| [**08 teradataml Series Object and Methods**](teradataml_function_reference/08_teradataml_Series_Object_and_Methods.md) | API reference for the `Series` object. | 
| [**09 teradataml General Functions...**](teradataml_function_reference/09_teradataml_General_Functions_(Utilities_Configuration_Versioning).md) | Full reference for utility functions (e.g., versioning, configuration). | 
| [**10 teradataml Plotting Functions**](teradataml_function_reference/10_teradataml_Plotting_Functions.md) | API details for all data visualization functions. | 
| [**11 teradataml SDK Functions**](teradataml_function_reference/11_teradataml_SDK_Functions.md) | Reference for lower-level Software Development Kit components. | 
| [**12 Enterprise Feature Store Functions**](teradataml_function_reference/12_Enterprise_Feature_Store_Functions.md) | API reference for managing features within the Enterprise Feature Store. | 
| [**13 teradataml Bring Your Own Analytics**](teradataml_function_reference/13_teradataml_Bring_Your_Own_Analytics.md) | API details for custom, user-defined analytic deployment. | 
| [**14 teradataml Database 16.20.xx, 17.00.xx, 17.05.xx Analytic Functions**](teradataml_function_reference/14_teradataml_Database_1620xx_1700xx_1705xx_Analytic_Functions.md) | Analytic function reference for older Vantage database versions. | 
| [**15 teradataml Database 17.10.xx Analytic Functions**](teradataml_function_reference/15_teradataml_Database_1710xx_Analytic_Functions.md) | Analytic function reference for Vantage 17.10.xx. | 
| [**16 teradataml Database 17.20.xx Analytic Functions**](teradataml_function_reference/16_teradataml_Database_1720xx_Analytic_Functions.md) | Analytic function reference for Vantage 17.20.xx. | 
| [**17 teradataml Database 20.00.xx Analytic Functions**](teradataml_function_reference/17_teradataml_Database_2000xx_Analytic_Functions.md) | Analytic function reference for the latest Vantage database version. | 
| [**18 teradataml Unbounded Array Framework Functions**](teradataml_function_reference/18_teradataml_Unbounded_Array_Framework_Functions.md) | API reference for functions utilizing the Unbounded Array Framework (UAF). | 
| [**19 teradataml Hyperparameter Tuning**](teradataml_function_reference/19_teradataml_Hyperparameter_Tuning.md) | Detailed API for hyperparameter optimization routines. | 
| [**20 teradataml AutoML**](teradataml_function_reference/20_teradataml_AutoML.md) | Full API reference for Automated Machine Learning components. | 
| [**21 teradataml OpenSourceML**](teradataml_function_reference/21_teradataml_OpenSourceML.md) | API details for integrating and managing Open Source ML models. | 
| [**22 teradataml Vantage Analytics Library Functions**](teradataml_function_reference/22_teradataml_Vantage_Analytics_Library_Functions.md) | Reference for functions provided by the Vantage Analytics Library (VAL). | 
| [**23 teradataml Formula Functions**](teradataml_function_reference/23_teradataml_Formula_Functions.md) | API reference for specialized formula parsing and execution. | 
| [**24 teradataml Data Preparation Functions**](teradataml_function_reference/24_teradataml_Data_Preparation_Functions.md) | Comprehensive reference for all data prep utility functions (string, date, math, etc.). | 
| [**25 Options and Configuration**](teradataml_function_reference/25_Options_and_Configuration.md) | Final reference for all global options and configuration management. | 

---

## 3. teradataml\_sample\_notebooks (Runnable Examples)

This folder contains **Jupyter Notebooks (.ipynb)** that provide ready-to-run, verified examples of common `teradataml` workflows. These notebooks serve as the **highest priority source for working code patterns**.

| File | Topics Covered |
| ----- | ----- |
| (Files in `teradataml_sample_notebooks/`) | Various practical applications, end-to-end recipes, and best-practice usage demonstrations. |