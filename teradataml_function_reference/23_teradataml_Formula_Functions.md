# teradataml Formula Functions

all_columns
teradataml.common.formula.Formula.all_columns
DESCRIPTION:
    Property to get the list of all columns used in formula.
PARAMETERS:
    None.
RETURNS:
    List of all columns used in formula.
RAISES:
    None.
EXAMPLES:
    # Load the data to run the example
    load_example_data("decisionforest", ["housing_train"])
    # Create teradataml DataFrame.
    housing_train = DataFrame.from_table("housing_train")
    # Example 1 -
    decision_forest_out1 = DecisionForest(formula = "homestyle ~ bedrooms + lotsize + gashw + driveway + stories + recroom + pr
                                          data = housing_train,
                                          tree_type = "classification",
                                          ntree = 50,
                                          nodesize = 1,
                                          variance = 0.0,
                                          max_depth = 12,
                                          mtry = 3,
                                          mtry_seed = 100,
                                          seed = 100)
    # Print all columns used in formula.
    decision_forest_out1.formula.all_columns
categorical_columns
teradataml.common.formula.Formula.categorical_columns
DESCRIPTION:
    Property to get the list of all independent categorical columns used in formula.
PARAMETERS:
    None.
RETURNS:
    List of categorical columns used in formula.
    If no categorical column is used in formula, property will return None.
RAISES:
    None.
EXAMPLES:
    # Load the data to run the example
    load_example_data("decisionforest", ["housing_train"])
    # Create teradataml DataFrame.
    housing_train = DataFrame.from_table("housing_train")
    # Example 1 -
    decision_forest_out1 = DecisionForest(formula = "homestyle ~ bedrooms + lotsize + gashw + driveway + stories + recroom + pr
9/14/2025, 16:50
Page 1,561 of 1,675
                                          data = housing_train,
                                          tree_type = "classification",
                                          ntree = 50,
                                          nodesize = 1,
                                          variance = 0.0,
                                          max_depth = 12,
                                          mtry = 3,
                                          mtry_seed = 100,
                                          seed = 100)
    # Print categorical columns used in formula.
    decision_forest_out1.formula.categorical_columns
numeric_columns
teradataml.common.formula.Formula.numeric_columns
DESCRIPTION:
    Property to get the list of all independent numerical columns used in formula.
PARAMETERS:
    None.
RETURNS:
    List of numerical columns used in formula.
    If no numerical column is used in formula, property will return None.
RAISES:
    None.
EXAMPLES:
    # Load the data to run the example
    load_example_data("decisionforest", ["housing_train"])
    # Create teradataml DataFrame.
    housing_train = DataFrame.from_table("housing_train")
    # Example 1 -
    decision_forest_out1 = DecisionForest(formula = "homestyle ~ bedrooms + lotsize + gashw + driveway + stories + recroom + pr
                                          data = housing_train,
                                          tree_type = "classification",
                                          ntree = 50,
                                          nodesize = 1,
                                          variance = 0.0,
                                          max_depth = 12,
                                          mtry = 3,
                                          mtry_seed = 100,
                                          seed = 100)
    # Print numeric columns used in formula.
    decision_forest_out1.formula.numeric_columns
response_columns
teradataml.common.formula.Formula.response_column
DESCRIPTION:
    Property to get the response column used in formula.
PARAMETERS:
    None.
RETURNS:
    Returns response column.
RAISES:
    None.
EXAMPLES:
    # Load the data to run the example
    load_example_data("decisionforest", ["housing_train"])
    # Create teradataml DataFrame.
    housing_train = DataFrame.from_table("housing_train")
    # Example 1 -
    decision_forest_out1 = DecisionForest(formula = "homestyle ~ bedrooms + lotsize + gashw + driveway + stories + recroom + pr
                                          data = housing_train,
                                          tree_type = "classification",
                                          ntree = 50,
                                          nodesize = 1,
                                          variance = 0.0,
                                          max_depth = 12,
                                          mtry = 3,
                                          mtry_seed = 100,
                                          seed = 100)