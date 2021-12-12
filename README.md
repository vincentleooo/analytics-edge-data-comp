## The Analytics Edge Fall 2021 Data Competition

Competition code for The Analytics Edge module in SUTD. Data can be found from [Kaggle](https://www.kaggle.com/c/2021tae/leaderboard).

### Data

Two columns: `tweet` and `sentiment`. `sentiment` is classified into negative, neutral, and positive with 1, 2, and 3 respectively. The objective is to classify the `sentiment` based on the `tweet` contents. FastText word embedding was used.

### Initial model

Doing a proof-of-concept of using Bidirectional LSTM to predict the sentiment classes, we achieved a total accuracy score of 90% over 25 epochs.

Todo:

- [ ] Add points on data preprocessing.

- [ ] Port the code over to R.

- [ ] Test out classical models, CNN, and BERT Transformer.


#### Loss

Using Cross Entropy Loss with the Adam optimiser at the learning rate of 0.001, the graph for the losses are below.

![](img/python/tae_lstm/loss.png)

#### Confusion matrix

![](img/python/tae_lstm/confusion.png)

#### Classification score

```

              precision    recall  f1-score   support

           1       0.85      0.90      0.88      2036
           2       0.95      0.89      0.92      2390
           3       0.90      0.90      0.90      2324

    accuracy                           0.90      6750
   macro avg       0.90      0.90      0.90      6750
weighted avg       0.90      0.90      0.90      6750

```

### Some resources not included

- Word embeddings file for FastText (`wiki-news-300d-1M.vec`) as it is greater than the 2 GB allowed in Git LFS.
