# AlgoTrading_NLP_based_portfolios
This is a project scheme aiming at constructing a portfolio using machine learning approaches for making dynamic investment decision. Serval perspectives and dimension are used as predictors based on both financial, economic and statistical concept to build a comprehensive model. These includes but not limited to market data and technical indicators, company's financial fundamentals, regional economic status and its expectation, and effect on financial news. 

A series of mini projects are performed for testing some hypothesis and thought. Important finding will be released and updated regularly. 


# Directory:
1. Basic Natural Language Processing on News Title to classify Stock Trend
2. First attempt: Forecast the stock signal by NLP-based features (you are here!)


## 2. First attempt: Forecast the stock signal by NLP-based features
#### Main question: 
Based on pervious project, we shows the existance of relation between financial news and the market. Can we further use financial news to 
predict intraday stock price?

#### Date: 
13 Dec 2020 (HKT/ GMT +8)

#### Trading Strategy
A dymanic trading strategy that adjust the weighting every 15 mintues within the market opening period. 

#### Detailed description: 
This mini project is to perform a multiclass classification task to the sp500 movement based on financial news. We restructure the dataset to fit our dynamic trading strategy, weights to be adjusted every 15 minutes within the trading period, by combining news released in the pass 1 hour as the predictors and the log return of the coming 15 minutes as the response variable. 3 classes are formed as BUY, HOLD and SELL signals and observations are distinguished by either larger or equal to 0.05, between -0.05 to 0.05, and less than 0.05. i.e. Using news released between 10:00 to 10:59 to predict the log return class of sp500 between 11:00 to 11:15. We employ simple n-grams td-idf statistics and doc2vec algorithm to convert news into numeric factors separately after performing special preprocessing on Natural Language including case lowering, punctation and non-alphabetic symbol removal, 
stop word removal, tokenization, and word lemmatization. Dimensionality of model employed n-grams td-idf is reduced by limiting the acceptance range of these td-idf, the exact acceptance range vary from models, and we treat it as a hyperparameter to turn. Text extracted from the training set is used as the corpus to train the doc2vec 
model and convert all sentence to a 50-dimensional numeric representation. LightGBM is then employed for forecasting the sp500 trading signal. In trading signal investing, we are much more interested in the BUY and SELL signal but NOT the HOLD one. Therefore, a average of f1-score of BUY signal and that of SELL signal is used to evaluate the model performance on validation set. Lastly, 2 Ridge Regressions using td-idf and doc2vec vectors as features are performed as a benchmark.

#### Finding and achievement: 
Unfortunately, the 1-gram td-idf model, 2-grams td-idf model and doc2vec models results in no more than 0.24 evaluation score (please refer to Detailed description), where 2 ridge regression results in no more than 0.22 evaluation score. One possible explanation is that we have too much assumption on this project, under the low signal high noise environment, we are not able to determine the bottlenecks of the end-to-end model. In the future, we may need to split our task into parts and detect the issue.

#### Possible improvement: 
1. Data Quality: The original pricing dataset has ~34% observations are missing. A better dataset would be used.
2. Feature engineering: 
  a. For each 15 minutes log return, 1 hour news is used to predict the response, more papers and researches are required to support this assumption.
  b. News released overnight is accumulated to the first observations within the day, this result in a much longer sentence. A better way to handle this problem are required.
  c. The significance of news is expected to be different among news. A better approach combining them with different weights maybe useful. 
  d. Doc2Vec model construct the Corpus by training set data only. A pretrained generalized model maybe used instead.
  e. Only 50 dimensional vectors are output in doc2vec, a larger and smaller dimension should also be tested for checking the suitability
3. Classes & Evaluation metric: Although the classes and Evaluation metric used are more aligned with the investment strategy, it may further confuse the model. In addition, the score we used dosen't have direct interpretation! 
4. A "training-validation-testing" approach is used, and the testing performance is a point estimator instead. One should apply walking forward or even sequential bootstrapped walking forward to obtain a more accurate testing performance.
3. Security pricing is indeed a sequential data. One can apply sequential model such as RNN/ LSTM/ transformer model which capture the delay effect on news. 

#### Data: 
News: "Financial News Dataset from Bloomberg and Reuters", This dataset was compiled and first used in Ding et al. (2014).

[Ding et al., 2014] Xiao Ding, Yue Zhang, Ting Liu, and Junwen Duan. Using structured events to predict stock price movement: An empirical investigation. In Proc. of EMNLP, pages 1415–1425, Doha, Qatar, October 2014. Association for Computational Linguistics.

Price: Yahoo finance for sp500

#### License: 
those datasets are no longer available online for research purposes (NLP...) due to copyright issues.

#### Last Update: 
22 Jul 2021

