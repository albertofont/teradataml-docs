# Introduction and Reference Front Matter

This document provides detailed description and complete usage information for all the functions in the Teradata® Package for Python, teradataml.
teradataml Function Categories
Categories of teradataml package functions:
<name>: Analytic functions available to use as class. Executing each analytical function means, generating instance of the analytic function class.
DataFrame.<name>: Create a teradataml DataFrame for the underlying Teradata table.
<name>_context: API functions that manage the connection and certain internal data structures called context.
<TDMLDF>.<name>: teradataml DataFrame methods available for data manipulation, preparation and exploration.
Notes:
name is part of a function name that indicates the speciﬁc task of the function.
TDMLDF is teradataml DataFrame object.
teradataml Analytic Function Default Execution Locations
The teradataml analytics package includes two subpackages:
teradataml.analytics.mle
Analytic functions in the teradataml.analytics.mle subpackage require your system to have the Vantage Machine Learning Engine, which is a separate machine learning
legacy engine that is not part of the current standard Vantage offer. If your Vantage system does not have the required ML Engine, an error or no-op behavior will occur
when functions in this subpackage are invoked.
teradataml.analytics.sqle
All teradataml analytic functions are in either of these two subpackages.
For Vantage 1.0 the following nine teradataml analytic functions were executed by default in the Advanced SQL Engine:
Attribution
DecisionForestPredict
DecisionTreePredict
GLMPredict
NaiveBayesPredict
NaiveBayesTextClassiﬁerPredict
NPath
Sessionize
SVMSparsePredict
For Vantage 1.1 or later versions, six new functions are added to the list. The following 15 teradataml analytic functions are executed by default in the Advanced SQL Engine:
Antiselect
Attribution
DecisionForestPredict
DecisionTreePredict
GLMPredict
MovingAverage
NaiveBayesPredict
NaiveBayesTextClassiﬁerPredict
NGramSplitter
NPath
Pack
Sessionize
StringSimilarity
SVMSparsePredict
Unpack
All other teradataml analytic functions are executed by default in the Teradata Machine Learning Engine.
Notes:
Teradata recommends importing an analytic function in one of the following ways:
Preferred: Import from the teradataml package.
To choose the engine where the analytic function is used: Import from the subpackage.
The teradataml package includes a module (load_example_data) with datasets for the examples in analytic functions. To execute these examples, you need the
following:
To have a connection to Teradata Vantage
Import the load_example_data module to load the data, using the command:
from teradataml.data.load_example_data import load_example_data
Release Notes
Package: teradataml
Type: Package
Title: Teradata Package for Python
Version: 20.00.00.06
Updated: Jul 2025
Maintainer: Teradata Corporation
Depends:
Python (>= 3.8)
    python-dotenv
    teradatamlwidges (>=20.00.00.5)
    Imports:
    teradatasql (>= 20.00.00.19)
    teradatasqlalchemy (>= 20.00.00.04)
    pandas (>= 0.22.00)
    psutil
    requests (>= 2.25.1)
    scikit-learn (>= 0.24.2)
    IPython (>= 8.10.0)
    imbalanced-learn (>= 0.8.0)
    pyjwt (>= 2.8.0)
    cryptography (>= 42.0.5)
    sqlalchemy (>= 2.0)
    lightgbm (>= 3.3.3)
    Support for Open Analytics Framework on VantageCloud Lake
    Description: This package enables users to access Teradata Vantage database objects using the Python language.
    License: ﬁle LICENSE + ﬁle LICENSE-3RD-PARTY
    Compatibility Matrix
    teradataml: Vantage 1.0 vs Vantage 1.1 or later Compatibility Matrix
    See the _teradataml: Vantage 1.0 vs Vantage 1.1 or later Compatibility Matrix. You can download this ﬁle.
    Deprecated Analytic Functions
    All the deprecated analytic functions under the teradataml.analytics module have been removed. Newer versions of the functions are available under
    the teradataml.analytics.mle and the teradataml.analytics.sqle modules.
    The removed deprecated functions are as follows:
    teradataml.analytics.Antiselect
    teradataml.analytics.Arima
    teradataml.analytics.ArimaPredictor
    teradataml.analytics.Attribution
    teradataml.analytics.ConfusionMatrix
    teradataml.analytics.CoxHazardRatio
    teradataml.analytics.CoxPH
    teradataml.analytics.CoxSurvival
    teradataml.analytics.DecisionForest
    teradataml.analytics.DecisionForestEvaluator
    teradataml.analytics.DecisionForestPredict
    teradataml.analytics.DecisionTree
    teradataml.analytics.DecisionTreePredict
    teradataml.analytics.GLM
    teradataml.analytics.GLMPredict
    teradataml.analytics.KMeans
    teradataml.analytics.NGrams
    teradataml.analytics.NPath
    teradataml.analytics.NaiveBayes
    teradataml.analytics.NaiveBayesPredict
    teradataml.analytics.NaiveBayesTextClassiﬁer
    teradataml.analytics.NaiveBayesTextClassiﬁerPredict
    teradataml.analytics.Pack
    teradataml.analytics.SVMSparse
    teradataml.analytics.SVMSparsePredict
    teradataml.analytics.SentenceExtractor
    teradataml.analytics.Sessionize
    teradataml.analytics.TF
    teradataml.analytics.TFIDF
    teradataml.analytics.TextTagger
    teradataml.analytics.TextTokenizer
    teradataml.analytics.Unpack
    teradataml.analytics.VarMax