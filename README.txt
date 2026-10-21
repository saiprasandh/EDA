EDA ASSIGNMENT 2
House Price Analysis and Feature Exploration
Datasets: Ames Housing, California Housing, House Sales in King County
=====================================================================

FILES
-----
EDA_Assignment_2.ipynb     Full notebook: load -> clean -> EDA/visualise -> model -> feature importance
EDA_Assignment_2_Report.pdf      Written report (method, charts, results, conclusions)
EDA_Assignment_2_Presentation.pptx   Slide deck summarising the work
original data ,cleaned data.xlsx   One workbook: Summary + original & cleaned sheet for each dataset
model_comparison.csv             Linear / Ridge / Random Forest / Gradient Boosting results for each dataset
feature_importance.csv           Permutation importance of every feature (best model per dataset)
ames_original.csv, ames_cleaned.csv
king_county_original.csv, king_county_cleaned.csv
california_original.csv, california_cleaned.csv      The same data as the workbook, as plain CSV
charts/                          11 PNG charts (matplotlib + seaborn); visualize.py regenerates them

DATASETS
--------
Ames Housing        2,930 sales, 82 columns (Ames Assessor's Office, 2006-2010). Target: SalePrice.
King County         21,613 sales, 21 columns (May 2014 - May 2015). Target: price.
California Housing  20,640 census block groups, 10 columns (1990 census). Target: median_house_value.

CLEANING SUMMARY
----------------
Ames        2,930 -> 2,925 rows, 82 -> 92 cols. 'NA' in categorical features means "feature absent" -> 'None';
            numeric basement/garage gaps -> 0; Lot Frontage = neighbourhood median (+flag); Garage Yr Blt fixed;
            5 homes above 4,000 sq ft removed (author's recommendation); engineered: house_age, total_sf,
            total_bath, has_* flags, log_saleprice.
King County 21,613 -> 21,603 rows, 21 -> 30 cols. Date parsed, zipcode as text, 33-bedroom typo fixed to 3,
            10 rows with 0 bathrooms removed; engineered: house_age, renovated, price_per_sqft, repeat_sale, log_price...
California  20,640 -> 20,561 rows, 10 -> 18 cols. total_bedrooms imputed from median bedroom/room ratio (+flag);
            capped values flagged (price/age/income); 79 implausible blocks removed; engineered ratio features.

MODELLING
---------
Target = log(price); 80/20 train/test split (random_state=42); 3-fold CV on the training set.
Metrics are reported on the original dollar scale (RMSE, MAE, R2).
Leakage guarded: price-derived columns (price_per_sqft, price_capped, log target) are excluded from predictors.
Feature importance = permutation importance (3 repeats) on the test set for the best model.

HOW TO RUN
----------
  pip install pandas numpy matplotlib seaborn scikit-learn jupyter
  Keep the *_original.csv files next to the notebook, then:  jupyter notebook EDA_Assignment_2.ipynb

DATA SOURCES
------------
Ames:       De Cock (2011), Journal of Statistics Education 19(3); copy used from the Raschka ML-book repository.
King County: Kaggle "House Sales in King County, USA".
California: 1990 US Census (Pace & Barry 1997), version from Geron, "Hands-On Machine Learning" repository.
