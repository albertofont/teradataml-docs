# Enterprise Feature Store Functions

Feature
__init__
teradataml.store.feature_store.models.Feature.__init__ = __init__(self, name, column, feature_type=<FeatureType.CONTINUOUS: 1>, description=None, tags=None,
status=<FeatureStatus.ACTIVE: 1>)
DESCRIPTION:
    Constructor for Feature.
PARAMETERS:
    name:
        Required Argument.
        Specifies the unique name of the Feature.
        Types: str.
    column:
        Required Argument.
        Specifies the DataFrame Column.
        Types: teradataml DataFrame Column
    feature_type:
        Optional Argument.
        Specifies whether feature is continuous or discrete.
        Default Value: FeatureType.CONTINUOUS
        Types: FeatureType Enum
    description:
        Optional Argument.
        Specifies human readable description for Feature.
        Types: str
    tags:
        Optional Argument.
        Specifies the tags for Feature.
        Types: str OR list of str
    status:
        Optional Argument.
        Specifies whether feature is archived or active.
        Types: FeatureStatus Enum
RETURNS:
    None.
RAISES:
    None
EXAMPLES:
    >>> from teradataml import DataFrame, Feature, FeatureType, load_example_data
    # Load the sales data to Vantage.
    >>> load_example_data("dataframe", "sales")
    # Create DataFrame on sales data.
    >>> df = DataFrame("sales")
    >>> df
    >>> df
                  Feb    Jan    Mar    Apr    datetime
    accounts
    Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
    Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
    Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
    Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
    Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
    # create a Categorical Feature for column 'Feb' for 'sales' DataFrame and name it as
    # 'sales_Feb'.
    >>> from teradataml import Feature
    >>> feature = Feature('sales_Feb', column=df.Feb, feature_type=FeatureType.CATEGORICAL)
    >>> feature
    Feature(name=sales_Feb)
    >>>
Entity
__init__
teradataml.store.feature_store.models.Entity.__init__ = __init__(self, name, columns, description=None)
DESCRIPTION:
    Constructor for creating Entity Object.
PARAMETERS:
    name:
        Required Argument.
        Specifies the unique name of the entity.
        Types: str.
    columns:
        Required Argument.
        Specifies the names of the columns.
        Types: teradataml DataFrame Column OR list of teradataml DataFrame Columns.
    description:
        Optional Argument.
        Specifies human readable description for Feature.
        Types: str
RETURNS:
    Object of Entity.
RAISES:
    None
EXAMPLES:
    >>> load_example_data('dataframe', ['sales'])
    >>> df = DataFrame("sales")
    # create a Entity with column 'accounts' for 'sales' DataFrame and name it as
    # 'sales_accounts'.
    >>> from teradataml import Entity
    >>> entity = Entity('sales_accounts', df.accounts)
    >>> entity
    Entity(name=sales_accounts)
    >>>
Data Source
__init__
teradataml.store.feature_store.models.DataSource.__init__ = __init__(self, name, source, description=None, timestamp_col_name=None)
DESCRIPTION:
    Constructor for creating DataSource Object.
PARAMETERS:
    name:
        Required Argument.
        Specifies the unique name of the DataSource.
        Types: str.
    source:
        Required Argument.
        Specifies the source query of DataSource.
        Types: str OR teradataml DataFrame.
    description:
        Optional Argument.
        Specifies human readable description for DataSource.
        Types: str
    timestamp_col_name:
        Optional Argument.
        Specifies the timestamp column indicating when the row was created.
        Types: str
RETURNS:
    Object of DataSource.
RAISES:
    None
EXAMPLES:
    >>> load_example_data('dataframe', ['sales'])
    >>> df = DataFrame("sales")
    # Example 1: create a DataSource for above mentioned DataFrame with name 'Sales_Data'.
    >>> from teradataml import DataSource
    >>> data_source = DataSource('Sales_Data', df)
    >>> data_source
    DataSource(Sales_Data)
    >>>
Feature Group
Methods of Feature Group
__init__
teradataml.store.feature_store.models.FeatureGroup.__init__ = __init__(self, name, features, entity, data_source, description=None)
DESCRIPTION:
    Constructor for creating FeatureGroup Object.
PARAMETERS:
    name:
        Required Argument.
        Specifies the unique name of the FeatureGroup.
        Types: str.
    features:
        Required Argument.
        Specifies the features required to create a group.
        Types: Feature or list of Feature.
    entity:
        Required Argument.
        Specifies the entity associated with corresponding features.
        Types: Entity
    data_source:
        Required Argument.
        Specifies the DataSource associated with Features.
        Types: str
    description:
        Optional Argument.
        Specifies human readable description for DataSource.
        Types: str
RETURNS:
    Object of FeatureGroup.
RAISES:
    None
EXAMPLES:
    >>> load_example_data('dataframe', ['sales'])
    >>> df = DataFrame("sales")
    # Example 1: create a FeatureGroup for above mentioned DataFrame.
    >>> # First create the features.
    >>> jan_feature = Feature("sales:Jan", df.Jan)
    >>> feb_feature = Feature("sales:Fan", df.Feb)
    >>> mar_feature = Feature("sales:Mar", df.Mar)
    >>> apr_feature = Feature("sales:Apr", df.Apr)
    >>> # Create Entity.
    >>> entity = Entity("sales:accounts", df.accounts)
    >>> # Create DataSource
    >>> data_source = DataSource("sales_source", df.show_query())
    >>> # Create FeatureGroup.
    >>> fg = FeatureGroup('Sales',
    ...                   features=[jan_feature, feb_feature, mar_feature, apr_feature],
    ...                   entity=entity,
    ...                   data_source=data_source)
apply
teradataml.store.feature_store.models.FeatureGroup.apply = apply(self, object)
DESCRIPTION:
    Register objects to FeatureGroup.
PARAMETERS:
    object:
        Required Argument.
        Specifies the object to update the FeatureGroup.
        Types: Feature OR DataSource OR Entity.
RETURNS:
    bool.
RAISES:
    TeradataMLException
EXAMPLES:
    >>> load_example_data('dataframe', ['sales'])
    >>> df = DataFrame("sales")
    >>> # Create FeatureGroup to use it in examples.
    >>> from teradataml import Feature, Entity, DataSource, FeatureGroup
    >>> feature = Feature('sales:Feb', df.Feb)
    >>> entity = Entity('sales:accounts', df.accounts)
    >>> data_source = DataSource('Sales_Data', df)
    >>> fg = FeatureGroup('Sales',
    ...                   features=feature,
    ...                   entity=entity,
    ...                   data_source=data_source)
    # Example 1: create a new Feature for column df.Mar and
    #            apply the feature to FeatueGroup.
    >>> # Create Feature.
    >>> feature = Feature('sales:Mar', df.Mar)
    >>> # Register the above Feature with FeatureGroup.
    >>> fg.apply(feature)
    True
    >>>
from_DataFrame
teradataml.store.feature_store.models.FeatureGroup.from_DataFrame = from_DataFrame(name, entity_columns, df, timestamp_col_name=None) method of builtins.type
instance
DESCRIPTION:
    Method to create FeatureGroup from DataFrame.
PARAMETERS:
    name:
        Required Argument.
        Specifies the name of the FeatureGroup.
        Note:
            * Entitiy, DataSource also get the same name as "name".
              User's can change the name of Entity or DataSource by accessing
              object from FeatureGroup.
        Types: str.
    entity_columns:
        Required Argument.
        Specifies the column names for the Entity.
        Types: str or list of str.
    df:
        Required Argument.
        Specifies teradataml DataFrame for creating DataSource.
        Types: teradataml DataFrame.
    timestamp_col_name:
        Optional Argument.
        Specifies the name of the column in the Query which
        holds the record creation time.
        Types: str
RETURNS:
    FeatureGroup
RAISES:
    None
EXAMPLES:
    >>> load_example_data('dataframe', ['sales'])
    >>> df = DataFrame("sales")
    # Example 1: create a FeatureGroup from DataFrame created on 'sales' table and
    #            consider 'accounts' column as entity and 'datetime' column
    #            as timestamp_col_name.
    >>> from teradataml import FeatureGroup
    >>> df = DataFrame("sales")
    >>> fg = FeatureGroup.from_DataFrame(
    ...             name='sales',
    ...             entity_columns='accounts',
    ...             df=df,
    ...             timestamp_col_name='datetime'
    ...         )
from_query
teradataml.store.feature_store.models.FeatureGroup.from_query = from_query(name, entity_columns, query, timestamp_col_name=None) method of builtins.type instance
DESCRIPTION:
    Method to create FeatureGroup from Query.
PARAMETERS:
    name:
        Required Argument.
        Specifies the name of the FeatureGroup.
        Note:
            * Entitiy, DataSource also get the same name as "name".
              Users can change the name of Entity or DataSource by accessing
              object from FeatureGroup.
        Types: str.
    entity_columns:
        Required Argument.
        Specifies the column names for the Entity.
        Types: str or list of str.
    query:
        Required Argument.
        Specifies the query for DataSource.
        Types: str.
    timestamp_col_name:
        Optional Argument.
        Specifies the name of the column in the Query which
        holds the record creation time.
        Types: str
RETURNS:
    FeatureGroup
RAISES:
    None
EXAMPLES:
    >>> load_example_data('dataframe', ['sales'])
    >>> df = DataFrame("sales")
    # Example 1: create a FeatureGroup from query 'SELECT * FROM SALES' and
    #            consider 'accounts' column as entity and 'datetime' column
    #            as timestamp_col_name.
    >>> from teradataml import FeatureGroup
    >>> query = 'SELECT * FROM SALES'
    >>> fg = FeatureGroup.from_query(
    ...             name='sales',
    ...             entity_columns='accounts',
    ...             query=query,
    ...             timestamp_col_name='datetime'
    ...         )
remove
teradataml.store.feature_store.models.FeatureGroup.remove = remove(self, object)
DESCRIPTION:
    Method to remove the objects from FeatureGroup. One can use this
    method to detach either Feature or DataSource or Entity from
    FeatureGroup. Much useful to remove existing Features from
    FeatureGroup.
PARAMETERS:
    object:
        Required Argument.
        Specifies the object to be removed from FeatureGroup.
        Types: Feature OR Entity OR DataSource OR FeatureGroup.
RETURNS:
    bool.
RAISES:
    TeradataMlException
EXAMPLES:
    >>> load_example_data('dataframe', ['sales'])
    >>> df = DataFrame("sales")
    >>> # First create the features.
    >>> jan_feature = Feature("sales:Jan", df.Jan)
    >>> feb_feature = Feature("sales:Fan", df.Feb)
    >>> mar_feature = Feature("sales:Mar", df.Mar)
    >>> apr_feature = Feature("sales:Jan", df.Apr)
    >>> # Create Entity.
    >>> entity = Entity("sales:accounts", df.accounts)
    >>> # Create DataSource
    >>> data_source = DataSource("sales_source", df.show_query())
    >>> # Create FeatureGroup.
    >>> fg = FeatureGroup('Sales',
    ...                   features=[jan_feature, feb_feature, mar_feature],
    ...                   entity=entity,
    ...                   data_source=data_source)
    # Example: Remove the Feature with name "sales:Feb" from FeatureGroup.
    >>> fg.remove(feb_feature)
    True
    >>>
reset_labels
teradataml.store.feature_store.models.FeatureGroup.reset_labels = reset_labels(self)
DESCRIPTION:
    Resets the labels for FeatureGroup.
PARAMETERS:
    None
RETURNS:
    bool
RAISES:
    None
EXAMPLES:
    >>> from teradataml import DataSource, Entity, Feature, FeatureGroup, load_example_data
    >>> load_example_data("dataframe", "admissions_train")
    >>> # Let's create DataFrame first.
    >>> df = DataFrame("admissions_train")
    >>> # create the features.
    >>> masters_feature = Feature("masters", df.masters)
    >>> gpa_feature = Feature("gpa", df.gpa)
    >>> stats_feature = Feature("stats", df.stats)
    >>> admitted_feature = Feature("admitted", df.admitted)
    >>> # Create Entity.
    >>> entity = Entity("id", df.id)
    >>> # Create DataSource
    >>> data_source = DataSource("admissions_source", df)
    >>> # Create FeatureGroup.
    >>> fg = FeatureGroup('Admissions',
    ...                   features=[masters_feature, gpa_feature, stats_feature, admitted_feature],
    ...                   entity=entity,
    ...                   data_source=data_source)
    >>> # Set feature 'admitted' as label.
    >>> fg.set_labels('admitted')
    True
    >>> # Remove the labels from FeatureGroup.
    >>> fg.reset_labels()
    True
    >>>
set_labels
teradataml.store.feature_store.models.FeatureGroup.set_labels = set_labels(self, labels)
DESCRIPTION:
    Sets the labels for FeatureGroup.
    This method is helpful, when working with analytic functions to consume the Features. 
    Note:
        Label is for the current session only.
PARAMETERS:
    labels:
        Required Argument.
        Specifies the name(s) of the features to refer as labels.
        Types: str or list of str
RETURNS:
    bool
RAISES:
    None
EXAMPLES:
    >>> from teradataml import DataSource, Entity, Feature, FeatureGroup, load_example_data
    >>> load_example_data("dataframe", "admissions_train")
    >>> # Let's create DataFrame first.
    >>> df = DataFrame("admissions_train")
    >>> # create the features.
    >>> masters_feature = Feature("masters", df.masters)
    >>> gpa_feature = Feature("gpa", df.gpa)
    >>> stats_feature = Feature("stats", df.stats)
    >>> admitted_feature = Feature("admitted", df.admitted)
    >>> # Create Entity.
    >>> entity = Entity("id", df.id)
    >>> # Create DataSource
    >>> data_source = DataSource("admissions_source", df)
    >>> # Create FeatureGroup.
    >>> fg = FeatureGroup('Admissions',
    ...                   features=[masters_feature, gpa_feature, stats_feature, admitted_feature],
    ...                   entity=entity,
    ...                   data_source=data_source)
    >>> # Set feature 'admitted' as label.
    >>> fg.set_labels('admitted')
    True
Properties of Feature Store
features
teradataml.store.feature_store.models.FeatureGroup.features
DESCRIPTION:
    Get's the features from FeatureGroup.
PARAMETERS:
    None
RETURNS:
    list
RAISES:
    None
EXAMPLES:
    >>> from teradataml import DataSource, Entity, Feature, FeatureGroup, load_example_data
    >>> load_example_data("dataframe", "sales")
    >>> # Let's create DataFrame first.
    >>> df = DataFrame("sales")
    >>> # create the features.
    >>> jan_feature = Feature("sales:Jan", df.Jan)
    >>> feb_feature = Feature("sales:Fan", df.Feb)
    >>> mar_feature = Feature("sales:Mar", df.Mar)
    >>> apr_feature = Feature("sales:Apr", df.Apr)
    >>> # Create Entity.
    >>> entity = Entity("sales:accounts", df.accounts)
    >>> # Create DataSource
    >>> data_source = DataSource("sales_source", df)
    >>> # Create FeatureGroup.
    >>> fg = FeatureGroup('Sales',
    ...                   features=[jan_feature, feb_feature, mar_feature, apr_feature],
    ...                   entity=entity,
    ...                   data_source=data_source)
    # Get the features from FeatureGroup
    >>> fg.features
    [Feature(name=sales:Jan), Feature(name=sales:Fan), Feature(name=sales:Mar), Feature(name=sales:Apr)]
    >>>
labels
teradataml.store.feature_store.models.FeatureGroup.labels
DESCRIPTION:
    Get's the labels from FeatureGroup.
    Note:
        Use this function only after setting the labels using "set_labels".
PARAMETERS:
    None
RETURNS:
    Feature OR list
RAISES:
    None
EXAMPLES:
    >>> from teradataml import DataSource, Entity, Feature, FeatureGroup, load_example_data
    >>> load_example_data("dataframe", "admissions_train")
    >>> # Let's create DataFrame first.
    >>> df = DataFrame("admissions_train")
    >>> # create the features.
    >>> masters_feature = Feature("masters", df.masters)
    >>> gpa_feature = Feature("gpa", df.gpa)
    >>> stats_feature = Feature("stats", df.stats)
    >>> admitted_feature = Feature("admitted", df.admitted)
    >>> # Create Entity.
    >>> entity = Entity("id", df.id)
    >>> # Create DataSource
    >>> data_source = DataSource("admissions_source", df)
    >>> # Create FeatureGroup.
    >>> fg = FeatureGroup('Admissions',
    ...                   features=[masters_feature, gpa_feature, stats_feature, admitted_feature],
    ...                   entity=entity,
    ...                   data_source=data_source)
    >>> # Set feature 'admitted' as label.
    >>> fg.set_labels('admitted')
    True
    # Get the labels from FeatureGroup
    >>> fg.labels
    Feature(name=admitted)
    >>>
Feature Store
Methods of Feature Store
__init__
teradataml.store.feature_store.feature_store.FeatureStore.__init__ = __init__(self, repo)
DESCRIPTION:
    Method to create FeatureStore in teradataml.
PARAMETERS:
    repo:
        Required Argument.
        Specifies the repository name.
        Types: str.
RETURNS:
    Object of FeatureStore.
RAISES:
    None
EXAMPLES:
    >>> # Create FeatureStore for repository 'vfs_v1'.
    >>> from teradataml import FeatureStore
    >>> fs = FeatureStore('vfs_v1')
    >>> fs
    FeatureStore(vfs_v1)-v1.0
    >>>
apply
teradataml.store.feature_store.feature_store.FeatureStore.apply = apply(self, object)
DESCRIPTION:
    Register objects to repository.
PARAMETERS:
    object:
        Required Argument.
        Specifies the object to update the repository.
        Types: Feature OR DataSource OR Entity OR FeatureGroup.
RETURNS:
    bool.
RAISES:
    TeradataMLException
EXAMPLES:
    >>> load_example_data('dataframe', ['sales'])
    >>> df = DataFrame("sales")
    # Example 1: create a Feature for column 'Feb' from 'sales' DataFrame
    #            and register with repo 'vfs_v1'.
    >>> # Create Feature.
    >>> from teradataml import Feature
    >>> feature = Feature('sales:Feb', df.Feb)
    >>> # Register the above Feature with repo.
    >>> fs = FeatureStore('vfs_v1')
    >>> fs.apply(feature)
    True
    >>>
    # Example 2: create Entity for 'sales' DataFrame and register
    #            with repo 'vfs_v1'.
    >>> # Create Entity.
    >>> from teradataml import Entity
    >>> entity = Entity('sales:accounts', df.accounts)
    >>> # Register the above Entity with repo.
    >>> fs = FeatureStore('vfs_v1')
    >>> fs.apply(entity)
    True
    >>>
    # Example 3: create DataSource for 'sales' DataFrame and register
    #            with repo 'vfs_v1'.
    >>> # Create DataSource.
    >>> from teradataml import DataSource
    >>> ds = DataSource('Sales_Data', df)
    >>> # Register the above DataSource with repo.
    >>> fs = FeatureStore('vfs_v1')
    >>> fs.apply(ds)
    True
    >>>
    # Example 4: create FeatureStore with all the objects
    #            created in above examples and register with
    #            repo 'vfs_v1'.
    >>> # Create FeatureGroup.
    >>> from teradataml import FeatureGroup
    >>> fg = FeatureGroup('Sales',
    ...                   features=feature,
    ...                   entity=entity,
    ...                   data_source=data_source)
    >>> # Register the above FeatureStore with repo.
    >>> fs = FeatureStore('vfs_v1')
    >>> fs.apply(fg)
    True
    >>>
archive_data_source
teradataml.store.feature_store.feature_store.FeatureStore.archive_data_source = archive_data_source(self, data_source)
DESCRIPTION:
    Archives DataSource from repository. Note that archived DataSource
    is not available for any further processing. Archived DataSource can be 
    viewed using "list_archived_data_sources()" method.
PARAMETERS:
    data_source:
        Required Argument.
        Specifies either the name of DataSource or Object of DataSource
        to archive from repository.
        Types: str OR DataSource
RETURNS:
    bool
RAISES:
    TeradataMLException, TypeError, ValueError
EXAMPLES:
    >>> from teradataml import DataSource, FeatureStore, load_example_data
    # Create a DataSource using SELECT statement.
    >>> ds = DataSource(name="sales_data", source="select * from sales")
    # Create FeatureStore for repo 'vfs_v1'.
    >>> fs = FeatureStore("vfs_v1")
    # Apply DataSource to FeatureStore.
    >>> fs.apply(ds)
    True
    # List the available DataSources.
    >>> fs.list_data_sources()
               description timestamp_col_name               source
    name
    sales_data        None               None  select * from sales
    # Archive DataSource with name "sales_data".
    >>> fs.archive_data_source("sales_data")
    DataSource 'sales_data' is archived.
    True
    >>>
    # List the available DataSources after archive.
    >>> fs.list_data_sources()
    Empty DataFrame
    Columns: [description, timestamp_col_name, source]
    Index: []
archive_entity
teradataml.store.feature_store.feature_store.FeatureStore.archive_entity = archive_entity(self, entity)
DESCRIPTION:
    Archives Entity from repository. Note that archived Entity
    is not available for any further processing. Archived Entity can be
    viewed using "list_archived_entities()" method.
PARAMETERS:
    entity:
        Required Argument.
        Specifies either the name of Entity or Object of Entity
        to remove from repository.
        Types: str OR Entity
RETURNS:
    bool.
RAISES:
    TeradataMLException, TypeError, ValueError
EXAMPLES:
    >>> from teradataml import DataFrame, Entity, FeatureStore
    >>> load_example_data('dataframe', ['sales'])
    # Create teradataml DataFrame.
    >>> df = DataFrame("sales")
    # Create Entity using teradataml DataFrame Column.
    >>> entity = Entity(name="sales_data", columns=df.accounts)
    # Create FeatureStore for repo 'staging_repo'.
    >>> fs = FeatureStore("staging_repo")
    # Apply the entity to FeatureStore.
    >>> fs.apply(entity)
    True
    # List all the available entities.
    >>> fs.list_entities()
                             description
    name       entity_column
    sales_data accounts             None
    # Archive Entity with name "sales_data".
    >>> fs.archive_entity(entity=entity.name)
    Entity 'sales_data' is archived.
    True
    # List the entities after archive.
    >>> fs.list_entities()
    Empty DataFrame
    Columns: [description]
    Index: []
archive_feature
teradataml.store.feature_store.feature_store.FeatureStore.archive_feature = archive_feature(self, feature)
DESCRIPTION:
    Archives Feature from repository. Note that archived Feature
    is not available for any further processing. Archived Feature can be
    viewed using "list_archived_features()" method.
PARAMETERS:
    feature:
        Required Argument.
        Specifies either the name of Feature or Object of Feature
        to archive from repository.
        Types: str OR Feature
RETURNS:
    bool
RAISES:
    TeradataMLException, TypeError, ValueError
EXAMPLES:
    >>> from teradataml import DataFrame, Feature, FeatureStore
    >>> load_example_data('dataframe', ['sales'])
    # Create teradataml DataFrame.
    >>> df = DataFrame("sales")
    # Create Feature for Column 'Feb'.
    >>> feature = Feature(name="sales_data_Feb", column=df.Feb)
    # Create FeatureStore for the repo 'staging_repo'.
    >>> fs = FeatureStore("staging_repo")
    # Apply the Feature to FeatureStore.
    >>> fs.apply(feature)
    True
    # List the available Features.
    >>> fs.list_features()
                   column_name description               creation_time modified_time  tags data_type feature_type  status group
    name
    sales_data_Feb         Feb        None  2024-10-
03 18:21:03.720464          None  None     FLOAT   CONTINUOUS  ACTIVE       None
    # Archive Feature with name "sales_data_Feb".
    >>> fs.archive_feature(feature=feature)
    Feature 'sales_data_Feb' is archived.
    True
    # List the available Features after archive.
    >>> fs.list_features()
    Empty DataFrame
    Columns: [column_name, description, creation_time, modified_time, tags, data_type, feature_type, status, group_name]
    Index: []
    >>>
archive_feature_group
teradataml.store.feature_store.feature_store.FeatureStore.archive_feature_group = archive_feature_group(self, feature_group)
DESCRIPTION:
    Archives FeatureGroup from repository. Note that archived FeatureGroup
    is not available for any further processing. Archived FeatureGroup can be
    viewed using "list_archived_feature_groups()" method.
    Note:
        The function archives the associated Features, Entity and DataSource
        if they are not associated with any other FeatureGroups.
PARAMETERS:
    feature_group:
        Required Argument.
        Specifies either the name of FeatureGroup or Object of FeatureGroup
        to archive from repository.
        Types: str OR FeatureGroup
RETURNS:
    bool.
RAISES:
    TeradataMLException, TypeError, ValueError
EXAMPLES:
    >>> from teradataml import DataFrame, FeatureGroup, FeatureStore
    >>> load_example_data('dataframe', ['sales'])
    # Create teradataml DataFrame.
    >>> df = DataFrame("sales")
    # Create FeatureGroup from teradataml DataFrame.
    >>> fg = FeatureGroup.from_DataFrame(name="sales", entity_columns="accounts", df=df, timestamp_col_name="datetime")
    # Create FeatureStore for the repo 'staging_repo'.
    >>> fs = FeatureStore("staging_repo")
    # Apply FeatureGroup to FeatureStore.
    >>> fs.apply(fg)
    True
    # List all the available FeatureGroups.
    >>> fs.list_feature_groups()
          description data_source_name entity_name
    name
    sales        None            sales       sales
    # Archive FeatureGroup with name "sales".
    >>> fs.archive_feature_group(feature_group='sales')
    FeatureGroup 'sales' is archived.
    True
    >>>
    # List all the available FeatureGroups after archive.
    >>> fs.list_feature_groups()
    Empty DataFrame
    Columns: [description, data_source_name, entity_name]
    Index: []
delete
teradataml.store.feature_store.feature_store.FeatureStore.delete = delete(self)
DESCRIPTION:
    Removes the FeatureStore and its components from repository.
    Notes:
         * The function removes all the associated database objects along with data.
           Be cautious while using this function.
         * The function tries to remove the underlying Database also once
           all the Feature Store objects are removed.
         * The user must have permission on the database used by this Feature Store
            * to drop triggers.
            * to drop the tables.
            * to drop the Database.
         * If the user lacks any of the mentioned permissions, Teradata recommends
           to not use this function.
PARAMETERS:
    None
RETURNS:
    bool.
RAISES:
    None
EXAMPLES:
    # Setup FeatureStore for repo 'vfs_v1'.
    >>> from teradataml import FeatureStore
    >>> fs = FeatureStore("vfs_v1")
    >>> fs.setup()
    True
    >>> # Delete FeatureStore.
    >>> fs.delete()
    True
    >>>
delete_data_source
teradataml.store.feature_store.feature_store.FeatureStore.delete_data_source = delete_data_source(self, data_source)
DESCRIPTION:
    Removes the archived DataSource from repository.
PARAMETERS:
    data_source:
        Required Argument.
        Specifies either the name of DataSource or Object of DataSource
        to remove from repository.
        Types: str OR DataSource
RETURNS:
    bool.
RAISES:
    TeradataMLException, TypeError, ValueError
EXAMPLES:
    >>> from teradataml import DataFrame, DataSource, FeatureStore, load_example_data
    >>> load_example_data('dataframe', ['sales'])
    # Create teradataml DataFrame.
    >>> df = DataFrame("sales")
    # Create DataSource with source as teradataml DataFrame.
    >>> ds = DataSource(name="sales_data", source=df)
    # # Create FeatureStore for repo 'vfs_v1'.
    >>> fs = FeatureStore("vfs_v1")
    # Apply the DataSource to FeatureStore.
    >>> fs.apply(ds)
    True
    # Let's first archive the DataSource.
    >>> fs.archive_data_source("sales_data")
    DataSource 'sales_data' is archived.
    True
    # Delete DataSource with name "sales_data".
    >>> fs.delete_data_source("sales_data")
    DataSource 'sales_data' is deleted.
    True
    >>>
delete_entity
teradataml.store.feature_store.feature_store.FeatureStore.delete_entity = delete_entity(self, entity)
DESCRIPTION:
    Removes archived Entity from repository.
PARAMETERS:
    entity:
        Required Argument.
        Specifies either the name of Entity or Object of Entity
        to delete from repository.
        Types: str OR Entity
RETURNS:
    bool.
RAISES:
    TeradataMLException, TypeError, ValueError
EXAMPLES:
    >>> from teradataml import DataFrame, Entity, FeatureStore
    >>> load_example_data('dataframe', ['sales'])
    # Create teradataml DataFrame.
    >>> df = DataFrame("sales")
    # Create Entity using teradataml DataFrame Column.
    >>> entity = Entity(name="sales_data", columns=df.accounts)
    # Create FeatureStore for repo 'staging_repo'.
    >>> fs = FeatureStore("staging_repo")
    # Apply the entity to FeatureStore.
    >>> fs.apply(entity)
    True
    # Let's first archive the entity.
    >>> fs.archive_entity(entity=entity.name)
    Entity 'sales_data' is archived.
    True
    # Delete Entity with name "sales_data".
    >>> fs.delete_entity(entity=entity.name)
    Entity 'sales_data' is deleted.
    True
    >>>
delete_feature
teradataml.store.feature_store.feature_store.FeatureStore.delete_feature = delete_feature(self, feature)
DESCRIPTION:
    Removes the archived Feature from repository.
PARAMETERS:
    feature:
        Required Argument.
        Specifies either the name of Feature or Object of Feature
        to remove from repository.
        Types: str OR Feature
RETURNS:
    bool.
RAISES:
    TeradataMLException, TypeError, ValueError
EXAMPLES:
    >>> from teradataml import DataFrame, Feature, FeatureStore
    >>> load_example_data('dataframe', ['sales'])
    # Create teradataml DataFrame.
    >>> df = DataFrame("sales")
    # Create Feature for Column 'Feb'.
    >>> feature = Feature(name="sales_data_Feb", column=df.Feb)
    # Create a feature store with name "staging_repo".
    >>> fs = FeatureStore("staging_repo")
    # Add the feature created above in the feature store.
    >>> fs.apply(feature)
    True
    # Let's first archive the Feature.
    >>> fs.archive_feature(feature=feature)
    Feature 'sales_data_Feb' is archived.
    True
    # Delete Feature with name "sales_data_Feb".
    >>> fs.delete_feature(feature=feature)
    Feature 'sales_data_Feb' is deleted.
    True
    >>>
delete_feature_group
teradataml.store.feature_store.feature_store.FeatureStore.delete_feature_group = execute_transaction(*args, **kwargs)
get_data_source
teradataml.store.feature_store.feature_store.FeatureStore.get_data_source = get_data_source(self, name)
DESCRIPTION:
    Get the data source from feature store.
PARAMETERS:
    name:
        Required Argument.
        Specifies the name of the data source.
        Types: str
RETURNS:
    Object of DataSource.
RAISES:
    TeradataMLException
EXAMPLES:
    >>> from teradataml import DataFrame, DataSource, FeatureStore, load_example_data
    # Load the admissions data to Vantage.
    >>> load_example_data("dataframe", "admissions_train")
    # Create DataFrame on admissions data.
    >>> df = DataFrame("admissions_train")
    >>> df
       masters   gpa     stats programming  admitted
    id
    34     yes  3.85  Advanced    Beginner         0
    32     yes  3.46  Advanced    Beginner         0
    11      no  3.13  Advanced    Advanced         1
    40     yes  3.95    Novice    Beginner         0
    38     yes  2.65  Advanced    Beginner         1
    36      no  3.00  Advanced      Novice         0
    7      yes  2.33    Novice      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    13      no  4.00  Advanced      Novice         1
    >>>
    # Create DataSource using DataFrame 'df' with name 'admissions'.
    >>> ds = DataSource('admissions', source=df)
    # Apply the DataSource to FeatureStore 'vfs_v1'.
    >>> fs = FeatureStore('vfs_v1')
    >>> fs.apply(ds)
    True
    >>>
    # Get the DataSource 'admissions' from repo 'vfs_v1'
    >>> ds = fs.get_data_source('admissions')
    >>> ds
    DataSource(name=admissions)
    >>>
get_dataset
teradataml.store.feature_store.feature_store.FeatureStore.get_dataset = get_dataset(self, group_name)
DESCRIPTION:
    Returns teradataml DataFrame based on "group_name".
PARAMETERS:
    group_name:
        Required Argument.
        Specifies the name of the feature group.
        Types: str
RETURNS:
    teradataml DataFrame.
RAISES:
    TeradataMLException
EXAMPLES:
    >>> from teradataml import DataFrame, FeatureStore, load_example_data
    # Load the sales data to Vantage.
    >>> load_example_data("dataframe", "sales")
    # Create DataFrame on sales data.
    >>> df = DataFrame("sales")
    >>> df
    >>> df
                  Feb    Jan    Mar    Apr    datetime
    accounts
    Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
    Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
    Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
    Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
    Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
    >>>
    # Create FeatureGroup with name 'sales' from DataFrame.
    >>> fg = FeatureGroup.from_DataFrame(
    ...    name="sales", df=df, entity_columns="accounts", timestamp_col_name="datetime")
    # Apply the FeatureGroup to FeatureStore.
    >>> fs = FeatureStore("vfs_v1")
    >>> fs.apply(fg)
    True
    # Get the DataSet for FeatureGroup 'sales'
    >>> df = fs.get_dataset('sales')
    >>> df
                  datetime    Jan    Feb    Apr    Mar
    accounts
    Orange Inc  04/01/2017    NaN  210.0  250.0    NaN
    Jones LLC   04/01/2017  150.0  200.0  180.0  140.0
    Blue Inc    04/01/2017   50.0   90.0  101.0   95.0
    Alpha Co    04/01/2017  200.0  210.0  250.0  215.0
    Yellow Inc  04/01/2017    NaN   90.0    NaN    NaN
    >>>
get_entity
teradataml.store.feature_store.feature_store.FeatureStore.get_entity = get_entity(self, name)
DESCRIPTION:
    Get the entity from feature store.
PARAMETERS:
    name:
        Required Argument.
        Specifies the name of the entity.
        Types: str
RETURNS:
    Object of Entity.
RAISES:
    None
EXAMPLES:
    >>> from teradataml import DataFrame, Entity, FeatureStore, load_example_data
    # Load the admissions data to Vantage.
    >>> load_example_data("dataframe", "admissions_train")
    # Create DataFrame on admissions data.
    >>> df = DataFrame("admissions_train")
    >>> df
       masters   gpa     stats programming  admitted
    id
    34     yes  3.85  Advanced    Beginner         0
    32     yes  3.46  Advanced    Beginner         0
    11      no  3.13  Advanced    Advanced         1
    40     yes  3.95    Novice    Beginner         0
    38     yes  2.65  Advanced    Beginner         1
    36      no  3.00  Advanced      Novice         0
    7      yes  2.33    Novice      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    13      no  4.00  Advanced      Novice         1
    >>>
    # Create Entity for column 'id' with name 'admissions_id'.
    >>> entity = Entity(name='admissions_id', description="Entity for admissions", columns=df.id)
    # Apply the Entity to FeatureStore 'vfs_v1'.
    >>> fs = FeatureStore('vfs_v1')
    >>> fs.apply(entity)
    True
    >>>
    # Get the Entity 'admissions_id' from repo 'vfs_v1'
    >>> entity = fs.get_entity('admissions_id')
    >>> entity
    Entity(name=admissions_id)
    >>>
get_feature
teradataml.store.feature_store.feature_store.FeatureStore.get_feature = get_feature(self, name)
DESCRIPTION:
    Retrieve the feature.
PARAMETERS:
    name:
        Required Argument.
        Specifies the name of the feature to get.
        Types: str
RETURNS:
    Feature.
RAISES:
    TeradataMLException
EXAMPLES:
    >>> from teradataml import DataFrame, FeatureStore, load_example_data
    # Load the sales data to Vantage.
    >>> load_example_data("dataframe", "sales")
    # Create DataFrame on sales data.
    >>> df = DataFrame("sales")
    >>> df
                  Feb    Jan    Mar    Apr    datetime
    accounts
    Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
    Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
    Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
    Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
    Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
    >>>
    # Create Feature for column 'Mar' with name 'sales_mar'.
    >>> feature = Feature('sales_mar', column=df.Mar)
    # Apply the Feature to FeatureStore.
    >>> fs = FeatureStore("vfs_v1")
    >>> fs.apply(feature)
    True
    # Get the feature 'sales_mar' from repo 'vfs_v1'.
    >>> feature = fs.get_feature('sales_mar')
    >>> feature
    Feature(name=sales_mar)
    >>>
get_feature_group
teradataml.store.feature_store.feature_store.FeatureStore.get_feature_group = get_feature_group(self, name)
DESCRIPTION:
    Retrieve the FeatureGroup using name.
PARAMETERS:
    name:
        Required Argument.
        Specifies the name of the feature group to be retrieved.
        Types: str
RETURNS:
    Object of FeatureGroup
RAISES:
    TeradataMLException
EXAMPLES:
    >>> from teradataml import DataFrame, FeatureStore, load_example_data
    # Load the sales data to Vantage.
    >>> load_example_data("dataframe", "sales")
    # Create DataFrame on sales data.
    >>> df = DataFrame("sales")
    >>> df
                  Feb    Jan    Mar    Apr    datetime
    accounts
    Orange Inc  210.0    NaN    NaN  250.0  04/01/2017
    Jones LLC   200.0  150.0  140.0  180.0  04/01/2017
    Blue Inc     90.0   50.0   95.0  101.0  04/01/2017
    Alpha Co    210.0  200.0  215.0  250.0  04/01/2017
    Yellow Inc   90.0    NaN    NaN    NaN  04/01/2017
    >>>
    # Create FeatureGroup with name 'sales' from DataFrame.
    >>> fg = FeatureGroup.from_DataFrame(
    ...    name="sales", df=df, entity_columns="accounts", timestamp_col_name="datetime")
    # Apply the FeatureGroup to FeatureStore.
    >>> fs = FeatureStore("vfs_v1")
    >>> fs.apply(fg)
    True
    # Get FeatureGroup with group name 'sales' from repo 'vfs_v1'.
    >>> fg = fs.get_feature_group('sales')
    >>> fg
    FeatureGroup(sales, features=
[Feature(name=Jan), Feature(name=Feb), Feature(name=Apr), Feature(name=Mar)], entity=Entity(name=sales), data_source=DataSource
    >>>
list_data_sources
teradataml.store.feature_store.feature_store.FeatureStore.list_data_sources = list_data_sources(self, archived=False) -> teradataml.dataframe.dataframe.DataFrame
DESCRIPTION:
    List all the Data Sources.
PARAMETERS:
    archived
        Optional Argument
        Specifies whether to list effective data sources or archived data sources.
        When set to False, effective data sources in FeatureStore are listed,
        otherwise, archived data sources are listed.
        Default Value: False
        Types: bool
RETURNS:
    teradataml DataFrame
RAISES:
    None
EXAMPLES:
    >>> from teradataml import DataSource, FeatureStore, load_example_data
    >>> load_example_data("dataframe", "admissions_train")
    # Create teradataml DataFrame.
    >>> admissions=DataFrame("admissions_train")
    # Create FeatureStore for repo 'vfs_v1'.
    >>> fs = FeatureStore("vfs_v1")
    # Create DataSource using teradataml DataFrame.
    >>> ds = DataSource(name='admissions', source=admissions)
    # Apply the DataSource to FeatureStore.
    >>> fs.apply(ds)
    True
    # Example 1: List all the effective DataSources in the repo 'vfs_v1'.
    >>> fs.list_data_sources()
               description timestamp_col_name                            source
    name
    admissions        None               None  select * from "admissions_train"
    >>>
    # Example 2: List all the archived DataSources in the repo 'vfs_v1'.
    # Let's first archive the DataSource.
    >>> fs.archive_data_source('admissions')
    DataSource 'admissions' is archived.
    True
    # List archived DataSources.
    >>> fs.list_data_sources(archived=True)
               description timestamp_col_name                            source               archived_time
    name
    admissions        None               None  select * from "admissions_train"  2024-09-30 12:05:39.220000
    >>>
list_entities
teradataml.store.feature_store.feature_store.FeatureStore.list_entities = list_entities(self, archived=False) -> teradataml.dataframe.dataframe.DataFrame
DESCRIPTION:
    List all the entities.
PARAMETERS:
    archived
        Optional Argument
        Specifies whether to list effective entities or archived entities.
        When set to False, effective entities in FeatureStore are listed,
        otherwise, archived entities are listed.
        Default Value: False
        Types: bool
RETURNS:
    teradataml DataFrame
RAISES:
    None
EXAMPLES:
    >>> from teradataml import DataFrame, FeatureStore, load_example_data
    >>> load_example_data('dataframe', 'sales')
    # Create FeatureStore for repo 'vfs_v1'.
    >>> fs = FeatureStore("vfs_v1")
    # Create teradataml DataFrame.
    >>> df = DataFrame("sales")
    # Create a FeatureGroup from teradataml DataFrame.
    >>> fg = FeatureGroup.from_DataFrame(name='sales',
    ...                                  entity_columns='accounts',
    ...                                  df=df,
    ...                                  timestamp_col_name='datetime')
    # Apply the FeatureGroup to FeatureStore.
    >>> fs.apply(fg)
    True
    # Example 1: List all the effective Entities in the repo 'vfs_v1'.
    >>> fs.list_entities()
                        description
    name  entity_column
    sales accounts             None
    >>>
    # Example 2: List all the archived Entities in the repo 'vfs_v1'.
    # Note: Entity cannot be archived if it is a part of FeatureGroup.
    #       First create another Entity, and update FeatureGroup with
    #       other Entity. Then archive Entity 'sales'.
    >>> entity = Entity('store_sales', columns=df.accounts)
    # Update new entity to FeatureGroup.
    >>> fg.apply(entity)
    # Update FeatureGroup to FeatureStore. This will update Entity
    #    from 'sales' to 'store_sales' for FeatureGroup 'sales'.
    >>> fs.apply(fg)
    True
    # Let's archive Entity 'sales' since it is not part of any FeatureGroup.
    >>> fs.archive_entity('sales')
    Entity 'sales' is archived.
    True
    >>>
    # List the archived entities.
    >>> fs.list_entities(archived=True)
        name description               creation_time modified_time               archived_time entity_column
    0  sales        None  2024-10-18 05:41:36.932856          None  2024-10-18 05:50:00.930000      accounts
    >>>
list_feature_groups
teradataml.store.feature_store.feature_store.FeatureStore.list_feature_groups = list_feature_groups(self, archived=False) -> teradataml.dataframe.dataframe.DataFrame
DESCRIPTION:
    List all the FeatureGroups.
PARAMETERS:
    archived
        Optional Argument
        Specifies whether to list effective feature groups or archived feature groups.
        When set to False, effective feature groups in FeatureStore are listed,
        otherwise, archived feature groups are listed.
        Default Value: False
        Types: bool
RETURNS:
    teradataml DataFrame
RAISES:
    None
EXAMPLES:
    >>> from teradataml import FeatureGroup, FeatureStore, load_example_data
    >>> load_example_data("dataframe", "admissions_train")
    # Create teradataml DataFrame.
    >>> admissions=DataFrame("admissions_train")
    # Create FeatureStore for repo 'vfs_v1'.
    >>> fs = FeatureStore("vfs_v1")
    # Create a FeatureGroup from DataFrame.
    >>> fg = FeatureGroup.from_DataFrame("admissions", df=admissions, entity_columns='id')
    # Apply FeatureGroup to FeatureStore.
    >>> fs.apply(fg)
    True
    # Example 1: List all the effective FeatureGroups in the repo 'vfs_v1'.
    >>> fs.list_feature_groups()
               description data_source_name entity_name
    name
    admissions        None       admissions  admissions
    >>>
    # Example 2: List all the archived FeatureGroups in the repo 'vfs_v1'.
    # Let's first archive the FeatureGroup.
    >>> fs.archive_feature_group("admissions")
    True
    >>>
    # List archived FeatureGroups.
    >>> fs.list_feature_groups(archived=True)
             name description data_source_name entity_name               archived_time
    0  admissions        None       admissions  admissions  2024-09-30 12:05:39.220000
    >>>
list_features
teradataml.store.feature_store.feature_store.FeatureStore.list_features = list_features(self, archived=False) -> teradataml.dataframe.dataframe.DataFrame
DESCRIPTION:
    List all the features.
PARAMETERS:
    archived:
        Optional Argument.
        Specifies whether to list effective features or archived features.
        When set to False, effective features in FeatureStore are listed,
        otherwise, archived features are listed.
        Default Value: False
        Types: bool
RETURNS:
    teradataml DataFrame
RAISES:
    None
EXAMPLES:
    >>> from teradataml import DataFrame, FeatureStore, load_example_data
    >>> load_example_data('dataframe', 'sales')
    # Create FeatureStore for repo 'vfs_v1'.
    >>> fs = FeatureStore("vfs_v1")
    # Create teradataml DataFrame.
    >>> df = DataFrame("sales")
    # Create a FeatureGroup from teradataml DataFrame.
    >>> fg = FeatureGroup.from_DataFrame(name='sales',
    ...                                  entity_columns='accounts',
    ...                                  df=df,
    ...                                  timestamp_col_name='datetime')
    # Apply the FeatureGroup to FeatureStore.
    >>> fs.apply(fg)
    True
    # Example 1: List all the effective Features in the repo 'vfs_v1'.
    >>> fs.list_features()
         column_name description               creation_time modified_time  tags data_type feature_type  status group_name
    name
    Mar          Mar        None  2024-09-30 11:21:43.314118          None  None    BIGINT   CONTINUOUS  ACTIVE      sales
    Jan          Jan        None  2024-09-30 11:21:42.655343          None  None    BIGINT   CONTINUOUS  ACTIVE      sales
    Apr          Apr        None  2024-09-30 11:21:44.143402          None  None    BIGINT   CONTINUOUS  ACTIVE      sales
    Feb          Feb        None  2024-09-30 11:21:41.542627          None  None     FLOAT   CONTINUOUS  ACTIVE      sales
    >>>
    # Example 2: List all the archived Features in the repo 'vfs_v1'.
    # Note: Feature can only be archived when it is not associated with any Group.
    #       Let's remove Feature 'Feb' from FeatureGroup.
    >>> fg.remove(fs.get_feature('Feb'))
    True
    # Apply the modified FeatureGroup to FeatureStore.
    >>> fs.apply(fg)
    True
    # Archive Feature 'Feb'.
    >>> fs.archive_feature('Feb')
    Feature 'Feb' is archived.
    True
    # List all the archived Features in the repo 'vfs_v1'.
    >>> fs.list_features(archived=True)
      name column_name description               creation_time modified_time  tags data_type feature_type  status              
    0  Feb         Feb        None  2024-09-30 11:21:41.542627          None  None     FLOAT   CONTINUOUS  ACTIVE  2024-09-
30 11:30:49.160000      sales
    >>>
list_repos
teradataml.store.feature_store.feature_store.FeatureStore.list_repos = list_repos() -> teradataml.dataframe.dataframe.DataFrame
DESCRIPTION:
    Function to list down the repositories.
PARAMETERS:
    None
RETURNS:
    teradataml DataFrame
RAISES:
    None
EXAMPLES:
    # list down all the FeatureStore repositories.
    >>> FeatureStore.list_repos()
        repos
    0  vfs_v1
    >>>
repair
teradataml.store.feature_store.feature_store.FeatureStore.repair = repair(self)
DESCRIPTION:
    Repairs the existing repo.
    Notes:
         * The method checks for the corresponding missing database objects which are
           required for FeatureStore. If any of the database object is not available,
           then it tries to create the object.
         * The method repairs only the underlying tables and not data inside the
           corresponding table.
PARAMETERS:
    None
RETURNS:
    bool
RAISES:
    TeradatamlException
EXAMPLES:
    # repair FeatureStore repo 'vfs_v1'.
    >>> from teradataml import FeatureStore
    >>> fs = FeatureStore("vfs_v1")
    >>> fs.repair()
    True
    >>>
set_features_active
teradataml.store.feature_store.feature_store.FeatureStore.set_features_active = set_features_active(self, names)
DESCRIPTION:
    Mark the feature status as active. Set the status as 'inactive' with
    "set_features_inactive()" method. Note that, inactive features are
    not available for any further processing.
PARAMETERS:
    names:
        Required Argument.
        Specifies the name(s) of the feature(s).
        Types: str OR list of str
RETURNS:
    bool
RAISES:
    teradataMLException
EXAMPLES:
    >>> from teradataml import DataFrame, DataSource, FeatureStore, load_example_data
    # Load the admissions data to Vantage.
    >>> load_example_data("dataframe", "admissions_train")
    # Create DataFrame on admissions data.
    >>> df = DataFrame("admissions_train")
    >>> df
       masters   gpa     stats programming  admitted
    id
    34     yes  3.85  Advanced    Beginner         0
    32     yes  3.46  Advanced    Beginner         0
    11      no  3.13  Advanced    Advanced         1
    40     yes  3.95    Novice    Beginner         0
    38     yes  2.65  Advanced    Beginner         1
    36      no  3.00  Advanced      Novice         0
    7      yes  2.33    Novice      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    13      no  4.00  Advanced      Novice         1
    >>>
    # Create FeatureGroup from DataFrame df.
    >>> fg = FeatureGroup.from_DataFrame(name='admissions', df=df, entity_columns='id')
    # Apply the FeatureGroup to FeatureStore 'vfs_v1'.
    >>> fs = FeatureStore('vfs_v1')
    >>> fs.apply(fg)
    True
    # Get FeatureGroup 'admissions' from FeatureStore.
    >>> fg = fs.get_feature_group('admissions')
    >>> fg
    FeatureGroup(admissions, features=
[Feature(name=masters), Feature(name=programming), Feature(name=admitted), Feature(name=stats), Feature(name=gpa)], entity=Enti
    # Set the Feature 'programming' inactive.
    >>> fs.set_features_inactive('programming')
    True
    # Get FeatureGroup again after setting feature inactive.
    >>> fg = fs.get_feature_group('admissions')
    >>> fg
    FeatureGroup(admissions, features=
[Feature(name=masters), Feature(name=stats), Feature(name=admitted), Feature(name=gpa)], entity=Entity(name=admissions), data_s
    >>>
    # Mark Feature programming from 'inactive' to 'active'.
    >>> fs.set_features_active('programming')
    # Get FeatureGroup again after setting feature active.
    >>> fg = fs.get_feature_group('admissions')
    >>> fg
    FeatureGroup(admissions, features=
[Feature(name=masters), Feature(name=programming), Feature(name=admitted), Feature(name=stats), Feature(name=gpa)], entity=Enti
    >>>
set_features_inactive
teradataml.store.feature_store.feature_store.FeatureStore.set_features_inactive = set_features_inactive(self, names)
DESCRIPTION:
    Mark the feature status as 'inactive'. Note that, inactive features are
    not available for any further processing. Set the status as 'active' with
    "set_features_active()" method.
PARAMETERS:
    names:
        Required Argument.
        Specifies the name(s) of the feature(s).
        Types: str OR list of str
RETURNS:
    bool
RAISES:
    teradataMLException
EXAMPLES:
    >>> from teradataml import DataFrame, DataSource, FeatureStore, load_example_data
    # Load the admissions data to Vantage.
    >>> load_example_data("dataframe", "admissions_train")
    # Create DataFrame on admissions data.
    >>> df = DataFrame("admissions_train")
    >>> df
       masters   gpa     stats programming  admitted
    id
    34     yes  3.85  Advanced    Beginner         0
    32     yes  3.46  Advanced    Beginner         0
    11      no  3.13  Advanced    Advanced         1
    40     yes  3.95    Novice    Beginner         0
    38     yes  2.65  Advanced    Beginner         1
    36      no  3.00  Advanced      Novice         0
    7      yes  2.33    Novice      Novice         1
    26     yes  3.57  Advanced    Advanced         1
    19     yes  1.98  Advanced    Advanced         0
    13      no  4.00  Advanced      Novice         1
    >>>
    # Create FeatureGroup from DataFrame df.
    >>> fg = FeatureGroup.from_DataFrame(name='admissions', df=df, entity_columns='id')
    # Apply the FeatureGroup to FeatureStore 'vfs_v1'.
    >>> fs = FeatureStore('vfs_v1')
    >>> fs.apply(fg)
    True
    # Get FeatureGroup 'admissions' from FeatureStore.
    >>> fg = fs.get_feature_group('admissions')
    >>> fg
    FeatureGroup(admissions, features=
[Feature(name=masters), Feature(name=programming), Feature(name=admitted), Feature(name=stats), Feature(name=gpa)], entity=Enti
    # Set the Feature 'programming' inactive.
    >>> fs.set_features_inactive('programming')
    True
    # Get FeatureGroup again after setting feature inactive.
    >>> fg = fs.get_feature_group('admissions')
    >>> fg
    FeatureGroup(admissions, features=
[Feature(name=masters), Feature(name=stats), Feature(name=admitted), Feature(name=gpa)], entity=Entity(name=admissions), data_s
    >>>
setup
teradataml.store.feature_store.feature_store.FeatureStore.setup = setup(self)
DESCRIPTION:
    Function to setup all the required objects in Vantage for the specified
    repository.
    Note:
        The function checks whether repository exists or not. If not exists,
        it first creates the repository and then creates the corresponding tables.
        Hence make sure the user with which is it connected to Vantage
        has corresponding access rights for creating DataBase and creating
        tables in the corresponding database.
PARAMETERS:
    None
RETURNS:
    bool
RAISES:
    TeradatamlException
EXAMPLES:
    # setup FeatureStore for repo 'vfs_v1'.
    >>> from teradataml import FeatureStore
    >>> fs = FeatureStore("vfs_v1")
    >>> fs.setup()
    True
    >>>
Properties of Feature Store
grant
teradataml.store.feature_store.feature_store.FeatureStore.grant
DESCRIPTION:
    Grants access on FeatureStore.
    Note:
        One must have admin access to grant access.
PARAMETERS:
    None
RETURNS:
    bool
RAISES:
    OperationalError
EXAMPLES:
    >>> from teradataml import FeatureStore
    # Create FeatureStore for repo 'vfs_v1'.
    >>> fs = FeatureStore("vfs_v1")
    # Setup FeatureStore for this repository.
    >>> fs.setup()
    True
    # Example 1: Grant read access on FeatureStore to user 'BoB'.
    >>> fs.grant.read('BoB')
    True
    # Example 2: Grant write access on FeatureStore to user 'BoB'.
    >>> fs.grant.write('BoB')
    True
    # Example 3: Grant read and write access on FeatureStore to user 'BoB'.
    >>> fs.grant.read_write('BoB')
    True
revoke
teradataml.store.feature_store.feature_store.FeatureStore.revoke
DESCRIPTION:
    Revokes access on FeatureStore.
    Note:
        One must have admin access to revoke access.
PARAMETERS:
    None
RETURNS:
    bool
RAISES:
    OperationalError
EXAMPLES:
    >>> from teradataml import FeatureStore
    # Create FeatureStore for repo 'vfs_v1'.
    >>> fs = FeatureStore("vfs_v1")
    # Setup FeatureStore for this repository.
    >>> fs.setup()
    True
    # Example 1: Revoke read access on FeatureStore from user 'BoB'.
    >>> fs.revoke.read('BoB')
    True
    # Example 2: Revoke write access on FeatureStore from user 'BoB'.
    >>> fs.revoke.write('BoB')
    True