# Zillow-Data-Analytics-Pipeline

## Project Overview
This project demonstrates the creation of an end-to-end ETL (Extract, Transform, Load) pipeline using various AWS services and Python. The primary goal is to extract data from Zillow's RapidAPI, transform it, and load it into Amazon Redshift for analysis and visualization using Amazon QuickSight.

<img width="1325" alt="Workflow image" src="https://github.com/Harsh-812/Zillow-Data-Analytics-Pipeline/assets/135538639/e1903545-b802-40c6-b112-087bcf206f87">
<hr>

### 1. Data Extraction and Ingestion
- **Extract Data:** Used Python to extract data from Zillow's RapidAPI. Set up an EC2 instance to run the extraction script.
- **Load Data into S3**: Store the extracted data in the Landing Zone S3 bucket. Used a Bash operator in Airflow to transfer the JSON data from the EC2 instance to the S3 bucket.
- **Trigger Lambda Function:** A Lambda function is triggered to copy the data to another S3 bucket (Intermediate Zone). Configure an S3 event to trigger the Lambda function upon file upload, the Lambda function copies the data from the Landing Zone to the Intermediate Zone.

### 2. Data Transformation and Loading
- **Transform Data:** Use another Lambda function to transform the data into CSV format. The Lambda function reads the JSON data from the Intermediate Zone S3 bucket. Use the pandas library to process and transform the data. Select relevant columns such as bathrooms, bedrooms, city, home status, home type, living area, price, rent estimate, and zip code.
Convert the processed data to CSV format.
- **Store Transformed Data in S3:** Load the transformed data into the Clean Data Zone S3 bucket.
The Lambda function writes the CSV data to the Clean Data Zone S3 bucket.
- **S3 Key Sensor:** Use an S3 Key Sensor in Airflow to ensure the presence of the transformed data in the S3 bucket. The S3 Key Sensor monitors the Clean Data Zone for the presence of the CSV file. Once the file is detected, the sensor triggers the next step in the pipeline.
- **Load Data into Redshift:** Use the Airflow S3 to Redshift Operator to load the transformed data from S3 into Redshift. Define the Redshift cluster, schema, and table structure. The operator copies the CSV data from the S3 bucket into the Redshift table.


### Dags - Workflow (Airflow)
![image](https://github.com/Harsh-812/Zillow-Data-Analytics-Pipeline/assets/135538639/d1cf43a1-79a6-4c1f-986a-739cad90f32a)
<hr>

### 3. Data Visualization
- **Set Up Amazon QuickSight:** Sign up for Amazon QuickSight and configure it to connect with Redshift. Create a QuickSight account and configure the necessary permissions. Ensure Redshift cluster is publicly accessible to allow QuickSight connection.
- **Create Data Source in QuickSight:** Connect QuickSight to the Redshift cluster and create a data source. Use the autodiscover feature to find the Redshift cluster.
Validate the connection by providing the database name, username, and password.
- **Data Visualization:** Create basic visualizations in QuickSight to analyze the data. Load the data source and create a new analysis. Use various chart types to visualize data such as price distribution by city, number of bedrooms, and other relevant metrics.


### Visualization - AWS QwickSight
![simpleDashboardZillow](https://github.com/Harsh-812/Zillow-Data-Analytics-Pipeline/assets/135538639/20d12ab2-bff7-41d1-9312-df14bba90138)
<hr>


### Tools and Services Used
- **AWS S3:** Storage for raw and transformed data.
- **AWS Lambda:** Functions to copy and transform data.
- **AWS Redshift:** Data warehouse for storing and querying transformed data.
- **Amazon QuickSight:** Visualization tool for data analysis.
- **Apache Airflow:** Orchestration tool for automating and monitoring the ETL pipeline.
- **Zillow RapidAPI:** Source of real estate data.
- **Python:** Programming language for scripting and automation.
- **IAM Roles:** AWS Identity and Access Management for securing and managing access to AWS services.
- **EC2:** Virtual servers in the cloud to run Python scripts and Airflow.

### Conclusion
This project provides a scalable and automated solution for Zillow data analytics using AWS services. By leveraging AWS Lambda, Amazon Redshift, and Apache Airflow, the pipeline ensures efficient data processing and insightful visualizations through Amazon QuickSight.




