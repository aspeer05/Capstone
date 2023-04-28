# Capstone
Capstone project for Masters in Data Analytics from NWMS University.  This report and study is authored by Amber Speer.

## Code Sources
### Ken Jee - Data Project from Scratch video series
Video Link: <https://www.youtube.com/watch?v=7O4dpR9QMIM>
GitHub Repo Link: <https://github.com/PlayingNumbers/ds_salary_proj>

### GreekDataGuy
<https://towardsdatascience.com/productionize-a-machine-learning-model-with-flask-and-heroku-8201260503d2>   

## Introduction
This project analyzes housing prices based on a myriad of factors.  The goal is to demonstrate a project that could answer the questions:
1.	How much is this house worth?
2.	How much would these updates increase the value of the house?

## Data 
Because of the limited timeline for this project and the fact that it is to serve as an example of what can be done with data from a given region, this study is using a dataset made for a competition. This type of dataset is made to provide data that is ready-made for analytics.  Given the limited time for this project, collecting data for the Hannibal, MO area was not a possibility. As it turns out, the Ames dataset
for 2-story, single family homes is relatively comparable to Hannibal, MO single family, two-story homes in 2021. In other situations, the data collection would be an enormous, and very important part of the process. However, given the time limitations of this study, this is a reasonable shortcut to take that showcases the process that would need to be taken with any local current data.  

The dataset for this project is the Ames Housing dataset [4] compiled by Dean De Cock. He complied it to be used data science education settings as a more modern alternative to the popular Boston Housing Dataset. It was found on Kaggle at: <https://www.kaggle.com/competitions/house-prices-advanced-regression-techniques>

## Project Report
The entire project report can be found at: <https://www.overleaf.com/read/dwwyzmrrvpzn>

## Project Files
In the HousingPriceData folder you will find all the files for the project.  They are as follows:

    - FlaskAPI folder: Contains all the files for the deployment API.  (This is currently a work in progress and not yet completed.
    - image folder: Contains all images used in the project
    - HousePricePredictionProject.ipynb: Jupyter notebook file with python code for the project
    - cleandf: Excel workbook used to explore and clean the data (using train.csv data)
    - cleantrain.csv: CSV data file created from Excel workbook to train models (cleaned from train.csv)
    - data_description.txt: Text file of attribute and data description downloaded with dataset
    - model_file.p: File created for API to store and transfer pickled data
    - Results.xlsx: Spreadsheet to create HomeValueGraph
    - test.csv: CSV file with test data downloaded with dataset (not used in order to showcase ablity to split data).
    - train.csv: Dataset used for project, downloaded from Kaggle link above.

## Project Overview

### Exploratory Data Analysis 
Before building a model, exploratory data analysis was performed to check assumptions and narrow the number of attributes used to only the most relevant. The first step in this process was checking the correlation of the attributes and the target variable. This was accomplished using by creating a heatmap using the seaborn library.  Studying the target value on the map showed that only a few attributes had a
high correlation to the SalePrice. Many of these features were all realted to square footage. These attributes were GrLivArea, BsmtFinSF1, and BsmtFinSF2. In order to combine these features into one attribute the square footages were
added together to create TotalFinSF. The following formula was used in Excel:

TotalFinSF = (TotalBsmtSF − BsmtUnfSF) + GrLivArea

![Heatmap of correlation between attributes shows a positive correlation
between TotalFinSF, TotRmsAbvGrd, and GarageCars and SalePrice, but a
slight negative correlation between BedroomsRatio and SalePrice](./images/heatmap.png "Correlation Heatmap")

The heatmap was then run again (see above figure). The attributes with the highest correlation to SalesPrice are TotalFinSF, TotalBath, TotalRmsAbvGr, and GarageCars. The highest and most unexpected correlation was GarageCars. Pair plot and relational plots were done on these attribute to confirm the positive correlation. The most telling plot was a relational plot between SalesPrice and
TotalFinSF with GarageCars set as a hue (see figure below).

![Relational plot between SalesPrice and TotalFinSF attributes with hue set on GarageCars attribute shows SalePrice rises with TotalFinSF and GarageCars](./images/RelationalPlot.png "Pair Plot")

Finally, Excel Power Query was used to confirm these correlations. All data exploration showed that these attributes all had a strong positive correlation to
SalePrice, but none more so than GarageCars. You can see the all the steps of the exploratory data analysis in the HousePricePredictionProject.ipynb file in this repository.

### Model Building and Training
Since the target variable is a numeric value, regression models were used. The pipeline for predictive analysis used the steps in the diagram below:

![Diagram showing predictive analysis pipeline](./images/pipeline.png "Pipeline Diagram")

To determine the best model to use, a few different regressions were trained and evaluated. This study is similar to one demonstrated by Ken Jee in a video
series(see sources section) and was thus modeled after it. The processed training set was split into 80/20 train/test sets and the three models were developed:
– Linear Regression
– Lasso Regression
– Random Forest Regression

### Testing and Validation
Cross-validation was used on each model to prevent over-fitting. GridsearchCV was used to tune the models and the optimal alpha was determined for the Lasso model. This was done by generating 1000 possible alphas evenly spread apart, calculating the error for each, and finally, selecting the one with the best error.

Once each model was chosen, developed, and trained, it was then tested on the test dataset. The MAE was recorded on both the test and training data for each of the trained models to determine which was the best fit.

## Results and Analysis

![Picture of trained regression model results shows that the best model isthe Random Forest Regression model with an MAE of 23,736 on the training dataset and 29,418 on the test set.](./images/ModelResults.pmg "Results Table")

These results (see figure above) were a little lack-luster as the MAE is still relatively high on each of the models. However, the housing market is extremely volatile
and has a very wide range of results. Although taking out much of the data that did not apply to our study house helped narrow the scope to exactly what we
were looking for, subtracting the data may have made it less precise. On the other hand, a $30,000 margin of error is not unheard of in the housing market.
Each of the models did better on the training data than the test, which is to be expected. However, the Random Forest Regression performed significantly
higher than all the rest. As a result, the Random Forest Regression was the model used to answer the posed questions.

The study house has the following features:
– Lot Area: 23958
– Overall Cond:8
– Year Built:1962
– Year Remodeled: 2005
– Total Finished Sqft: 2322
– Bathrooms: 1.5
– Bedrooms: 4
– GarageCars: 2
– Year Sold: 2011
– Foundation: Poured Concrete

These features were put into the model and the Sale Price prediction came back at $208,490. The upgrades that the homeowners are looking at making will increase the following: Year Remodeled: 2011 (this is an old dataset, so 2011 would be the most recent year), Total Finish Sqft: 2822, Bathrooms: 2.5, and Bedrooms: 5. With the new parameters, the model predicts a Sale Price of $266,028. That is an increase in value of $57,538. Since the remodel will cost $20,000 even with a margin of error of nearly $30,000 the homeowner can be very confident that they will get their money out of the remodel should they choose to sell their home in the near future.

![Graph of prediction results shows a net gain of $37,538 from the updates made to home. Upper and lower error limits are the predicted value +/- MAE.](./images/HomeValueGraph.pmg "Predictions Bar Chart")

## Limitations

This, as any study would be, is solely dependent upon the accuracy of the data. Given current, localized data there are other measures that would be considered.
For example, although Urban and Suburban areas would have more similarity between houses in the same area, rural areas have much greater variation. For example, Hannibal, MO was built on hills. When the houses in the downtown area were originally built, the nicer houses were built at the tops of the hills, cheaper
ones were built on the bottom, and mid-range homes were built in between.  Although houses built more recently do not follow that same pattern, they have their own unique patterns. Even close neighborhoods can vary in value tremendously. These differences could be because they are in different school districts
or perhaps because one falls along a walking path between public housing and commercial areas. These are just a few of the examples of how measures would
have to be adjusted for a given area and thus what the limitations of this study are.

## Conclusion and Future Work

In conclusion, this study shows a model that forecasts 2-story, single family home values and how that value can change based on updates and renovations made
to the home. Homeowners can then use this model to predict the change in value of there home based on proposed updates and determine if those updates would
increase the value enough to warrant them. Although this model would be more accurate given local current data, the Ames dataset gives us a usable model that
can give us good ball-park estimates.

Future work on this study should include running this same analysis with data from the Hannibal, MO area as well as other rural areas. In place of the
Neighborhood feature, a numeric category feature for neighborhood value should be developed. That way neighborhoods in different locations but with similar
values could be included in the analysis. A feature for school district would also be helpful in Hannibal as it has a large bearing on home values. In addition,
adding the test set to the data, might give better insight. For this study, the author’s ability to split the data was showcased. Therefore, the ready-made test
set was not used. However, it would be beneficial for future study. Other future work would be to complete the API to give users an web interface with which
to input their own data into the model and receive Sale Price predictions.

## References

Cock, D.D.: Ames housing dataset, <https://www.kaggle.com/competitions/house-prices-advanced-regression-techniques>

GreekDataGuy: Data science project from scratch, <https://towardsdatascience.com/productionize-a-machine-learning-model-with-flask-and-heroku-8201260503d2> 

Jee, K.: Data science project from scratch, <https://github.com/PlayingNumbers/ds_salary_proj>

Sigur: Response to ’how to present a python code snippet efficiently in latex?’, <https://tex.stackexchange.com/questions/475826/how-to-present-a-python-code-snippet-efficiently-in-latex>

Speer, A.: Capstone pipeline (2023), <https://github.com/aspeer05/Capstone>

