# Final Project

## Course
**CS-3338 — Software and Engineering Tools**  
California State University, Los Angeles

## Team Members
- @EverestMC  
- @Ceziech  
- @Jamschowder  
- @Miguel C  
- @Narfrogerson23  

# Project Overview(Simulation)
As part of the Santa Barbara Public Defender’s transition to a fully paperless case management system, our team developed a simulated environment for automating the discovery file workflow using a serverless cloud-native architecture.

Each month, the Public Defender’s Office ingests over 5–6 terabytes of electronically stored information (ESI). To manage this volume efficiently, the system parses uploaded PDFs for Bates stamps, renames the files using a consistent format, and transfers them to appropriate folders in Box.com based on case metadata.

In earlier stages, a standalone desktop tool was developed to help with Bates number parsing and file renaming. However, this approach required users to download, process, and reupload files manually, a workflow prone to inefficiency and error. To solve this, the application was fully migrated to the cloud using AWS Lambda and related services.

This project simulates the entire cloud-native pipeline in a local environment using Docker Compose. It enables testing of the end-to-end automation process, webhook handling, Bates number extraction, file renaming, and error notifications—without relying on actual AWS or Box.com infrastructure. This simulation helps developers test, extend, and debug the system in isolation before deploying it to the cloud.

This system leverages Docker to enable scalable deployments, running multiple instances of both the application and parsing services to handle high volumes of files quickly and accurately. The backend is powered by Spring Boot, and cloud-based storage (provider to be determined) will host the SQL database for remote accessibility.

# Features
- 📄 **Bates Stamp Extraction**  
  Automatically scans uploaded PDF files for Bates numbers, ensuring they are sequential and correctly formatted.
- 🔁 **Automated File Renaming**  
  Renames discovery files using extracted metadata (Bates range, case number, disc number) to ensure consistency and traceability.
- 📂 **Box Folder Integration**  
  Simulates the relocation of files into specific Box folders based on parsed case information, mimicking real-world folder structures.
- 📧 **Error Notification System**  
  Sends email notifications (via a simulated SES) to alert users of both critical and non-critical processing errors.
- 🔐 **Secure Credential Simulation**  
  Uses environment variables to replicate AWS Secrets Manager functionality for safely storing sensitive Box API credentials.
- ⚙️ **Webhook-Triggered Processing**  
  Models event-driven Lambda invocation by simulating webhook calls from Box that initiate the entire processing pipeline.
- 🧪 **Containerized Local Simulation**  
  Allows the entire serverless application stack to be run and tested locally using Docker and Docker Compose.

# Technologies Used

## Core Languages & Frameworks
- **Python 3.11** – Used to write all Lambda function logic.
## Containerization & Orchestration
- **Docker** – Creates isolated environments for each simulated service.
- **Docker Compose** – Orchestrates the multi-container application for local testing.
## AWS Cloud Services (Simulated)
- **AWS Lambda** – Represented by individual Python scripts running in containers.
- **AWS Secrets Manager** – Simulated using environment variables.
- **AWS SES (Simple Email Service)** – Simulated using MailHog.
## Supporting Libraries
- **PyMuPDF** – Extracts text and Bates numbers from PDF files.
- **Requests** – Handles HTTP communication with Box API endpoints.
- **box-python-sdk-gen** – Simulates communication with the Box.com API.
## Mocking & Testing Tools
- **Prism by Stoplight** – Mocks the Box.com REST API using an OpenAPI specification.
- **MailHog** – Provides a local SMTP server and web UI to simulate email sending and viewing.

These tools work together to replicate a realistic cloud-based document processing pipeline, enabling development and debugging without accessing production services.

# Installation & Setup Guide

This section describes how to set up the simulated cloud-native workflow locally using Docker and Docker Compose.

## Prerequisites

Ensure you have the following installed:
- [Docker](https://www.docker.com/products/docker-desktop)
- [Docker Compose](https://docs.docker.com/compose/install/)
- Git (optional for cloning the repository)

## Setup Instructions

### 1. Clone or Download the Repository
```
git clone https://github.com/your-org/box-discovery-simulation.git
cd box-discovery-simulation
```
### 2. Add Your Lambda Function Code
Place your Python scripts inside the lambda_functions/ directory. Each file should match the Lambda function name and contain the corresponding logic.

### 3. Start the Simulation
Run the following command to start all services:
```
docker-compose up --build
```
This will launch:
- Simulated Lambda containers
- A mock Box API service (Prism)
- A local SMTP server with a web UI (MailHog)

### 5. Access Services
- MailHog UI (for viewing error emails): http://localhost:8025
- Mock Box API (if using Prism): http://localhost:4010

# Usage Instructions

This section explains how to run and interact with the simulation once it is set up locally.

## Step 1: Start the Simulation
From the root of the project directory, start all services with:
```
docker-compose up --build
```
This will initialize:
- Simulated AWS Lambda containers
- A mocked Box.com API service (if configured)
- A local email testing server using MailHog

## Step 2: Simulate a File Upload Event
Trigger the workflow by:
Manually invoking the BoxInputFunction.py script with a sample payload, 
*OR* by modifying the script to simulate a webhook event from Box containing:
- file_id
- file_name
- folder_id
- access_token
This simulates Box detecting a new file upload and starts the pipeline.

## Step 3: Observe Lambda Chain Execution
Functions execute in the following sequence:
```
BoxInputFunction
  ↓
BoxFolderGetter
  ↓
DiscoveryBatesNamer
  ↓
BoxFileUpdater
```
If an error occurs at any point, the BoxErrorNotification function is triggered.

## Step 4: View Logs & Results
You can view container logs to see each function’s output:
```
docker logs box_input_function
docker logs discovery_bates_namer
# ...repeat for other services
```
## Step 5: Check Email Notifications
Visit MailHog's web interface to see simulated email notifications for any errors:
👉 http://localhost:8025
Emails include:
- The file name that failed
- Type of error (e.g. missing Bates number, incorrect format)
- A link to the file in Box (simulated)

## Step 6: Test With Your Own Files
Place sample PDFs in the appropriate test directory or modify Lambda code to pull from your sample_data/ folder.

Ensure files are named with the format:
```
PDCaseNumber_DiscNumber.pdf
Example: PD251234_02.pdf
```
This will ensure correct parsing by the system.

# Final Notes

This simulation project replicates the cloud-native workflow developed by the Santa Barbara Public Defender’s Office for automating the processing of discovery documents in a secure, scalable, and efficient manner.

By using Docker and Docker Compose to mirror AWS Lambda behavior and Box.com integration, this simulation provides a safe environment for:

- Testing and validating new features
- Debugging without cloud costs
- Training developers and demonstrating pipeline flow

While it does not replace the actual AWS-hosted system, it models key components including:

- Event-driven processing
- PDF Bates stamp extraction
- Metadata-based renaming and routing
- Error handling and notifications

## Future Improvements

- Integration with a real Box.com sandbox environment
- Support for non-PDF formats and legacy Bates formats
- Full CI/CD deployment pipeline for automated testing
- API Gateway and real webhook trigger simulation

## IMPORTANT: THIS README IS FOR SIMULATION AND SCHOOL PURPOSES
