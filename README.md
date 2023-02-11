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

After tuning the process, the best values of the hyperparameters were obtained, as shown on the slide. With these parameters it was possible to achieve a test metric of 1.0909.
![Слайд8](https://user-images.githubusercontent.com/33491221/218243534-2b8a12d7-e421-4d39-9c2f-8135e8b981db.JPG)

The current slide shows the error estimates of the obtained predictions for each of the geographic points presented in the test dataset.
![Слайд9](https://user-images.githubusercontent.com/33491221/218243628-2573f6d9-e141-4248-8f81-d18798448bb5.JPG)

Next on the slide is an example for predictions in a specific location, namely the Lakhta Center area. On the graph comparing the test and predicted data you can see that LSTM manages to catch the periodic pattern of time series.
![Слайд10](https://user-images.githubusercontent.com/33491221/218243678-9b64e73b-55bd-4e36-b66b-3d8a78a000d1.JPG)

Here is another example in which the model was able to capture the pattern of time series for a geographic point near a subway station and a large shopping mall. 
![Слайд11](https://user-images.githubusercontent.com/33491221/218243782-4dfe6f22-caa5-46e4-b962-752044896783.JPG)

The following is a heat map of predictions by geographic points with target values as weights.
![Слайд12](https://user-images.githubusercontent.com/33491221/218243853-b3a9dc80-866f-47aa-b46a-0b427aa40a2c.JPG)

In this example, you can see by the color that the highlighted point has atypically many publications on the current day, even though it is located quite far from the center. As it turned out, there was a concert of a music band at the Ice Palace on that day.
![Слайд13](https://user-images.githubusercontent.com/33491221/218243907-056562fa-9398-4230-8403-b06c2d1458ec.JPG)

Here we also see high activity at two points away from the center. In the first case, this can be explained by the opening of several exhibitions in the gallery. In the second case, activity was probably taking place at the ice rink near the bay.
![Слайд14](https://user-images.githubusercontent.com/33491221/218244075-f6f291a6-0a1e-479a-8a0a-2fd504c9babf.JPG)

Finally, the slide shows individual predictions versus target values. It can be seen that the continuous predictions are fairly close to the discrete target points, indicating the quality of the developed system.
![Слайд15](https://user-images.githubusercontent.com/33491221/218244169-7570e147-8485-4694-8515-0b642e03d517.JPG)
