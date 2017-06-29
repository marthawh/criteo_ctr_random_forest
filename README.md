# criteo_ctr_random_forest
Internet advertising is a multi-billion dollar business and is growing rapidly. There are several major channels on the web for online advertising such as display advertising and search advertising. Display advertising is different from the search advertising in that it uses graphical banners placed on the publishers’ web pages. Among them, CPC is the most popular option, in which advertisers only pay when a user clicks on the ad. As a consequence, click through rate (CTR) prediction, which is defined as the problem of estimating the probability that a user clicks on an ad in a specific context, is crucial to online advertising.

The Criteo data is a click prediction dataset that is approximately 370GB of gzip compressed TSV files (~1.3TB uncompressed), comprising more than 4.3 billion records. It is taken from 24 days of click data made available by Criteo. Each record in this dataset contains 40 columns: The first column is a label column that indicates whether a user clicks an add (value 1) or does not click one (value 0). The next 13 columns are numeric, and last 26 are categorical columns

The columns are anonymized. The Click through rate (CTR): This is the percentage of clicks in the data. In this Criteo dataset, the CTR is about 3.3% or 0.033. I will be working with day_21 of the dataset on my local machine.

When I decided to work with the Criteo terabyte click predication dataset, I knew dealing with this large dataset would be challenging and fun. My plan was to work with one day of the 24 day dataset. As I found out, most analysis of this dataset took a similar approach.  

I choose day 21 and downloaded it to my computer. The download took most of the day. I used an ipython notebook and loaded only the first 100 records into a panda dataframe. I examined my data finding the 13 integer count columns (I1-I13) and 26 categorical columns (C1-C26) and the click response column (Label). All column names are generic and the categorical columns hashed because the data was anonymized. The goal was to predict a click on an advertisement being served up on the website. When the advertisement is click, the response is a 1.

Next I took a closer look at the data. The histogram of the response column show an extremely unbalanced classification problem. I decided to work with Logistic Regression and Random Forest. Looking at the anonymized data made me think twice about this data, but I was also excited to explore and learn. 

I found that the integer count data was skewed and would need log transformation. I also found that some of the integers were clustered and looked more discrete than continuous. The data contained nulls and a fair amount of zeros in some columns. Most of the categorical data was highly dimensional.
I worked with sklearn using Random Forest and Logistical Regression.
For my first model I replaced the nulls with zeros and used the FeatureHasher from sklearn. The baseline was .031 because of the extreme low number of 1’s and my first models did not do much better. After examining my data with EDA, I decided on the following tasks.

•	Log transformations to the integer columns. The highly skewed I1 column, I transformed twice.

•	Caped and floored the outliers above 95% and below 5%.

•	Replaced the Nans in the integer columns with the mean.

•	Replaced the Nans in the categorical columns with mode

•	Columns with more than 35% null where turned into Boolean features.

•	Examined column I8 and found the only negative value was -1. Because these are count columns, I assumed this is a data error or meant to be 0. Converted to 0.

•	Binned I9 after studying the histogram and confirming 3 clusters of data.

•	Created new features from correlations found in data that resulted in a 1 response.

•	Used LabelEncoder on the highly dimensional categorical columns

•	Used get_dummies on the lower dimensional categorical columns
   
With all my work above, the needle wasn’t moving. My model was not learning. It was time to deal with the unbalanced data. I wanted to balance my data. To do this I read 7 million records from the data file grabbing the rows with 1’s. This got me 223,582 rows of the data with the response a 1.  Then I randomly read in 200,000 rows from my dataset and combined the two.  This give me a baseline of .542, more 1’s than 0’s.

I modeled my data with RandomForestClassifier using the class_ weight option. I added a GridSearch for some parameter tuning using 200 as my top number of estimators.  I set a timer on the model to see how long it would take to run. The model took 38 minutes to run.
 
 Balancing the data got the model learning and I started to see results.  My test accuracy was 81.8%    I added some code to change the threshold and distributed my results. 
I learned so much from working with this data: binning, capping outliers, dealing with unbalanced data, working with bigger data and dimensionality of categorical data.  I’ll be posting some future blogs on the topics above. In the meantime you can take a look at my code.

