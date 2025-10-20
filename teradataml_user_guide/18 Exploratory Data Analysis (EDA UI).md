# Exploratory Data Analysis (EDA UI)

Exploratory Data Analysis User Interface, also known as EDA UI, is a User Interface (UI) generated on
teradataml DataFrame where you can run additional functions on teradataml DataFrame without adding any
additional code.
Once the teradataml DataFrame is returned to Jupyter cell, along with the result, additional UI will be given
to users to perform the following functionalities on the resultant teradataml DataFrame.
* Generates column statistics for the resultant teradataml DataFrame.
* Generates python types and teradata types for resultant teradataml DataFrame.
* Generates the summary for every teradataml DataFrame Column which has details about count of
* NULLs, not NULLs etc.
* Generates Categorical columns summary.
* Generates details about Futile columns.
* Can plot the data and generates the corresponding image.
* Can run teradataml analytic function or a series of analytic functions.
* Can persist the teradataml DataFrame to Vantage.
**Note:**
**Important notes regarding EDA UI:**
    * EDA UI is available only for Jupyter notebooks.
    * EDA UI won't be displayed if a teradataml DataFrame is printed. It will be displayed only when
    * teradataml DataFrame is returned to a Jupyter notebook cell.
    * EDA UI is displayed only with a valid Jupyter session. Hence, a downloaded Jupyter notebook will
    * not display EDA UI. However, if you start executing the downloaded notebook, it will display the
    * EDA UI again for that session.
    * Exploratory Data Analysis UI (EDA UI) works based on ipywidgets. In some cases, ipywidgets do
    * not work on a Jupyter notebook. If this occurs, Teradata recommends selecting a kernel again.
The following image shows the EDA UI for teradataml DataFrame.
16

## Enable or Disable EDA UI
The enable_ui option is available under display options. By default, this option is set to True so the EDA UI
is displayed for teradataml DataFrame. You can set this flag to False to disable the EDA UI.
By default, EDA UI is available for teradataml DataFrame if it is returned to Jupyter cell.
### Example: Control the EDA UI with enable_ui flag
## EDA UI Tabs
The following sections document the functionality of each EDA UI tab.
16: Exploratory Data Analysis

### Save / Reset buttons
Use the  Save  button to save the existing EDA UI settings for the current Jupyter session for corresponding
DataFrame. For example, build a pipeline and save the settings so you can use it for another DataFrame.
Note that the settings can pass different values to the saved pipeline.
The  Reset  button resets your saved settings.
### Describe tab
Use the Describe tab to retrieve different statistics on the resultant teradataml DataFrame.
The following example creates a sales DataFrame and displays the Describe tab.
Descriptions for each sub-tab follow.
Shape and Size
    * Retrieves the total number of rows and total number of columns on teradataml DataFrame.
Column Statistics
    * Retrieves the statistical values such as count, min, max, mean, standard deviation, percentiles
    * (25, 50, 75) for every possible teradataml DataFrame Column.
Column Types
    * Retrieves the Teradata type and corresponding Python type for every teradataml
    * DataFrame Column.
Column Summary
    * Retrieves Null value count, non-Null value count, total number of positive values, total number
    * of negative values for every possible teradataml DataFrame Column.
16: Exploratory Data Analysis

Categorical Summary
    * Retrieves the categorical values and their total count in corresponding column for every
    * possible teradataml DataFrame Column.
Futile Columns
    * Retrieves the Futile columns from teradataml DataFrame Column.
Source Query
    * Retrieves the underlying source query for corresponding teradataml DataFrame.
### Visualize tab
Use the Visualize tab to generate a plot on a teradataml DataFrame. It uses DataFrame.plot() function
internally. See  Plotting in teradataml  for options available in the UI.
### Analyze tab
Use the Analyze tab to run analytic functions such as VAL, VantageCloud Enterprise, and UAF on
teradataml DataFrame. You can select the corresponding function from the dropdown so the UI can pass
other arguments for the function. Once the arguments are specified, select  Execute  to execute the function.
Along with these functions, you run AutoML by selecting AutoML from the dropdown.
### Pipeline (Analyze tab option)
The Analyze tab includes a  pipeline  option to build a chain of actions using the output for any number of
**functions. The process to build a pipeline follows:**
16: Exploratory Data Analysis

Lets look at steps involved in building a pipeline.
1.
* Select a function from the dropdown.
* If the function accepts any parameters, UI will show additional options to provide parameters.
2.
* Select  Add to pipeline .
3.
* Select other functions as needed and specify parameters if the function accepts parameters.
    * Select  Remove from pipeline  to remove functions.
    * Select  Move <<  to move up a function in the list.
    * Select  Move >>  to move down a function in the list
4.
* After all functions are added, select  Execute  to execute the pipeline and display the final result.
The following example builds a pipeline using two functions to get Column Summary of all columns from
sales DataFrame and exclude column NonNullCount from final output.
1.
* Select  TD_Column Summary  from the dropdown and provide arguments for the function.
2.
* Exclude the column NonNullCount using AntiSelect.
* a.
    * Select  Add to pipeline  and select  AntiSelect  from the dropdown.
* b.
    * Provide the NonNullCount column name in arguments.
3.
* Select  Execute  to run the pipeline.
### Persist tab
Use the Persist tab to persist teradataml DataFrame to a table in Teradata Vantage. It uses copy_to_sql()
internally. See  copy_to_sql()  for options available in persist.
16: Exploratory Data Analysis