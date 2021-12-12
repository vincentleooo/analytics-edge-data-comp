## The Analytics Edge Fall 2021 Data Competition

Competition code for The Analytics Edge module in SUTD. Data can be found from [Kaggle](https://www.kaggle.com/c/2021tae/leaderboard).

### For code submission on eDimension

We did not include the actual model that was produced by the code due to its size (~1.4 GB). The model can be found on our GitHub repository (requires Git LFS to be installed to clone the big model) or on [Google Drive](https://drive.google.com/drive/folders/1WQumb-2VD9DelqRvd5IA45li4ufyp6zT?usp=sharing) (availability not guaranteed long-term).

### Overview

Our data consisted of observations gathered by citizens on the social media platform Twitter. The task was to develop an approach that determines, with the highest accuracy, the kind of weather a given set of tweets is referring to. Specifically, the task was to determine the sentiments of tweets.


### Approach

The approach taken was to stand on the shoulders of giants by using transfer learning from a pre-trained model. Various advances in the field of Natural Language Processing have yielded highly effective pre-trained models that were built on the idea of handling text as sequential data. While earlier models use Recurrent Neural Networks (RNN), one notable example being the Long Short-Term Memory (LSTM), it was eventually superseded by various Transformer-based models.

For our case, the pre-trained model selected was Facebook’s RoBERTa, which had improved upon Google’s BERT language model. In a nutshell, the corpus was separated into training and validation sets. RoBERTa has its own tokeniser, and this was used to tokenise the data. These tokenised data were then both truncated and padded such that all the rows were of the same length.

The model itself was fine-tuned to the task at hand by, firstly, passing the input data into the pre-trained model. The mean of its output was then passed through two fully-connected layers (128-dimensional and 64-dimensional respectively) with ReLU activation. The output layer of the model was three-dimensional with the Softmax activation. The dimension with the highest probability denotes the sentiment class. To compute loss, the categorical cross-entropy loss was used along with the Adam optimiser for the gradient-based optimisation. The model was run for three epochs, since the validation loss increased very slightly at around the third epoch, in general.

Due to the stochastic nature of the neural network model, the exact model, and hence, the resulting predictions will differ slightly with different runs. As such, the final model used is freezed and can be reloaded to produce the same predictions as the one submitted on Kaggle. 

### Dependencies

A Python (3.6+) environment is required, preferably with Torch, Keras, and Transformers installed already. If not, the R script should download it after setting Python up with RStudio. The default method is to have this Python virtual environment ready and connect it to RStudio in its options. If this fails, below is a comprehensive alternative guide using Conda to get it up and running.

1. Install Conda. Preferably, a light install using Miniconda is preferred.
2. Using the terminal (on macOS/Linux/other UNIX systems) or the Anaconda Prompt (on Windows), create a new Conda virtual environment with the command: `conda create --name {{environment_name}} python=3.9`. The command is without the quotation mark, with `{{environment_name}}` being the chosen environment name. For example, `conda create --name data_competition python=3.9`.
3. Take note of the environment path. This can be found using `conda env list`.
4. Open RStudio.
5. Using the console, install the `usethis` package using `install.packages('usethis')`.
6. Again, in the console, use the command `usethis` to create a new window called `.Renviron` which is like this: `usethis::edit_r_environ()`.
7. Edit the `.Renviron` file by adding the line `RETICULATE_PYTHON="{{environment_path}}"`. For example, `RETICULATE_PYTHON="C:\\Users\\vinle\\miniconda3\\envs\\data_competition"`
8. To note: For Windows-styled path names, the symbol `\` is an escape character. That should be replaced with `\\` instead. If there is any whitespace within that path (e.g. `“C:\\Users\\Vincent Leonardo”`), it needs to be replaced with `\` (e.g. `“C:\\User\\Vincent\ Leonardo”`).
9. Save the `.Renviron` file and restart the R session. The installation process within the first two R blocks of the file should work now.

The scripts were validated on the x86 architecture on R 4.1.2 and Python 3.9.7. The code optimises for the use of a GPU (the script to be commented out when using only CPU will be denoted in the script). It is not validated within the ARM architecture (e.g. Apple Silicon, Surface Pro X), and using it will require that care be taken to ensure both the R installation and Python installation are of the same architecture.

### R Packages Used

- `usethis` (if needed)
- `reticulate`
- `keras`
- `tensorflow`
- `dplyr`
- `tfdatasets`

### Results

The model was able to predict the sentiments of the tweets to an accuracy of around 97.79% on the training set, 96.02% on the validation set, and 96.266% in the Kaggle data competition’s public leaderboard. As mentioned above, due to the stochastic nature of the neural network, we can expect to see some slight differences in accuracy with each run of the script. 

### Interpretability and Limitations

Although the model was able to predict the sentiments to a respectable degree of accuracy, our understanding of exactly how RoBERTa works, at this point, is limited. The neural network made the modelling process at times akin to a black box algorithm, where debugging and manual tuning were more challenging. 

As for the limitations of the model, one possible limitation of using the pre-trained RoBERTa model is that the model is pre-trained on unfiltered content from the internet, where neutrality is not guaranteed. This could result in the possibility of the model having more biased predictions compared to a model pre-trained on data from another source. 

