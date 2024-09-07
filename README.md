
<h3>all_records_pull.py</h3>

The **all_records_pull.py** script is responsible for making API calls using predefined queries, processing the data, and handling logging functionalities. This script works in conjunction with other modules such as gen_config, utils, work_rqsts, scratch2 and queries to automate the process of retrieving data from an API and handling log file management.

<h3>How It Works</h3>

- **API Authentication**:
    The script authenticates using OAuth2 credentials (client ID, client secret, username, password), which are loaded from environment variables.
- **Query Processing**:
    It processes multiple queries (defined in the queries module) by sending requests to the API and handling the response data.
- **Logging**:
    Logging is configured for debugging and tracking script execution.
- **Log File Management**:
    The script compresses old log files located in a specified directory to manage disk space.

<h3>classes.py</h3>

The AccessTokenHandler class in **classes.py** is designed to manage API authentication tokens. It handles retrieving cached tokens, saving new tokens, checking token validity, and refreshing tokens when necessary.

**Attributes**

- **cache_file**: The file used to store the cached token (default is token_cache.json).
- **client_id**: The client ID for OAuth2 authentication.
- **client_secret**: The client secret for OAuth2 authentication.
- **auth_url**: The URL to request the access token from.
- **username**: The username for OAuth2 authentication (if using password grant type).
- **password**: The password for OAuth2 authentication (if using password grant type).

**Methods**

- **__init__**:
    Initializes the AccessTokenHandler class with API credentials and cache file location.
- **load_token()**:
    Attempts to load the cached token from the specified cache file. Returns the token data if available and valid; otherwise, returns None.
- **save_token(token_response)**:
    Saves the token response to the cache file. It calculates the expiration time by adding the expires_in value (in seconds) to the current time.
- **is_token_valid(token_response)**:
    Checks if the token is still valid by comparing the current time to the token’s expiration timestamp (expires_at field). Returns True if the token is valid, False otherwise.
- **get_cached_token()**:
    Retrieves the access token from the cache if it exists and is still valid. Returns the token string or None if no valid token is found.
- **refresh_token()**:
    Makes an API request to refresh the access token if the cached token is invalid or expired. It requires the auth_url, client_id, client_secret, username, and password to be set.
    On success, the new token is saved to the cache, and the token is returned. On failure, it prints an error message and returns None.

<h3>gen_config.py</h3>

The **gen_config.py** file is responsible for configuring a logging system for Python scripts. It sets up a logger that writes log messages both to a dynamically named log file and the console. This logging utility helps with tracking the execution and debugging of scripts in a structured and easily accessible format.

-----
**Function: configure_logger(script_name)**

The configure_logger function creates and returns a logger instance that logs messages to both a log file and the console.

**Parameters:**

- **script_name**: A string representing the name of the script or the logger instance. This name is used to identify the log messages and is part of the log file name.

**How It Works:**

- **Log Directory Creation**:
    The function checks if the log directory (Documents/Dossier7_Files/Logs) exists on the user's system. If it does not exist, it creates the directory using the utils.create_directory_if_not_exists_log() utility function.
- **Log File Name**:
    A log file is created with a dynamic timestamp (in the format YYYY-MM-DD_HH-MM-SS) to ensure unique log file names.
- **Logger Configuration**:
    A logger instance is initialized with the script_name.
    The logger is configured to log messages at the DEBUG level (the most detailed logging level).
- **Handlers**:
    The logger has two handlers:
       **File Handler**: Logs all messages to a .txt file inside the Logs directory.
       **Stream Handler**: Outputs log messages to the console (standard output).
- **Log Format**:
    Each log message is formatted with a timestamp, the logger's name (script_name), the log level, and the message.
- **Returns:**
   The configured logger instance, which can be used to log messages from the script.

<h3>queries.py</h3>

The **queries.py** file defines a set of pre-configured queries used for making API requests to various endpoints of a system such as assets, part_usage, parts_in_storerooms, personnel, sites, tasks, preventive_mx_comp, purchase_orders, time_segments, top_level_parts, work_ord and many others. Each query is represented as a dictionary that contains information like the API endpoint, filters, ordering, and expanded fields to be included in the API call.

This file is structured to allow seamless integration with API handling logic in the codebase. The queries defined here are intended to pull specific sets of data from the system's API.

-----
**Structure**

The file contains a Python list called queries, where each item in the list is a dictionary representing an API query. Each dictionary includes the following components:

-**Fields in Each Query:**

- **name**: A string representing the name of the query. This is used to identify what data the query is fetching (e.g., "assets", "part_usage").
- **result_type**: Specifies the type of data returned. It could be entity (representing individual objects) or transactional (representing transactional records).
- **payload**: This dictionary contains the parameters that control how the query is executed, including:
   - **page**: The page number of the results (used for pagination).
   - **amount**: The number of records to return per page.
   - **groupBy**: An empty list (if no grouping is applied).
   - **filter**: A set of filters applied to the query to retrieve only relevant records.
   - **orderBy**: Specifies the sorting order of the results.
   - **expands**: Specifies which related fields to expand (nested objects).
- **baseUrl**: The API endpoint to which the query is sent.

 <h3>requirements.txt</h3>

   The **requirements.txt** packages contain the required packages to run the project. Use this file to install the necessary dependencies.

 <h3>scratch2.py</h3>

   The **scratch2.py** script is designed to make API calls to retrieve data about interactive lists and save the results to an Excel file. It uses token-based authentication to interact with the API, handles pagination to retrieve all available data, and processes the data into a structured format for output.

   -----
   **Features**

- **API Interaction**: Retrieves data from the interactiveLists endpoint.
- **Pagination**: Handles paginated API responses to ensure all data is collected.
- **Logging**: Uses a logging system to record the process.
- **File Output**: Saves the retrieved data to an Excel file.
-----
**Structure**

**Imports**

- **requests**: To make HTTP requests.
- **urllib3**: For HTTP connection handling, including disabling SSL warnings.
- **os**: For interacting with the operating system, e.g., file paths.
- **utils**: Custom utility functions (assumed to be defined in utils.py).
- **json**: For handling JSON data.
- **gen_config**: Custom configuration functions (assumed to be defined in gen_config.py).
- **datetime**: For handling timestamps.
- **time**: For measuring execution time.

**Constants**

- **auth_url**: URL for obtaining an authentication token.
- **base_url**: Base URL for the API endpoint to fetch interactive lists.
- **table_name**: A string used to identify the type of data being processed ("checklists").

**Functions**

**main()**

- **Setup**:
    Configures logging using gen_config.configure_logger().
    Loads environment variables from a .env file using utils.load_env_file().
    Retrieves authentication credentials and access token.
- **API Call Setup**:
    Defines the initial_query payload for the API request.
    Configures pagination and processing of the API response.
- **API Calls**:
    Makes API calls to fetch data, handling pagination to collect all available pages.
    Measures and prints the time taken for each call.
- **Data Processing**:
    Saves the collected data to an Excel file using the utils.process_data_to_df_checklists() function.
- **Completion**:
    Prints the total time taken for the entire operation and completion message.

 <h3>work_rqsts.py</h3>

The work_rqsts.py script is designed to interact with an API to retrieve work request data, process it, and save it to a CSV file. The script handles token-based authentication, paginated API requests, and file operations.

-----
**Features**

- **Token Management**: Uses an access token for API authentication, with automatic refresh handling.
- **API Interaction**: Retrieves work request data from the API, handling pagination to ensure complete data retrieval.
- **Data Output**: Saves the retrieved data to a CSV file with a timestamp in the filename.
-----
**Structure**

**Imports**

- **json**: For handling JSON data.
- **time**: For measuring execution time and handling delays.
- **os**: For interacting with the operating system, e.g., file paths.
- **datetime**: For generating timestamps.
- **urllib3**: For HTTP connection handling, including disabling SSL warnings.

**Constants**

- **auth_url**: URL for obtaining an authentication token.
- **base_url**: Base URL for the API endpoint to fetch work requests.
- **client_id**: Client ID for authentication.
- **client_secret**: Client secret for authentication.
- **username**: Username for authentication.
- **password**: Password for authentication.

**Functions**

**main()**

- **Setup**:
    Configures the script for execution.
    Initializes the token handler using classes.AccessTokenHandler to manage authentication tokens.
- **Token Management**:
    Attempts to load a cached token and check its validity.
    If the token is invalid or not present, refreshes it using the authentication credentials.
- **API Requests**:
    Configures the query parameters for the API request.
    Makes paginated API requests to retrieve work request data.
    Measures and prints the time taken for each page of data retrieval.
- **File Operations**:
    Creates a directory for storing results if it does not exist.
    Saves the retrieved data to a CSV file with a timestamped filename.
- **Completion**:
    Prints the total execution time and a completion message.








