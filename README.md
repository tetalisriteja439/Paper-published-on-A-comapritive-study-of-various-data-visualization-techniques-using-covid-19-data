The **all_records_pull.py** script is responsible for making API calls using predefined queries, processing the data, and handling logging functionalities. This script works in conjunction with other modules such as gen_config, utils, work_rqsts, scratch2 and queries to automate the process of retrieving data from an API and handling log file management.

##How It Works

 **API Authentication**:
    The script authenticates using OAuth2 credentials (client ID, client secret, username, password), which are loaded from environment variables.
