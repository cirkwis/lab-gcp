# Overview

BigQuery is Google's fully managed, NoOps, low cost analytics database. With BigQuery you can query terabytes and terabytes of data without having any infrastructure to manage or needing a database administrator. BigQuery uses SQL and can take advantage of the pay-as-you-go model. BigQuery allows you to focus on analyzing data to find meaningful insights.

The dataset you'll use is an ecommerce dataset that has millions of Google Analytics records for the Google Merchandise Store loaded into BigQuery. You have a copy of that dataset for this lab and will explore the available fields and row for insights.

In this lab, you will ingest several types of datasets into tables inside of BigQuery.

# Create a new dataset to store tables

In the BigQuery console, click on the name of your project, then click Create Dataset. Set the Dataset ID to ecommerce. Leave the other fields at their default values and Click Create dataset. You'll now see the ecommerce dataset under your project name.

# Ingest a new Dataset from a CSV

Scenario: Your marketing team is looking to you to help guide them with what products should be up for promotions based on inventory stock levels. They have also asked how each product is trending in customer sentiment based on the product reviews.

Your existing ecommerce transactional dataset does not have inventory stock levels or product review data in it, but your operations and marketing teams have provided you with new datasets for you to analyze.

Download the product stock level dataset locally onto your computer:
https://storage.googleapis.com/data-insights-course/exports/products.csv

Specify the below table options:

Source:

- Create table from: Upload
- Select file: select the file you downloaded locally earlier
- File format: CSV

Destination:

- Table name: products
Leave other settings at their default value.

Schema:

- Check Auto Detect for Schema and input parameters
Tip: Not seeing the checkbox? Ensure the file format is CSV and not Avro.

- Partition and Cluster settings: Leave at default settings
  
Advanced Options: Leave at default settings

Click Create Table and You should now see the products table below the ecommerce dataset.

**BigQuery command line solution**

```shell
wget https://storage.googleapis.com/data-insights-course/exports/products.csv
bq load --autodetect [project id]:[dataset name].products products.csv
```

# Ingest data from Google Cloud Storage
Select the ecommerce dataset and click Create Table.

Specify the below table options:

- Source:
  - Create table from: Google Cloud Storage
  - Select file from GCS bucket: gs://data-insights-course/exports/products.csv
  - File format: CSV
- Destination:
  - Table name: products
Leave all other settings as default.
- Schema:
  - Check Auto Detect for Schema and input parameters.
- Advanced Options: Write **Preference dropdown menu**, select **Overwrite table**.

Click Create Table.
**BigQuery command line solution**

```shell
bq load --autodetect --replace [project id]:[dataset name].products gs://data-insights-course/exports/products.csv
```

# Ingest a new dataset from a Google Spreadsheet

