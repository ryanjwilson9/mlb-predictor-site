# mlb-predictor-site

predicts future mlb game winners based on game logs from 2023 and completed 2024 games, updated daily. 

little data is used, project more focused on practicing workflow 

**Data Extraction:** 
  - Insert Game:
    - _insert_game.py_: fetches data from internet -> postgreSQL database
    - _insert_game_Dockerfile_: containerizes script + packages
    - _insert_game_job.yaml_: deploys Docker image, runs in kubernetes
    - _insert_game_cronjob.yaml_: updates image daily at 3AM
  - Pull Schedule:
    - _pull_schedule_: fetches 2024 schedule from internet once, saves .csv
      
**Data Preprocessing and Feature Engineering:**
  - _feature_preprocessing.py:_
    - loads data from posgreSQL database
    - pairs game logs from both teams 
    - creates new features based on differences in stats
    - normalizes some features
    - important: creates feature for winner of game
    - saves preprocessed_data.csv
  - _feature_preprocessing_Dockerfile_: containerizes script + packages 
  - _feature_preprocessing_job.yaml_: deploys Docker image, runs in kubernetes
  - _feature_preprocessing_cronjob.yaml_: updates image daily at 4AM

**Training and Testing ML Models**
  - train_model.py:
    - trains models on previous game data to predict future winners
    - (not trained on predicting features within each game log)
    - models tested: RandomForest, GradientBoosting, LogRegression
    - selects best model based on simple binary classifification acc
    - saves best model, and notes only necessary packages to run
  - _train_model_Dockerfile_: containerizes script + packages
  - _train_model_job.yaml_: deploys Docker Image, runs in kubernetes
  - _train_model_cronjob.yaml_: updates image WEEKLY at 5AM

**Applying Best Model to Predict Future Games**
  - _apply_model.py_: 
	- splices schedule to identify games that have yet to be played
    - uses preprocessed data and best model to make predictions
    - saves predicitons.csv
  - _apply_model_Dockerfile:_ containerizes script + packages suggested from training
  - _apply_model_job.yaml_: deploys Docker image, runs in kubernetes
  - _apply_model_cronjob.yaml_: updates image daily at 6AM


    
                    
    
