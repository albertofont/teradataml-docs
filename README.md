# Teradata Package for Python (teradataml) Documentation

This repository hosts the full, converted documentation for the **Teradata Package for Python (`teradataml`)** library. The content is extracted from the official Teradata Vantage documentation, converted into clean, searchable Markdown format, and organized into numbered chapters.

The primary goal of this repository structure is to create highly effective, local, and searchable documentation that is indexed efficiently by search engines, allowing developers to quickly find function syntax, usage examples, and conceptual guidance.

## Repository Structure

The documentation is organized into two main directories, reflecting the distinction between conceptual guides and API-level reference materials:

1. **`teradataml_user_guide`**: Conceptual overview, installation, tutorials, and high-level feature explanations.

2. **`teradataml_function_reference`**: Detailed documentation for every class, function, and method in the API, including parameters, return types, and code examples.

All files are prefixed with a two-digit number or letter (e.g., `01_...`, `A ...`) to maintain the original logical flow and chapter ordering of the source material.

---

## 1. teradataml\_user\_guide (Conceptual & Tutorials)

This folder contains the **User Guide**, focusing on how to set up, use, and manage `teradataml` components for practical application development.

| File | Topics Covered | 
 | ----- | ----- | 
| [**00 Table of Contents**](teradataml_user_guide/00%20Table%20of%20Contents.md) | Navigation map for the entire User Guide. | 
| [**01 Introduction to Teradata Package for Python**](teradataml_user_guide/01%20Introduction%20to%20Teradata%20Package%20for%20Python.md) | Core introduction, architecture, and concepts of the package. | 
| [**02 Installing Uninstalling and Upgrading...**](teradataml_user_guide/02%20Installing%20Uninstalling%20and%20Upgrading%20Teradata%20Package%20for%20Python.md) | Practical guide to managing the installation lifecycle. | 
| [**03 teradataml Components**](teradataml_user_guide/03%20teradataml%20Components.md) | Detailed breakdown of the main structural components of the library. | 
| [**04 DataFrames Setup and Basics...**](teradataml_user_guide/04%20DataFrames%20Setup%20and%20Basics%20(Sources%20Non%20Default%20DB%20UAF).md) | Initial setup of DataFrames, non-default database usage, and Unbounded Array Framework (UAF) basics. | 
| [**05 DataFrame Manipulation (Core API)**](teradataml_user_guide/05%20DataFrame%20Manipulation%20(Core%20API).md) | Comprehensive guide to all standard DataFrame manipulation and transformation methods. | 
| [**06 DataFrame Metadata Rotation Saving and Export**](teradataml_user_guide/06%20DataFrame%20Metadata%20Rotation%20Saving%20and%20Export.md) | Managing data persistence, rotation, metadata, saving, and export operations. | 
| [**07 Executing Python Functions Inside Database Engine 20**](teradataml_user_guide/07%20Executing%20Python%20Functions%20Inside%20Database%20Engine%2020.md) | Running user-defined Python logic directly within the Vantage Analytic Engine. | 
| [**08 teradataml DataFrame Column**](teradataml_user_guide/08%20teradataml%20DataFrame%20Column.md) | Working with the `DataFrameColumn` object for expressions and transformations. | 
| [**09 teradataml Window Aggregates**](teradataml_user_guide/09%20teradataml%20Window%20Aggregates.md) | Advanced data analysis using window functions in Vantage. | 
| [**10 Context to Teradata Vantage**](teradataml_user_guide/10%20Context%20to%20Teradata%20Vantage.md) | Managing and configuring the connection and session context with Vantage. | 
| [**11 teradataml Options**](teradataml_user_guide/11%20teradataml%20Options.md) | Setting and retrieving global configuration options. | 
| [**12 teradataml Utility and General Functions**](teradataml_user_guide/12%20teradataml%20Utility%20and%20General%20Functions.md) | Collection of miscellaneous helper and general-purpose functions. | 
| [**13 teradataml Open Source Machine Learning Functions**](teradataml_user_guide/13%20teradataml%20Open%20Source%20Machine%20Learning%20Functions.md) | Integrating open-source ML libraries into Vantage workflows. | 
| [**14 Script Methods (SCRIPT Table Operator)**](teradataml_user_guide/14%20Script%20Methods%20(SCRIPT%20Table%20Operator).md) | Using the `SCRIPT` table operator for parallel processing of custom scripts. | 
| [**15 Series (DataFrame Column Sequence)**](teradataml_user_guide/15%20Series%20(DataFrame%20Column%20Sequence).md) | Reference for the `Series` object and its sequence-based operations. | 
| [**16 BYOM (Bring Your Own Model) Management**](teradataml_user_guide/16%20BYOM%20(Bring%20Your%20Own%20Model)%20Management.md) | Procedures for managing and deploying custom machine learning models (BYOM). | 
| [**17 Working with Geospatial Data**](teradataml_user_guide/17%20Working%20with%20Geospatial%20Data.md) | Guide to handling and analyzing geospatial data types in `teradataml`. | 
| [**18 Exploratory Data Analysis (EDA UI)**](teradataml_user_guide/18%20Exploratory%20Data%20Analysis%20(EDA%20UI).md) | Using built-in tools for initial data exploration and profiling. | 
| [**19 Plotting in teradataml**](teradataml_user_guide/19%20Plotting%20in%20teradataml.md) | Generating visualizations and charts directly from DataFrame objects. | 
| [**20 Hyperparameter Tuning in teradataml**](teradataml_user_guide/20%20Hyperparameter%20Tuning%20in%20teradataml.md) | Methods for optimizing machine learning model parameters. | 
| [**21 AutoML Overview and Methods**](teradataml_user_guide/21%20AutoML%20Overview%20and%20Methods.md) | Introduction to the Automated Machine Learning methods. | 
| [**22 AutoML Examples**](teradataml_user_guide/22%20AutoML%20Examples.md) | Practical, working examples demonstrating AutoML usage. | 
| [**23 AutoDataPrep**](teradataml_user_guide/23%20AutoDataPrep.md) | Automated data preparation and feature engineering tools. | 
| [**24 Feature Store in teradataml**](teradataml_user_guide/24%20Feature%20Store%20in%20teradataml.md) | Utilizing the Feature Store for centralized feature management. | 
| [**25 Using Teradata Vantage Analytic Functions...**](teradataml_user_guide/25%20Using%20Teradata%20Vantage%20Analytic%20Functions%20with%20Teradata%20Package%20for%20Python.md) | High-level guide to accessing and using built-in Vantage analytic functions. | 
| [**A Teradata Package for Python Limitations and Considerations**](teradataml_user_guide/A%20Teradata%20Package%20for%20Python%20Limitations%20and%20Considerations.md) | Appendix covering known constraints and best practices. | 
| [**B Using teradataml with Native Object Store**](teradataml_user_guide/B%20Using%20teradataml%20with%20Native%20Object%20Store.md) | Appendix detailing integration with external data storage (NOS). | 
| [**C teradataml Extension with SQLAlchemy**](teradataml_user_guide/C%20teradataml%20Extension%20with%20SQLAlchemy.md) | Appendix on the SQLAlchemy extension for lower-level database interaction. | 
| [**D Data Type Mapping**](teradataml_user_guide/D%20Data%20Type%20Mapping.md) | Appendix providing a reference for data type conversions between Python and Vantage. | 
| [**E Additional Information**](teradataml_user_guide/E%20Additional%20Information.md) | Final appendix containing supplementary reference material. | 

---

## 2. teradataml\_function\_reference (API Reference)

This folder contains the **Function Reference**, which provides exhaustive, alphabetized details on every callable object within the `teradataml` library.

| File | Topics Covered | 
 | ----- | ----- | 
| [**01 Introduction and Reference Front Matter**](teradataml_function_reference/01%20Introduction%20and%20Reference%20Front%20Matter.md) | Introductory content and high-level structure of the reference manual. | 
| [**02 teradataml Context Manager**](teradataml_function_reference/02%20teradataml%20Context%20Manager.md) | Detailed API reference for connection and session management functions. | 
| [**03 teradataml DataFrame Object and Methods**](teradataml_function_reference/03%20teradataml%20DataFrame%20Object%20and%20Methods.md) | Comprehensive reference for all methods and properties of the DataFrame class. | 
| [**04 teradataml Time Series Methods**](teradataml_function_reference/04%20teradataml%20Time%20Series%20Methods.md) | Reference for functions tailored for time series data analysis. | 
| [**05 teradataml DataFrameColumn Expressions**](teradataml_function_reference/05%20teradataml%20DataFrameColumn%20Expressions.md) | Detailed syntax and usage for `DataFrameColumn` expressions. | 
| [**06 teradataml Geospatial Types and DataFrames**](teradataml_function_reference/06%20teradataml%20Geospatial%20Types%20and%20DataFrames.md) | Reference for all geospatial data types and associated functions. | 
| [**07 teradataml Window Aggregates**](teradataml_function_reference/07%20teradataml%20Window%20Aggregates.md) | Full syntax and examples for all supported window aggregate functions. | 
| [**08 teradataml Series Object and Methods**](teradataml_function_reference/08%20teradataml%20Series%20Object%20and%20Methods.md) | API reference for the `Series` object. | 
| [**09 teradataml General Functions...**](teradataml_function_reference/09%20teradataml%20General%20Functions%20(Utilities%20Configuration%20Versioning).md) | Full reference for utility functions (e.g., versioning, configuration). | 
| [**10 teradataml Plotting Functions**](teradataml_function_reference/10%20teradataml%20Plotting%20Functions.md) | API details for all data visualization functions. | 
| [**11 teradataml SDK Functions**](teradataml_function_reference/11%20teradataml%20SDK%20Functions.md) | Reference for lower-level Software Development Kit components. | 
| [**12 Enterprise Feature Store Functions**](teradataml_function_reference/12%20Enterprise%20Feature%20Store%20Functions.md) | API reference for managing features within the Enterprise Feature Store. | 
| [**13 teradataml Bring Your Own Analytics**](teradataml_function_reference/13%20teradataml%20Bring%20Your%20Own%20Analytics.md) | API details for custom, user-defined analytic deployment. | 
| [**14 teradataml Database 16.20.xx, 17.00.xx, 17.05.xx Analytic Functions**](teradataml_function_reference/14%20teradataml%20Database%2016.20.xx%2017.00.xx%2017.05.xx%20Analytic%20Functions.md) | Analytic function reference for older Vantage database versions. | 
| [**15 teradataml Database 17.10.xx Analytic Functions**](teradataml_function_reference/15%20teradataml%20Database%2017.10.xx%20Analytic%20Functions.md) | Analytic function reference for Vantage 17.10.xx. | 
| [**16 teradataml Database 17.20.xx Analytic Functions**](teradataml_function_reference/16%20teradataml%20Database%2017.20.xx%20Analytic%20Functions.md) | Analytic function reference for Vantage 17.20.xx. | 
| [**17 teradataml Database 20.00.xx Analytic Functions**](teradataml_function_reference/17%20teradataml%20Database%2020.00.xx%20Analytic%20Functions.md) | Analytic function reference for the latest Vantage database version. | 
| [**18 teradataml Unbounded Array Framework Functions**](teradataml_function_reference/18%20teradataml%20Unbounded%20Array%20Framework%20Functions.md) | API reference for functions utilizing the Unbounded Array Framework (UAF). | 
| [**19 teradataml Hyperparameter Tuning**](teradataml_function_reference/19%20teradataml%20Hyperparameter%20Tuning.md) | Detailed API for hyperparameter optimization routines. | 
| [**20 teradataml AutoML**](teradataml_function_reference/20%20teradataml%20AutoML.md) | Full API reference for Automated Machine Learning components. | 
| [**21 teradataml OpenSourceML**](teradataml_function_reference/21%20teradataml%20OpenSourceML.md) | API details for integrating and managing Open Source ML models. | 
| [**22 teradataml Vantage Analytics Library Functions**](teradataml_function_reference/22%20teradataml%20Vantage%20Analytics%20Library%20Functions.md) | Reference for functions provided by the Vantage Analytics Library (VAL). | 
| [**23 teradataml Formula Functions**](teradataml_function_reference/23%20teradataml%20Formula%20Functions.md) | API reference for specialized formula parsing and execution. | 
| [**24 teradataml Data Preparation Functions**](teradataml_function_reference/24%20teradataml%20Data%20Preparation%20Functions.md) | Comprehensive reference for all data prep utility functions (string, date, math, etc.). | 
| [**25 Options and Configuration**](teradataml_function_reference/25%20Options%20and%20Configuration.md) | Final reference for all global options and configuration management. |
