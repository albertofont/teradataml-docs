# teradataml Database 20.00.xx Analytic Functions

FEATURE ENGINEERING TRANSFORM functions
SMOTE
SMOTE
Functions
SMOTE(data=None, encoding_data=None, id_column=None, response_column=None, input_columns=None, categorical_columns=None,
median_standard_deviation=None, minority_class=None, oversampling_factor=5, sampling_strategy='smote', ﬁll_sampleid=True,
noninput_columns_value='sample', n_neighbors=5, seed=None, **generic_arguments)
    DESCRIPTION:
        SMOTE() function generates data by oversampling a minority class using 
        smote, adasyn, borderline-2 or smote-nc algorithms.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        encoding_data:
            Optional Argument, Required when "sampling_strategy" is set to 'smotenc' algorithm.
            Specifies the teradataml dataframe containing the ordinal encoding information.
            Types: teradataml DataFrame
        id_column:
            Required Argument.
            Specifies the name of the column in "data" that 
            uniquely identifies a data sample.
            Types: str
        response_column:
            Optional Argument.
            Specifies the name of the column in "data" that contains the 
            numeric value to be used as the response value for a sample.
            Types: str
        input_columns:
            Required Argument.
            Specifies the name of the input columns in "data" for oversampling.
            Types: str OR list of Strings (str)
        categorical_columns:
            Optional Argument, Required when "sampling_strategy" is set to 'smotenc' algorithm.
            Specifies the name of the categorical columns in the "data" that 
            the function uses for oversampling with smotenc.
            Types: str OR list of Strings (str)
        median_standard_deviation:
            Optional Argument, Required when "sampling_strategy" is set to 'smotenc' algorithm.
            Specifies the median of the standard deviations computed over the 
            numerical input columns.
            Types: float
        minority_class:
            Required Argument.
            Specifies the minority class for which synthetic samples need to be 
            generated. 
            Note:
                * The label for minority class under response column must be numeric integer.
            Types: str
        oversampling_factor:
            Optional Argument.
            Specifies the factor for oversampling the minority class.
            Default Value: 5
            Types: float
        sampling_strategy:
            Optional Argument.
            Specifies the oversampling algorithm to be used to create synthetic samples.
            Default Value: "smote"
            Permitted Values: "smote", "adasyn", "borderline", "smotenc"
            Types: str
        fill_sampleid:
            Optional Argument.
            Specifies whether to include the id of the original observation used 
            to generate each synthetic observation.
            Default Value: True
            Types: bool
        noninput_columns_value:
            Optional Argument.
            Specifies the value to put in a sample column for columns not 
            specified as input columns.
            Default Value: "sample"
            Permitted Values: "sample", "neighbor", "null"
            Types: str
        n_neighbors:
            Optional Argument.
            Specifies the number of nearest neighbors for choosing the sample to 
            be used in oversampling.
            Default Value: 5
            Types: int
        seed:
            Optional Argument.
            Specifies the random seed the algorithm uses for repeatable results. 
            The function uses the seed for random interpolation and generate the 
            synthetic sample.
            Types: int
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table or not. When set to True, 
                    results are persisted in a table; otherwise, 
                    results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table or not. When set to 
                    True, results are stored in a volatile table, 
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQL Engine function supports, else an 
                exception is raised.
    RETURNS:
        Instance of SMOTE.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as SMOTEObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage, before importing the 
        #        function in user space.
        #     2. User can import the function, if it is available on 
        #        Vantage user is connected to.
        #     3. To check the list of analytic functions available on 
        #        Vantage user connected to, use 
        #        "display_analytic_functions()".
        # Load the example data.
        load_example_data("dataframe", "iris_test")
        load_example_data("teradataml", "titanic")
        # Create teradataml DataFrame objects.
        iris_input = DataFrame.from_table("iris_test").iloc[:25]
        titanic_input = DataFrame("titanic").iloc[:50]
        # Create Encoding DataFrame objects.
        encoded_data = OrdinalEncodingFit(data=titanic_input,
                                          target_column=['sex','embarked'],
                                          approach="AUTO")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Import function SMOTE.
        from teradataml import SMOTE
        # Example 1 : Generate synthetic samples using smote algorithm.
        smote_out = SMOTE(data = iris_input,
                          n_neighbors = 5,
                          id_column='id',
                          minority_class='3',
                          response_column='species',
                          input_columns =['sepal_length', 'sepal_width', 'petal_length', 'petal_width'],
                          oversampling_factor=2,
                          sampling_strategy='smote',
                          seed=10)
        # Print the result DataFrame.
        print(smote_out.result)
        # Example 2 : Generate synthetic samples using smotenc algorithm with categorical columns.
        smote_out2 = SMOTE(data = titanic_input, 
                           encoding_data = encoded_data.result, 
                           id_column = 'passenger', 
                           response_column = 'survived', 
                           input_columns = ['parch', 'age', 'sibsp'], 
                           categorical_columns = ['sex', 'embarked'],
                           median_standard_deviation = 31.47806044604718,
                           minority_class = '1', 
                           oversampling_factor = 5,
                           sampling_strategy = "smotenc", 
                           noninput_columns_value = "null", 
                           n_neighbors = 5)
        # Print the result DataFrame.
        print(smote_out2.result)
    MODEL TRAINING functions
    KMeans
    KMeans
    Functions
    KMeans(data=None, centroids_data=None, id_column=None, target_columns=None, num_clusters=None, seed=None, threshold=0.0395, iter_max=10,
    num_init=1, output_cluster_assignment=False, initialcentroids_method='RANDOM', embedding_size=1, **generic_arguments)
    DESCRIPTION:
        The K-means() function groups a set of observations into k clusters
        in which each observation belongs to the cluster with the nearest mean
        (cluster centers or cluster centroid). This algorithm minimizes the
        objective function, that is, the total Euclidean distance of all data points
        from the center of the cluster as follows:
            1. Specify or randomly select k initial cluster centroids.
            2. Assign each data point to the cluster that has the closest centroid.
            3. Recalculate the positions of the k centroids.
            4. Repeat steps 2 and 3 until the centroids no longer move.
        The algorithm doesn't necessarily find the optimal configuration as it
        depends significantly on the initial randomly selected cluster centers.
        User can run the function multiple times to reduce the effect of this limitation.
        The above limitation can also be overcome by selecting initial centroids using 'KMeans++' algorithm.
        'KMeans++' algorithm is a smarter way for choosing initial centroids for KMeans clustering algorithm.
        The main idea is to select the initial centroids far away from each other. It reduces the possibility of 
        initial centroids being chosen from the same cluster. 
        'KMeans++' improves the overall quality of clustering and in some of the cases, can also speed up the convergence
        of KMeans algorithm.
        Also, this function returns the within-cluster-squared-sum, which user can use to
        determine an optimal number of clusters using the Elbow method.
        Note:
            * This function doesn't consider the "data" and "centroids_data"
              input rows that have a NULL entry in the specified "target_columns".
            * The function can produce deterministic output across different machine
              configurations if user provide the "centroids_data".
            * The function randomly samples the initial centroids from the "data",
              if "centroids_data" not provided. In this case, use of "seed"
              argument makes the function output deterministic on a machine with an
              assigned configuration. However, using the "seed" argument won't guarantee
              deterministic output across machines with different configurations.
            * This function requires the UTF8 client character set for UNICODE data.
            * This function does not support Pass Through Characters (PTCs).
            * For information about PTCs, see Teradata Vantage™ - Analytics Database
              International Character Set Support.
            * This function does not support KanjiSJIS or Graphic data types.
    PARAMETERS:
        data:
            Required Argument.
            Specifies the input teradataml DataFrame.
            Types: teradataml DataFrame
        centroids_data:
            Optional Argument.
            Specifies the input teradataml DataFrame containing
            set of initial centroids.
            Note:
                * This argument is not required if "num_clusters" provided.
                * If provided, the function uses the initial centroids
                  from this input.
            Types: teradataml DataFrame
        id_column:
            Required Argument.
            Specifies the input data column name that has the
            unique identifier for each row in the input.
            Types: str
        target_columns:
            Required Argument.
            Specifies the name(s) of the column(s) in "data" for clustering.
            Types: str OR list of Strings (str)
        num_clusters:
            Optional Argument.
            Specifies the number of clusters to be created.
            Note:
                This argument is not required if "centroids_data" provided.
            Types: int
        seed:
            Optional Argument.
            Specifies a non-negative integer value to randomly select the initial
            cluster centroid positions from the input.
            Note:
                * This argument is not required if "centroids_data" provided.
                * Random integer value will be used for "seed", if not passed.
            Types: int
        threshold:
            Optional Argument.
            Specifies the convergence threshold. The algorithm converges if the distance
            between the centroids from the previous iteration and the current iteration
            is less than the specified value.
            Default Value: 0.0395
            Types: float OR int
        iter_max:
            Optional Argument.
            Specifies the maximum number of iterations for the K-means algorithm.
            The algorithm stops after performing the specified number of iterations
            even if the convergence criterion is not met.
            Default Value: 10
            Types: int
        num_init:
            Optional Argument.
            Specifies the number of times, the k-means algorithm will be run with different
            initial centroid seeds. The function will emit out the model having
            the least value of Total Within Cluster Squared Sum.
            Note:
                This argument is not required if "centroids_data" is provided.
            Default Value: 1
            Types: int
        output_cluster_assignment:
            Optional Argument.
            Specifies whether to output Cluster Assignment information.
            Default Value: False
            Types: bool
        initialcentroids_method:
            Optional Argument.
            Specifies the initialization method to be used for selecting initial set of centroids.
            Permitted Values: 
                * 'RANDOM': The initial set of centroids are selected randomly.
                * 'KMEANS++': The initial set of centroids are selected using KMeans++ algorithm.
            Default Value: 'RANDOM'
            Note:
                * This argument is not required if "centroids_data" is provided.
            Types: str
        embedding_size:
            Optional Argument.
            Specifies the embedding size of the vectors.
            Default Value: 1
            Types: int
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local
            order the input data. These generic arguments are available
            for each argument that accepts teradataml DataFrame as
            input and can be accessed as:
                * "<input_data_arg_name>_partition_column" accepts str or
                  list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list
                  of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list
                  of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if
                the underlying SQL Engine function supports, else an
                exception is raised.
    RETURNS:
        Instance of KMeans.
        Output teradataml DataFrames can be accessed using attribute
        references, such as KMeansObj.<attribute_name>.
        Output teradataml DataFrame attribute names are:
            1. result
            2. model_data
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("kmeans", "computers_train1")
        load_example_data("kmeans",'kmeans_table')
        # Create teradataml DataFrame objects.
        computers_train1 = DataFrame.from_table("computers_train1")
        kmeans_tab = DataFrame('kmeans_table')
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Grouping a set of observations into 2 clusters in which
        #             each observation belongs to the cluster with the nearest mean.
        KMeans_out = KMeans(id_column="id",
                            target_columns=['price', 'speed'],
                            data=computers_train1,
                            num_clusters=2)
        # Print the result DataFrames.
        print(KMeans_out.result)
        print(KMeans_out.model_data)
        # Example 2 : Grouping a set of observations by specifying initial
        #             centroid data.
        # Get the set of initial centroids by accessing the group of rows
        # from input data.
        kmeans_initial_centroids_table = computers_train1.loc[[19, 97]]
        kmeans_initial_centroids = kmeans_tab.loc[[2, 4]]
        KMeans_out_1 = KMeans(id_column="id",
                              target_columns=['price', 'speed'],
                              data=computers_train1,
                              centroids_data=kmeans_initial_centroids_table)
        # Print the result DataFrames.
        print(KMeans_out_1.result)
        print(KMeans_out_1.model_data)
        # Example 3 : Grouping a set of observations into 2 clusters by
        #             specifying the number of clusters and seed value
        #             with output cluster assignment information.
        obj = KMeans(data=kmeans_tab,
                     id_column='id',
                     target_columns=['c1', 'c2'],
                     threshold=0.0395,
                     iter_max=3,
                     centroids_data=kmeans_initial_centroids,
                     output_cluster_assignment=True)
        # Print the result DataFrames.
        print(obj.result)
        # Example 4 : Grouping a set of observations into 3 clusters by
        #             specifying the number of clusters for initial centroids
        #             method as KMEANS++.
        obj = KMeans(data=kmeans_tab,
                     id_column='id',
                     target_columns=['c1', 'c2'],
                     threshold=0.0395,
                     iter_max=3,
                     num_clusters=3,
                     output_cluster_assignment=True,
                     initialcentroids_method="KMEANS++")
        # Print the result DataFrames.
        print(obj.result)
    VectorDistance
    VectorDistance
    Functions
    VectorDistance(target_data=None, reference_data=None, target_id_column=None, target_feature_columns=None, ref_id_column=None,
    ref_feature_columns=None, distance_measure='COSINE', topk=10, pvalue=2, search_thresholds=None, largereference_input=False, output_similarity=False,
    embedding_size=1, **generic_arguments)
    DESCRIPTION:
        The VectorDistance() function accepts a dataframe of target vectors and a dataframe of reference vectors and
        returns output containing the distance between target-reference pairs.
        The function computes the distance between the target pair and the reference pair from the same input
        if you provide only one input.
        You must have the same column order in the "target_feature_columns" argument and the "ref_feature_columns"
        argument. The function ignores the feature values during distance computation if the value is either
        None, NAN, or INF.
        The function returns N*N output if you use the "topk" value as -1 because the function includes
        all reference vectors in the output.
        Note:
            * The algorithm used in this function is of the order of N*N (where N is the number of rows).
                Hence, expect the function to run significantly longer as the number of rows increases in either
                the "target_data" or the "reference_data".
            * Because the reference data is copied to the spool for each AMP before running the query, the user
                spool limits the size and scalability of the input.
            * This function requires the UTF8 client character set for UNICODE data.
            * This function does not support Pass Through Characters (PTCs).
                For information about PTCs, see Teradata Vantage™ - Analytics Database International Character Set
                Support.
            * This function does not support KanjiSJIS or Graphic data types.
    PARAMETERS:
        target_data:
            Required Argument.
            Specifies the teradataml DataFrame containing target data vectors.
            Types: teradataml DataFrame
        reference_data:
            Optional Argument.
            Specifies the teradataml DataFrame containing reference data vectors.
            Types: teradataml DataFrame
        target_id_column:
            Required Argument.
            Specifies the name of the "target_data" column that contains
            identifiers of the target data vectors.
            Types: str
        target_feature_columns:
            Required Argument.
            Specifies the names of the "target_data" columns that contain features
            of the target data vectors.
            Note:
                You can specify up to 2018 feature columns.
            Types: str OR list of Strings (str)
        ref_id_column:
            Optional Argument.
            Specifies the name of the "reference_data" column that contains
            identifiers of the reference data vectors.
            Types: str
        ref_feature_columns:
            Optional Argument.
            Specifies the names of the "reference_data" columns that contain
            features of the reference data vectors.
            Note:
                You can specify up to 2018 feature columns.
            Types: str OR list of Strings (str)
        distance_measure:
            Optional Argument.
            Specifies the distance type to compute between the target and the reference vector.
            Default Value: "COSINE"
            Permitted Values:
                * Cosine: Cosine distance between the target vector and the reference vector.
                * Euclidean: Euclidean distance between the target vector and the reference vector.
                * Manhattan: Manhattan distance between the target vector and the reference vector.
                * DotProduct: Dot product between the target vector and the reference vector.
                * Minkowski: Minkowski distance between the target vector and the reference vector.
            Types: str OR list of strs
        topk:
            Optional Argument.
            Specifies the maximum number of closest reference vectors to include in the output table
            for each target vector. The value k is an integer between 1 and 100.
            Note:
                * If the "search_thresholds" argument is specified, then "topk" argument cannot be used.
            Default Value: 10
            Types: int
        pvalue:
            Optional Argument.
            Specifies the p-value for the Minkowski distance.
            Default Value: 2
            Types: int
        search_thresholds:
            Optional Argument.
            Specifies the thresholds between a pair of target and reference vectors.
            Note:
                * If the "topk" argument is specified, then "search_thresholds" argument cannot be used.
            Types: float OR list of floats
        largereference_input:
            Optional Argument.
            Specifies that if the "reference_data" is much larger than the "target_data", setting this to True  
            treats the reference data as 'Partition By Any' and the target data as 'Dimension'.
            Default Value: False
            Types: bool
        output_similarity:
            Optional Argument.
            Specifies whether to output similarity instead of distance.
            Default Value: False
            Types: bool
        embedding_size:
            Optional Argument.
            Specifies the embedding size of the vectors.
            Default Value: 1
            Types: int
        **generic_arguments:
            Specifies the generic keyword arguments SQLE functions accept. Below 
            are the generic keyword arguments:
                persist:
                    Optional Argument.
                    Specifies whether to persist the results of the 
                    function in a table or not. When set to True,
                    results are persisted in a table; otherwise,
                    results are garbage collected at the end of the 
                    session.
                    Default Value: False
                    Types: bool
                volatile:
                    Optional Argument.
                    Specifies whether to put the results of the 
                    function in a volatile table or not. When set to
                    True, results are stored in a volatile table,
                    otherwise not.
                    Default Value: False
                    Types: bool
            Function allows the user to partition, hash, order or local 
            order the input data. These generic arguments are available 
            for each argument that accepts teradataml DataFrame as 
            input and can be accessed as:    
                * "<input_data_arg_name>_partition_column" accepts str or 
                    list of str (Strings)
                * "<input_data_arg_name>_hash_column" accepts str or list 
                    of str (Strings)
                * "<input_data_arg_name>_order_column" accepts str or list 
                    of str (Strings)
                * "local_order_<input_data_arg_name>" accepts boolean
            Note:
                These generic arguments are supported by teradataml if 
                the underlying SQLE Engine function supports, else an 
                exception is raised.
    RETURNS:
        Instance of VectorDistance.
        Output teradataml DataFrames can be accessed using attribute 
        references, such as VectorDistanceObj.<attribute_name>.
        Output teradataml DataFrame attribute name is:
            result
    RAISES:
        TeradataMlException, TypeError, ValueError
    EXAMPLES:
        # Notes:
        #     1. Get the connection to Vantage to execute the function.
        #     2. One must import the required functions mentioned in
        #        the example from teradataml.
        #     3. Function will raise error if not supported on the Vantage
        #        user is connected to.
        # Load the example data.
        load_example_data("vectordistance", ["target_mobile_data_dense", "ref_mobile_data_dense"])
        # Create teradataml DataFrame objects.
        target_mobile_data_dense=DataFrame("target_mobile_data_dense")
        ref_mobile_data_dense=DataFrame("ref_mobile_data_dense")
        # Check the list of available analytic functions.
        display_analytic_functions()
        # Example 1 : Compute the cosine, euclidean, manhattan distance between the target and reference vectors.
        VectorDistance_out = VectorDistance(target_id_column="userid",
                                            target_feature_columns=['CallDuration', 'DataCounter', 'SMS'],
                                            ref_id_column="userid",
                                            ref_feature_columns=['CallDuration', 'DataCounter', 'SMS'],
                                            distance_measure=['Cosine', 'Euclidean', 'Manhattan'],
                                            topk=2,
                                            target_data=target_mobile_data_dense,
                                            reference_data=ref_mobile_data_dense)
        # Print the result DataFrame.
        print(VectorDistance_out.result)
        # Example 2 : Compute the Minkowski distance between the target and reference vectors.
        VectorDistance_out = VectorDistance(target_data = target_mobile_data_dense,
                                            reference_data = ref_mobile_data_dense,
                                            target_id_column = 'userid',
                                            ref_id_column = 'userid', 
                                            ref_feature_columns = ['CallDuration','DataCounter','SMS'],
                                            target_feature_columns = ['CallDuration','DataCounter','SMS'],
                                            distance_measure = ['euclidean','Minkowski'],
                                            topk = 1,
                                            pvalue = 2)
        # Print the result DataFrame.
        print(VectorDistance_out.result)