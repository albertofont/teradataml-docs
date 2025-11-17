# teradataml SDK Functions

Client
Client
teradataml.sdk.Client = class Client(builtins.object)
teradataml.sdk.Client(base_url=None, auth=None, ssl_verify=True, config_file=None)
    Methods deﬁned here:
    __init__(self, base_url=None, auth=None, ssl_verify=True, conﬁg_ﬁle=None)
    DESCRIPTION:
        Initializes the Client object and sets up the configuration for the client.
    PARAMETERS:
        base_url:
            Optional Argument.
            Specifies the base URL of API endpoint. All requests are made to a relative path to
            this URL. It can be provided directly, or derived from the "BASE_URL" environment
            variable or the YAML configuration variable "base_url". If not provided in any of
            these 3 ways, appropriate Exception is raised.
            Types: str or None
        auth:
            Optional Argument.
            Specifies the type of authentication to be used. It can be one of the following
            authentication type objects:
            - ClientCredentialsAuth: For client credentials authentication.
            - DeviceCodeAuth: For device code authentication.
            - BearerAuth: For bearer token authentication.
            Authentication mode is property of the auth object that is passed to this argument.
            If this argument is not provided, authentication mode is derived from the
            "BASE_API_AUTH_MODE" environment variable or the YAML configuration variable
            "auth_mode". If this argument and the above 2 (environment variable and YAML
            configuration variable) are not provided, appropriate Exception is raised.
            For Authentication modes through environment variables without passing auth
            argument and config file, the following environment variables are used:
            - BASE_URL: Specifies the base URL of API endpoint.
            - BASE_API_AUTH_MODE: Specifies the authentication mode. It can be one of the
              following:
                - "client_credentials": For client credentials authentication.
                - "device_code": For device code authentication.
                - "bearer": For bearer token authentication.
            - BASE_SSL_VERIFY: Similar to 'ssl_verify' argument.
            - BASE_API_AUTH_CLIENT_ID: OAuth2 client ID. Required for "client_credentials" and
                "device_code" authentication modes.
            - BASE_API_AUTH_CLIENT_SECRET: OAuth2 client secret. Required for "client_credentials"
                and "device_code" authentication modes.
            - BASE_API_AUTH_TOKEN_URL: OAuth2 token endpoint. Required for "client_credentials"
                and "device_code" authentication modes.
            - BASE_API_AUTH_DEVICE_AUTH_URL: OAuth2 device code endpoint. Required for
                "device_code" authentication mode.
            - BASE_API_AUTH_BEARER_TOKEN: The raw bearer token. Required for "bearer"
                authentication.
            Note:
                - For device_code authentication, token is searched in "~/.teradataml/sdk/.token".
                  If it is not present, token is created, used for authentication and saved in
                  the same location.
            Types: ClientCredentialsAuth, DeviceCodeAuth, BearerAuth or None
        ssl_verify:
            Optional Argument.
            Specifies whether to enable or disable TLS Cert validation. If True, TLS cert
            validation is enabled. If False, TLS cert validation is disabled. It can be provided
            directly, or derived from the "BASE_SSL_VERIFY" environment variable or the YAML
            configuration variable "ssl_verify". If ssl_verify is not provided in any of
            these 3 ways, it is set to True by default.
            Default Value: True (Enable TLS cert validation)
            Types: bool or None
        config_file:
            Optional Argument.
            Specifies the path to the YAML configuration file.
            Note:
                - At least one of YAML config file or environment variables or above arguments
                   must be provided. If not provided, appropriate Exception is raised.
                - If config_file is not provided, the default config file
                  ~/.teradataml/sdk/config.yaml is used. If the default config file is not
                  found, this function tries to read the configuration from the environment
                  variables or other arguments.
            If YAML file is provided, it should have the following details, depending on the
            what is provided in other arguments:
            - ssl_verify (bool) : Same as ssl_verify argument.
            - base_url (str) : Same as base_url argument.
            - auth_mode (str) : Authentication mode to be used. It can be one of the following:
                - client_credentials: For client credentials authentication.
                - device_code: For device code authentication.
                - bearer: For bearer token authentication.
            - auth_client_id (str) : OAuth2 client ID. Required for client_credentials and
                device_code authentication modes.
            - auth_client_secret (str) : OAuth2 client secret. Required for client_credentials
                and device_code authentication modes.
            - auth_token_url (str) : OAuth2 token endpoint. Required for client_credentials and
                device_code authentication modes.
            - auth_device_auth_url (str) : OAuth2 device code endpoint. Required for device_code
                authentication mode.
            - auth_bearer (str) : The raw bearer token. Required for bearer authentication mode.
            Types: str or None
    RETURNS:
        None
    RAISES:
        - TeradataMlException: If the base_url, auth_mode, or ssl_verify is not provided in
          any of the 3 ways (YAML config file, environment variables, or constructor arguments).
        - ValueError: If the base_url is not provided in any of the 3 ways.
        - FileNotFoundError: If the specified YAML file does not exist.
        - yaml.YAMLError: If there is an error in parsing the YAML file.
    EXAMPLES:
        >>> from teradataml.sdk import Client, ClientCredentialsAuth, DeviceCodeAuth
        >>> import os
        # Example 1: Using a custom configuration file for client credentials authentication.
        >>> cc_auth_dict = {"auth_client_id": "your_client_id",
                            "auth_client_secret": "your_client_secret",
                            "auth_token_url": "https://example.com/token",
                            "auth_mode": "client_credentials",
                            "base_url": "https://example.com",
                            "ssl_verify": False}
        >>> # Write data to config file.
        >>> cc_config_file = "custom_config.yaml"
        >>> import yaml
        >>> with open(cc_config_file, "w") as f:
                yaml.dump(cc_auth_dict, f, sort_keys=False)
        >>> # Create client object using config file.
        >>> obj = Client(config_file=cc_config_file)
        # Example 2: Using environment variables.
        >>> os.environ["BASE_URL"] = "https://example.com"
        >>> os.environ["BASE_API_AUTH_MODE"] = "client_credentials"
        >>> os.environ["BASE_API_AUTH_CLIENT_ID"] = "your_client_id"
        >>> os.environ["BASE_API_AUTH_CLIENT_SECRET"] = "your_client_secret"
        >>> os.environ["BASE_API_AUTH_TOKEN_URL"] = "https://example.com/token"
        >>> os.environ["BASE_SSL_VERIFY"] = "false"
        >>> obj = Client()
        # Example 3: Using constructor arguments.
        >>> obj = Client(
                base_url="https://example.com",
                auth=DeviceCodeAuth(
                    auth_client_id="your_client_id",
                    auth_client_secret="your_client_secret",
                    auth_token_url="https://example.com/token",
                    auth_device_auth_url="https://example.com/device_auth"
                ),
                ssl_verify=True
            )
    delete_request(self, path, header_params: Dict[str, str], query_params: Dict[str, str], body: Dict[str, str])
    DESCRIPTION:
        Sends a DELETE request to the specified API endpoint with the provided body and parameters.
    PARAMETERS:
        path:
            Required Argument.
            Specifies the URL path for the API endpoint along with path parameters.
            Note:
                It should be a relative path to the base URL.
            Types: str
        header_params:
            Required Argument.
            Specifies the header parameters to be included in the request.
            Types: dict
        query_params:
            Required Argument.
            Specifies the query parameters to be included in the request.
            Types: dict
        body:
            Required Argument.
            Specifies the request body to be sent in the DELETE request.
            Types: dict
    RETURNS:
        dict for resources if the request is successful, or str for errors.
    RAISES:
        HTTPError: If the response contains an HTTP error status code.
    EXAMPLES:
        >>> client = Client(...)
        >>> client.delete_request("/api/projects/123", header_params, query_params, body)
    get_request(self, path, header_params: Dict[str, str], query_params: Dict[str, str])
    DESCRIPTION:
        Sends a GET request to the specified API endpoint using the provided parameters.
    PARAMETERS:
        path:
            Required Argument.
            Specifies the URL path for the API endpoint along with path parameters.
            Note:
                It should be a relative path to the base URL.
            Types: str
        header_params:
            Required Argument.
            Specifies the header parameters to be included in the request.
            Types: dict
        query_params:
            Required Argument.
            Specifies the query parameters to be included in the request.
            Types: dict
    RETURNS:
        dict for resources if the request is successful, None if resource is not found (404), or str for errors.
    RAISES:
        TeradatamlRestException: If a network or HTTP error occurs.
        HTTPError: If the response contains an HTTP error status code.
    EXAMPLES:
        >>> client = Client(...)
        >>> client.get_request("/api/projects", header_params, query_params)
    patch_request(self, path, header_params: Dict[str, str], query_params: Dict[str, str], body: Dict[str, str])
    DESCRIPTION:
        Sends a PATCH request to the specified API endpoint with the provided body and parameters.
    PARAMETERS:
        path:
            Required Argument.
            Specifies the URL path for the API endpoint along with path parameters.
            Note:
                It should be a relative path to the base URL.
            Types: str
        header_params:
            Required Argument.
            Specifies the header parameters to be included in the request.
            Types: dict
        query_params:
            Required Argument.
            Specifies the query parameters to be included in the request.
            Types: dict
        body:
            Required Argument.
            Specifies the request body to be sent in the PATCH request.
            Types: dict
    RETURNS:
        dict for resources if the request is successful, or str for errors.
    RAISES:
        HTTPError: If the response contains an HTTP error status code.
    EXAMPLES:
        >>> client = Client(...)
        >>> client.patch_request("/api/projects/123", header_params, query_params, body)
    post_request(self, path, header_params: Dict[str, str], query_params: Dict[str, str], body: Dict[str, str])
    DESCRIPTION:
        Sends a POST request to the specified API endpoint with the provided body and parameters.
    PARAMETERS:
        path:
            Required Argument.
            Specifies the URL path for the API endpoint along with path parameters.
            Note:
                It should be a relative path to the base URL.
            Types: str
        header_params:
            Required Argument.
            Specifies the header parameters to be included in the request.
            Types: dict
        query_params:
            Required Argument.
            Specifies the query parameters to be included in the request.
            Types: dict
        body:
            Required Argument.
            Specifies the request body to be sent in the POST request.
            Types: dict
    RETURNS:
        dict for resources if the request is successful, or str for errors.
    RAISES:
        HTTPError: If the response contains an HTTP error status code.
    EXAMPLES:
        >>> client = Client(...)
        >>> client.post_request("/api/projects", header_params, query_params, body)
    put_request(self, path, header_params: Dict[str, str], query_params: Dict[str, str], body: Dict[str, str])
    DESCRIPTION:
        Sends a PUT request to the specified API endpoint with the provided body and parameters.
    PARAMETERS:
        path:
            Required Argument.
            Specifies the URL path for the API endpoint along with path parameters.
            Note:
                It should be a relative path to the base URL.
            Types: str
        header_params:
            Required Argument.
            Specifies the header parameters to be included in the request.
            Types: dict
        query_params:
            Required Argument.
            Specifies the query parameters to be included in the request.
            Types: dict
        body:
            Required Argument.
            Specifies the request body to be sent in the PUT request.
            Types: dict
    RETURNS:
        dict for resources if the request is successful, or str for errors.
    RAISES:
        HTTPError: If the response contains an HTTP error status code.
    EXAMPLES:
        >>> client = Client(...)
        >>> client.put_request("/api/projects/123", header_params, query_params, body)
    Data descriptors deﬁned here:
    __dict__
    dictionary for instance variables (if defined)
    __weakref__
    list of weak references to the object (if defined)
    Data and other attributes deﬁned here:
    DEFAULT_CONFIG_DIR = r'~\.teradataml\.sdk'
    DEFAULT_CONFIG_FILE_PATH = r'~\.teradataml\.sdk\conﬁg.yaml'
    DEFAULT_TOKEN_CACHE_FILE_PATH = r'~\.teradataml\.sdk\.token'
    MAX_RETRIES = 3
    SDK_NAME = 'BaseSDK'
    BearerAuth
    Methods of BearerAuth
    __init__
    teradataml.sdk.BearerAuth.__init__ = __init__(self, auth_bearer)
    DESCRIPTION:
        Initializes the object for bearer token authentication mode.
    PARAMETERS:
        auth_bearer:
            Required Argument.
            Specifies the raw bearer token to be used for authentication.
            Types: str
    RETURNS:
        None
    RAISES:
        None
    EXAMPLES:
        >>> from teradataml.sdk import BearerAuth
        >>> auth = BearerAuth(auth_bearer="your_bearer_token")
    Properties of BearerAuth
    authenticate
    teradataml.sdk.BearerAuth.authenticate = authenticate(self)
    DESCRIPTION:
        Authenticates using the provided bearer token.
    PARAMETERS:
        None
    RETURNS:
        requests.Session: Authenticated session with the bearer token.
    RAISES:
        None
    EXAMPLES:
        >>> from teradataml.sdk import BearerAuth
        >>> auth = BearerAuth(auth_bearer="your_bearer_token")
        >>> session = auth.authenticate()
    ClientCredentialsAuth
    Methods of ClientCredentialsAuth
    __init__
    teradataml.sdk.ClientCredentialsAuth.__init__ = __init__(self, auth_token_url, auth_client_id, auth_client_secret)
    DESCRIPTION:
        Initializes the object for client credentials authentication mode.
    PARAMETERS:
        auth_token_url:
            Required Argument.
            Specifies the token endpoint URL to fetch the access token.
            Types: str
        auth_client_id:
            Required Argument.
            Specifies the client ID for authentication.
            Types: str
        auth_client_secret:
            Required Argument.
            Specifies the client secret for authentication.
            Types: str
    RETURNS:
        None
    RAISES:
        None
    EXAMPLES:
        >>> from teradataml.sdk import ClientCredentialsAuth
        >>> auth = ClientCredentialsAuth(
                auth_token_url="https://example.com/token",
                auth_client_id="your_client_id",
                auth_client_secret="your_client_secret"
            )
    Properties of ClientCredentialsAuth
    authenticate
    teradataml.sdk.ClientCredentialsAuth.authenticate = authenticate(self)
    DESCRIPTION:
        Authenticates using client credentials and fetches the access token.
    PARAMETERS:
        None
    RETURNS:
        requests.Session: Authenticated session with the access token.
    RAISES:
        None
    EXAMPLES:
        >>> from teradataml.sdk import ClientCredentialsAuth
        >>> auth = ClientCredentialsAuth(
                auth_token_url="https://example.com/token",
                auth_client_id="your_client_id",
                auth_client_secret="your_client_secret"
            )
        >>> session = auth.authenticate()
    DeviceCodeAuth
    Methods of DeviceCodeAuth
    __init__
    teradataml.sdk.DeviceCodeAuth.__init__ = __init__(self, auth_token_url, auth_device_auth_url, auth_client_id, auth_client_secret=None)
    DESCRIPTION:
        Initializes the object for device code authentication mode.
    PARAMETERS:
        auth_token_url:
            Required Argument.
            Specifies the token endpoint URL to fetch the access token.
            Types: str
        auth_device_auth_url:
            Required Argument.
            Specifies the device code endpoint URL to initiate device code flow.
            Types: str
        auth_client_id:
            Required Argument.
            Specifies the client ID for authentication.
            Types: str
        auth_client_secret:
            Optional Argument.
            Specifies the client secret for authentication.
            Types: str or None
    RETURNS:
        None
    RAISES:
        None
    EXAMPLES:
        >>> from teradataml.sdk import DeviceCodeAuth
        >>> auth = DeviceCodeAuth(
                auth_token_url="https://example.com/token",
                auth_device_auth_url="https://example.com/device_auth",
                auth_client_id="your_client_id",
                auth_client_secret="your_client_secret"
            )
    Properties of DeviceCodeAuth
    authenticate
    teradataml.sdk.DeviceCodeAuth.authenticate = authenticate(self, token_cache_ﬁle_path=None)
    DESCRIPTION:
        Authenticates using device code flow and fetches the access token.
    PARAMETERS:
        token_cache_file_path:
            Optional Argument.
            Specifies the file path to load and/or cache the token data.
            Types: str
    RETURNS:
        requests.Session: Authenticated session with the access token.
    RAISES:
        None
    EXAMPLES:
        >>> from teradataml.sdk import DeviceCodeAuth
        >>> auth = DeviceCodeAuth(
                auth_token_url="https://example.com/token",
                auth_device_auth_url="https://example.com/device_auth",
                auth_client_id="your_client_id",
                auth_client_secret="your_client_secret"
            )
        >>> session = auth.authenticate(token_cache_file_path="/path/to/token_cache.json")
    ModelOps
    Methods of ModelOps
    blueprint
    teradataml.sdk.modelops.blueprint = blueprint()
    DESCRIPTION:
        Prints the available classes for the ModelOps SDK.
    PARAMETERS:
        None
    RETURNS:
        None
    RAISES:
        None
    EXAMPLES:
        >>> blueprint()
        ----------------------------------------------------------------
        Available classes for ModelOps SDK:
            * teradataml.sdk.modelops.ClassName1
            * teradataml.sdk.modelops.ClassName2
        ----------------------------------------------------------------
    Properties of ModelOps
    MOdelOpsClient
    teradataml.sdk.modelops.ModelOpsClient = class ModelOpsClient(teradataml.sdk.api_client.Client)
    teradataml.sdk.modelops.ModelOpsClient(base_url=None, auth=None, ssl_verify=True, config_file=None, project_id=None)
    Method resolution order:
    ModelOpsClient
    teradataml.sdk.api_client.Client
    builtins.object
    Methods deﬁned here:
    __init__(self, base_url=None, auth=None, ssl_verify=True, conﬁg_ﬁle=None, project_id=None)
    DESCRIPTION:
        Initializes the client object and sets up the configuration for the ModelOps client.
    PARAMETERS:
        base_url:
            Optional Argument.
            Specifies the base URL of API endpoint. All requests are made to a relative path to
            this URL. It can be provided directly, or derived from the "BASE_URL" environment
            variable or the YAML configuration variable "base_url". If not provided in any of
            these 3 ways, appropriate Exception is raised.
            Types: str or None
        auth:
            Optional Argument.
            Specifies the type of authentication to be used. It can be one of the following
            authentication type objects:
            - ClientCredentialsAuth: For client credentials authentication.
            - DeviceCodeAuth: For device code authentication.
            - BearerAuth: For bearer token authentication.
            Authentication mode is property of the auth object that is passed to this argument.
            If this argument is not provided, authentication mode is derived from the
            "BASE_API_AUTH_MODE" environment variable or the YAML configuration variable
            "auth_mode". If this argument and the above 2 (environment variable and YAML
            configuration variable) are not provided, appropriate Exception is raised.
            For Authentication modes through environment variables without passing auth
            argument and config file, the following environment variables are used:
            - BASE_URL: Specifies the base URL of API endpoint.
            - BASE_API_AUTH_MODE: Specifies the authentication mode. It can be one of the
              following:
                - "client_credentials": For client credentials authentication.
                - "device_code": For device code authentication.
                - "bearer": For bearer token authentication.
            - BASE_SSL_VERIFY: Similar to 'ssl_verify' argument.
            - BASE_API_AUTH_CLIENT_ID: OAuth2 client ID. Required for "client_credentials" and
                "device_code" authentication modes.
            - BASE_API_AUTH_CLIENT_SECRET: OAuth2 client secret. Required for "client_credentials"
                and "device_code" authentication modes.
            - BASE_API_AUTH_TOKEN_URL: OAuth2 token endpoint. Required for "client_credentials"
                and "device_code" authentication modes.
            - BASE_API_AUTH_DEVICE_AUTH_URL: OAuth2 device code endpoint. Required for
                "device_code" authentication mode.
            - BASE_API_AUTH_BEARER_TOKEN: The raw bearer token. Required for "bearer"
                authentication.
            Note:
                - For "device_code" authentication, token is searched in
                  "~/.teradataml/sdk/modelops/.token". If it is not present, token is created,
                  used for authentication and saved in the same location.
            Types: ClientCredentialsAuth, DeviceCodeAuth, BearerAuth or None
        ssl_verify:
            Optional Argument.
            Specifies whether to enable or disable TLS Cert validation. If True, TLS cert
            validation is enabled. If False, TLS cert validation is disabled. It can be provided
            directly, or derived from the "BASE_SSL_VERIFY" environment variable or the YAML
            configuration variable "ssl_verify". If 'ssl_verify' is not provided in any of
            these 3 ways, it is set to True by default.
            Default Value: True (Enable TLS cert validation)
            Types: bool or None
        config_file:
            Optional Argument.
            Specifies the path to the YAML configuration file.
            Note:
                - At least one of YAML config file or environment variables or above arguments
                   must be provided. If not provided, appropriate Exception is raised.
                - If config_file is not provided, the default config file
                  ~/.teradataml/sdk/modelops/config.yaml is used. If the default config file
                  is not found, this function tries to read the configuration from the
                  environment variables or other arguments.
            If YAML file is provided, it should have the following details, depending on the
            what is provided in other arguments:
            - ssl_verify (bool) : Same as ssl_verify argument.
            - base_url (str) : Same as base_url argument.
            - auth_mode (str) : Authentication mode to be used. It can be one of the following:
                - client_credentials: For client credentials authentication.
                - device_code: For device code authentication.
                - bearer: For bearer token authentication.
            - auth_client_id (str) : OAuth2 client ID. Required for client_credentials and
                device_code authentication modes.
            - auth_client_secret (str) : OAuth2 client secret. Required for client_credentials
                and device_code authentication modes.
            - auth_token_url (str) : OAuth2 token endpoint. Required for client_credentials and
                device_code authentication modes.
            - auth_device_auth_url (str) : OAuth2 device code endpoint. Required for device_code
                authentication mode.
            - auth_bearer (str) : The raw bearer token. Required for bearer authentication mode.
            Types: str or None
        project_id:
            Specifies the project ID to be used by the client. If provided, it sets the project
            context for the client. If not provided, the project context is not set, and the
            client operates without a specific project context.
            Note:
                Some functions may require a project context to be set, and if 'project_id'
                is not provided, it can be set using the `<client_obj>.set_project_id()` method.
            Types: str or None
    RETURNS:
        None
    RAISES:
        - TeradataMlException: If the base_url, auth_mode, or ssl_verify is not provided in
          any of the 3 ways (YAML config file, environment variables, or constructor arguments).
        - ValueError: If the base_url is not provided in any of the 3 ways.
        - FileNotFoundError: If the specified YAML config file does not exist.
        - yaml.YAMLError: If there is an error in parsing the YAML file.
    EXAMPLES:
        >>> from teradataml.sdk import ClientCredentialsAuth, DeviceCodeAuth
        >>> import os
        >>> from teradataml.sdk.modelops import ModelOpsclient
        # Example 1: Using a custom configuration file for client credentials authentication.
        >>> cc_auth_dict = {"auth_client_id": "your_client_id",
                            "auth_client_secret": "your_client_secret",
                            "auth_token_url": "https://example.com/token",
                            "auth_mode": "client_credentials",
                            "base_url": "https://example.com",
                            "ssl_verify": False}
        >>> # Write data to config file.
        >>> cc_config_file = "custom_config.yaml"
        >>> import yaml
        >>> with open(cc_config_file, "w") as f:
                yaml.dump(cc_auth_dict, f, sort_keys=False)
        >>> # Create client object using config file.
        >>> obj = ModelOpsclient(config_file=cc_config_file)
        # Example 2: Using environment variables and set project id using 
        #            set_project_id() method.
        >>> os.environ["BASE_URL"] = "https://example.com"
        >>> os.environ["BASE_API_AUTH_MODE"] = "client_credentials"
        >>> os.environ["BASE_API_AUTH_CLIENT_ID"] = "your_client_id"
        >>> os.environ["BASE_API_AUTH_CLIENT_SECRET"] = "your_client_secret"
        >>> os.environ["BASE_API_AUTH_TOKEN_URL"] = "https://example.com/token"
        >>> os.environ["BASE_SSL_VERIFY"] = "false"
        >>> obj = ModelOpsclient()
        >>> obj.set_project_id("70d4659b-92a2-4723-841a-9ba5629b5f27")
        # Example 3: Using constructor arguments and passing "project_id" argument.
        >>> obj = ModelOpsclient(
                base_url="https://example.com",
                auth=DeviceCodeAuth(
                    auth_client_id="your_client_id",
                    auth_client_secret="your_client_secret",
                    auth_token_url="https://example.com/token",
                    auth_device_auth_url="https://example.com/device_auth"
                ),
                ssl_verify=True,
                project_id="70d4659b-92a2-4723-841a-9ba5629b5f27"
            )
    dataset_connections(self)
    get dataset connections client
    dataset_templates(self)
    get dataset templates client
    datasets(self)
    get datasets client
    deployments(self)
    get deployments client
    describe_current_project(self)
    get details of currently selected project
    feature_engineering(self)
    get feature engineering client
    get_current_project(self)
    get project id
    Return:
       project_id (str): project id(uuid)
    get_default_connection_id(self)
    get default dataset connection id
    job_events(self)
    get job events client
    jobs(self)
    get jobs client
    models(self)
    get models client
    projects(self)
    get projects client
    set_project_id(self, project_id: str)
    set project id
    Parameters:
       project_id (str): project id(uuid)
    trained_model_artefacts(self)
    get trained model artefacts client
    trained_model_events(self)
    get trained model events client
    trained_models(self)
    get trained models client
    user_attributes(self)
    get user attributes client
    Data and other attributes deﬁned here:
    DEFAULT_CONFIG_DIR = r'~\.teradataml\sdk\modelops'
    SDK_NAME = 'ModelOps'
    Methods inherited from teradataml.sdk.api_client.Client:
    delete_request(self, path, header_params: Dict[str, str], query_params: Dict[str, str], body: Dict[str, str])
    DESCRIPTION:
        Sends a DELETE request to the specified API endpoint with the provided body and parameters.
    PARAMETERS:
        path:
            Required Argument.
            Specifies the URL path for the API endpoint along with path parameters.
            Note:
                It should be a relative path to the base URL.
            Types: str
        header_params:
            Required Argument.
            Specifies the header parameters to be included in the request.
            Types: dict
        query_params:
            Required Argument.
            Specifies the query parameters to be included in the request.
            Types: dict
        body:
            Required Argument.
            Specifies the request body to be sent in the DELETE request.
            Types: dict
    RETURNS:
        dict for resources if the request is successful, or str for errors.
    RAISES:
        HTTPError: If the response contains an HTTP error status code.
    EXAMPLES:
        >>> client = Client(...)
        >>> client.delete_request("/api/projects/123", header_params, query_params, body)
    get_request(self, path, header_params: Dict[str, str], query_params: Dict[str, str])
    DESCRIPTION:
        Sends a GET request to the specified API endpoint using the provided parameters.
    PARAMETERS:
        path:
            Required Argument.
            Specifies the URL path for the API endpoint along with path parameters.
            Note:
                It should be a relative path to the base URL.
            Types: str
        header_params:
            Required Argument.
            Specifies the header parameters to be included in the request.
            Types: dict
        query_params:
            Required Argument.
            Specifies the query parameters to be included in the request.
            Types: dict
    RETURNS:
        dict for resources if the request is successful, None if resource is not found (404), or str for errors.
    RAISES:
        TeradatamlRestException: If a network or HTTP error occurs.
        HTTPError: If the response contains an HTTP error status code.
    EXAMPLES:
        >>> client = Client(...)
        >>> client.get_request("/api/projects", header_params, query_params)
    patch_request(self, path, header_params: Dict[str, str], query_params: Dict[str, str], body: Dict[str, str])
    DESCRIPTION:
        Sends a PATCH request to the specified API endpoint with the provided body and parameters.
    PARAMETERS:
        path:
            Required Argument.
            Specifies the URL path for the API endpoint along with path parameters.
            Note:
                It should be a relative path to the base URL.
            Types: str
        header_params:
            Required Argument.
            Specifies the header parameters to be included in the request.
            Types: dict
        query_params:
            Required Argument.
            Specifies the query parameters to be included in the request.
            Types: dict
        body:
            Required Argument.
            Specifies the request body to be sent in the PATCH request.
            Types: dict
    RETURNS:
        dict for resources if the request is successful, or str for errors.
    RAISES:
        HTTPError: If the response contains an HTTP error status code.
    EXAMPLES:
        >>> client = Client(...)
        >>> client.patch_request("/api/projects/123", header_params, query_params, body)
    post_request(self, path, header_params: Dict[str, str], query_params: Dict[str, str], body: Dict[str, str])
    DESCRIPTION:
        Sends a POST request to the specified API endpoint with the provided body and parameters.
    PARAMETERS:
        path:
            Required Argument.
            Specifies the URL path for the API endpoint along with path parameters.
            Note:
                It should be a relative path to the base URL.
            Types: str
        header_params:
            Required Argument.
            Specifies the header parameters to be included in the request.
            Types: dict
        query_params:
            Required Argument.
            Specifies the query parameters to be included in the request.
            Types: dict
        body:
            Required Argument.
            Specifies the request body to be sent in the POST request.
            Types: dict
    RETURNS:
        dict for resources if the request is successful, or str for errors.
    RAISES:
        HTTPError: If the response contains an HTTP error status code.
    EXAMPLES:
        >>> client = Client(...)
        >>> client.post_request("/api/projects", header_params, query_params, body)
    put_request(self, path, header_params: Dict[str, str], query_params: Dict[str, str], body: Dict[str, str])
    DESCRIPTION:
        Sends a PUT request to the specified API endpoint with the provided body and parameters.
    PARAMETERS:
        path:
            Required Argument.
            Specifies the URL path for the API endpoint along with path parameters.
            Note:
                It should be a relative path to the base URL.
            Types: str
        header_params:
            Required Argument.
            Specifies the header parameters to be included in the request.
            Types: dict
        query_params:
            Required Argument.
            Specifies the query parameters to be included in the request.
            Types: dict
        body:
            Required Argument.
            Specifies the request body to be sent in the PUT request.
            Types: dict
    RETURNS:
        dict for resources if the request is successful, or str for errors.
    RAISES:
        HTTPError: If the response contains an HTTP error status code.
    EXAMPLES:
        >>> client = Client(...)
        >>> client.put_request("/api/projects/123", header_params, query_params, body)
    Data descriptors inherited from teradataml.sdk.api_client.Client:
    __dict__
    dictionary for instance variables (if defined)
    __weakref__
    list of weak references to the object (if defined)
    Data and other attributes inherited from teradataml.sdk.api_client.Client:
    DEFAULT_CONFIG_FILE_PATH = r'~\.teradataml\.sdk\conﬁg.yaml'
    DEFAULT_TOKEN_CACHE_FILE_PATH = r'~\.teradataml\.sdk\.token'
    MAX_RETRIES = 3