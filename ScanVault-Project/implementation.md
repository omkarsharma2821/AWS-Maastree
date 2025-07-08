## 🛠️ Step-by-Step Implementation: Automating Receipt Processing Using AWS

This guide walks you through the full setup of a serverless receipt processing pipeline using AWS services.

---

## 1️⃣ Create the S3 Bucket (for uploading receipts)

### ✅ Steps:
1. Go to the **S3 Console** → Click **Create Bucket**
2. Name your bucket (e.g., `storage-receipt-omkarsharma2821`)
3. Choose a region (e.g., `ap-south-1`)
4. Click **Create bucket**
5. Create Organizational folder inside bucket.
6. Name it **incoming** inside this you will upload files.

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3icc4oa0gfvnh6esy42c.png)


![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/givj5x1x1p3gkh0yznh8.png)


---

## 2️⃣ Create a DynamoDB Table (to store extracted data)

### ✅ Steps:
1. Go to the **DynamoDB Console** → Click **Create Table**
2. Table name: `Receipts-table`
3. Partition key: `receipt_id` (String)
4. Sort-key: `date` (String)
5. Click **Create**

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ap92lkq8vkshjjosads2.png)


![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fkaz6i00ipsphtezv5le.png)


---

## 3️⃣ Set Up Amazon SES (to send emails)

### ✅ Steps:
1. Go to **Amazon SES Console**
2. Verify your sender email under **Verified Identities**
3. (Optional) Verify recipient email if your account is in sandbox mode
4. Note the region (e.g., `ap-south-1`) – you’ll need it in your Lambda


![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jldyfpdjdqmn5hw7atrd.png)


![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/defvikewszhidxqlblnw.png)


---

## 4️⃣ Create IAM Role for Lambda Execution

### ✅ Steps:
1. Go to the **IAM Console** → Roles → Create Role
2. Choose **Lambda** as the use case
3. Attach the following policies:
  
```
   - `AmazonS3ReadOnlyAccess`
   - `AmazonTextractFullAccess`
   - `AmazonDynamoDBFullAccess`
   - `AmazonSESFullAccess`
   - `AWSLambdaBasicExecutionRole`
```
4. Name the role: `LambdaReceiptProcessingRole`


![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/nd2vf36787amdvumen4h.png)


![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6agolp4ok5p1jqgc2dal.png)


---

## 5️⃣ Create Lambda Function (processing engine)

### ✅ Steps:
1. Go to **AWS Lambda Console** → Click **Create Function**
2. Name: `ProcessReceiptFunction`
3. Runtime: **Python 3.9** or **Node.js**
4. Choose existing role → Select `LambdaReceiptProcessingRole`
5. Go to **configuration** tab inside **environment** varibales add this.
6. Go to the **Code** tab and add the pyhton code that I provide in **python.py** file and click **Deploy**.
7. Go to configuration tab > General configuration  > edit
8. Increase the timeout from 0.3 sec to 2 min for complex file.

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0ppkncdvrmqehby7zcek.png)


![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lyjwdlsgkmdmlfl4m3k3.png)


![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lztzzvz53r5nkteflc4y.png)



![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8dd0g01epq54qdxzawez.png)
 

## 6️⃣ Again go to S3 Bucket.
✅ Steps:
1. In the `Properties` Tab
2. Add the Event Notification 
3. Prefix : incoming/
4. Object creation : Select All object create events

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pnflppcl44h3fa0q1fwa.png)

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/owjjmzvial3se2ptdsjo.png)

> **_Wait for 30 sec and also check in spam folder for the mail....if you do not receive mail after 2 min go to the monitor tab in Lambda Function and check the log groups in cloudwatch. you can verify in dynamodb table as well below are the attached proofs_**

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qokguz90tc7vmitr2bvn.png)

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/lmya0q81j7k2ezkqxkv4.png)

![omkarsharma2821](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yssnalhbnoi1u9t8song.png)


✍️ **Author**: *Omkar Sharma*  
📬 *Feel free to connect on [LinkedIn](https://www.linkedin.com/in/omkarsharmaa/)*  
