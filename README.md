# Prediction and classification of video games 🎮

<p align="left">
    <img width="800" src="https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/blob/main/Notebooks/Images/video_games.jpg" alt="Video games">
</p>

# Project Overview
The online gaming industry experiences unpredictable changes in sales and performance metrics due to the continuous advent and improvement of video games. This project takes a deep dive into historical data on video game sales to:

- Understand what the gaming industry market in the last three decades has been like
- Identify key features influencing the performance of these games globally and regionally.
- Predict global sales based on relevant properties
- Provide a solution to the question: what game feature combinations will turn in high or low sales?

The project workflow below seeks to address these pain points.

# Workflow
- Data collection
- Data preparation
  - Missing data treatment
  - Feature engineering
- Exploratory data analysis
- Impact of features
- Prediction of global sales
- Classifier for sales category
- Model deployment and hosting



# Packages and tools used
Some tools and packages used in the course of this project include:

- Pandas and NumPy
- KNNImputer
- Seaborn, and matplotlib
- Scikit
- Joblib
- Streamlit


# Data
The data used here was obtained from [Kaggle](https://www.kaggle.com/datasets/ibriiee/video-games-sales-dataset-2022-updated-extra-feat/data). It contains information about video game sales worldwide, including factors such as critic and user reviews, genre, platform, and more. Note that sales is in millions.

| Column Name      | Description                                            |
|------------------|--------------------------------------------------------|
| Name             | The name of the video game.                            |
| Platform         | The platform on which the game was released.           |
| Year_of_Release  | The year in which the game was released.               |
| Genre            | The genre of the video game.                           |
| Publisher        | The company responsible for publishing the game.       |
| NA_Sales         | The sales of the game in North America.                |
| EU_Sales         | The sales of the game in Europe.                       |
| JP_Sales         | The sales of the game in Japan.                        |
| Other_Sales      | The sales of the game in other regions.                |
| Global_Sales     | The total sales of the game across the world.          |
| Critic_Score     | The average score given to the game by professional critics. |
| Critic_Count     | The number of critics who reviewed the game.           |
| User_Score       | The average score given to the game by users.          |
| User_Count       | The number of users who reviewed the game.             |
| Developer        | The company responsible for developing the game.       |
| Rating           | The rating of the game.                                |


P.S: All accompanying codes for the next steps are contained in the respective notebooks to be linked. This is done to ensure the brevity and conciseness of this readme. This proceeding steps below will contain the thought process for them, relevant results and codes if necessary.



# Data preparation
Following best practices, a copy of the dataset was made after importation and all analysis were carried out on that copy. Conducting an initial exploratory data analysis revealed:
- No duplicate rows
- Inappropriate data types for some columns
- Missing data as seen below

![image](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/assets/116895464/1206757d-6859-4763-9223-5e21f38b212d)

- Summary statistics for numerical columns
- Unique values for categorical columns

## Tackling missing data
Upon further analysis, three categories of missing data was observed. Each were treated differently.

1. Missing Completely at Random(MCAR)
Where the probability of a data point missing is entirely unrelated to any other observed/unobserved data. The name and genre columns fell into this category. Since the number of missing values for this were negligible, they were therefore dropped from the dataset

2. Categorical columns like Publisher, rating, etc
The NaN was replaced with "missing" to indicate unavailability of relevant data

3. Missing at Random(MAR)
This applied to the missing values in the numerical columns where missingness is not completely random but can be explained by some other known information. These rows cannot dropped as that will lead to gross information loss thereby impacting the efficiency of our model in the future. To handle this, I used a a multivariate approach - the KNNImputer with k=5 nearest neighbors which allows the imputer to find the 5 most similar rows in the dataset and make imputations for each.

After proper handling of all the cases aforementioned, we have this:

![image](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/assets/116895464/31dd3732-5ab7-4ab5-ab1d-49c2a4c0f5bd)

## Feature engineering

This involved creating new features based on already available information. I created a new feature called release_era that groups the release year of games into three eras - pre-2000s, 2000-2010, and post-2010. This was created to enable me perform some group level analysis during the EDA process.


# Exploratory Data Analysis
The notebook for a detailed breakdown of the EDA process including univariate and bivariate analysis can be found [here](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/blob/main/Notebooks/Exploratory%20Data%20Analysis.ipynb). Here, I present some questions I answered and insights revealed from this stage in my analysis.

1. What have the sales through the years been like regionally?

![image](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/assets/116895464/54f3c83f-2587-487f-af46-90634b573321)

**INSIGHT**

Generally, sales spiked for all regions in 1995. In North America, there was a wave of fluctuations from 1980 till 1995 when it picked up and rose steadily with a few dips here and there. This reached peak sales of 350 million copies. This was not sustained though, the sales began dwindling through the years till 2016. The sales in all regions were low compared to a decade before that. Sales in Europe and Japan follow a similar pattern. For sales in "other" regions, we see a relatively steady growth from less than one million sales to its highest point around 70 million and then a decline.


2.  Is there a correlation between critic scores and user scores? Do they tend to agree or disagree?

![image](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/assets/116895464/390aeebb-a115-45c3-805f-c72bfd08dbe6)

**INSIGHT**

Based on the correlation plot and a heatmap created during the [analysis](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/blob/main/Notebooks/Exploratory%20Data%20Analysis.ipynb), there is a moderate positive linear association (0.5) between the user score and the critic score. What does this mean?

This means that there is a noticeable trend between the critic scores and the user scores, even though it is not a perfect relationship. It also tells us that professional critics and users tend to agree to some extent on their assessment of video games. Therefore, stakeholders need to consider both critic and user scores when making decisions about game development and marketing. 


Want to see more? Check out the complete EDA process [here](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/blob/main/Notebooks/Exploratory%20Data%20Analysis.ipynb).


# Impact of features on regional sales
The next stage of my analysis sought to answer the question: What effect do the number of critics and users and their review scores have on the sales of video games in North America, Europe,  Japan, and "Other" regions?

Find the notebook for this section [here](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/blob/main/Notebooks/Impact%20of%20features.ipynb).

For this, I conducted an initial examination to find out if the relationship between the variables was linear.

![image](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/assets/116895464/8659a0e1-8de3-404c-99eb-bdaa4e658671)

There was no discernable linear relationship between the variables. This was backed further by the results of the R2 when I used multiple linear regression. After performing transformations to the data, the relationships between the variables provided no hint of linearity. To answer this question, I therefore used a non-linear model - RandomForest - to determine feature importance of the variables in the regional sales. The results can be seen below.

For North America:

![image](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/assets/116895464/38a4ed6e-bc8d-446a-909b-2f9384137bb2)


Europe:

![image](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/assets/116895464/4be95af6-8d88-44df-8a8b-fefbcfd9a127)



Japan:

![image](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/assets/116895464/44627496-3872-4d15-8553-2c1514b620e4)


"Other" regions


![image](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/assets/116895464/2fa63b4e-7481-494e-a7d9-29f73be98d10)

**INSIGHT**

In all the regions, we see that the number of users accounts for a greater percentage of the effect on the sales of video games. This indicates the popularity of games amongst individuals where the more people use the game, the higher the sales in the region can increase. 

Second to this is the number of professional critics who review the games. The least contributing factors are the user and critic scores. Though the user and critic score is an important metric to track during and after game development, it is however more important that the game becomes a favorite among the audience while keeping an eye on the user score which provides an understanding of the overall user satisfaction or impression of the game on the audience and critics.


# Prediction of global sales

The focus here was to create a model that can predict the global sales. The step-by-step process can be found [here](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/blob/main/Notebooks/Predicting%20global%20sales.ipynb).

The steps here included:
- Removal of outliers
- Feature selection: see the selected columns below

![image](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/assets/116895464/58932152-7e54-4d59-a192-eab67220c836)


- One-hot encoding using pd.dummies to transform categorical data into a set of binary columns
- Data splitting
- Data rescaling on the train and test data differently to prevent data leakage
- Defining the models/algorithms

```Python
##define models

dt_model = tree.DecisionTreeRegressor(max_depth=200)
svr_model = svm.SVR(kernel='rbf')
rf_model = RandomForestRegressor(n_estimators=100, random_state=42)
bayes_model = BayesianRidge(compute_score=True)
lass_model = Lasso(alpha=0.1)
xgb_model = XGBRegressor(n_estimators=100, learning_rate=0.1, random_state=42)
linear_model = LinearRegression()

```

- Training the algorithm
- Tests
- Model evaluation and conclusion
The model was evaluated based on the R2 and RSME as seen below:

![image](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/assets/116895464/1113ae70-db4c-4dd4-8b86-5e7175af70b4)

**INSIGHT**

The Support Vector regression model is the best-performing model in this comparison, with the lowest RMSE and the highest R² value. It best explains the variability in global sales. The RF and XGB models also perform well, with relatively low RMSE and decent R² values. The DT model however performs the worst as indicated by its high RMSE and negative R² values.


# Classifier: what game feature combinations will turn in high or low sales?

This classifier predicts whether a video game will have high (>= 1 million), or low (< 1 million) global sales based on the inputs given by the user such as:

- Platform: The platform on which the game will be released (e.g., PS2, X360, PS3, Wii, etc.).
- Year of Release: The year in which the game will be released.
- Genre: The genre of the video game (e.g., Action, Adventure, Sports, etc.).
- Publisher: The company responsible for publishing the game.
- Developer: The company responsible for developing the game.
- Critic Score and counts: The average score given and the number of professional critics who review the games.
- User Score and counts: The average score given and the number of users.
- Rating: The rating of the game (e.g., E, T, M, etc.).

This classification model is useful to simulate what a game with certain features/characteristics would sell for, that is if it would have reasonable sales globally. 

This [notebook](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/blob/main/Notebooks/Classifier%20for%20Game%20sales.ipynb) contains details on building the classifier model. The steps here included:

- Feature engineering: creating the `global_sales_cat` binary feature indicating high or low sales
- Feature selection as seen below:

![image](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/assets/116895464/4e1fb515-6b58-40aa-817c-67f3bccec073)

- One-hot encoding using pd.dummies to transform categorical data into a set of binary columns
- Data splitting
- Data rescaling on the train and test data differently to prevent data leakage
- Training the algorithm and handling data imbalance using `balanced weights`
- Tests
- Model evaluation, conclusion and saving the best model for deployment
The model was evaluated based on the train score, test score, average cv score, and standard deviation of the cv scores as seen below:

![image](https://github.com/HannahIgboke/Prediction-and-classification-of-video-games/assets/116895464/ea778aac-1b85-4a34-8c6c-1210f9fe1a3b)


**Choosing the best model to use**

The significant difference in the training and testing accuracy as can be observed for most models indicate cases of overfit. 

The LR model has similar training and testing accuracy with an average cross-validation score of 75% and a low standard deviation (0.14) indicating consistent and reliable performance across different data splits. **LR is therefore the best-performing model.**


# Model deployment and hosting




# Conclusion




# Future work 




# License







