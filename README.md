# Speech-machine-analysis-for-diagnosing-cognitive-and-psychiatric-disorders---depression
A thesis submitted to the Graduate Studies Office In partial fulfillment of the requirements for The degree of Master's degree in Computer Engineering. University of Tehran  College of Engineering  Faculty of  Electrical and Computer Engineering

By:Mina Zarifi - Supervisors:Dr. A Vahabie ,Dr. BN Araabi - June, 2023 -
mina.zarifi@ut.ac.ir 



It should be noted that almost at the end of each section, the obtained data is saved and used in the next section. Therefore, you do not need to execute the cells in order. Instead, you can use any section you need with appropriate data.

The Python code file is saved as Zarifi_project.ipynb, and the explanations are stored in a .docx file. The remaining files are in CSV format, which contains the data obtained from each section. To run each code snippet, you can download the relevant section and use it.

Abstract:
    In this study, online data was collected from various adult subjects, which dataset included two different speech tasks. The first task involved answering three questions about the past, present, and future, while the second task involved describing eight images by the subjects. Additionally, four inventory named the Beck Depression Inventory, Beck Anxiety Inventory, Beck State-Trait Anxiety Inventory, and P.A.N.A.S. were also collected from the subjects.
The aim of the research was to study depression as a disease. To evaluate the symptoms of depression, the speech task of describing eight pictures was used by 119 random subjects over eighteen years of age, and the scores of individuals in the Beck Depression Inventory. This research has been carried out with the aim of analyzing the relationship between the scores of the Beck Depression Inventory and the characteristics of the text. Using semantic vector similarity analysis, it was shown that some of these features have an effect on depression symptoms. For example adults with more depression symptoms had a greater standard deviation of Euclidean and cosine distances between semantic vectors in sentences. In addition, while it was found that some features are effective in describing depression symptoms, a linear regression machine learning model was used to determine their importance in predicting these psychological states. The examined features include speech graph, text sentiments, jump of ideas, and semantic embedding vectors. Embedding vectors were also used to analyze the relationship between features and Beck Depression Inventory scores. The embedding vectors were obtained using three methods: token-based, sentence-based, and sentence-averaging embedding. All stages (finding the correlation between features and Beck Depression Inventory scores, observing similarity matrix for each image, predicting Beck Depression Inventory scores using a linear regression model) were performed for all three cases. They were also used to analyze the similarity matrix so that if a feature has a specific trend according to the Beck Depression Inventory  scores and the similarity matrix between semantic embedding vectors, that feature can be used as input for regressing and predicting the Beck Depression Inventory scores. 

1.Create only image CSV


Please note that if you have the CSV data file, you don't need to run this section. The dataset consists of audio test data from two speech tasks. One speech task involves three questions about the past, present, and future of the participants, and the second speech task involves participants talking about eight images. In this project, only the data from the second speech task, which is related to 8 images, is used. That's why the "Create only image CSV" section is performed in the code. In this section, the data is first converted from mp3 format to wav (this code only needs to be executed once). After that, speech to text conversion is performed using Vosk (the code cells of this section should be executed in order).


preproccess 


In the preprocess section, incomplete data was identified and manually completed by the participants by either deleting or filling in the missing parts (others do not need this section, and this part was done specifically for my project's data).

The transcripts obtained automatically through Vosk were examined in this section and the data was cleaned. There were eight images in the task. It was observed that those who spoke clearly had a very good transcript available. Those who mistakenly skipped speaking about one image due to a weak internet connection or other reasons and only one voice recording was not uploaded to the server were also included in the dataset, and their text was manually entered into the dataset. Some files were shuffled (the order of audio files related to images was mixed up), and they were corrected as well. Those who accidentally did not record the audio file for one image and spoke about the previous image in the next image were also manually corrected.

The duration of each audio was also included in the dataset.
The number of remaining individuals is 123. Only the data related to eight images is saved in one file and will be analyzed from now on. The dataset has a total of 984 rows, and every 8 rows correspond to one participant.



2.COMPUTE BDI



In the COMPUTE BDI section, the scores of the Beck Depression Inventory questionnaire are calculated. It should be noted that the scores of other Inventorys are calculated using different formulas, which you need to implement yourself. In this section, the BDI scores of individuals are computed and added to the dataset to be later used as labels. Individuals who have not completed the BDI or whose Inventory responses were not uploaded to the server due to internet speed issues were sent a message to complete it. The task then proceeds to determine the Inventorys that have been answered more than once.
The Inventorys that have been answered more than once are retained only for their first occurrence, and the rest of the responses are deleted. Individuals who manually changed their Inventory IDs were identified based on the time of completing the Inventory and the audio test, and they were added to the dataset. The dataset columns were also modified so that each row corresponds to an audio file. Every 8 rows belong to one person who spoke about 8 audio files. They are grouped in such a way that each group has only one ID in the new dataset, and the lengths of the conversations are summed, and the text of all audio files is merged together. The "first block: read images CSV and first BDI" and "second block: read only second BDI CSV" sections are related to calculating the BDI for a set of data initially. Later, when a new set of data is added, the BDI scores for the new data are calculated and added to the previous ones. If you also want to add new data to your dataset, this section will be useful for you.


3. categorization


This section is used when you want to use classification instead of regression. In this case, you will have 6 classes:

Total Score : Levels of Depression
1-10 : These ups and downs are considered normal
11-16 : Mild mood disturbance
17-20 : Borderline clinical depression
21-30 : Moderate depression
31-40 : Severe depression
over 40 : Extreme depression


groupby id

In the "groupby id" section, the data for each person, which consists of 8 rows, is merged into one row (all their words are placed side by side in relation to all images). The total length of each person's conversations is also added as a column called "total_length" to the data. In the "vectorized groupbied data" section, all the conversations of each person about each of the eight images are vectorized using ParsBERT. The vectorized dataset has dimensions (118, 773).
It should be noted that this part was used to test different methods, but it was not used anymore because the results were not satisfactory. Another solution was used, which will be mentioned later.

4. vectorized - analysis

   
 In the "vectorized with ParsBERT" section, the original data, which includes individuals' speech for each image separately, is vectorized using ParsBERT. Note that the continuation of this section is only for further analysis and familiarity with the data, and you do not need to execute it because the required analyses will be conducted in the following sections.
In the "Similarity Matrix" section, the vector representations are used to create a similarity matrix. Since the dimensions of the vectorized data are (768, 952), the dimensions of the similarity matrix are (952, 952). The similarity matrix is plotted for the vectors. Then, they are sorted based on the BDI scores, the duration of individuals' speech, the order of the images, and the categorized BDI scores.
Several data analyses are performed in this section, which you can use if desired.

5. similarity matrix

   
With the available vectorized data, which also includes linguistic features in the dataset, it is possible to analyze and evaluate the similarity matrix based on the desired features, even distinguishing between analytical images. This process will be performed subsequently.
The level of cognitive leaps experienced by the participants is also calculated using the Euclidean distance and the cosine angle between the embedded semantic vectors of each sentence in the texts. The average, standard deviation, and sum of distances are used as features. With n sentences in a text, we will have n-1 metrics in that text. This means that for texts with only one sentence, this feature cannot be calculated and they are excluded, leading to a value of nall.
In the "Sort by Euclidean_mean" feature and then show the similarity matrix section, the data is sorted based on the Euclidean mean, and then the cosine similarity between the calculated vectors is measured. The obtained similarity matrix is then visualized. (The reason for reducing the data to 686 out of 686 is that in datasets containing only one sentence, the cognitive leap features cannot be calculated, so they are removed.)
This process is performed for six cognitive leap features, including mean, standard deviation, and sum of distances in both Euclidean and cosine distances.
By examining the correlation between each of these features and the depression questionnaire scores, we can identify important features and further study them. Even negative but significant scores can be important. This correlation is acceptable when its value is greater than dividing by the square root of n. The following features are calculated for 119 individuals, each describing eight images, resulting in calculations for a total of 952 utterances. This means that correlations above dividing by the square root of 952, which corresponds to a value of 0.06, have a meaningful relationship with the depression questionnaire scores. It can be observed that the values of standard deviation and sum of Euclidean and cosine distances are acceptable.



7. scatter plot

In this section, to become more familiar with the data, the similarity matrix is plotted when it is reduced to two dimensions.

8. unsupervised

This section is used when you want to analyze and draw conclusions only from audio data without using questionnaire data. However, since satisfactory results were not obtained from this section, it was not used in the final outcome.
In this section, after transforming the grouped data of each person (each row corresponds to all the speeches of one person and is not separated for different images) into vectors using the TF-IDF vectorization method, the data is clustered using k-means and hierarchical methods.


9. feature extraction

    
Natural Language Processing (NLP) is the extraction of meaningful information from natural language data and manipulating them using computers. NLP models are usually based on the English language. For other languages, multilingual models with limited resources are used. ParsBERT suggests a monolingual BERT model for the Persian language. ParsBERT has been trained on large public datasets (Persian Wikidumps, MirasText) and numerous website data. Part of the ParsBERT methodology includes extensive preprocessing to bring the dataset into an appropriate format. The ParsBERT model has been specifically trained on specific language vocabulary with a large amount of Persian text data.


sentiment analysis

One of the features used is sentiment analysis. Text sentiment is calculated using two methods and used as a feature in prediction. In the first method, a defined function is used to tokenize and annotate a string of Persian words into the smallest meaningful unit using the hazm library. Then, using a text classification model, the sentiment score of each text is predicted. In the second method, a pre-trained machine learning model loaded from the Transformers library is used for natural language processing and sentiment analysis. Sentiments are represented numerically between 0 and 1, indicating the likelihood of a text being positive. When the sentiment score is close to 1, it indicates more positive sentiments, while being close to 0 indicates more negative sentiments. If the score is close to 0.5, it means that the model cannot confidently determine the sentiments of the text.

Speeh Graph Analysis

Using a speech graph, where each word is represented as a node and the connections between consecutive words are represented as edges, a series of features have been extracted from the texts. The extracted features are as follows:
Language features include general attributes such as the total word count , the number of graph nodes , and the number of edges. connected components include largest connected component, Largest strongly connected components.recurrence attributes include  Related edges, parallel edges, and L1 and L2. Additionally, global attributes such as graph density, diameter, clustering coefficient , average shortest path, average total degree , etc., have been calculated.

correlation


In this section, the correlation between each of the extracted features and the BDI-score has been calculated. By examining the correlation between these features and the depression questionnaire scores, important features can be identified and further explored. Even negative but large scores can be significant. The acceptable correlation is when its value exceeds the division by the square root of n. The following features apply to 119 individuals, each of whom has spoken about describing eight images. Therefore, a total of 952 speeches have been analyzed for these features. This means that correlations above the division by the square root of 952, which corresponds to a value of 0.06, have a significant relationship with the BDI-score. The figure below illustrates the correlation between each of these features and the BDI-score. It can be observed that the PE, L1, L2, LCC, LSC, graph density, graph diameter, and ASP features show acceptable correlation values higher than a certain threshold.


However, we did not stop there, and after that, the correlation between the features of each image and the BDI-score was separately calculated. In this case, interesting correlations between the features within each image can be observed. In the subsequent sections, this analysis has been performed on each image separately using three methods: token-centric, sentence-centric, and average embedding of sentence-centric approaches. The correlation values and their acceptability have also been obtained and presented in tables.

10. SVM - RF- DT - balance - LOO


In this section, three methods, SVM, Random Forest, and Decision Tree, have been implemented, and balanced data using the leave-one-out method has been used. 

TFIDF with 2 class

Firstly, the data has been divided into two classes: normal and depressed, ensuring balance between the two classes so that one class does not have significantly more or fewer data instances than the other. However, this approach may discard some information as it does not take into account the level of depression. Then, dimensionality reduction has been performed on the TF-IDF vectors, and after implementing leave-one-out on the SVM model, various evaluation metrics, confusion matrix, ROC curve, and Precision-Recall curve have been displayed.

Scatter Plot between features:

In this section, scatter plots have been generated between different features to provide a more concrete visualization and facilitate analysis to identify and remove irrelevant features.

TFIDF with 63 class _ balance

In this section, data balancing is not achieved by dividing the data into two classes. Instead, it is achieved by removing data instances from classes that have a large number of instances, ensuring equal data distribution in each class. However, it should be noted that this method may not be suitable, and these steps were only implemented for experimentation with different solutions, yielding unsatisfactory results.

visualized similarity matrix

In this section, the similarity matrix has been visualized. The conversations within each image have also been sorted according to BDI-score. By using percentiles, a better visualization of the similarity matrix can be obtained. The analysis and interpretation of the data have been performed using a representational dissimilarity matrix to provide a more tangible representation of the similarity between images and their relationship with BDI-scores. The similarity matrix has been calculated based on the vectors obtained for each speech, and it has been sorted based on the BDI-scores, allowing for the observation of meaningful connections. The similarity matrix has also been plotted separately based on images. The plotting is done in terms of percentiles to enhance understanding. Transforming a similarity matrix into a percentile matrix can be useful for better understanding and comparing similarities between different sets of data. Percentiles provide a way to rank values relative to each other in a distribution, which can help identify outliers or patterns that may not be immediately apparent from raw similarity scores.

By having a similarity matrix that indicates the similarity between different vectors, converting it into percentiles can help identify which vectors are specifically related to each other (i.e., high percentile values) and which ones are less related (i.e., lower percentile values). This can be useful for classification purposes or for identifying evolving relationships between different images.

In the adjacency matrix, each row and column represent a speech. The similarity of semantic vectors between rows of the adjacency matrix and columns of the adjacency matrix is indicated by the color within each cell. The main diagonal of the adjacency matrix is all ones, meaning that each speech has the highest similarity with itself. The adjacency matrix is a square matrix with dimensions equal to the number of speeches. The correlation between the features and the BDI-scores is displayed in the plots in a normal and unsorted manner (like the figure below).

Each of these sections can also be zoomed in for a closer view, and the code for that is written in the subplots section.


11. jump of idees & correlation:


Each person has talked about eight images. Therefore, in the similarity matrix, the information of each participant is actually an eight by eight matrix. The main diagonal elements, which represent the similarity of each speech with itself, are all the same, and the upper and lower elements on the main diagonal are also identical. For this reason, only the 28 values above the main diagonal are used. The similarity matrix for each individual, which only uses the elements above the main diagonal, is used because the elements below the main diagonal are redundant and the elements on the main diagonal are all the same. When using the information from the 8 by 8 matrices of individuals with respect to Euclidean and cosine distances, the prediction of the depression questionnaire scores can be categorized into three modes:
1.      Token-based embedding mode
2.      Sentence-based embedding mode
3.      Mean word-based embedding mode
In each of these three modes, similar tasks are performed, with only the vector transformation method being different. In each section, after obtaining the Euclidean and cosine distances, semantic feature vectors are calculated based on the mean, standard deviation, and sum of the features. Then, the correlation between each feature and the BDI-score is calculated. The significance value is also computed, representing a certain number of data points, which is displayed.


12. percentile similarity matrix :


In this section, the adjacency matrix is plotted in terms of percentiles for the following three modes:
1.      Token-based embedding mode
2.      Sentence-based embedding mode
3.      Mean word-based embedding mode

To further analyze the similarity matrix, it is used to interpret dissimilarity in the representational dissimilarity matrix space, making the relationship between images and depression questionnaire scores more tangible. The similarity matrix is calculated based on the obtained vectors for each text, and it is sorted according to the depression questionnaire scores in order to observe meaningful connections. This matrix is also plotted separately based on percentiles, providing a better understanding. Transforming a similarity matrix into a percentile matrix can be useful for better comprehension and comparison of similarities between different datasets. Percentiles provide a way to rank values relative to each other in a distribution, which can help identify outliers or patterns that may not be immediately apparent from raw similarity scores.

By having a similarity matrix that indicates the similarity between different vectors, transforming it into percentiles can help determine which vector refers specifically to which images (i.e., having higher percentile values) and which one is less related to others (i.e., having lower percentile values). This can be useful for classification purposes or identifying evolving relationships between different images.

In the adjacency matrix, each row and column represent a speech. The color within each cell indicates the semantic similarity between the semantic feature vectors of the adjacency matrix rows and columns. The main diagonal of the adjacency matrix is entirely filled with ones, meaning that each speech has the highest similarity with itself. The adjacency matrix is a square matrix with dimensions equal to the number of speeches.

13. prediction SVM:


It should be noted that this section was conducted for further analysis and experimentation, and the main results were obtained in the following section.

Each person has talked about eight images. Therefore, in the similarity matrix, the information of each participant is actually an eight by eight matrix, which is used as a feature for the model. The main diagonal elements, which represent the similarity of each speech with itself, are all the same, and the upper and lower elements on the main diagonal are also identical. For this reason, only the 28 values above the main diagonal are used. The similarity matrix for each individual, which only uses the elements above the main diagonal, is used because the elements below the main diagonal are redundant and the elements on the main diagonal are all the same.
When using the information from the 8 by 8 matrices of individuals with respect to Euclidean and cosine distances, the prediction of the depression questionnaire scores can be categorized into three modes:
1.      Token-based embedding mode
2.      Sentence-based embedding mode
3.      Mean word-based embedding mode
4.  
In each of these three modes, similar tasks are performed, with only the vector transformation method being different.

The separate features of Euclidean and cosine distances have been used for prediction. Each section indicates the specific implementation performed in that particular mode.


14. prediction : Linear regression


 In this section, the linear regression model is used for prediction. The performance of the prediction of depression severity is evaluated based on the R-squared and the root mean squared error (RMSE).
Linear regression is a statistical method used for modeling the relationship between a dependent variable and one or more independent variables. The goal of linear regression is to find the best line that describes this relationship, allowing us to predict the dependent variable based on the values of the independent variables. The main idea behind linear regression is to plot the data points on a scatter plot and then place a straight line among them that minimizes the distance between the line and the data points. This line can then be used to predict future values of the dependent variable based on new values of the independent variables. Linear regression models are used in a wide range of applications, including identifying trends and relationships between variables, which can assist in decision-making in various domains.


As mentioned in the previous sections, each person has provided information about eight images. Therefore, the similarity matrix for each participant is essentially an 8x8 matrix, and it has been used as a feature for the model. The main diagonal elements, which represent the similarity of each utterance with itself, are all ones, and the upper and lower elements of the main diagonal are also the same. Hence, only the 28 values above the main diagonal have been used. The similarity matrix for each person is considered separately, utilizing only the values above the main diagonal, as the elements below the main diagonal are redundant and the elements on the main diagonal are all ones.

When predicting the scores of the depression questionnaire using the information from the 8x8 matrices of individuals, based on the Euclidean and cosine distances as relevant features, three scenarios are considered:
1. Token-based embedding: The TF-IDF method is used for embedding.
2. Sentence-based embedding: The LaBSE method is used for embedding.
3. Mean word-based embedding: The ParsBERT method is used for embedding.

For each of these three scenarios, the prediction is performed using linear regression separately for the two features (Euclidean and cosine distances).











