# :octocat: IoT-Mini-Project-2-Data-Engineering-in-IoT-Pipeline

---

### Group Members

* *Shalika Dulaj Amarathunga*
* *Kavinda Rathnayaka*

##  Video Demonstration

[Click here for video Demostration](https://youtu.be/SOcVkectUzI)



# Data Engineering in IoT Pipeline

### Introduction

This project constitutes the second mini-project for the Internet of Things (IoT) course at the University of Oulu. The core objective is developing and implementing an IoT pipeline with a primary focus on data engineering. The central theme involves extracting, pre-processing, visualization, and storing temperature and pressure sensor data obtained from IoT devices deployed across three distinct remote locations. Each location has three temperature sensors, contributing to a comprehensive dataset.

The project initiates the retrieval of temperature data from IoT devices strategically positioned in the three remote locations. These devices serve as sources of real-time temperature measurements, providing valuable insights into environmental conditions at each site. The distributed nature of the deployment, with three sensors from each sensor type at each location, enhances the granularity and comprehensiveness of the collected data.

The data engineering pipeline encompasses crucial stages, beginning with data pre-processing. Raw temperature data is subjected to necessary transformations and cleaning processes to ensure accuracy and consistency. Subsequently, the pre-processed data is channeled into a visualization module, where it is translated into informative visual representations. To facilitate long-term data management, a cloud-based storage system is implemented.




<div align="center">

![IoT Flow chart (1)](https://github.com/shalikadulaj/IoT-Mini-Project-2-Data-Engineering-in-IoT-Pipeline/assets/58818511/c1fcfc1f-c70d-4148-a688-04eb56665910)


</div>

## Advanced IoT Data Analysis Model ( DEP Unit )

### Overview

We have developed a sophisticated model for IoT data analysis, primarily focusing on temperature and pressure data received from an IOT device's remote destinations. This model is an integration of advanced data processing techniques and machine learning, specifically designed to enhance the capabilities of IoT systems in environmental monitoring.

### Key Features
- **MQTT Communication**: Utilizes AWS IoT MQTT and MQTTSN for data transmission.
- **Data Processing**: Implements outlier detection, linear regression for future data prediction, and median filtering.
- **Threaded Execution**: Employs threading for simultaneous data processing tasks.
- **Environmental Monitoring**: Focused on processing temperature and pressure data from IoT sensors.

### Technologies Used
- Python
- AWSIoTPythonSDK
- numpy
- threading
- sklearn

### Setup and Installation
- Install Python dependencies: `sklearn`, `numpy`, `AWSIoTPythonSDK`.
- Configure AWS IoT MQTT Client with the required certificates and endpoint.
  > See gudied steps in "Getting Started Section"

### Usage
1. Configure MQTT client with appropriate credentials and endpoint.
2. Subscribe to desired MQTT topics for receiving temperature and pressure data.
3. The script processes incoming data, detecting outliers ( See  "Outliers/Anomalies handling with  Median Filtering, Zscore method and Impution " ) and predicting future values using linear regression ( check the detailed description on "Linear Regression Model" section.
4. Processed data is published back to an MQTT topic.

### Code Structure
- **Data Collection**: The `Callback` class handles incoming MQTT messages and triggers data processing.
- **Outlier Detection**: Implements Z-score calculation to identify outliers in temperature and pressure data.
- **Data Processing Threads**: Separate threads for processing temperature and pressure data using linear regression.
- **MQTT Configuration and Management**: Setup and manage MQTT and MQTTSN clients for data communication.
- **Data Synchronization**: Uses threading locks for thread-safe operations on shared data.


In summary, our modal takes raw sensor data, cleanses it through outlier detection and handling, and then uses the cleaned data to train a linear regression model for predicting future values. This process enhances the reliability of the data used for predictions and ensures that the model's outputs are based on accurate and representative information.

---
<details>

<summary> Outliers/Anomalies handling with  Median Filtering, Zscore method and Imputation  </summary>

Median filtering is a non-linear digital filtering technique, often used to remove noise from a signal or image. It is particularly effective at removing 'salt and pepper' type noise and can preserve edges while reducing random noise. The median filter works by sliding a window over the data, replacing each entry with the median of neighboring entries. The size of the window determines the extent of smoothing: a larger window leads to more smoothing.

median filtering is applied in the context of IoT sensor data processing, specifically for handling outliers in temperature and pressure data. Here's how it works in our code:

1. **Identification of Outliers**: our modal calculates the Z-score for each new temperature or pressure reading. If the absolute value of a Z-score is greater than 3, the reading is considered an outlier. This is based on the statistical rule of thumb that most (99.7%) data points should fall within three standard deviations from the mean in a normal distribution.

2. **Handling Outliers with Median Filtering**: When an outlier is detected, our modal replaces it with the median of the last 10 readings. The median is a robust measure of central tendency, which is less affected by outliers compared to the mean. Therefore, replacing the outlier with the median helps maintain the integrity of the data.

3. **Application in Data Processing**:
   - Temperature Data: If a temperature reading is identified as an outlier, it's replaced with the median of the previous 10 temperature readings.
   - Pressure Data: Similarly, if a pressure reading is an outlier, it's replaced with the median of the previous 10 pressure readings.

4. **Updating the Data Lists**: After handling outliers, the filtered (cleaned) data points are appended to their respective data lists (`temperature_data` and `pressure_data`). 

This median filtering process helps in maintaining a more accurate and reliable data stream by mitigating the impact of anomalous readings that might skew the analysis or predictions.


</details>


---


<details>

<summary> Linear Regression Model  </summary>
 


Linear regression is a statistical method used for modeling the relationship between a dependent variable and one or more independent variables. In the context of our code, the linear regression model is used for predictive analysis based on the historical data of temperature and pressure readings from IoT sensors. 

Here's how it works:

1. **Objective**:

Linear regression aims to find a linear relationship between the independent variable(s) (predictors) and the dependent variable (outcome). It does this by fitting a linear equation to the observed data.

3. **Linear Equation**: The equation of a simple linear regression is `y = a + bx + e`, where:
   - `y` is the dependent variable (outcome).
   - `x` is the independent variable (predictor).
   - `a` is the y-intercept.
   - `b` is the slope of the line.
   - `e` is the error term.

4. **Predictions**: Once the model parameters (`a` and `b`) are estimated from the training data, the model can make predictions for new, unseen data.

### Implementation 
In our script, linear regression is used separately for temperature and pressure data. Here's the breakdown:

1. **Data Preparation**: The last 10 readings of temperature or pressure data are used as the dataset. These readings form the dependent variable `y`, and their corresponding indices (time points) are the independent variable `x`.

2. **Model Training**: A linear regression model is created and trained using this data. The training process involves finding the best-fit line that minimizes the difference between the predicted values and the actual data points.

3. **Prediction**: After training, the model predicts the next temperature or pressure value. This is achieved by inputting the next index (i.e., the length of the temperature or pressure data array) into the trained model to forecast the subsequent reading.

4. **Usage of Predicted Data**: These predicted values are then used to update a shared data structure (`combined_data`), which appears to be subsequently published to an MQTT topic.

This implementation allows the system to not only process and analyze current sensor data but also to forecast future readings. This can be particularly useful for proactive monitoring and decision-making in IoT applications.

</details>


---

### Getting Started

#### ( click the arrow to open guidelines for each step )

<details>

<summary> STEP 1: Setup FIT IOT-LAB Testbed and Configure The Network environment </summary>

#### logged in to FIT IOT-LAB and ssh to the Grenoble site.

> Note: This is the second part of the project series. we strongly recommend you to follow [Part One](https://github.com/shalikadulaj/IoT-Mini-Project-1/blob/main/README.md) for better understanding.
  
Connect to the SSH frontend of the Grenoble site of FIT/IoT-LAB by using the username you created when you registered with the testbed:

submit an experiment

Step 1: logged in
![Screenshot 2023-12-31 at 11 19 34](https://github.com/shalikadulaj/IoT-Mini-Project-2-Data-Engineering-in-IoT-Pipeline/assets/153508129/87616d1e-95be-46c3-8290-47290aef46ca)
Step 2: Select nodes
![Screenshot 2023-12-31 at 11 20 45](https://github.com/shalikadulaj/IoT-Mini-Project-2-Data-Engineering-in-IoT-Pipeline/assets/153508129/d3af4678-7c8f-4730-b1f4-15e7b6b7936d)
Step 3: Open the experiment
![Screenshot 2023-12-31 at 11 21 49](https://github.com/shalikadulaj/IoT-Mini-Project-2-Data-Engineering-in-IoT-Pipeline/assets/153508129/320612ba-2805-4a0b-a122-3092c5c5495f)
Step 4: Browse the clone directory on your local PC. ( IoT-Mini-Project-2-Data-Engineering-in-IoT-Pipeline )

<div align="center">

![Screenshot 2023-12-31 at 12 05 34](https://github.com/shalikadulaj/IoT-Mini-Project-2-Data-Engineering-in-IoT-Pipeline/assets/153508129/3c3f1b75-ad7f-4ed8-ae52-0b29d2dab3b1)


</div>


Now Open a Terminal from the front end and follow the below steps.

```ruby
   ssh <username>@grenoble.iot-lab.info
```
Now you can configure the network of the border router on m3-1 and propagate an IPv6 prefix with ethos_uhcpd.py

```ruby
username@grenoble:~$ sudo ethos_uhcpd.py m3-1 tap0 2001:660:5307:3100::1/64
```
The network is finally configured and you will see a similar response below:

```ruby
net.ipv6.conf.tap0.forwarding = 1
net.ipv6.conf.tap0.accept_ra = 0
----> ethos: sending hello.
----> ethos: activating serial pass-through.
----> ethos: hello reply received
```
> Note 1: leave the terminal open (you don’t want to kill ethos_uhcpd.py, it bridges the BR to the front-end network)

> Note 2: If you have an error “Invalid prefix – Network overlapping with routes”, it’s because another experiment is using the same ipv6 prefix
> (e.g. 2001:660:5307:3100::1/64).

Open the other Sensor Nodes shell in a different terminals from frontend and check the Global IPV6 prefix is obtained from the border router subnet using help -> ifconfig

```ruby
 username@grenoble:~/RIOT$ nc m3-2 20000
```
if all nodes have global ipv6 propagated from the border router, you can start the stations. 

> Note: before starting you need to start the broker. see Step 2.

```ruby
 >start 2001:660:5307:3000::67 1885 1 
```
```ruby
 >start 2001:660:5307:3000::67 1885 2 
```
```ruby
 >start 2001:660:5307:3000::67 1885 3 
```

</details>
<details>

<summary> STEP 2. Start DEP station and communication with aws-iot </summary>

####  In another terminal, log on to the A8 node, node-a8-1. We are going to configure and start the MQTT-SN broker as follows:

```ruby
   my_computer$ ssh <login>@grenoble.iot-lab.info
  login@grenoble:~$ ssh root@node-a8-1
```

#### Clone the AWS and Project Repo to the A8 node:

execute the setup.sh script to do it automatically for you.

```ruby
   root@node-a8-1:~#chmod +x setup.sh
   root@node-a8-1:~#./setup.sh
```
now start the mqttsn-broker.

```ruby
   root@node-a8-1:~#chmod +x setup_Broker.sh
   root@node-a8-1:~#./setup_Broker.sh
```
open another terminal and start the DEP station.

```ruby
  root@node-a8-1:~#cd DEP
  root@node-a8-1:~#python3 main.py
```
if everything is working you should see a similar screen like below.

**** Please Note that we already have included forecasting using linear Regression "main_lR.py". you may need to download and install "sci-kit learn" before running the code in A8 Board.  




![Screenshot 2023-12-31 at 12 21 03](https://github.com/shalikadulaj/IoT-Mini-Project-2-Data-Engineering-in-IoT-Pipeline/assets/153508129/51f4aeb1-3a3b-4671-9820-5170c57f1925)



</details>

---

## Data Management and Visualization






![31 12 2023_14 16 50_REC](https://github.com/shalikadulaj/IoT-Mini-Project-2-Data-Engineering-in-IoT-Pipeline/assets/58818511/a3cdbe98-efd8-4c7a-a4f8-fcebe6ca280f)




## :cloud: AWS 

[Amazon Web Services (AWS)](https://docs.aws.amazon.com/index.html ) is a leading cloud platform offering a comprehensive suite of services for data processing, storage, visualization, and alerting. 

### *( click the arrow to open guidelines for each step )*

<details>



<summary> AWS IoT Core </summary>

AWS IoT Core is a managed cloud service that facilitates secure communication between IoT devices and the AWS Cloud. It ensures encrypted connectivity, device management, and seamless integration with AWS services. With features like device shadows and a scalable architecture, it's ideal for building secure and scalable IoT applications. According to the rule actions it sends data to amazon timestream table and  Lambda function.

https://docs.aws.amazon.com/iot/ 


The border router publishes sensor data from FIT IoT Lab to the specific topic in AWS IoT core. There are rules to control data, which receive to the IoT core.



<details>


<a name="Createathing"> </a>

<summary>  Create a thing and Certificates </summary> 

A thing resource is a digital representation of a physical device or logical entity in AWS IoT. Your device or entity needs a thing resource in the registry to use AWS IoT features such as Device Shadows, events, jobs, and device management features.

Follow the below steps to create a thing 

	AWS IoT Core > Manage > All Device > Things > Create Things 
- Specify thing properties 

- Configure device certificate 

- Attach policies to the certificate 


Finally, you must download the device certificate, key files, and Root CA Certificates. These certificates should be added to the code. It is mentioned in the code, that you can replace the certificates with yours's. 



Now you need to add the Endpoint to the code. You can get the Endpoint from the below path.

	AWS IoT  > Settings > Device data endpoint 




At this moment you can check whether the data is receiving. If not, you have to check the above steps again. To check follow the below steps. 

	AWS IoT > Test > MQTT test client > Subscribe to a topic ("Grenoble/Data") > Subscribe 

Replace the topic with your topic. Now you can see the data is receiving as below. 

<div align="center">


![31 12 2023_08 48 05_REC](https://github.com/shalikadulaj/IoT-Mini-Project-2-Data-Engineering-in-IoT-Pipeline/assets/58818511/fb8435d7-c632-4592-b097-862b01c65956)


</div>
<details>
<summary> Policy </summary> 
	
	{
	"policy": {
	"purpose": "The purpose of this policy is to establish guidelines for the secure and efficient handling of sensor data from remote IoT devices, including temperature and pressure readings. This data will be processed using Lambda functions, stored in DynamoDB, and visualized through Grafana.",
	"dataCollectionAndTransmission": {
	"instructions": "All IoT devices must securely transmit temperature and pressure data to the designated AWS Lambda endpoint using encrypted communication to ensure data integrity and confidentiality."
	},
	"lambdaFunctionDataProcessing": {
	"configuration": "Implement AWS Lambda functions for processing incoming sensor data before storage in DynamoDB. This includes data validation, transformation, and any necessary enrichment.",
	"security": "Ensure that Lambda function execution roles have the least privilege necessary, and regularly review and update permissions based on the principle of least privilege."
	},
	"dataStorageInDynamoDB": {
	"database": "Utilize Amazon DynamoDB as the primary database for storing processed IoT sensor data.",
	"tableAndSchema": "Define appropriate tables and schema within DynamoDB to efficiently store and retrieve temperature and pressure readings."
	},
	"dataVisualizationUsingGrafana": {
	"platform": "Grafana will be the designated platform for visualizing IoT sensor data.",
	"configuration": "Configure Grafana dashboards to display real-time and historical temperature and pressure readings.",
	"accessControl": "Access to Grafana should be restricted to authorized personnel only."
	},
	"securityMeasures": {
	"accessControls": "Implement access controls to restrict unauthorized access to AWS Lambda functions, DynamoDB, and Grafana.",
	"encryption": "Utilize encryption for data in transit and at rest, both between IoT devices and Lambda functions, as well as between Lambda functions and DynamoDB."
	},
	"complianceWithPrivacyRegulations": {
	"regulations": "Ensure that the collection, storage, and visualization of sensor data comply with relevant privacy laws and regulations.",
	"consents": "Obtain necessary consents or permissions for data collection as required by applicable regulations."
	},
	"monitoringAndAlerts": {
	"monitoring": "Implement monitoring solutions to detect abnormal patterns or potential security incidents in Lambda functions, DynamoDB, and Grafana.",
	"alerts": "Set up alerts to notify relevant personnel in case of system anomalies or failures."
	},
	"documentationAndTraining": {
	"documentation": "Maintain comprehensive documentation for the configuration and setup of Lambda functions, DynamoDB, and Grafana."
	},
	"policyReview": "This policy will be reviewed annually and updated as necessary to align with changes in technology, regulations, or organizational requirements.",
	"acknowledgment": "I acknowledge that I have read and understood the IoT Sensor Data Management Policy. I agree to comply with all the stipulated rules and guidelines.",
	"approval": "[Shalika, Kavinda] [2023/12/31]"
	}
	}



</details>
</details>

</details>



<details>
<summary> AWS Timestream </summary> 

AWS Timestream is a fully managed, serverless time-series database service provided by Amazon Web Services (AWS). It is specifically designed to handle time-series data at scale. Time-series data is characterized by data points associated with timestamps. In this project, the data from the IoT core is ingested into the AWS Timestream database using AWS rules.

**Ingesting data into Timestream**

Sample JSON data

	{
	    "temperature": 41.0,
  	    "pressure": 983.0,
 	    "site": "Grenoble",
  	    "timestamp": "2023-12-31 01:57:07"
	}

First, you need to add rules. Follow the below steps to add rules 

	AWS IoT > Message Routing > Rules > Create rule 

- Specify rule properties 

- Configure SQL statement 
	- Write this quarry to select all the data coming from the topic, and ingest to the timestream. 

			SELECT * FROM 'Grenoble/Data'   

*Note - In this project, data comes from three sites (Grenoble, Saclay, Paris). We get the processed data from the 'Grenoble/Data', 'Saclay/Data', and 'Paris/Data' topics . And we get unprocessed data from each node (there are 9 nodes). Use sensor/node1, sensor/node2, sensor/node3 ... etc, topics to get noisy data (before preprocessing). We use this noisy data only for visualizing purposes. We do not store this noisy data in the DynamoDB database.*


- Attach rule actions - This is the action when receiving data. 
	- Select - “Timestream table (write message into a Timestream table)” 
	- Add database - If you have not created a database, you can create a database by clicking on “Create Timetream database”. Select standard database. 
	- Add Table – Click on "create timestream table" 
	- Add an "IAM role" – Click on create new role 

- Review and create 
</details>


<details>


<summary> AWS Managed Grafana </summary>


AWS Managed Grafana is a fully managed and scalable service that simplifies the deployment, operation, and scaling of Grafana for analytics and monitoring. It integrates seamlessly with other AWS services, offering a user-friendly interface for creating dashboards and visualizations to gain insights from diverse data sources. We are using Grafana for visualizing data using AWS Timestream as a data source.

You can create the workspace as below 

	Amazon Managed Grafana > All workspaces > Create workspace 

- Specify workspace details 
	- Give a unique name 
	- Select Grafana version – We are using Version 8.4
  

 

- Configure settings 
	- Select Authentication access - “AWS IAM Identity Center (successor to AWS SSO)” 

- Service managed permission settings 
	- Select data sources  - “Amazon TimeStream” 

- Review and create 

**Creating user**

	Amazon Managed Grafana > All workspaces > Select workspace created above > Authentication > Assign new user or group > Select User > Action > Make admin 

If you can't find a user, you have to add a user by the below method 

	IAM Identity Center > Users >  Add user (giving email and other information) 

After adding you can see the user under "configure users" in your workspace 
 

Login to Grafana workspace 

	Amazon Managed Grafana > All workspaces > Select workspace created above >  Click on “Grafana workspace URL” 

Sign in with AWS SSO 

	Add Data Source > Select Amazon Timestream > Select default region (should be equal to Endpoint region) 

 We are using the “US East (N. Virginia) us-east-1” region. Add database, table, and measure. Then save. Now you are successfully connected to the data source. Then using Grafana, you can create a dashboard as you need. 

</details>







<details>
<summary> AWS DynamoDB </summary>



AWS DynamoDB, a fully managed NoSQL database, it is used for storing all the processed data. With seamless scalability and low-latency access, DynamoDB ensures reliable and fast retrieval of alert information. Its flexible schema accommodates evolving data needs, making it a robust solution for storing and retrieving dynamic data.


**To create a DynamoDB database follow the below steps**

	
- Search DynamoDB in the AWS console
  
  		tables> Create table
 
	- Provide table details (table name, partition key) 
	- create a table with default settings. 
	
When you are writing the code for the lambda function, this table name will be required.

</details>



<details>

<summary> AWS Lambda - Cloud Data Preprocessing </summary>

AWS Lambda is a serverless computing service provided by Amazon Web Services (AWS). It allows developers to run code without the need to provision or manage servers. This serverless architecture enables developers to focus solely on writing code to meet business requirements, without worrying about the underlying infrastructure.

In the architecture designed for our data processing workflow, we leverage AWS Lambda to seamlessly transmit data to DynamoDB. Before storing this data in the DynamoDB database, a crucial step is introduced within the Lambda function itself to address potential noise or missing values. While initial data preprocessing is performed in the node, noise can be generated during transmission. To mitigate this, the Lambda function incorporates a dedicated data preprocessing stage just before the data is committed to the database. This ensures that any discrepancies or inconsistencies in the incoming data are systematically rectified. The preprocessing logic, housed within the Lambda function, allows us to tailor the data precisely before persisting it in DynamoDB. This approach not only fortifies the integrity of the stored information but also streamlines the entire data-handling process within the serverless architecture.


**To create a lambda function follow the below steps**

- Search AWS lambda in aws console

		Dashboard > Create function 


	- Select -Author from scratch
	 - Add basic information -  (Function name-“LambdaFunction”)
	 - Runtime - Python 3.12
 	- Architecture x86_64
	 - Click on the Create function

Now you have a function. Then need to link the trigger with the function. There are two options. You can use any option. The first one is, to click on add trigger button and select a source. You may select  AWS IoT as the source. Because this function receives sensor data through AWS IoT.
The second one is,

	AWS IoT > Message Routing > Rules > Create rule 

- Specify rule properties
- Configure SQL statement
	- Write this quarry to select all the data coming from the topic, and ingest to the lambda function.

			SELECT * FROM 'Grenoble/Data'   


*Note - in this project, data comes from three sites (Grenoble, Saclay, Paris).*



- Attach rule actions - This is the action when receiving data.

	- Select - “Lambda (send a message to a Lambda function)”
	- Lambda function - select the function that you created in the above step (“LambdaFunction”) 
	- Click next and create


Now you can start coding on [lambda_function.py](https://github.com/shalikadulaj/IoT-Mini-Project-2-Data-Engineering-in-IoT-Pipeline/blob/main/lambda_function.py).     The data processing method in this Lambda function focuses on handling outliers in temperature and pressure data before storing it in DynamoDB. Using the Interquartile Range (IQR) method, the function identifies outliers, replacing values beyond calculated thresholds with the nearest threshold value. Based on your requirements you can add any kind of data processing algorithm here. This ensures that extreme data points do not skew the dataset. The function then constructs a DynamoDB database with both original and processed values for temperature and pressure, contributing to the overall robustness of the stored data.



When you run this code you will get a permission error. To solve it follow the below steps.


	IAM > Click on Roles > create role > AWS service >choose service as DynamoDB > Next > Add “AmazonDynamoDBFullAccess” policy > next > give role name > click on create role



Then go back to the lambda,

		Configuration > permission > Edit Execution role > Select the role just created > save

Now all the data received from each topic will be processed and stored in DynamoDB.

   

</details>

## Contributing
Contributions to enhance the script, add new features, or improve the data processing algorithms are welcome.



