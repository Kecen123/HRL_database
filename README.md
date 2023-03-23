# HRL Database Project
This repository is an NYUSH HRL Project. We cleaned CECF CRD Database and predicted some missing values using XGBRegressor.

## Introduction
The Chinese Restaurant Database was designed by Professor Heather Ruth Lee to connect two halves of Chinese American history: the multitude of Chinese businesses and the experiences of Chinese immigrants under Chinese Exclusion. It captures through data what it was like to invest, operate, and work in Chinese restaurants, as well as the dealings that business partners had with one another and their suppliers. It also uses data to describe how restaurant owners and employees coped with Chinese Exclusion, how they immigrated, and the ways they maximized opportunities in U.S. immigration policies to stay connected with the people and places in China that were most dear to them. Neither Chinese business nor immigration data has been compiled so comprehensively before.


Professor Heather Ruth Lee and research assistants constructed the Chinese Restaurant Database from the Chinese Exclusion Act Case Files at the National Archives and Records Administration in New York. These records revealed the business operations of Chinese restaurants, because of the immense human and capital resources invested in enforcing Chinese Exclusion, a series of federal immigration laws that worked in conjunction with treaties with China and agreements with international transportation companies to prevent Chinese laborers from immigrating. As a result, Chinese restaurant owners had to prove their right to enter by submitting to investigations into their businesses, families, and immigration histories. The unintended result of these enforcement practices was that the Chinese Exclusion Act Case Files, the surviving records of Chinese individuals who applied for admission or were deported from the United States, are the most voluminous and robust records on Chinese restaurants.


The Chinese Restaurant Database contains three types of information: demographic details about applicants; business statistics about Chinese restaurants; and the migration history of applicants. Professor Lee will release a beta version of the database that contains cleaned, verified, and predicted restaurant business data though Dataverse in tandem with the release of her forthcoming book, Gastrodiplomacy (University of Chicago Press) 2025. We focused on cleaning and predicting restaurant business data, because of its unique value to the history of Chinese restaurants. 
## Data Collection
### Sampling Methodology
The New York branch of the National Archives and Records Administration holds 18,563 Chinese Exclusion Act Case Files. To identify files for Chinese restaurant owners and employees, Professor Lee searched the index created by Betty Lee Sung for the below search terms in columns for occupation (1, 2, and 3), sponsor and comments.  


Professor Lee marked individuals whose occupations were listed as bookkeeper, cashier, cook, manager and assistant manager of restaurant, restaurateur, restaurant keeper, restaurant owner, restaurant proprietor, and waiter. She flagged files whose sponsor or comment columns contained historical English terms for restaurant (café, garden, inn, palace, restaurant, tea garden), transliterations of Chinese terms for restaurant (lo, low, lowe), indexer abbreviations for food service business (rest., co.). She also used common misspellings of the above search terms. 


Between 2012 and 2017, Professor Lee requested that the archive screen 1281 Chinese Exclusion Act Case Files for research access. Professor Lee or research assistants photographed each page of those files in the order they appeared, including any backsides that contain information. A total of 790 files were photographed over the course of the project. Not all files were consulted, due to time and cost constraints. Some files were not photographed because the files were empty. 


### Data Extraction
The data extraction process captured demographic, migration, and business information each time a Chinese restaurant owner applied for entry or reentry with the Bureau of Immigration. Some Chinese Exclusion Act Case Files contained multiple applications and the demographic, migration, and business information was captured for the time of application. Chinese restaurant owners also used their immigration status to sponsor their dependents and those Chinese Exclusion Act Case Files were created in the name of minor children and/or spouses. The business information associated with the restaurant owner was captured, while the demographic and migration information for the dependent was captured.


Transcribers read an immigration file in its entirety and then input relevant data into a digital intake form (Google Form). In total, 319 Chinese Exclusion Act Case Files were transcribed and contained 1063 (pre-cleaning) / 504 (post-cleaning) unique applications for entry or reentry. Not all files contained the relevant information, and those files were skipped. Not all digitized files were consulted, due to time and cost constraints.

### Data Verification
179 were assigned to two transcribers, who completed the data extraction process independently of one another. Their responses were compared with one another to assess the quality and reliability of transcription using the data merging process described below. 

When transcribers recorded different answers, Professor Lee or research assistants consulted the original file to determine the most accurate response. 

### Recruitment and Training
The summer research assistants who assisted with hand transcription were NYU Shanghai undergraduate students. They applied through a job call, were interviewed based on their qualifications, and were trained on the history of Chinese Exclusion and the data extraction procedure. They were able to pose questions about the work through a shared google document, drop in during open hours with Professor Lee, and during biweekly check-ins. 
## Data Cleaning
Professor Lee and Research Assistants selectively cleaned numeric and string data using python, Open Refine, and manually. The python notebook “Data Cleaning” contains the final code with marks down explanations. We focused on ensuring that the archival source, application, and restaurant business data columns were cleaned of duplicate or conflicting responses and had low rates of missing values. 


At the end of the data cleaning process, we consolidated and cleaned 1063 entries in the original database to have 504 unique entries. 


### Preprocessing
Before cleaning variables, we retained the original data and coped original data into a new column and added the suffix “MOD” to original variable name. This step was performed in Google Refine.


Because transcribers represented data differently, the dataset had to be normalized:
All missing values are represented as NULL (performed in python) 
Eliminated empty/ blank entries (performed in python)
Missing information for file source looked up (performed manually)
Missing or conflicting information for application type and year looked up (performed manually)
Names of restaurants standardized and verified (performed in Open Refine and manually). 
Addresses of restaurants standardized and verified (performed in Open Refine and manually). 
Applicant’s positions in restaurants, working start dates, investments (when applicable), 


We cleaned string values in Open Refine using cluster cleaning functions (key collision & nearest neighbor) to standardize differences in spelling. We preserved  meaningful variations (e.g., Far East Garden Restaurant v. Far East Tea Garden Restaurant).


We clearned number ranges for house number by accepting the smallest number as the house number. 

```
File Source
SOURCE: Regional NARA archive MOD
Eliminate empty records for DEL (2 cases), Missing (1 case), Washington DC (1 case), ID 357, 258, 363
Eliminted duplicate entry for ID 242
Manual input for file 12/267; 34/742; 6/1607; 4/4

SOURCE: Folder Number
Manual input for 5 folders
```
### Merging Records
In the data collection process, multiple transcribers worked on a subset of files, in order to assess accuracy in the data cleaning stage. Their results needed to be merged into one entry for comparison and verification. The below steps were completed in python. 


1. Identifying Duplicates
We identified as single entries filter those records that had unique values for the folder number (variable name: “SOURCE: Folder No MOD”), the application date (variable names: "APPLICATION: Year filed MOD","APPLICATION: Month MOD", and "APPLICATION: Day MOD."). Having unique values in these fields indicated that one research assistant transcribed these files. 


    We identified as duplicates entries those records that had identical values for the folder number (variable name: “SOURCE: Folder No MOD”), the application date (variable names: "APPLICATION: Year filed MOD","APPLICATION: Month MOD", and "APPLICATION: Day MOD."). Having identical values in these fields indicated that multiple research assistants transcribed the same files and applications for status within them. 


2. Merging Duplicate Entries
When entries that share identical values for folder number (variable name: “SOURCE: Folder No MOD”), the application date (variable names: "APPLICATION: Year filed MOD","APPLICATION: Month MOD", and "APPLICATION: Day MOD."), we collapsed the values in other variables into single fields.


    When merging those other fields, we observed the following rules.

    - If values are identical, then the transcribers’ answers matched one another. In these cases, the agreed-upon response is accepted and only one value is recorded. 
    - If values are different, then transcribers disagreed on what the proper answer should be. In those cases, we created a list, using “,” to separate their answers. We also preserved order, so that transcribers’ responses could be disentangled in the future. For example, all Transcriber A’s responses would appear first, followed by “,” and then followed by Transcriber B’s responses. 


    For example. Duplicate A,B,C has the same value in "SOURCE: Folder No MOD","APPLICATION: Year filed MOD","APPLICATION: Month MOD", and "APPLICATION: Day MOD". Duplicate A have value of 1 in the field ExampleField, Duplicate B has a value of 1 in the field ExampleField, Duplicate C has a value of 2 in the field ExampleField. The final merged record will have value 1,2 in the field ExampleField. 


### Categorizing Duplicates
We created the new variable “DATA TYPE” to enable us to easily distinguish duplicate and unique entries from one another. We made this distinction because we employed data-cleaning rules for each data type. 


We categorized duplicates as “Family Data,” when one application was used for multiple people (e.g. primary applicant, minor children, spouses), because family members migrating together have the same file name and application date and, therefore, meet the merging criteria we mentioned above. However, their answers need to be cleaned differently. We searched for the words child, wife, daughter, and son in the field “APPLICATION: Type” to identify families migrating together. 


We categorized it as  “Single data W/ multiple input,” when one application was transcribed by different research assistants. If the record does not contain the words child, wife, daughter, or son in “APPLICATION: Type,” and it is a merged record, we marked the record as “Single data W/ multiple input.”


We categorized unique entries as “Single data W/ 1 input.”  If the record has no child, wife, daughter, or son in 'APPLICATION: Type', and it is a not merged record, we marked the record as “Single data W/ 1 input.” 


At this stage, we assigned a unique 6- digit ID composed of uppercase letter characters, and numeric digits were assigned to each entry. We then separate unique and duplicate records into two datasets for the data cleaning stage. 


## Data Reconciliation
For files with duplicate entries, we evaluated conflicting answers and determined the most fitting answers.


For numeric values, we applied the following rules
When comparing missing/null values to value, we accepted the the value (performed in python)
When comparing three different values, we accepted the most common value (performed in python)
When looking at year ranges of 5 years or less, we accepted the oldest/ smallest year value; when looking at year ranges over 5 years, we consulted the original archival source (performed manually)
When looking at month and day ranges, we accepted the oldest/ smallest value (performed manually)
When no month or day values were provided, we used “1” as the default (performed manually)
When looking at range for wages and investment, we accepted the highest figure (performed manually)


For all other cases cannot resolve the conflict, the rules taht are apply in [here](https://docs.google.com/document/d/1yZ_G_xTvKL4G0DCtbEBZ8D3R7niK0ua3kfaylXFXBhw/edit?usp=sharing).


## Data Prediction
### Introduction
We used XGBoost, a powerful machine learning technique for building supervised regression models, to predict missing numeric values in the restaurant business data section. We tested and explored several models and ultimately selected XGBoost, because when comparing actual and predicted values, it returned the most fitting predictions. 


Widely used for predictive analytics by data scientists, XGBoost is particularly good at handling missing values in a dataset. XGBoost creates a decision tree model using gradient boosting, a machine learning technique that combines multiple weak models to create a strong regression model. 


### Restaurant Data
The restaurant business data section contains 16 columns: 4 string and 12 numeric values and 504 records. We chose the numeric data in the restaurant section because it we found relatively dependencies amont the variables. For variables tested, the missing rates was typicall >70%. The goal of predictions is to accurately predict the missing values. 
| Variable Name              | Number of Missing Values | % of Total Values |
| -------------------------- | ------------------------ | ----------------- |
| REST: Monthly Expenses MOD | 489                      | 97.0     |
| REST: No Ktch Wkr MOD      | 416                      | 82.5   |
| REST: Dividends MOD        | 409                      | 81.2              |
| REST: No Wtr MOD           | 401                      | 79.6              |
| REST:  No Pass Part MOD    | 387                      | 76.8    |
| REST: Monthly Sales MOD    | 381                      | 75.6      |
| REST: No Act Part MOD      | 381                      | 75.6     |
| REST: No tables MOD        | 378                      | 75.0   |
| REST: Staff size MOD | 377                      | 74.8       |
| REST: Monthly rent MOD     | 361                      | 71.6      |
| REST: Total Capital MOD    | 335                      | 66.5              |
| REST: Total Partners MOD   | 332                      | 65.9     |

The string values are: restaurant’s name, address (street, city, state), applicant’s relationship to the restaurant (manager, investor, worker), applicant’s working position (when applicable) 


The numeric values are:  Applicant’s monthly wages, when they started working (day, month, year), size of investment (when applicable), when they made the investment (when applicable) (day, month, year), restaurant’s monthly sales, monthly rent, monthly expenses, annual dividends, staff size (number of waiters + number of kitchen staff), number of tables, total capital, total investors (number of active +  passive partners) 


### Model Selection: MS Excel Linear Regression v. XGBoost
We evaluated two options for predicting missing numeric values: MS Excel’s linear regression function and XGBoost. 


### MS Excel Linear Regression
When testing MS Excel’s linear regression, we group the restaurant business data variables into 6 clusters of 3 or more variables. We created the clusters based on historical knowledge of the variables and their relationship to one another, a heatmap visualizing their correlation, and examining the t-test results for the different combinations. Some variables appeared in multiple clusters.


Cluster 1: Staff Size, Number of Waiters, and Number of Kitchen Staff
Cluster 2: Total Number of Partners, Number of Active Partners, Number of Passive Partners
Cluster 3:  Applicant’s Investment, Total Capital for the Restaurant, Total number of Partners
Cluster 4: Monthly Rent, Staff Size, Monthly Expenses
Cluster 5: Monthly Sales, Total Capital for the Restaurant, Annual Dividends
Cluster 6: Number of Active Partners,  Number of Partner/ Managers; Number of Partner/ Cashiers, Number of Partner/Bookkeepers; Number of Partner/Cooks; Number of Partner/Waiters; Number of Partner/Treasurers; Number of Partner/MISC


We tried testing and refining the linear regression. However, the end results of the linear regression function were not satisfying. More importantly, the predicted result were impossible in the real world (e.g. restaurant staff of over 100 when the capitalization was 44,000), even though the values met the t-test. 


### XGBoost
XGBoost both met our accuracy tests and returned values that are meaningful in the real world. 


We created the training-testing datasets to evaluate XGBoost. The training-testing dataset is a subset of the Chinese Restaurant Database, where the observations contain no missing values in the target variables/column of the restaurant business data section. Using the sklearn model function “test_train_split,” we spilt the training-testing data in two with a 0.85:0.15 proportion: the former for training and the later for testing. Then, the target values in the restaurant business data section were removed from the test set for the latter performance evaluation. 


During training, XGBoost uses a decision tree model to identify patterns and relationships in the available variables of the training dataset. During testing, XGBoost applies what it ‘learned’ from the training data to create a decision tree model for the testing dataset to predict missing values. The predicted values are then added to the testing data. 


To evaluate the performance of XGBoost, we compared the actual values in the training data to the predicted values in the testing data. We used Mean Absolute Percentage Error (MAPE) and sought a low MAPE (<.35) result. A lower MAPE means means the prediction is “closer” to the actual value. There is no clear cut standard for an unacceptable MAPE score, though generally MAPE >.50 is considered too high. 


Hyperparameters are preset and therefore not part of the “training” process of the model. Examples of hyperparameters include the learning rate and regularization coefficient. To find the best hyperparameters for our dataset, we used Grid Search, which specifies a grid of possible values for each hyperparameter and then systematically evaluating the model performance for every combination of hyperparameters in the grid. We used a low MAPE score, i.e. our  performance metric, to determining the best hyperparameters. We iterated this process until we achieve the desired level of performance for the test dataset (A low MAPE value), which indicates better performance on our prediction.


### XGBoost Layered Predictions
Then, we used our trained models to make predictions. We completed three rounds of iterative training-testing process on the restaurant business data section of the complete Chinese Restaurant Database (504 observations). In each round, we accepted predictions if the XGBoost performance MAPE < 0.35 for the first round and MAPE < 0.5 for the second round. The accepted data for Round 1 became part of the training data for the Round 2, and the accepted data for Round 2 became part of the training data for Round 3. The predicted data are marked as (p[Number of Round]), where [Number of Round] is the round in which this missing value is predicted. 


We consider the predictions in Round 1 reliable, whereas predictions in Round 3 should be studied with caution. 


### Summary
In summary, the process of selecting and using the XGB Regressor model for predicting missing values in restaurant business data is listed below. First, the reasons for choosing the XGB Regressor model over Linear Regression are explained, which include its reliability and ability to handle missing values. Then, the data preparation and model training steps are described, where the XGB Regressor algorithm learns patterns and relationships in the data to make predictions about the target variable. Next, the process of prediction using the trained model is explained, and evaluation using metrics like MAPE is highlighted. Finally, to improve the accuracy of the prediction, multiple rounds of training and testing are done, and the predicted missing values are joined with the training data for the next round of prediction. The reliability of the predicted values is assessed by the MAPE score, and earlier rounds of prediction are considered more reliable than later rounds.
