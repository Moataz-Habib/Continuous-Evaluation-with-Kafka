# Continuous-Evaluation-with-Kafka

## Installation instructions:
To achieve the real time data delivery, I used Kafka and Dockers to simulate an independent server which will send information.

![Example Image](/Images/img1.PNG)

1.To install the local Docker and Docker Compose programs on our computers. Please use the following link to download it just take into consideration your operative system. https://docs.docker.com/get-docker
2.Once your installation is complete, you will have the docker’s desktop interface as shown in the following image. It is not necessary to create an 3.account or to perform any upgrade, with the free version we will be able to complete the assignment
Once it is fully installed download and unzip the folder provided that contains the scripts and the .csv with the data.

![Example Image](/Images/img2.PNG)

Once unzipped in your computer, run one of the following files depending on your
operative system:

- If you are using windows then run the **docker_script.bat** file
- Otherwise run the **docker_script.sh** file on Linux/MacOS.
  
This file will create two docker images wurstmeister/kafka and wurstmeister/zookeeper.
For this activity you don’t have to worry about what they are or how they run, as everything
is already configured. At the end of the installation.
To validate that the container images were created successfully open Dockers and you
should see two images running like this

![Example Image](/Images/img3.PNG)

If you have any inconvenience, you can open the file and run it line by line.

Now let’s make use of the Producer’s API to upload the stream of data into the server. This
step will automatically perform the partition and will store the information into the topic,
ready for the consumer to read it. Please open the .ipynb called
**“dns_kafka_producer.ipynb”**. Run all cells and wait for the information to be uploaded,
this file will install the necessary libraries in case you don’t have them already and create
the producer object with the topic and server’s configuration pointing to our docker images.
Finally it will read the CSV file and push all the data to the server into the specific topic.

![Example Image](/Images/img4.PNG)

This process will take approximately 10 minutes, so be patient, a conclusion message will
be displayed when the process is done and you are able to see inside a red strip the number
of rows already inserted; this time can vary depending on your machine. The last step in
this process will be to run the **“DNS_Kafka Consumer.ipynb”** which will set up the
configuration to read data from the Docker’s server and the data ingested by the Producer.

![Example Image](/Images/img5.PNG)

Now you are ready to capture the data stream and start the data treatment for your model.
Good luck and have fun!

**References:**

1. [Apache Kafka Documentation](https://kafka.apache.org/documentation.html)
2. [DataFlair Apache Kafka Tutorial](https://data-flair.training/blogs/apache-kafka-tutorial/)
3. [Hello Kafka World - The Complete Guide to Kafka with Docker and Python](https://medium.com/big-data-engineering/hello-kafka-world-the-complete-guide-to-kafka-with-docker-and-python-f788e2588cf)

## 1 - Static Model

**Validate data imbalanced with justifications.**

'No' (target not attacked) accounts for 120,895 instances, which is about 54.90% of the data. ‘Yes' (target attacked) accounts for
147,179 instances, which is about 45.10% of the data. The dataset is relatively balanced since both classes are well-represented.

![Example Image](/Images/img6.PNG)

**Statistical analysis of data**

Feature distributions:

Histograms: Indicate a right-skewed distribution for most features with most values at the lower end.

Boxplots: Confirm the skewness, showing medians towards the lower end and highlighting outliers.

<table>
  <tr>
    <th colspan="2" style="text-align:center; font-size:24px;">Histograms</th>
    <th colspan="2" style="text-align:center; font-size:24px;">Boxplots</th>
  </tr>
  <tr>
    <td colspan="2">
      <img src="/Images/img7.PNG" alt="Histograms" style="display:block; margin:auto;"/>
    </td>
    <td colspan="2">
      <img src="/Images/img8.PNG" alt="Boxplots" style="display:block; margin:auto;"/>
    </td>
  </tr>
</table>

Skewness: Positive skewness values for features like 'upper' show a right skew, while negative values for others like 'subdomain'
suggest a left skew, indicating the direction and asymmetry of the distribution.

<table>
  <tr>
    <th colspan="2" style="text-align:center; font-size:24px;">Right Skew</th>
    <th colspan="2" style="text-align:center; font-size:24px;">Left Skew</th>
  </tr>
  <tr>
    <td colspan="2">
      <img src="/Images/img9.PNG" alt="Histograms" style="display:block; margin:auto;"/>
    </td>
    <td colspan="2">
      <img src="/Images/img10.PNG" alt="Boxplots" style="display:block; margin:auto;"/>
    </td>
  </tr>
</table>

**Data Cleansing and Feature creation**

I have found 8 missing values in the longest_word, I dropped them.
The dataset contains 91,803 duplicate rows. I Removed them to help ensure data cleanliness.
I created new features from existing categorical string variables 'sld' and 'longest_word' using feature hashing, a technique that converts categorical
data into a fixed-size numerical format that machine learning algorithms can work with. It then removes the original columns.
I removed the ‘timestamp' column as the issue at hand does not pertain to time series analysis.

**Feature Filtering with more than 2 methods**

For feature selection, Mutual Information, Recursive Feature Elimination (RFE), and ANOVA F-score methods were employed to optimize Random
Forest and XGBoost models. A subset of common features identified by these methods was also analyzed to assess their combined predictive
strength using F1 scores within both modeling.

<table>
  <tr>
    <th style="text-align:center; font-size:24px;">Mutual Information</th>
    <th style="text-align:center; font-size:24px;">RFE</th>
    <th style="text-align:center; font-size:24px;">ANOVA</th>
  </tr>
  <tr>
    <td>
      <img src="/Images/img11.PNG" alt="Histograms" style="display:block; margin:auto; width:100%; max-width:300px;"/>
    </td>
    <td>
      <img src="/Images/img12.PNG" alt="Boxplots" style="display:block; margin:auto; width:100%; max-width:300px;"/>
    </td>
    <td>
      <img src="/Images/img13.PNG" alt="Third Image" style="display:block; margin:auto; width:100%; max-width:300px;"/>
    </td>
  </tr>
</table>

Common Features: ['len', 'subdomain_length', 'subdomain', 'numeric', 'special', 'FQDN_count', 'lower', 'labels']

**Data Splitting and justification**

The data is split into training and testing sets, with 30% reserved for testing, allocating a significant portion of data to the training set
ensures the model can learn effectively, MinMaxScaler is then applied to normalize the training data and subsequently transform the
test data using the same scale.

**Choose and justify the correct performance metric**

The F1-Score has been adopted as the preferred metric to account for the minor data imbalance, ensuring a balanced trade-off
between precision and recall. This metric is critical in our context as it equally weighs the importance of reducing false positives and
avoiding missed attacks, providing an equilibrium that mitigates both excessive false alarms and overlooked threats.

**Compare and describe the two models you will use**

Random Forest and XGBoost were chosen for their robustness and effectiveness in handling diverse datasets and feature sets. Both
models performed comparably across different feature selection methods (Mutual Information, RFE, ANOVA) and when applied to a
common feature set. Notably, the common features (8 features) nearly matched the F1 scores achieved with the full selected
features (11 features), indicating that a smaller, more focused feature set can yield similar predictive performance, streamlining the
model without compromising on accuracy. This suggests that the models are efficient in extracting signal from the most relevant
features, which is advantageous for computational efficiency and model interpretability

**Plot the models’ results.**

This plot depicts the F1 scores for each model, comparing the baseline performance with the performance after applying the three
different feature selection methods.

<table>
  <tr>
    <th style="text-align:center; font-size:24px;">F1 Scores for Different Feature Selection Methods (Random Forest)</th>
    <th style="text-align:center; font-size:24px;">F1 Scores for Different Feature Selection Methods (XGBoost)</th>
    <th style="text-align:center; font-size:24px;">F1 Scores for Random Forest and XGBoost on Common Features</th>
  </tr>
  <tr>
    <td>
      <img src="/Images/img14.PNG" alt="Histograms" style="display:block; margin:auto; width:100%; max-width:300px;"/>
    </td>
    <td>
      <img src="/Images/img15.PNG" alt="Boxplots" style="display:block; margin:auto; width:100%; max-width:300px;"/>
    </td>
    <td>
      <img src="/Images/img16.PNG" alt="Third Image" style="display:block; margin:auto; width:100%; max-width:300px;"/>
    </td>
  </tr>
</table>

   

