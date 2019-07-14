# Overview

BigQuery is Google's fully managed, NoOps, low cost analytics database. With BigQuery you can query terabytes and terabytes of data without having any infrastructure to manage or needing a database administrator. BigQuery uses SQL and can take advantage of the pay-as-you-go model. BigQuery allows you to focus on analyzing data to find meaningful insights.

The dataset you'll use is an ecommerce dataset that has millions of Google Analytics records for the Google Merchandise Store loaded into BigQuery. You have a copy of that dataset for this lab and will explore the available fields and row for insights.

This lab you will learn how to create new permanent reporting tables and logical reviews from an existing ecommerce dataset.

Source: [Creating Permanent Tables and Access-Controlled Views in BigQuery](https://google.qwiklabs.com/focuses/3690?parent=catalog)

# Create a new dataset to store the tables

In BigQuery, click on your project name, then click Create Dataset. Name the new dataset **ecommerce**. Leave the other options at their default values (Data Location, Default table Expiration).

# Troubleshooting CREATE TABLE statements

Rules for creating tables with SQL in BigQuery

Read through these create table rules which you will use as your guide when fixing broken queries:

1. Only one CREATE statement is allowed.

2. Either the specified column list or inferred columns from a query_statement (or both) must be present.

3. When both the column list and the as query_statement clause are present, BigQuery ignores the names in the as query_statement clause and matches the columns with the column list by position.

4. When the as query_statement clause is present and the column list is absent, BigQuery determines the column names and types from the as query_statement clause.

5. Column names must be specified either through the column list or as query_statement clause.

6. Duplicate column names are not allowed.

# Creating views

Views are saved queries that are run each time the view is called. In BigQuery, views are logical and not materialized. Only the query is stored as part of the view -- not the underlying data.

## Scenario Extra Credit

The anti-fraud department is grateful for the query and they are monitoring it daily for suspicious orders. They have now asked you to include a sample of the products that are part of each order along with the results you returned previously.

```sql
#standardSQL
CREATE OR REPLACE VIEW ecommerce.vw_large_transactions
OPTIONS(
  description="large transactions for review",
  labels=[('org_unit','loss_prevention')]
)
AS
SELECT DISTINCT
  date,
  fullVisitorId,
  visitId,
  channelGrouping,
  totalTransactionRevenue / 1000000 AS totalTransactionRevenue,
  currencyCode,
  STRING_AGG(DISTINCT v2ProductName ORDER BY v2ProductName LIMIT 10) AS products_ordered
 FROM `data-to-insights.ecommerce.all_sessions_raw`
 WHERE
  (totalTransactionRevenue / 1000000) > 1000
  AND currencyCode = 'USD'

 GROUP BY 1,2,3,4,5,6
  ORDER BY date DESC # latest transactions

  LIMIT 10
```

## Using SESSION_USER() in views for limiting data access
Scenario: Your data team lead has asked you to come up with a way to limit who in your organization can see the data returned by the view you just created. Order information is especially sensitive and needs to be shared only with users that have a need to see such information.

Task: Modify the view you created earlier to only allow logged in users with a gmail.com session domain to be able to see the data in the underlying view. (Note: You will be creating specific user group whitelists in a later lab on access; for now you are validating based on the session user's domain).

Re-create and replace the vw_large_transactions view with the new query above. As an additional OPTIONS parameter, add an expiration_timestamp for the entire view to be 90 days from now. 

```sql
#standardSQL
CREATE OR REPLACE VIEW ecommerce.vw_large_transactions
OPTIONS(
  description="large transactions for review",
  labels=[('org_unit','loss_prevention')],
  expiration_timestamp=TIMESTAMP_ADD(CURRENT_TIMESTAMP(), INTERVAL 90 DAY)
)
AS
#standardSQL
SELECT DISTINCT
  SESSION_USER() AS viewer_ldap,
  REGEXP_EXTRACT(SESSION_USER(), r'@(.+)') AS domain,
  date,
  fullVisitorId,
  visitId,
  channelGrouping,
  totalTransactionRevenue / 1000000 AS totalTransactionRevenue,
  currencyCode,
  STRING_AGG(DISTINCT v2ProductName ORDER BY v2ProductName LIMIT 10) AS products_ordered
 FROM `data-to-insights.ecommerce.all_sessions_raw`
 WHERE
  (totalTransactionRevenue / 1000000) > 1000
  AND currencyCode = 'USD'
  AND REGEXP_EXTRACT(SESSION_USER(), r'@(.+)') IN ('qwiklabs.net')

 GROUP BY 1,2,3,4,5,6,7,8
  ORDER BY date DESC # latest transactions

  LIMIT 10;
```