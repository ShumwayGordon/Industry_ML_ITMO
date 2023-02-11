# Industry_ML_ITMO
Works related to Machine Learning for Industrial Data


## Task 1
### Introduction
Social media occupies a very important place in modern human life and has a strong influence on daily life. A huge flow of information related to a person's thoughts, experiences, emotions, and the world that surrounds him/her is transmitted through them. Exploring social media streams allows you to observe processes taking place in the city, to predict emerging anomalies and events, allowing you to react to them in a timely manner. In this assignment you will learn how to use historical social network data about the frequency of publications in different urban areas to predict future distribution.

### Assignment 
You are presented with a dataset of one popular social network that includes more than 8.5 million records with meta-information of publications over 13 months (January 2019 to February 2020).
Each publication is described by the following meta-information:
- lon, lat – geoposition coordinates rounded up to a 250x250 meter polygon (geographical longitude and latitude, respectively)
- timestamp – timestamp of the publication accurate to one hour
- likescount – number of "likes" in the publication
- commentscount – number of comments of the publication
- symbols_cnt – number of all symbols in the publication
- words_cnt – number of words (meaningful, not counting special characters and other meta-information)
- hashtags_cnt – number of hashtags 
- mentions_cnt – the number of mentions of other users
- links_cnt – number of links
- emoji_cnt – number of emoji.
Using this data, you will need to predict the number of publications in each 250x250 meter polygon for each hour 4 weeks (28 days) ahead of the last publication in the training set.

### Solution

First of all, an exploratory analysis of the data was conducted
![Слайд2](https://user-images.githubusercontent.com/33491221/218242808-6525325e-e3b2-4b43-bbfc-3f0e27181504.JPG)

Right after that the first approach was tested with several features and the CatboostRegressor model.
![Слайд3](https://user-images.githubusercontent.com/33491221/218242891-eeece7c9-fe7c-49cf-a69c-3bbe53b65f11.JPG)

Although the metrics were good, there was a significant difference in the distributions of the original and predicted data.
![Слайд4](https://user-images.githubusercontent.com/33491221/218242958-66795717-a58a-4f48-8250-1a5559dea079.JPG)

Therefore, it was further decided to subject the data to more processing.
![Слайд5](https://user-images.githubusercontent.com/33491221/218243062-5d22af87-23b3-477b-b170-bde853a0b6be.JPG)

After working with the data, we worked through the general solution pipelines on the slide. The diagram shows that after preprocessing, the training data from each geographic sector are fed to the input of the corresponding LSTM network. After receiving the predictions of each of the LSTMs, we calculate the metrics achieved.
![Слайд6](https://user-images.githubusercontent.com/33491221/218243124-c2559ce1-0458-45e7-a25d-34ab05e4ce8d.JPG)

Let's look at the individual LSTM used in more detail. The layers of the keras Sequential model are LSTMs with a given number of neurons and the Dense layer. The prediction horizon, number of train epochs and number of LSTM layer neurons are set as tuned parameters. This simple structure allows to obtain sufficiently good predictions without taking up much RAM space.
![Слайд7](https://user-images.githubusercontent.com/33491221/218243341-ee6298f3-a485-424b-8bdd-112e813bd90f.JPG)





