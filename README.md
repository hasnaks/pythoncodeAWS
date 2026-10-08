# Secure Deployment of a Flask Web Application on AWS

## Project Overview

This project demonstrates the deployment of a Flask-based student registration web application on Amazon Web Services (AWS). The application allows users to register student details and upload photos. Student information is stored in an Amazon RDS MySQL database, while uploaded photos are stored in Amazon S3.

The project focuses on secure cloud deployment, environment-based configuration, access control, and communication between AWS services.

## Objectives

* Deploy a Flask web application on an Amazon EC2 instance.
* Configure Amazon RDS MySQL to store student registration details.
* Store uploaded student photos in an Amazon S3 bucket.
* Use IAM roles to grant the EC2 instance permission to access S3.
* Configure security groups to control network access.
* Use environment variables to keep database passwords out of application code.
* Verify application functionality and security configurations.

## Technologies Used

* **Python** — Application programming language
* **Flask** — Web application framework
* **PyMySQL** — MySQL database connectivity
* **Boto3** — AWS SDK for Python
* **Amazon EC2** — Application hosting
* **Amazon RDS MySQL** — Database service
* **Amazon S3** — Photo storage
* **AWS IAM** — Identity and access management
* **Amazon VPC** — Network isolation and configuration
* **Gunicorn and systemd** — Application serving and service management
* **Git and GitHub** — Version control and source code management

## System Architecture

The application uses the following AWS architecture:

1. Users access the Flask application through the public IP address of the EC2 instance.
2. The Flask application processes student registration details.
3. Student information is stored in the Amazon RDS MySQL database.
4. Uploaded photos are stored in the Amazon S3 bucket.
5. The EC2 instance uses an attached IAM role to access S3 without storing AWS access keys in the application.
6. Security groups control access between the web server and database.

```text
User / Web Browser
        |
        | HTTP
        v
Amazon EC2 Instance
Flask + Gunicorn
        |
        | MySQL - Port 3306
        v
Amazon RDS MySQL
(Private Access)

Amazon EC2 Instance
        |
        | IAM Role / Boto3
        v
Amazon S3 Bucket
(Photo Storage)
```

## Application Features

* Student registration form
* Collection of student name, email, and course
* Student photo upload
* Storage of registration details in MySQL
* Storage of uploaded photos in Amazon S3
* Confirmation after successful registration

## Security Measures

* The database password is read from the `DB_PASSWORD` environment variable.
* The RDS database is configured as not publicly accessible.
* The RDS security group allows MySQL traffic from the EC2 security group.
* SSH access is restricted to the configured client IP address.
* The EC2 instance uses an IAM role for S3 access.
* S3 permissions are configured through IAM policies and bucket settings.
* Sensitive credentials must not be committed to the Git repository.

**Important:** Configure the `DB_PASSWORD` environment variable on the deployment server before starting the application. Never publish the actual database password, AWS access keys, private keys, or other credentials in this repository.

## Configuration

The application requires the following database settings:

| Variable      | Purpose                             |
| ------------- | ----------------------------------- |
| `DB_PASSWORD` | Password for the RDS MySQL database |

The database host, database name, and username are configured in `app.py`. Update these settings if deploying the application in a different AWS environment.

The S3 bucket name is also configured in the application and should match the bucket used for deployment.

## Running the Application Locally

### 1. Clone the repository

```bash
git clone https://github.com/hasnaks/pythoncodeAWS.git
cd pythoncodeAWS
```

### 2. Create a virtual environment

```bash
python -m venv venv
```

Activate it on Windows:

```powershell
.\venv\Scripts\Activate.ps1
```

Activate it on Linux:

```bash
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure the database password

Linux:

```bash
export DB_PASSWORD='YOUR_DATABASE_PASSWORD'
```

Windows PowerShell:

```powershell
$env:DB_PASSWORD = "YOUR_DATABASE_PASSWORD"
```

Replace the placeholder with your database password locally. Do not commit it to GitHub.

### 5. Run the application

```bash
python app.py
```

Open `http://127.0.0.1:5000` in your browser.

A working AWS configuration, network access to the database, and appropriate IAM permissions are required for registration and photo uploads to succeed.

## Testing and Verification

The deployment can be verified by checking:

* The registration form loads successfully.
* A student registration returns a success message.
* The submitted student details appear in the RDS MySQL table.
* The uploaded photo appears in the S3 bucket.
* The RDS database cannot be accessed directly from an unauthorized external connection.
* S3 bucket listing is not publicly available unless explicitly required and securely configured.

## Repository

GitHub fork: https://github.com/hasnaks/pythoncodeAWS

## Author

**Hasna K S**

B.Tech Computer Science and Engineering

## Conclusion

This project demonstrates how a Flask web application can be deployed on AWS using EC2, RDS MySQL, S3, IAM, and VPC security configurations. It applies environment-based secret management, controlled database access, and role-based permissions to improve the security and reliability of the application.
