# HRL Database Project
This repositoriy is a NYUSH HRL Project. We cleaned CECF CRD Database and predicted some missing values using XGBRegressor.

## Introduction
// some introduction to the database
#
## Original Database
//name

//the process of collecting the  data

## Records Mergeing
// introduce the merging and py file

## Data Preprocessing
After we merge different record, we conduct data cleaning in the preprocessing satge to clean the duplicates.

For numeric values, we reconciled conflicting responses according to these rules: number overrides missing or null values and most common number overrules. If those rules cannot resolve the conflict, these rules apply in these cases
For date ranges, we cleaned according to the following rules: when the year range was smaller than 5, we took the smallest (i.e., oldest) number (if ranger larger than 5, archival source is consulted); in month and day ranges, took the smallest (i.e., oldest) number; and when there were no month or day records, we used “1” as the default. 
For wage and investment, the largest number taken.  

For string values, we used the cluster cleaning functions in Open Refine (key collision & nearest neighbor) to standardize differences in spelling. In cases where variations were meaningful (e.g., Far East Garden Restaurant v. Far East Tea Garden Restaurant), we preserved differences in spelling. 

For street addresses, we used the cluster cleaning functions in Open Refine (key collision & nearest neighbor) to standardize differences in spelling street names. In cases where the house number is a range, we selected the smaller number. 

Detailed steps can be find [here](https://docs.google.com/document/d/1PBgD0DWsrzOnmubfnC92FbGEmAuqilxUOiA4A8xzoag/edit#heading=h.mz4nrr7tzxjw]).
#
After we finished cleaning the database, we aim at prediction the missing value s in the REST Section.
## Data Visualization
Code can be find [here](data_visual.ipynb). In the document, we did basic analysis to the dataset.
## Data Prediction
We used machine learning techniques to predict missing values from the restaurant section, there are 14 columns in restaurant section, with 504 data records. We conducted two layers of prediction. The first layer of predictions was based on the existing data related to the restaurant data scope (i.e. 17 columns in total, 14 columns in restaurant section and 3 related columns outside restaurant section), while the second layer of prediction was based on the good prediction made by the first layer(i.e. MAPE < 0.35) as well as the existing data mentioned above. Detailed procedure will be discussed in the following section. In the data set, the first layer of prediction is marked with (p1) after the numerical prediction while the second layer of prediction is marked with (p2) after the numerical prediction. The model we used in both layers is Xgbregressor. We use Cross Validation to give scores of the model, and we choose MAPE to be our scoring method. The following chart demonstrate the MAPE value of two layers of prediction.

In the first layer of prediction, we have 5 columns that reach below 0.35 MAPE value(i.e. REST : Staff size MOD, REST : Wtr MOD, REST : Ktch Wkr MOD, REST : tables MOD, and REST : Pass Part MOD, notice here that REST : Pass Part MOD have NULL value because MAPE cannot deal with 0 value). We added predicted data from these 5 columns and conduct second round of prediction for the rest of the columns. 
