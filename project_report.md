Introduction

This cloud computing portfolio project demonstrates the implementation of a data analytics pipeline using AWS services. The project focuses on analyzing the 2023 Storefront Inventory for food and beverage businesses in Downtown Vancouver. Through a series of cloud-based steps—data ingestion, cleaning, transformation, cataloging, analysis, and governance—the goal is to gain insights into spatial business patterns while showcasing proficiency in AWS tools such as S3, Glue, Athena, KMS, CloudTrail, and CloudWatch.

## DAP Design and Implementation

## Descriptive Analysis

The third dataset that will be analyzed is the annual Storefronts Inventory in the city of Vancouver. The dataset, download from City of Vancouver Website, is filtered by Geo Local Area: “Downtown”, Year recorded: “2023”, and Retail category: “Food and Beverage” and the total record is 510 records. From that data, descriptive analysis will be performed to answer the question below.

What is the food and beverage business spatial distribution in Downtown Vancouver for the year of 2023?

Table 1

Column Name and Description of CSV file downloaded from City of Vancouver Website.

Note. Columns are extracted from City of Vancouver. Own work.

Figure 1

DAP Platform Diagram

Note. DAP platform diagram has been drawn using draw.io. Own work.

## Step 1: Data Ingestion

To start the analytic process, the first step was to gather the data. This was done by accessing the City of Vancouver website and choosing a specific dataset, as follows:

Themes: Business and economy

Data Set: Storefronts Inventory

Source :

Before starting the ingestion process, a folder was created in Amazon S3 Bucket named SGVS-Sam. The folder had the structure shown in the screenshot: s3://storefrontinventory-raw-sam/PlanningDesign_Sustainability/storefront_inventory/year=2023/SGVS-Sam/ 
	According to the information provided by the website, this is an annual inventory, hence the ingestion happening only once a year. In this particular case, the year of 2023.

Figure 2

Data ingestion successfully stored in S3 BucketNote. Data ingestion successfully stored in S3 Bucket, screenshot is taken from AWS platform. Own work.

The screenshot above shows a successful data ingestion with a CSV file stored in Folder SGVS-Sam, that stands for Storefrontinventory General Virtual Server (SGVS-Sam).

## Step 2: Data Profiling

After Data Ingestion performed in Step 1, the data used was stored in S3 Bucket (storefrontinventory-raw-sam). This information was profiled using AWS Glue DataBrew, a tool used to understand better the data, detecting missing values and making it easier to perform analysis in the future.

Figure 3 shows the profiling job successfully created, and Figure 4 shows the Data profile overview, with correlations among the variables that could be found after the profiling job.

Figure 3

Profile job successfully createdNote. Profile job successfully created, screenshot is taken from AWS platform. Own work.

Figure 4

Data profile overviewNote. Data profile overview, screenshot is taken from AWS platform. Own work.

After profiling, the result was stored in a S3 Bucket previously created for transformed data, as illustrated in Figure 5.

Figure 5

Result of Data Profiling stored in transformed S3 Bucket

Note. Result of Data Profiling stored in transformed S3 Bucket, screenshot is taken from AWS platform. Own work.

## Step 3: Data Cleaning

Data Cleaning is necessary to improve the quality of the information. This can be done by detecting and removing errors and inconsistencies. In this case, AWS Glue DataBrew was used to perform data cleaning. And after the process, the results were stored in S3 Bucket.

The recipe for the cleaning process was to rename blank spaces, as follows:
1. Retail category renamed to RetailCategory.

2. Year recorded renamed to YearRecorded.

3. Geo Local Area renamed GeoLocalArea.

4. geo_point_2d renamed to GeoPoint2d

Figure 6

Recipe for the cleaning process for the dataset

Note. Recipe for the cleaning process for the dataset, screenshot is taken from AWS platform. Own work.

Figure 7

Recipe job successfully created

Note. Recipe job successfully created, screenshot is taken from AWS platform. Own work.

After cleaning the dataset, the results were stored in S3 Bucket, in a previously created Transformed Bucket. In this step, two different folders hold the data produced after data cleaning: User and System.

Figure 8

User folder used to store the user friendly file after cleaning job

Note. User folder used to store the user friendly file after cleaning job, screenshot is taken from AWS platform. Own work.

Figure 9

System folder used to store the system friendly file after cleaning job

Note. System folder used to store the system friendly file after cleaning job, screenshot is taken from AWS platform. Own work.

## Step 4: Data Cataloging

For this step, the tool used was AWS Glue Crawler. Data Cataloging is an important step to help organize the dataset and help the user to find data quickly, and understand and manage it more efficiently.

The dataset created after data cleaning and stored in the transformed bucket was used to perform this step. The Figure 10 and 11 show, respectively, a crawler run successfully completed and the database created.

Figure 10

Storefrontinventory crawler run

Note. Storefrontinventory crawler run, screenshot is taken from AWS platform. Own work.

Figure 11

Database created after Data CatalogingNote. Database created after Data Cataloging, screenshot is taken from AWS platform. Own work.

## Step 5: Data Summarization

This last step is important to create a smaller, summarized dataset, after performing automated actions to aggregate and reduce large dataset and make it easier to analyze. The tool used was AWS Glue, and an ETL (Extract, Transform, and Load) job was necessary to prepare the data. The actions performed in this stage were:

Extract the data from data catalog

Transform schema by changing name of a few columns:

civic_number_-_parcel to civic_number_parcel

street_name_-_parcel to street_name_parcel

retailcategory to retail_category

yearrecorded to year_recorded

geolocalarea to geo_local_area

geopoint2d to geopoint_2d

Transform - Filter data by year_recorded >= 2023

Transform – Aggregate:

street_name_parcel

retail_category

Transform - add Report_Date to summary

Transform - convert to Report_Date_LTZ

Transform - prepare for load, remove Report_Date column

Data target - load to system

Transform - convert to one file

Data target - load to user

Figure 12

Summarization process for the Storefrontinventory dataset

Note. Summarization process for the Storefrontinventory dataset, screenshot is taken from AWS platform. Own work.

Figure 13

StorefrontInventory-Summarization output schemaNote. StorefrontInventory-Summarization output schema, screenshot is taken from AWS platform. Own work.

Figure 14

Summarization job successfully created

Note. Summarization job successfully created, screenshot is taken from AWS platform. Own work.

Figure 15

New table created to store summarized result

Note. New table created to store summarized result, screenshot is taken from AWS platform. Own work.

Figure 16

System friendly result of summarization stored in curated S3 BucketNote. System friendly result of summarization stored in curated S3 Bucket, screenshot is taken from AWS platform. Own work.

Figure 17

User friendly result of summarization stored in curated S3 Bucket

Note. User friendly result of summarization stored in curated S3 Bucket, screenshot is taken from AWS platform. Own work.

Figure 18

User friendly file output

Note. User friendly file output, screenshot is taken from downloaded file under user folder AWS platform. Own work.

Figure 19

ETL job monitoring

Note. ETL job monitoring, screenshot is taken from AWS platform. Own work.

## Data Analysis and Security

## Step 6: Data Analysis

In this step, AWS Athena was used as a tool to query the data and analyze the data to answer questions 1 and 2.

Descriptive Question 1: What is the food and beverage business spatial distribution in Downtown Vancouver for the year 2023?

The purpose of this query is to answer question 1 and analyze how food and beverage businesses are distributed in different areas and streets in Downtown Vancouver for the year 2023. When we understand the distribution and the spatial patterns, we can identify areas with a higher density of this kind of business, commercial hotspots, if there are too many businesses offering similar products, and potential gaps in the region market.

This insight is valuable to improve urban planning, market research, and to develop business strategies. Based on these insights, the stakeholders can have more data to make better decisions about the opportunities and competition in the region they are prospecting.

Figure 20

SQL Query and result for Question 1 using Athena

Note. SQL Query and result for Question 1 using Athena. Screenshot is taken from AWS platform. Own work.

Descriptive Question 2: What are the top five streets in Downtown Vancouver with the highest concentration of food and beverage businesses?

The purpose of this query is to answer question 2 and identify the key commercial streets in Downtown Vancouver that had the top 5 highest concentration of food and beverage businesses. The analysis of these areas helps us to understand the business clusters, the competitive landscapes, and potential market opportunities in the chosen area.

Figure 21

SQL Query and result for Question 2 using Athena

Note. SQL Query and result for Question 2 using Athena. Screenshot is taken from AWS platform. Own work.

## Step 7: Data Security

To increase Data Security, AWS Key Management Service was used to create a key and to encrypt and protect the data, as seen in Figure 22 below.

Figure 22

Key created in KMS

Note. Key created in KMS. Screenshot is taken from AWS platform. Own work.

After creating the Key for encryption to protect S3 buckets Raw, Transformed, and Curated,, a bucket versioning was enabled for all three buckets, to keep different versions of an object in the same bucket. This was performed to make it easier to recover files if we have an application failure or accidentally delete the object.

Figure 23

Raw Bucket Versioning and Encryption Enabled

Note. Raw Bucket Versioning and Encryption Enabled. Screenshot is taken from AWS platform. Own work.

Figure 24

Transformed Bucket Versioning and Encryption Enabled

Note. Transformed Bucket Versioning and Encryption Enabled. Screenshot is taken from AWS platform. Own work.

Figure 25

Curated Bucket Versioning and Encryption Enabled

Note. Curated Bucket Versioning and Encryption Enabled. Screenshot is taken from AWS platform. Own work.

As an extra security, compliance, and availability measure, an S3 Replication rule was configured to automatically create a copy of the objects from one S3 bucket and save them in a backup bucket.

Figure 26

Replication rules for Raw bucket

Note. Replication rules for Raw bucket. Screenshot is taken from AWS platform. Own work.

Figure 27

Replication rules for Transformed bucket Note. Replication rules for Transformed bucket. Screenshot is taken from AWS platform. Own work.

Figure 28

Replication rules for Curated bucket

Note. Replication rules for Curated bucket. Screenshot is taken from AWS platform. Own work.

## Step 8: Data Governance

For this step, a Data Quality Check was performed using AWS Glue to validate the data before using it for the actual analysis. It helped detect issues such as missing values, inconsistencies, duplicates, and errors that could lead to an impact in the decision-making process. After the ETL job was created, the results of the Data Quality Check were stored in two folders: Passed and Failed.

Figure 29

ETL pipeline for Data Quality Check

Note. ETL pipeline for Data Quality Check. Screenshot is taken from AWS platform. Own work.

Figure 30

Output Schema for the Data Quality Check result

Note. Output Schema for the Data Quality Check result. Screenshot is taken from AWS platform. Own work.

Figure 31

Passed folder

Note. Passed Folder. Screenshot is taken from AWS platform. Own work.

Figure 32

Failed folder

Note. Failed Folder. Screenshot is taken from AWS platform. Own work.

## Step 9: Data Monitoring

For monitoring the AWS resources and applications, the service used was Amazon CloudWatch. Three alarms were set, one for each bucket, to monitor raw, transformed and curated S3 buckets for the customized period of 3 weeks. A notification is triggered if the threshold defined is exceeded for the bucket sizes.

Figure 33

CloudWatch Dashboard

Note. CloudWatch Dashboard. Screenshot is taken from AWS platform. Own work.

AWS CloudTrail is a service used to track user activity, to investigate unauthorized access or misconfigurations. It is used for auditing and compliance. It was configured to create logs of all activities performed.

Figure 34

CloudTrail set

Note. CloudTrail set. Screenshot is taken from AWS platform. Own work.

## Conclusion

This project successfully demonstrates the creation and execution of a cloud-based analytics pipeline using the AWS ecosystem. The data was ingested, cleaned, and transformed through AWS Glue and stored across structured S3 buckets. Descriptive analysis was performed using AWS Athena, and governance was ensured through data encryption, monitoring, and logging tools. The insights gained from the Storefront Inventory dataset support improved urban planning and market research, while the infrastructure reflects best practices in secure, scalable cloud computing. This portfolio highlights both technical capability and real-world application.