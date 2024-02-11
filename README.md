# COMPARATIVE STUDY OF Hybrid RECOMMENDATION SYSTEMS

The rapidly developing e-commerce sector has increased the demand for effective recommendation systems to provide clients with personalized and accurate product recommendations. These systems are critical in improving the customer experience, increasing sales, and cultivating customer loyalty. While traditional recommendation systems depend solely on star ratings for evaluating customer satisfaction, recent research has demonstrated the value of using sentiment analysis to capture the nuances and emotions represented in textual evaluations. This dataset, sourced from the Amazon website, comprises user reviews, star ratings, and anonymized product information. It is the foundational resource for our comparative analysis of various hybrid recommendation systems.

This project aims to contribute to the area by comparing hybrid recommendation systems for e-commerce, emphasising integrating sentiment analysis and star ratings using the weighted hybrid approach. Other objectives of this project include:

- Performing sentiment analysis on customer reviews.
- Analyze the direction and strength of the correlation to assess the alignment between
sentiment and star ratings
- Identifying keywords used in customer reviews and exploring the relationship between keywords and customer behaviours.

### Data Preparation

Given the computational needs of deep learning models, especially the BERT model used in this project, the initial steps were performed on Google Colab, a cloud-based platform that offers GPU runtime capabilities. Utilizing the Python pandas library, the data was imported from a CSV file stored on Google Drive. The dataset presents an array of attributes, with the key columns being 'overall' (representing star ratings), 'reviewerID', 'asin' (product ID), and 'reviewText' (the textual content of the review). A snapshot inspection of the dataset was carried out to gain an initial understanding of its structure and the nature of the stored data.


Text preprocessing plays a crucial role in enhancing the quality and consistency of data. To achieve this, several measures were implemented in the review texts. Firstly, the review texts were converted to lowercase, ensuring uniformity and eliminating potential discrepancies arising from variant text cases. Subsequently, any non-alphabetic characters were purged from the text, which allows for more precise tokenization and sentiment analysis. The tokenization process then came into play, where the text was systematically divided into individual words or tokens, a fundamental procedure in the Natural Language Processing (NLP) continuum as it segments the text into analyzable fragments. Once tokenized, lemmatization was executed on each word, converting them to their base or root forms. This action ensures that diverse word forms with an identical root meaning, exemplified by words like 'running', 'runs', and 'ran', are uniformly recognized as 'run'. During this step, a specialized function, get_wordnet_pos, was employed to map the part-of-speech tags from NLTK's pos_tag function to those identified by WordNet, thereby refining the lemmatization process. As a result of these combined measures, the review text column in the data frame underwent comprehensive preprocessing, priming it for subsequent feature extraction. Lastly, the Term Frequency-Inverse Document Frequency (TF-IDF) technique was utilized to metamorphose the preprocessed text into a matrix of features. This method assigns values to words based on their significance in a document about their prevalence in the broader corpus. To optimize this, words with common occurrences across documents, often labelled "stop words" due to their lack of distinctive insights, were intentionally left out.

### Exploratory  Analysis 

During the exploratory analysis phase, descriptive statistics were leveraged to succinctly represent the central tendency, dispersion, and shape of the 'star ratings' and subsequently introduced 'sentiment scores' distributions. Histograms visually represented these distributions, making identifying patterns or anomalies straightforward. Additionally, to ensure the integrity and accuracy of the analysis, outlier detection was meticulously executed on the 'star ratings' and 'sentiment' columns. Using specific criteria, rows that fell outside the expected range (1 to 5) for both 'star ratings' and 'sentiment' were isolated. However, after a thorough examination, it was determined that neither the 'star ratings' nor 'sentiment' columns had any outliers, ensuring the robustness of the dataset. Building on these insights, a correlation heatmap was crafted using Pearson correlation coefficients. This heatmap was a powerful tool to visually depict and numerically quantify the degree of linear relationship between the star ratings and the calculated sentiment scores. Finally, a chi-square statistical test was performed to determine the correlation strength by first discretizing the sentiment and star ratings into categories.

![](https://github.com/odogwu25/Hybrid-Recommendation-Systems/blob/main/Images/Histogram%20dist%20star%20and%20sentiment.png)

### Sentiment Analysis Approach

The sentiment analysis was primarily driven by a pre-trained BERT (Bidirectional Encoder Representations from Transformers) model due to its advantages, including its benchmark-setting performance, its ability to grasp contextual word subtleties because of its bidirectional nature, and its transfer learning advantages from pre-training on large text corpora. Specifically, a fine-tuned variant of the BERT model intended for sentiment classification on the IMDB dataset was employed. Sourced from the Transformers library, this model stands at the forefront of natural language processing and is renowned for its prowess in sentiment extraction from textual reviews. For the sentiment analysis, each review undergoes a series of transformations. Firstly, the review text is tokenized and formatted to be compatible with the BERT model. This involves segmenting the text into chunks or tokens and encoding them into a format the BERT model can understand. Given the vastness of certain reviews, they might exceed BERT's maximum input length, thus necessitating truncation to fit within the limit. To maximize efficiency and reduce computational time, the model's operations were executed on the GPU runtime provided by Google Colab, a platform that allows for robust cloud-based computations. Once the input is prepared, it's transferred to the GPU for fast processing. Utilizing the GPU over a traditional CPU ensures that the vast matrix operations inherent in deep learning models like BERT are executed swiftly, leading to quicker sentiment score derivations for each review. The model then processes these tokenized reviews and provides an output consisting of logits or raw prediction scores for each sentiment category. These logits are then passed through a softmax function to transform them into probabilities. The sentiment score for each review is computed by taking the difference between the probabilities of the positive and negative classes. This sentiment score captures the polarity of the sentiment, with positive values indicating a positive sentiment and negative values indicating a negative sentiment.

Given that the original star ratings lie within a range of 1 to 5, aligning the sentiment scores with this scale is crucial. To achieve this alignment, the MinMaxScaler from the sklearn library was employed. This scaler transformed the sentiment scores to fit within the range of 1 to 5, mirroring the original star ratings range. However, the nature of star ratings is inherently discrete, meaning they often exist as whole numbers. The scaled sentiment scores were rounded to the nearest whole number to emulate this and facilitate straightforward comparisons. By the end of this process, each review in the dataset had a corresponding sentiment score, offering a machine-driven perspective on its sentiment tailored to mirror the original star ratings.

![](https://github.com/odogwu25/Hybrid-Recommendation-Systems/blob/main/Images/Screenshot%202024-02-11%20at%2016.06.34.png)

### Customer behaviour Analysis
To better understand customer behaviour, reviews were delved into to extract key insights. Using the TF-IDF method, which measures a word's importance in a document relative to a collection of documents, the reviews were converted into an organized data frame, allowing for easier analysis. Reviews were then categorized by their star ratings: those rated 4 or higher were considered positive, highlighting what customers liked, while those rated 2 or lower pointed out areas for improvement.

This classification made it possible to gather TF-IDF scores specific to each sentiment. The core of this analysis identified the top 10 recurring words in both positive and negative reviews. Bar charts vividly displayed the main topics in both positive and negative feedback. Additionally, word clouds were used for each sentiment category. These clouds visually emphasized the most important words: the more significant the word, the larger its display. Words frequently linked with customer satisfaction stood out in the positive cloud, while the negative cloud highlighted common customer concerns. These visuals offer a quick yet deep dive into customer opinions, aiding in informed future decisions.

![](https://github.com/odogwu25/Hybrid-Recommendation-Systems/blob/main/Images/positive%20comments.png)

![](https://github.com/odogwu25/Hybrid-Recommendation-Systems/blob/main/Images/negative%20comments.png)

The word frequency analysis illuminated the specific terminologies recurrent in customer reviews. For those expressing satisfaction, keywords like 'Great', 'use', 'work', 'good', 'easy', 'product', 'year', 'love', 'program' and ‘software’ were top 10, reflecting contentment, practicality, and a generally affirmative experience with the product or service. In contrast, reviews tinged with dissatisfaction frequently featured terms such as 'work', 'product', 'software', 'use', 'program', 'buy', 'version', 'computer', 'try' and ‘time’. From these, it's evident that many negative sentiments stemmed from functionality-related issues, challenges with specific software versions, and overarching concerns about usability.

![](https://github.com/odogwu25/Hybrid-Recommendation-Systems/blob/main/Images/top%2010.png)

### StarRatingsCorrelationwithSentimentAnalysis

The correlation between sentiment scores and star ratings was notably high, with a Pearson's correlation coefficient of 0.7501. This suggests a strong positive relationship between the review's sentiment and the given star rating. In simpler terms, reviews with positive sentiments tend to have higher star ratings and vice versa. Further statistical verification was conducted using the Chi-squared test, resulting in a value of 254,699.58. The associated P-value was 0.0, indicating this correlation is statistically significant.

![](https://github.com/odogwu25/Hybrid-Recommendation-Systems/blob/main/Images/corr.png)

### ModelDevelopment

A subset of 70,000 samples was chosen from the main dataset. This decision was made to manage computational limitations, ensuring that the model development went smoothly without system hiccups. The data was partitioned into an 80-20 split, with 80% allocated for training to allow the model to adequately learn and recognize patterns, and 20% reserved for testing to verify the model's predictive accuracy and prevent overfitting, thereby ensuring a well-rounded evaluation of its performance (Najafabadi et al., 2015). To make sure these steps would not be repeated in the future, the processed data and the training-test sets were saved as CSV files and uploaded to one drive.

Various aspects were considered in the selection of acceptable models for this project to ensure the highest degree of accuracy and relevance to the specific nature of the task at hand. The Random Forest model was chosen for its remarkable accuracy and classification abilities, particularly in interpreting complex sentiment categories. This model is well-known for handling high- dimensional spaces and large numbers of training examples, making it especially adept at deciphering the subtle sentiment data in our dataset (Al Amrani et al., 2018). Similarly, Logistic

Regression was selected as a key component of the hybrid approach owing to its proven efficiency and balanced classification outcomes in star rating predictions. This model, characterized by its simplicity and the ability to provide probabilities as well as classifications, allows for a more nuanced understanding and prediction of user ratings. Its unparalleled accuracy in predicting star ratings serves as a critical tool in achieving a comprehensive analysis of user sentiments, aligning with the project's goal to delve deep into user reviews to provide precise recommendations (Shakhovska et al., 2020). Furthermore, incorporating the Recurrent Neural Network (RNN) was prompted by its ability to analyse sequential input. Given the textual nature of the sentiment analysis component of this research, RNNs' ability to analyse patterns and sequences in text data is critical. This is primarily because RNNs can use their internal state (memory) to process sequences of inputs, allowing them to discover intricate patterns within data that would be beyond the capability of conventional models. This characteristic makes RNNs particularly well-suited for tasks involving sentiment analysis and star rating prediction, aiming to significantly enhance the efficacy of the recommendation system through a better understanding of user sentiments. (An et al., 2018).

By integrating these models into a unified framework, the project seeks to leverage the distinct strengths of each, fostering a synergistic effect that would facilitate more nuanced and accurate recommendations than could be achieved using any single model in isolation. Moreover, these models were chosen for their complementary strengths, offering a balanced and robust approach to tackling the multi-faceted challenges presented in the e-commerce domain.

![](https://github.com/odogwu25/Hybrid-Recommendation-Systems/blob/main/Images/pipeline.png)

- Random Forest for Sentiment-Based model: A Random Forest classifier was employed for the sentiment model. The ratings were binarized using a threshold of 4, marking ratings of 4 and above as positive (encoded as 1) and the rest as negative (encoded as 0). The reviews underwent vectorization through the TF-IDF vectorizer, focusing on the top 5,000 terms by frequency. To ensure unbiased learning, especially in scenarios with significant class imbalances, the Random Forest was set with balanced class weights. Post-training, the model furnished both class probabilities and straightforward class predictions for the test dataset.
- Recurrent Neural Network for Sentiment Based model: In the development of sentiment models using RNN, TensorFlow played a pivotal role. Reviews were tokenized, focusing on the most frequent 5,000 words from the training data, and sequences were standardized to a consistent length of 200. Using the earlier stipulated threshold, star ratings underwent a transformation into a binary format. The RNN's structural blueprint encompassed an embedding layer set to a 200-length input and 64 dimensions. This was followed by a 64-unit RNN layer with L2 regularization and sequence returns. To mitigate the risk of overfitting, a dropout layer with a 0.5 rate was integrated. Subsequent layers included a second RNN layer with 32 units and L2 regularization, culminating in a dense output layer utilizing a sigmoid activation function tailored for binary classification. Training was fine-tuned using the Adam optimizer, configured with a 0.001 learning rate, and to further fortify against overfitting, early stopping was implemented with a patience threshold of three.

