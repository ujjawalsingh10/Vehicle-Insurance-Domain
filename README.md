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

## AWSing

23. Before moving to next component of Model Evaluation, some AWS services setup is needed:
      * Login to AWS console.
      * Keep region set as - us-east-1
      * Go to IAM >> Create new user (name: firstproj)
      * Attach policy >> select AdministratorAccess >> next >> create user
      * Go to the user >> Security Credentials >> Access Keys >> Create access key
      * Select CLI >> agree to condition >> next >> Create Access Key >> download csv file
      * Set env variables with above csv values using below method:
      ====================================================================================
         >> Set env var from bash terminal: <<
         export AWS_ACCESS_KEY_ID="AWS_ACCESS_KEY_ID"
         export AWS_SECRET_ACCESS_KEY="AWS_SECRET_ACCESS_KEY"
         >> Check env var from bash terminal: <<
         echo $AWS_ACCESS_KEY_ID
         echo $AWS_SECRET_ACCESS_KEY

         >> Set env var from powershell terminal: <<
         $env:AWS_ACCESS_KEY_ID="AWS_ACCESS_KEY_ID"
         $env:AWS_SECRET_ACCESS_KEY="AWS_SECRET_ACCESS_KEY"
         >> Check env var from powershell terminal: <<
         echo $env:AWS_ACCESS_KEY_ID
         echo $env:AWS_SECRET_ACCESS_KEY
      ====================================================================================
      * Now add the access key, secret key, region name to constants.__init__.py
      * Add code to src.configuration.aws_connection.py file (To work with AWS S3 service)
      * Ensure below info in constants.__init__.py file:
            MODEL_EVALUATION_CHANGED_THRESHOLD_SCORE: float = 0.02
            MODEL_BUCKET_NAME = "my-model-mlopsproj"
            MODEL_PUSHER_S3_KEY = "model-registry"
      * Go to S3 service >> Create bucket >> Region: us-east-1 >> General purpose >>
        Bucket Name: "my-model-mlopsproj" >> uncheck: "Block all public access" and acknowledge >>
        Hit Create Bucket
      * Now inside "src.aws_storage" code needs to be added for the configurations needed to pull 
        and push model from AWS S3 bucket. 
      * Inside "entity" dir we will have an "s3_estimator.py" file containing all the func to pull/push
        data from s3 bucket.

24. Now we will start our work on "Model Evaluation" and "Model Pusher" component.


