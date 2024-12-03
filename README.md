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