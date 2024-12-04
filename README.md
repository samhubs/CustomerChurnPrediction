# CustomerChurnPrediction
## Steps
1. Set up AWS Environment (if you have an existing Redshift cluster, skip to 1.3)

- Log in to the AWS Management Console.
- Navigate to Redshift and create a new cluster. Choose appropriate instance size and node count based on your data size (a small cluster will suffice for this project). Take note of the cluster endpoint and credentials.
- Navigate to S3 and create a new bucket. Give it a unique name.

2. Download the sample data from this Kaggle dataset: [Customer Churn Dataset](https://www.kaggle.com/datasets/muhammadshahidazeem/customer-churn-dataset/data). \
- This dataset contains 440833 rows with the following columns: ['Age', 'Gender', 'Tenure', 'Usage Frequency', 'Support Calls','Payment Delay', 'Subscription Type', 'Contract Length', 'Total Spend',
'Last Interaction', **'Churn'**]. 
- Upload the csv file to the S3 bucket from step 1.

3. Copy data from S3 to Redshift
- Create a cluster using Redshift ensuring the cluster can be accessed via 
a public ip. Ensure the security group is set such that only your machine
ip can access it
- Use DBeaver (or any other client) to connect to the Redshift DB
- Create a table `customers` following the same schema as the csv columns
```sh
create table customers (
CustomerID INT primary key,
Age INT,
Gender VARCHAR,
Tenure INT,
"Usage Frequency" INT,
"Support Calls" INT,
"Payment Delay" INT,
"Subscription Type" VARCHAR,
"Contract Length" VARCHAR,
"Total Spend" INT,
"Last Interaction" INT,
Churn INT)
```
- Copy S3 csv to the new table
```sh
COPY customers
FROM 's3://your-bucket-name/your-file.csv'
IAM_ROLE 'arn:aws:iam::your-account-id:role/your-redshift-role'
DELIMITER ',' 
IGNOREHEADER 1 
CSV;
```
- Verify and preview the data
```sh
SELECT * FROM customers LIMIT 10;
```
    - for troubleshooting, check the schema and the sequence of the columns
    in the csv carefully and ensure they match with the table.

4. Great, you have the data to work with now. Let's perform some enrichment by adding an additional column containing avg spend per day :
```sh
alter table customers add column avg_spend FLOAT;
update customers set avg_spend = "total spend" /tenure;
```

## Phase 2: Model training
2.1 Create a poetry environment
```sh
curl -sSL https://install.python-poetry.org | python3 -
poetry init
poetry add pandas scikit-learn psycopg2-binary
```
