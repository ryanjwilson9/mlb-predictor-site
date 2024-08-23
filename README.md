# mlb-predictor-site

predicts future mlb game winners based on game logs from 2023 and completed 2024 games, updated daily. 

little data is used, project more focused on practicing workflow 

Data Extraction: 
    Insert Game:
      insert_game.py: fetches data from internet -> postgreSQL database
      insert_game_Dockerfile: containerizes script + packages
      insert_game_job.yaml: deploys Docker image, runs in kubernetes
      insert_game_cronjob.yaml: updates image daily at 3AM
    Pull Schedule:
      pull_schedule: fetches 2024 schedule from internet once, saves .csv
      
Data Preprocessing and Feature Engineering:
    feature_preprocessing.py: loads data from posgreSQL database
                              pairs game logs from both teams 
                              creates new features based on differences in stats
                              normalizes some features
                              important: creates feature for winner of game
                              saves preprocessed_data.csv
    feature_preprocessing_Dockerfile: containerizes script + packages 
    feature_preprocessing_job.yaml: deploys Docker image, runs in kubernetes
    feature_preprocessing_cronjob.yaml: updates image daily at 4AM

Training and Testing ML Models
    train_model.py: trains models on previous game data to predict future winners
                    (not trained on predicting features within each game log)
                    models tested: RandomForest, GradientBoosting, LogRegression
                    selects best model based on simple binary classifification acc
                    saves best model, and notes only necessary packages to run
    train_model_Dockerfile: containerizes script + packages
    train_model_job.yaml: deploys Docker Image, runs in kubernetes
    train_model_cronjob.yaml: updates image WEEKLY at 5AM

Applying Best Model to Predict Future Games
    apply_model.py: splices schedule to identify games that have yet to be played
                    uses preprocessed data and best model to make predictions
                    saves predicitons.csv
    apply_model_Dockerfile: containerizes script + packages suggested from training
    apply_model_job.yaml: deploys Docker image, runs in kubernetes
    apply_model_cronjob.yaml: updates image daily at 6AM


    
                    
    
