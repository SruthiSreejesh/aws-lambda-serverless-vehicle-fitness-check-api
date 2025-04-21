### aws-lambda-serverless-vehicle-fitness-check-api

# 🚗 Vehicle Fitness Test API using AWS Lambda and MariaDB

📌 Project Overview

This project demonstrates a serverless API built using AWS Lambda, Python, and MariaDB to manage and retrieve vehicle records for fitness inspection purposes. The system is designed to return vehicle details such as Owner Name, Vehicle Number, Make, Model, Insurance Details, and Inspection Status using a secure and scalable architecture.

## 💡 Key Features
🔁 Serverless architecture: Deployed via AWS Lambda with API access through Lambda Function URLs.
 
📦 Data storage: Vehicle data is stored in a MariaDB database hosted on an Amazon EC2 instance.

🌍 Flexible query: Users can query vehicle records by owner_id directly via the endpoint.

🔐 Environment-specific configuration: Database credentials are securely managed using environment variables in Lambda.

🧪 Fitness test use-case: Each vehicle record includes an inspection_status field with values like Passed or Failed, suitable for real-world inspection workflows.

📊 Structured JSON response: Clean and structured responses make the data easily consumable for frontend applications or other APIs.

 ## 🧳 Prerequisites
 
   
  AWS Account with access to EC2, S3, Lambda, IAM

  A MariaDB database running on an EC2 instance

  Python 3.11 environment on the EC2 instance

  IAM roles with appropriate policies


## 🧱 Database Table Structure
The vehicle_records table includes the following columns:

owner_name (VARCHAR)

owner_id (VARCHAR, used for querying)

vehicle_number (VARCHAR)

make (VARCHAR)

model (VARCHAR)

insurance_details (VARCHAR)

inspection_status (VARCHAR – e.g., Passed, Failed)

date_of_first_registration was initially included but later removed to simplify the response schema.



## 🛠️ Technologies Used
 AWS Lambda – Serverless compute

 Amazon EC2 – Hosting the MariaDB database

 MariaDB – Relational database for storing vehicle data

 Python (boto3, mysql-connector) – Lambda function runtime

 S3 – Used for uploading zipped deployment packages

 CloudWatch Logs – For function monitoring and debugging


## 🚀 How It Works
 A Lambda Function URL receives a GET request with the owner_id as the path parameter.

 The function connects to the MariaDB instance using credentials stored in environment variables.

 It executes a SQL query to fetch the matching vehicle record.

 The result is converted to JSON and returned to the client.

 If no match is found, a 404 Not Found message is returned.

 ###  ✅ Example API Usage
#### Endpoint:
    https://<lambda-function-url>/<owner_id>

#### Example Call:
    GET https://xyz123.lambda-url.region.on.aws/OWNR007

#### Sample JSON Response:
    {
    "owner_name": "Ahmed Al-Farsi",
    "owner_id": "OWNR007",
    "vehicle_number": "KL 11 F 6858",
     "make": "Toyota",
     "model": "Camry",
     "insurance_details": "Takaful Co. - Valid until 2025-05-31",
     "inspection_status": "Passed"
     }
     


### 🛠️ Database Setup with MariaDB
  Follow these steps to set up your MariaDB database for the Vehicle Fitness Test API:

#### 📦 1. Install MariaDB

     sudo apt update
     sudo apt install mariadb-server -y

#### 🔄 2. Enable and Start MariaDB Service

    sudo systemctl enable mariadb
    sudo systemctl start mariadb

#### 🔐 3. Secure Your MariaDB Installation (Optional but Recommended)
    sudo mysql_secure_installation

Follow the prompts to set a root password and secure your installation.

#### 🧑‍💻 4. Login to MariaDB

    sudo mysql -u root -p

#### 🗃️ 5. Create Database and User
    CREATE DATABASE vehicle;

    CREATE USER 'vehicleinspctr'@'%' IDENTIFIED BY 'yourpassword';

    GRANT ALL PRIVILEGES ON vehicle.* TO 'vehicleinspctr'@'%';

    FLUSH PRIVILEGES;

💡 Replace 'yourpassword' with a strong password.

#### 🏗️ 6. Use the Database and Create Table
    USE vehicle;

    CREATE TABLE vehicle_records (
    owner_name VARCHAR(100),
    owner_id VARCHAR(20) PRIMARY KEY,
    vehicle_number VARCHAR(20),
    make VARCHAR(50),
    model VARCHAR(50),
    insurance_details VARCHAR(150),
    inspection_status ENUM('Passed', 'Failed')
    );

#### 📝 7. Insert Sample Data

    INSERT INTO vehicle_records (owner_name, owner_id, vehicle_number, make, model, insurance_details, inspection_status) VALUES
    ('Ali Hassan', 'OWNR001', 'KSA 23 B 1098', 'Toyota', 'Land Cruiser', 'Policy#783429 - Gulf Insurance', 'Passed'),
    ('Fatima Noor', 'OWNR002', 'UAE 12 D 9987', 'Nissan', 'Patrol', 'Policy#563872 - Noor Takaful', 'Passed'),
    ('Omar Said', 'OWNR003', 'OMN 44 G 4455', 'Hyundai', 'Tucson', 'Policy#904231 - Dhofar Insurance', 'Failed'),
    ('Giovanni Rossi', 'OWNR004', 'ITA 21 K 7453', 'Fiat', 'Panda', 'Policy#112234 - AXA Italy', 'Passed'),
    ('Priya Mehra', 'OWNR005', 'IND 33 H 7782', 'Maruti Suzuki', 'Baleno', 'Policy#991223 - LIC India', 'Passed'),
    ('Ayesha Karim', 'OWNR006', 'QAT 88 J 6654', 'Kia', 'Sportage', 'Policy#778823 - Qatar Insurance', 'Failed'),
    ('Liam O’Connor', 'OWNR007', 'IRL 71 Z 1267', 'Volkswagen', 'Passat', 'Policy#661892 - Irish Life', 'Passed'),
    ('Chen Wei', 'OWNR008', 'CHN 05 M 4567', 'Geely', 'Coolray', 'Policy#889911 - PICC', 'Passed'),
    ('Yusuf Elmas', 'OWNR009', 'TUR 16 L 3948', 'Renault', 'Symbol', 'Policy#223344 - Anadolu Sigorta', 'Failed'),
    ('Leila Nader', 'OWNR010', 'IRN 90 T 8321', 'Peugeot', 'Pars', 'Policy#765421 - Asia Insurance', 'Passed');


## 🚀 Deployment Instructions
 This section outlines the end-to-end process for deploying the Vehicle Fitness Test API on AWS using Lambda, S3, and EC2.

### 📦 Create the Deployment Package
1. create requirements.txt file and add necessary modules
    vim requirements.txt
   cat requirements.txt
   mysql-connector-python
   
3. Install dependencies locally (inside the EC2 instance):
    pip install -r requirements.txt -t .

4. Add your Lambda handler script:
    vim lambda_function.py

5. Zip all files for deployment:
    zip -r lambda_function.zip .


### 📁 Project Structure on EC2

    ls -ltr

total 57024

-rw-r--r-- 1 user1 user1       23 Apr 21 07:25 requirements.txt 

-rwxr-xr-x 1 user1 user1 23970824 Apr 21 07:29 _mysql_connector.cpython-312-x86_64-linux-gnu.so

drwxr-xr-x 5 user1 user1     4096 Apr 21 07:29 mysql/

drwxr-xr-x 3 user1 user1     4096 Apr 21 07:29 mysql_connector_python-9.3.0.dist-info/

-rw-r--r-- 1 user1 user1     2205 Apr 21 10:18 lambda_function.py

-rw-r--r-- 1 user1 user1 34394767 Apr 21 10:30 lambda_function.zip

#### 📦 Lambda Function Package

 The lambda_function.zip contains:

 The Lambda handler (lambda_function.py), which handles the core logic for processing vehicle inspection data.

 All dependencies listed in requirements.txt, including the mysql-connector-python module required for connecting to the MariaDB data.


## ☁️ Upload to S3 Bucket from EC2
 The EC2 instance is granted permissions to upload directly to S3 without using AWS CLI by assigning it a proper IAM role.

### ✅ IAM Role Required for EC2 Instance:

 Attach the following IAM policy to the EC2's instance profile:
  
 Use the following IAM policy to allow your EC2 instance to upload the zip to S3:
<pre> ```json { "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Action": [ "s3:PutObject", "s3:GetObject", "s3:ListBucket" ], "Resource": [ "arn:aws:s3:::<your-bucket-name>", "arn:aws:s3:::<your-bucket-name>/*" ] } ] } ``` </pre>

#### 📤 Then upload the package:
    aws s3 cp lambda_function.zip s3://<your-bucket-name>/lambda-function/

## 🔄 Deploy Lambda from S3

 1. Go to AWS Lambda Console.

 2. Create a new function or select an existing one.

 3. Under Code, choose Upload from → Amazon S3.

 4. Provide the S3 URL of the uploaded ZIP file.

 5. Under Runtime, select Python 3.12.

 6. Set Environment Variables:

     DB_HOST

     DB_USER

     DB_PASSWORD

     DB_DATABASE

 7. Assign a basic Lambda execution role (with CloudWatch logging at minimum).
   
## 🌐 Expose Function via URL
 Go to Configuration → Function URL.

 Enable Auth Type: NONE for public testing or IAM for secure access.

 Copy the URL and test using your browser or a tool like Postman.

#### ✅ Test Example

    GET https://xyz123.lambda-url.region.on.aws/OWNR007

##### Expected Response:

    {
      "owner_name": "Ahmed Al-Farsi",
      "owner_id": "OWNR007",
      "vehicle_number": "KL 11 F 6858",
      "make": "Toyota",
      "model": "Camry",
      "insurance_details": "Takaful Co. - Valid until 2025-05-31",
      "inspection_status": "Passed"
     }


