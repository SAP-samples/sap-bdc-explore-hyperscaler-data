# Federating Queries from SAP Datasphere to Amazon S3 via Amazon Athena

This guide explains how to integrate SAP Datasphere with Amazon Athena to enable federated querying of CSV data stored in Amazon S3 without moving data out of SAP systems.

---

## Summary of Architecture

- **Amazon S3**: Stores raw CSV files.
- **Amazon Athena**: Acts as a query engine on top of S3 using SQL.
- **SAP Datasphere**: Connects to Athena to run live federated queries.

---

## Step 1: Prepare Input Data in Amazon S3

1. **Log in to the AWS Console** with an IAM user.
2. Navigate to **S3 service** → Create a bucket → Create a folder.
3. **Upload CSV files** to that folder.
![upload files to S3](./images/s3_upload_dataset.png)
4. After uploading, go to the **object's Properties tab** and **copy the S3 folder path** — you'll need this for table definition in Athena.
![Copy S3 URI path](./images/s3_copy_uri_path.png)
---

## Step 2: Configure Amazon Athena

### 2.1 Launch Athena

- Go to **Services → Athena**
- Click **Get Started**
![Athena Get Started Page](./images/athena_getstarted.png)
### 2.2 Create a Workgroup

- Create a new **Workgroup** (you can also use `primary`)
- Switch to the new workgroup.
![Athena Create Workgorup](./images/athena_create_workgroup.png)
![Athena switch to workgroup](./images/athena_switch_to_workgroup.png)

### 2.3 Define Tables

**Option 1: Use Athena Query Editor**

```sql
CREATE EXTERNAL TABLE `salesdata`(
  `region` string,
  `country` string,
  `itemtype` string,
  `saleschannel` string,
  `orderpriority` string,
  `orderdate` string,
  `orderid` string,
  `shipdate` string,
  `unitssold` int,
  `unitprice` float,
  `unitcost` float,
  `totalrevenue` double,
  `totalcost` double,
  `totalprofit` double
)
ROW FORMAT DELIMITED 
  FIELDS TERMINATED BY ',' 
STORED AS INPUTFORMAT 'org.apache.hadoop.mapred.TextInputFormat' 
OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
LOCATION 's3://your-bucket/dataset/sales_data'
TBLPROPERTIES ('has_encrypted_data'='false');
```
## Option 2: Use Glue Crawler & UI Wizard

- Click **Connect data source** and proceed through:
  - **Default data source**
  - **Glue catalog** or **manual table definition**
  - **Input format**: CSV
  - Define columns **manually** or in **bulk**
![Athena Connect Datasource](./images/athena_connect_datasource.png)
![Athena Glue Datasource](./images/athena_connect_datasource_glue.png)
![Create Database](./images/athena_create_database.png)
![Bulk Add Columns](./images/athena_bulk_add_columns.png)
---

## 2.4 Validate Setup

- Open **Athena Query Editor** and test queries on your newly created table.
![Query in Athena](./images/athena_query.png)
---

## 2.5 Prepare for Datasphere Integration

### Create an IAM User

- Create a new **IAM user** for SAP Datasphere with **programmatic access**.
- Assign the following permissions:
  - `AmazonS3ReadOnlyAccess`
  - `AmazonAthenaFullAccess`
  - `AWSGlueConsoleFullAccess`
- Save the generated **Access Key** and **Secret Key**.
![AWS Permissions](./images/aws_permissions.png)
### Download SSL Certificates

- **AWS root CA** (.pem file)
![AWS root CA](./images/aws_root_cert.png)
- **Baltimore CyberTrust Root CA** (via S3 Object URL in browser)

---

## Step 3: Set Up SAP Datasphere Connection

### 3.1 Upload SSL Certificates

In SAP Datasphere:

- Go to **Configuration → Security**
- Upload both:
  - AWS root CA
  - S3 Baltimore certificate
  ![Upload Certs to Datasphere](./images/datasphere_upload_certs.png)

### 3.2 Create Amazon Athena Connection

- Go to **Space Management → Connections**
- Click **+ → Amazon Athena**
![Connect to Athena in Datasphere](./images/datasphere_connect_athena.png)
- Provide the following:
  - **Region** of Athena
  - **Workgroup** name
  - **Access Key** and **Secret Key**
- Click **Validate** to confirm the connection
![Athena enter connection details](./images/datasphere_enter_connection.png)

---

## Step 4: Build Models in SAP Datasphere

### 4.1 Create Virtual Table from Athena

- Open **Data Builder**
- Create a new **SQL View**
- Drag the Athena table to the canvas (it becomes a **virtual table**)
- Save and deploy the view
![Create a new view in Datasphere](./images/datasphere_new_view.png)


### 4.2 Create Local Table

- Define a **SAP HANA local table** inside Datasphere
- This table represents your internal SAP business data

### 4.3 Join & Analyze

- Create a **JOIN** view between the local and virtual (Athena) tables
- Build an **Analytical Model** or **Story**
- Deploy the model for interactive analysis
![Create a JOINED view in Datasphere](./images/datasphere_joined_view.png)
![Analytics](./images/datasphere_analytical_story.png)

---

## Observations & Monitoring

- Federated queries run from SAP Datasphere are logged in **Athena Query History**
- Use the Athena console to monitor:
  - Query performance
  - Query cost
  - Usage patterns
![Datasphere query history](./images/datasphere_query_history.png)
---

## Benefits

- **No data replication** – query live S3 data through Athena
- **Reduced cost** – no ETL or storage duplication required
- **Fast insights** – combine SAP and external data instantly
- **Enterprise-grade security** – IAM roles and TLS certificate authentication

---

## Further Resources

- [SAP Discovery Center Mission – Athena Integration](https://discovery-center.cloud.sap/missiondetail/3401/3441)
- For questions, contact: `ci_sce@sap.com`
