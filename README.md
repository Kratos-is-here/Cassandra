# Cassandra

## Ultimately, the task was to build a model that helps estimating when an invoice will be paid. The estimation doesn't have to be an exact date and time, a rough estimation in terms of days is sufficient for this task. You need to predict the Number of Days until Payment using the Dataset provided to you.

- Description : The textual description entered by the user during recording the invoice on the accounting system (string).
- Vendor Name : The name of the vendor/supplier who provided the goods or services (string).
- Created : The date and time of entering the invoice details on the accounting system (datetime).
- Invoice Date : The date and time of the invoice. It represents when the goods or services have been delivered to Traxes (datetime).
- Due Date : The date and time when the invoice is due to be paid by Traxes (datetime).
- Amount : The cost of the goods/service provided by vendors and due to be paid by Traxes (float).
- Settled : The amount that has been paid by Traxes to vendors on the payment date (float).
- Outstanding : The unpaid part of the invoice in which Traxes is required to pay (float)
- Number of Days until Payment : count of days after Invoice Date after which payment was made.

Our goal was to predict ##Number of Days until Payment## feature by training a Machine Learning model on the Data given.

## Approach used:
### Data Preprocessing:
1. Replace Nan Values with Empty Strings in the Description
2. Text Preprocessing on Description Text: list of stop list of 25 semantically non-selective words were taken from list used by Stanford NLP Group which are common in Reuters-RCV1.
3. Count Vectorizer to numerically encode text features: It is great tool which is used to transform a given text into a vector on the basis of the frequency (count) of each word that occurs in the entire text
4. Converted Dates into Date-Time Format: We noticed that dataset had various columns which corresponded to dates like Created, Invoice date etc. But these columns were in string format so we split them into day, date, weekday, month and year to improve inference.

### Feature Engineering:
1. Due_Invoice_delta - difference between the due date and invoice date
2. As the “Outstanding” column was mostly zero, we made a new feature called “Outstanding_zero” which was 1 if the column was zero and 0 otherwise.
3. We had three continuous numerical columns : [“Outstanding:, “Amount”, “Settled”]. We took ratios of these three columns to create three new features.
4. To utilize “Vendor_Name” features, we took mean, median, minimum, maximum, std-dev and count of numerical columns for each unique Vendor_Name and made these new features. These new features would help model learn about properties of vendors and how they affect the target.

### Training & Validation
1. CatBoost is a high-performance algorithm for gradient boosting on decision trees.
2. Categorical features supported without any preprocessing
3. Reduces overfitting when constructing your models with a novel gradient-boosting scheme.
4. We used 5-Fold Cross Validation on our model to evaluate our model.

### Other Approachers we tried:
1. Classical Neural Network with one hot encoding for the categorical features.
2. Decision Trees
3. StackingRegressor by stacking DecisionTree, XGB, CatBoost and RandomForest but CatBoost alone outperformed them.


[Presentation slides explaining our approach](https://www.canva.com/design/DAE9kvzlI1c/FKcPR9OgjUXJzkgQlmdnLg/view?utm_content=DAE9kvzlI1c&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)

[Competition Link](https://www.kaggle.com/competitions/cassandra-udyam-2022/leaderboard)
