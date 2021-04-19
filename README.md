# Cement-Strength-Prediction


#Problem Statement

To build a regression model to predict the concert compressive strength based on the different features in the training data. Based on the quantity of the different materials we have to predict what will be the compressive strength of the cement.
Example: Let’s assume that a civil engineer who have to build a bridge so now engineer had to decide based on the load that will be anticipated what should be the mixture to be used. In such situation we have to make a balanced mixture of concert also it should be cost efficient. Here we are predicting that what will be the maximum strength/ maximum concert compressive that we can have using the quantity of the raw materials that we have.

#Data Validation

In this step, we perform different sets of validation on the given set of training files.  
1.	  Name Validation- We validate the name of the files based on the given name in the schema file. We have created a regex pattern as per the name given in the schema file to use for validation. After validating the pattern in the name, we check for the length of date in the file name as well as the length of time in the file name. If all the values are as per requirement, we move such files to "Good_Data_Folder" else we move such files to "Bad_Data_Folder."

2.	 Number of Columns - We validate the number of columns present in the files, and if it doesn't match with the value given in the schema file, then the file is moved to "Bad_Data_Folder."


3.	 Name of Columns - The name of the columns is validated and should be the same as given in the schema file. If not, then the file is moved to "Bad_Data_Folder".

4.	 The datatype of columns - The datatype of columns is given in the schema file. This is validated when we insert the files into Database. If the datatype is wrong, then the file is moved to "Bad_Data_Folder".


5.	Null values in columns - If any of the columns in a file have all the values as NULL or missing, we discard such a file and move it to "Bad_Data_Folder".


#Data Insertion in Database

Here we are using in memory databases i.e MySQL Lite
1) Database Creation and connection - Create a database with the given name passed. If the database is already created, open the connection to the database. 
2) Table creation in the database - Table with name - "Good_Data", is created in the database for inserting the files in the "Good_Data_Folder" based on given column names and datatype in the schema file. If the table is already present, then the new table is not created and new files are inserted in the already present table as we want training to be done on new as well as old training files.     
3) Insertion of files in the table - All the files in the "Good_Data_Folder" are inserted in the above-created table. If any file has invalid data type in any of the columns, the file is not loaded in the table and is moved to "Bad_Data_Folder".
 

#Model Training

1) Data Export from Db - The data in a stored database is exported as a CSV file to be used for model training.
2) Data Preprocessing -  We do data preprocessing as per the EDA created. 
3) Clustering - KMeans algorithm is used to create clusters in the preprocessed data. The optimum number of clusters is selected by plotting the elbow plot, and for the dynamic selection of the number of clusters, we are using "KneeLocator" function. The idea behind clustering is to implement different algorithms
To train data in different clusters. The Kmeans model is trained over preprocessed data and the model is saved for further use in prediction.
4) Model Selection - After clusters are created, we find the best model for each cluster. We are using two algorithms, "Random Forest" and "Linear regression". For each cluster, both the algorithms are passed with the best parameters derived from GridSearch. We calculate the AUC scores for both models and select the model with the best score. Similarly, the model is selected for each cluster. All the models for every cluster are saved for use in prediction.


#Prediction Data Description

Client will send the data in multiple set of files in batches at a given location. Data contain multiple columns “Cement”, “Blast Furnace Slag _component_2”, “Fly Ash_component_3”, “Water_component_4”, “Superplasticizer_component_5”, “Coarse Aggregate_component_6”, “Fine Aggregate_component_7”, “Age_day”. For prediction there will be 1 column less I.e of “Concrete_compressive _strength”.
Apart from prediction files, we also require a "schema" file from client which contains all the relevant information about the training files such as:
Name of the files, Length of Date value in FileName, Length of Time value in FileName, Number of Columns, Name of the Columns and their datatype.
Prediction Data Validation  
In this step, we perform different sets of validation on the given set of Prediction files.  
1) Name Validation- We validate the name of the files on the basis of given Name in the schema file. We have created a regex pattern as per the name given in schema file, to use for validation. After validating the pattern in the name, we check for length of date in the file name as well as length of time in the file name. If all the values are as per requirement, we move such files to "Good_Data_Folder" else we move such files to "Bad_Data_Folder". 
2) Number of Columns - We validate the number of columns present in the files, if it doesn't match with the value given in the schema file then the file is moved to "Bad_Data_Folder". 
3) Name of Columns - The name of the columns is validated and should be same as given in the schema file. If not, then the file is moved to "Bad_Data_Folder". 
4) Datatype of columns - The datatype of columns is given in the schema file. This is validated when we insert the files into Database. If dataype is wrong then the file is moved to "Bad_Data_Folder". 
5) Null values in columns - If any of the columns in a file has all the values as NULL or missing, we discard such file and move it to "Bad_Data_Folder".  

#Prediction

1) Data Export from Db - The data in the stored database is exported as a CSV file to be used for prediction.
2) Data Preprocessing – All Preprocessing steps that are applied while training data that all steps will apply here also.
3) Clustering - KMeans model created during training is loaded, and clusters for the preprocessed prediction data is predicted.
4) Prediction - Based on the cluster number, the respective model is loaded and is used to predict the data for that cluster.
5) Once the prediction is made for all the clusters, the predictions I.e the percentage of concert compressive strength  are saved in a CSV file at a given location and the location is returned to the client.

