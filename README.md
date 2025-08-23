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

25. Create the code structure of "Prediction Pipeline" and setup your app.py
26. Add "static" and "template" dir to the project.

27. Getting started with CI-CD process:
      * Setup the dockerfile and .dockerignore file
      * Setup the .github\workflows dir and aws.yaml file within
      * Go to AWS console and create a new IAM user exactly the way we did earlier (name: "usvisa-user") >>
        Go inside user >> Security Credentials >> Access Keys >> create access key >> CLI >> check agreement
        >> next >> create access key >> download csv
      * Now create one ECR repo to store/save docker image:
        AWS console >> Go to ECR >> Region: us-east-1 >> Hit create repository >>
        repo name: vehicleproj >> hit create repository >> copy and keep uri
      * Now create EC2 Ubuntu server >> AWS console >> EC2 >> Launch Instance >> name: vehicledata-machine
        >> Image: Ubuntu >> AMI: Ubuntu Server 24.04 (free tier) >> Instance: T2 Medium (~chargeable-3.5rs/hr)
        >> create new key pair (name: proj1key) >> allow for https and http traffic >> storage: 30gb >> Launch
        >> Go to instance >> click on "Connect" >> Connect using EC2 Instance Connect 
        >> Connect (Terminal will be launched) 

28. Open EC2 and Install docker in EC2 Machine:
      ## Optinal
      sudo apt-get update -y
      sudo apt-get upgrade
      ## Required (Because Docker is'nt there in our EC2 server - [docker --version])
      curl -fsSL https://get.docker.com -o get-docker.sh
      sudo sh get-docker.sh
      sudo usermod -aG docker ubuntu
      newgrp docker

29. Next step is to connect Github with EC2(Self hosted runner):
      * select your project on Github >> go to settings >> Actions >> Runner >> New self hosted runner
        >> Select OS (Linux) >> Now step by step run all "Download" related commands on EC2 server 
        >> run first "Configure" command (hit enter instead of setting a runner group, runner name: self-hosted)
        >> enter any additional label (hit enter to skip) >> name of work folder (again hit enter)
        >> Now run second "Configure" command (./run.sh) and runner will get connected to Github
        >> To crosscheck, go back to Github and click on Runner and you will see runner state as "idle"
        >> If you do ctrl+c on EC2 server then runner will shut then restart with "./run.sh"

30. Setup your Github secrets: (Github project>Settings>SecretandVariable>Actions>NewRepoSecret)
      AWS_ACCESS_KEY_ID
      AWS_SECRET_ACCESS_KEY
      AWS_DEFAULT_REGION
      ECR_REPO

31. CI-CD pipeline will be triggered at next commit and push.
32. Now we need to activate the 5000 port of our EC2 instance:
      * Go to the instance > Security > Go to Security Groups > Edit inbound rules > add rule
        > type: Custom TCP > Port range: 5080 > 0.0.0.0/0 > Save rules
33. Now paste the public ip address on the address bar +:5080 and your app will be launched.
34. You can also do model training on /training route

