# MLOps-Project

### create env
- python -m venv .venv  ## create env

## MOngo DB
- create cluster
- set username pass
- allow access from anywhere in network access
- goto project and "get connection string"
- driver python 3.6 or later
- copy and save the use this connection string in app

### setup pyproject.toml and setup.py
- use -e . in requirements
- code the pyproject.toml and setup.py
- pip install - r requirements.txt

### setting up notebook for mongodb
- read the data
- change connection url links in the notebook
- set the mongo db setup
- browse collections in cluster0

### logging and exception modules
- write logger and exception files
- eda and feature engg notebook

## Data ingestion
- before data ingestion .. declare variable with constants.__init__.py
- add configuration.mongo_db_connections.py and define the func for mongodb connection
- inside data_access folder, create a new file proj1_data.py and __init__.py that will use mongodbconnecyions.py    ## to connect with DB, fetch data in key-val format and transform that to df >>
- add code to entity.config_entity.py file 
- add code to entity.artifact_entity.py
- add code to components.data_ingestion.py 
- add code to pipeline.training_pipeline
- run demo.py to test mongobd connection url
### setting env for important keys
- to set: $env:MONGODB_URL = 'mongodb+srv://<username>:<db_password>@cluster0.dlwdf7o.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0'
- to check: echo $env:MONGODB_URL
- run demo.py ## creates artifacts

## Data validation, transformation and model trainer

- complete work on utils.main_utils.py and config.schema.yaml ( add entire info about dataset for validation step)
- complete the data validation 
- do the data transformation component ( add estimator.py  to entity folder)
- do the model trainer component and also add estimator.py in entity


