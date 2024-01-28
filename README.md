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
- 
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
1 https://kafka.apache.org/documentation.html
2 https://data-flair.training/blogs/apache-kafka-tutorial/
3 https://medium.com/big-data-engineering/hello-kafka-world-the-complete-guide-to-
kafka-with-docker-and-python-f788e2588cfc
