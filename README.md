
# Operationalizing Machine Learning

## Overview
In this project we have we used Azure ML to create a machine learning model and deploy it on the production environment to publish and consume it. We first ran an AutoML experiement to identify the best model for our Bank-Marketing dataset and then deployed the best model which is Voting Ensemble and then consumed it through the endpoint.py file. We also documented our model using the swagger UI. Then to automate all of this we created a pipeline in the notebook and included all the steps that created an AutoML model and then trained it. Then in this also we saved the best model as pickel. We also tested our best running model using a test dataset and created a confusion matrix. Then we created a rest endpoint to this best model and then scheduled an experiment on it.

## Data
The data is taken from the UCI repository: [UCI Machine Learning Repository: Bank Marketing Data Set](https://archive.ics.uci.edu/ml/datasets/Bank+Marketing). 


#### Attribute Information:

Input variables:  
bank client data:  
1 - age (numeric)  
2 - job : type of job (categorical: 'admin.','blue-collar','entrepreneur','housemaid','management','retired','self-employed','services','student','technician','unemployed','unknown')  
3 - marital : marital status (categorical: 'divorced','married','single','unknown'; note: 'divorced' means divorced or widowed)  
4 - education (categorical: 'basic.4y','basic.6y','basic.9y','high.school','illiterate','professional.course','university.degree','unknown')  
5 - default: has credit in default? (categorical: 'no','yes','unknown')  
6 - housing: has housing loan? (categorical: 'no','yes','unknown')  
7 - loan: has personal loan? (categorical: 'no','yes','unknown')  

related with the last contact of the current campaign:  
8 - contact: contact communication type (categorical: 'cellular','telephone')  
9 - month: last contact month of year (categorical: 'jan', 'feb', 'mar', ..., 'nov', 'dec')  
10 - day_of_week: last contact day of the week (categorical: 'mon','tue','wed','thu','fri')  
11 - duration: last contact duration, in seconds (numeric). Important note: this attribute highly affects the output target (e.g., if duration=0 then y='no'). Yet, the duration is not known before a call is performed. Also, after the end of the call y is obviously known. Thus, this input should only be included for benchmark purposes and should be discarded if the intention is to have a realistic predictive model.  

other attributes:  
12 - campaign: number of contacts performed during this campaign and for this client (numeric, includes last contact)  
13 - pdays: number of days that passed by after the client was last contacted from a previous campaign (numeric; 999 means client was not previously contacted)  
14 - previous: number of contacts performed before this campaign and for this client (numeric)  
15 - poutcome: outcome of the previous marketing campaign (categorical: 'failure','nonexistent','success')  

social and economic context attributes  
16 - emp.var.rate: employment variation rate - quarterly indicator (numeric)  
17 - cons.price.idx: consumer price index - monthly indicator (numeric)  
18 - cons.conf.idx: consumer confidence index - monthly indicator (numeric)  
19 - euribor3m: euribor 3 month rate - daily indicator (numeric)  
20 - nr.employed: number of employees - quarterly indicator (numeric)  
  
Output variable (desired target):  
21 - y - has the client subscribed a term deposit? (binary: 'yes','no')

Note: The above information about the features is taken from [UCI Machine Learning Repository: Bank Marketing Data Set](https://archive.ics.uci.edu/ml/datasets/Bank+Marketing)



## Architectural Diagram
The Architectural diagram is as follows. This covers the most crucial steps we did in the project. 
![Architecture](Architecture.png)

## Key Steps
1. We have the dataset on which we will be training the ML model which is the bankmarketing dataset. We will be using this dataset to identify the best model to predict the output Y.
![Dataset](1%20dataset.png)
2. To identify the best model for our data. We created an AutoML experiment. Specified the parameters so that our experiment finishes within time and started its execution.
![AutoML Experiment](2%20automl%20finished.png)
3. After the execution of the AutoML experiement, we identified that the best model was a VotingEnsemble which had the accuracy of 92%.
![Best Model](3%20best%20model.png)
4. We will be using our best model for deployment. Using this deployment we created an endpoint.
![Endpoint](4%20endpoint%20deployed.png)
5. Now we want to make sure that we get all the logs from our deployment. Using our logs.py file, we first enabled application insights so that in Azure in real time we get all the information about the requests and other factors. Then we fetch all the logs from our deployment. In the logs you can see the line where we enable the application insights.
![Logs](5%20logs.py.png)
6. This is where we verified that the application insight was running.
![App Insights](6%20app%20insight%20enabled.png)
7. Now we want to make sure that we document our model properly. For that we download the latest version of swagger UI image from docker repository. We do this by executing the docker commands in swagger.sh.
![swagger sh](swagger%20sh.png)
9. Now we want to make sure our swagger.json that downloaded from the endpoint details, available on the localhost. Because we will be using this location of swagger.json on swagger UI to get the list of all the services, input and output. For this we open the swagger UI image and put this URL. We can see that our model has a score URL on which we can provide inputs.
![http api score](7%20http%20api%20score.png)
10. And this is the response that our model provides. On successful execution it returns the score otherwise it returns an error code with description.
![http api response](8%20http%20api%20response.png)
11. We then copied the rest URI and primary key from the endpoint/consume tab, and updated it in the endpoint.py. The endpoint.py file contains the necessary code to execute our deployed model through the endpoint. We used the data.json as input, and executed the model to get the response.
![endpoint response](9%20endpoint%20response.png)
12. Now we want to automate everything using a pipeline. for that we downloaded and created the bankmarketing dataset through the code provided in the notebook.
![bankmarketing datased from notebook](12%20bankmarketing%20datased%20from%20notebook.png)
13. We then created a pipeline so that we can update the model remotely.
![pipeline created](10%20pipeline%20created.png)
14. We also need to provide all the necessary steps. The steps involve creating an AutoML experiement and then running it. We executed our pipeline and this is how the runWidget output looks like.
![pipeline runwidget](14%20pipeline%20runwidget.png)
15. Now we also want to see what all the steps we performed. after extracting the best model we used its `steps` attribute to see all the hyperparemeters learned and what various activation functions and steps our best model performed. 
![best model steps](15%20best%20model%20steps.png)
16. And like we did before we will deploy our best model and create an endpoint to consume and rerun through the pipeline.
![pipeline endpoints](11%20pipeline%20endpoints.png)
17. Now the reason why we created the pipeline, we create an AutoML run through the endpoint using the pipeline.
![scheduled run completed](16%20scheduled%20run%20completed.png)


## Screen Recording
Here's a link to my Screen recording: https://www.awesomescreenshot.com/video/2403338?key=c8b31d5f38a4212a94eef70e24e18705

## Standout Suggestions
1. We could improve the accuracy of the model even more by including more data
2. We can also include deep learning and allow execution of the models for a longer time to find an even more accurate model
3. We can improve the preprocessing step to improve the input data provided through techniques like ordinal, label or one-hot encoding.
4. We can also normalize the input data to make sure that the values are between 0 to 1. This retains the relative importance of the features.
