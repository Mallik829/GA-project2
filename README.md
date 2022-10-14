# Ames Iowa Real Estate Prediction

## Problem Statement

My goal in this project is to predict real estate prices in Ames, Iowa using various regression models. Being able to predict housing prices has a wide variety of uses, to both real estate agents, building developers and potential buyers. Most important is what to look for in a house that adds value to both buyers and sellers. This can be used for those looking for a new home to determine what they want in a house and how much they should spend, by real estate agents to determine what features to focus on when selling, or by home developers to determine what to build, how much money to spend and why. By testing different models on different combinations of features from Ames real estate data, I will be able to draw conclusions on what adds or removes value to a property.

## Data Structure
The original data for this project came in the form of two csvs: test.csv and train.csv. These were cleaned and feature engineered to produce several versions and saved in the datasets folder. All the code I used is saved in the code folder and seperated into notebooks as shown below. Useful visualizations were saved in the figures folder. Final submissions by model were saved in the submissions folder

## Data Dictionary
Below is a dicitonary of the columns I used in my final data, after cleaning, feature engineering, and feature elimination. The full data dictionary can be found at [The Ames Housing Kaggle Competition](https://www.kaggle.com/competitions/stat101ahouseprice)

| feature | dype |Description
| --- | --- | --- |
| lot_frontage | float64 |Linear feet of street connected to property
| overall_qual | int64 |Overall material and finish quality
| bsmtfin_sf_1 | float64 |Type 1 finished basement square feet
| total_bsmt_sf | float64 |Total square feet of basement area
| 1st_flr_sf | int64 |First Floor square feet
| gr_liv_area | int64 |Above grade (ground) living area square feet
| full_bath | int64 |Full bathrooms above grade
| totrms_abvgrd | int64 |Total rooms above grade (does not include bathrooms)
| fireplaces | int64 |Number of fireplaces
| garage_area | float64 |Size of garage in square feet
| porch_sf | int64 |Total Square Feet of Porch-like areas
| house_remod_sold | int64 |Years since the last remodel when the house was purchased
| house_age_sold | int64 |House age when sold
| sale_price | int64 |The property's sale price in dollars
| 3 kitchen_qual features | int64 |3 One hot encoded features showing kitchen quality
| 25 neighborhood features | int64 |25 One hot encoded features showing neighborhood

## Work Flow

### Exploratory Data Analysis and Data Cleaning
#### EDA_and_Cleaning01.ipynb

I begin the process with visualizing the shape of the original data and dealing with missing values. First, columns with over 1/3 of their values missing were removed. The lot_frontage missing values were imputed with the mean lot_frontage for the neighborhood of the property. Certain features like mas_vnr_area which were mostly 0s were removed from the data. Rows with 6 or more missing values were also removed. Some features such as garage_area were filled missing values with 0s, while remaining columns with missing data were removed. These decisions to remove were based on my interpretation of their importance via the data dictionary, and the amount that would have to be assumed to impute the missing values. I attempted to minimize these assumptions. Outliers for several remaining numeric columns were observed. It was found that these outliers belonged to the same approximately 500 properties, which had a higher distribution of salesprices compared to the overall distribution. Since large properties were expected in our Test data, these were kept in.

### Feature Engineering and Feature Elimination
#### Preprocessing_and Feature_Engineering02.ipynb

First in this process, three new features were created. porch_sf was made by combining the square feet of porch-like structures from wood_deck_sf, open_porch_sf, and screen_porch. House_age_sold, representing the age of the house when sold, was made by subtracting year_built from yr_sold. Likewise, house_remod_sold was made similarly using the latest remodel date.
Next, I took a close look at the correlations between numerical features and saleprice. features with low correlation were considered for removal, while those with high correlation were kept in the data. Several categorical features were visualized to determine if they should be kept and One hot encoded. Ultimately, neighborhood and kitchen_qual seemed most impactful and were One hot encoded. The rest were removed. 

### Test Data Preparation
#### Test_Data _Preparation03.ipynb

In This step, I repeated all steps from notebooks 01 and 02 on the test data to ensure that both have the same features.

### Preliminary Modelling and Validation
#### Scaling_Modelling_and_Validation04.ipynb

In this step I began to test regression models. First, the RMSE of the mean model was determined to serve as a baseline for further RMSE scores. Linear regression was fit and cross-validated. The coefficients of the features from this process were observed and analyzed. This was compared to its performance with OHE features removed and it was determined that these features improved performance.
Next, the data was standard scaled for Lasso and Ridge regression. Both of these were tuned for the alpha hyperparamer and cross validated on the training data.
It was determined in this notebook that linear regression had the best R2 score and lowest RMSE.

### Final Predictions
#### Final_Predictions05.ipynb

In this notebook, the models that were tested and validated earlier were used to predict saleprice on the test data. These were written to csv for final submission.

## Observations

Unsurprisingly, the neighborhood the house was being sold in was a large factor in the sale price. This feature was one hot encoded and was useful in my final predictions. Kitchen quality was another feature which represented a large amount of variance in saleprice. The overall material and finish was correlated highly with sale price and was an important feature in my modeling. Having larger outdoor structures such as porches and decks also appeared to be a desirable characteristic. The total rooms and the square footage of the first floor was also highly correlated with price, but second floor area was less so. Sale price incread noticably with the number of bathrooms. Fireplaces were desireable, as well as large garage and basement areas. Finally, the age of the house when sold and the time since the last remodel were both important factors

Of the models I used, Linear Regression performed the best and resulted in my lowest RMSE predictions.
