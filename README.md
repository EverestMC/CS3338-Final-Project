# Final Project: Auto-File System

## Course
**CS-3338 — Software and Engineering Tools**  
California State University, Los Angeles

## Team Members
- @EverestMC  
- @Ceziech  
- @Jamschowder  
- @Miguel C  
- @Narfrogerson23  

## Project Overview
The **Auto-File System** is a web-based tool developed to assist government workers in efficiently managing and processing large-scale document uploads. Through a secure login portal, users can upload PDF documents in bulk. These files are automatically parsed to extract key information and stored in a structured SQL database.

This system leverages Docker to enable scalable deployments, running multiple instances of both the application and parsing services to handle high volumes of files quickly and accurately. The backend is powered by Spring Boot, and cloud-based storage (provider to be determined) will host the SQL database for remote accessibility.

## Features
- Secure web-based login for authorized personnel
- Bulk PDF upload support (up to 500GB per upload)
- Real-time file parsing and upload progress indicators
- Intelligent parsing for metadata extraction and data organization
- Automatic assignment of primary and foreign keys for database storage
- Upload summary with error detection and logging

## Technologies Used
- **Docker** — For containerization and deployment of services
- **Spring Boot** — Backend application framework
- **Cloud-Based SQL Storage** — Platform TBD (e.g., AWS, Azure)
- **Web Interface** — Browser-based access for end users

## Installation & Setup
To run and demonstrate the Auto-File System locally:

1. Clone the repository:
2. Build and run Docker containers
3. Once running, access the system via your web browser
4. Log in using the appropriate credentials

## Usage Instructions
1. After logging in, navigate to the upload section of the site.
2. Select and upload one or more PDF files (up to 500GB per batch).
3. The system will display live progress as files are parsed and uploaded.
4. After processing is complete, a report will show:
  - Total number of files uploaded successfully
  - Files with missing or incomplete data
  - Files rejected due to invalid format, duplication, or runtime errors

## Notes
- Cloud database hosting is in progress and will be finalized before deployment.
- Ensure Docker is installed and running before executing setup steps(Need to confrim steps, commandline prompts, etc.)
- For testing purposes, smaller upload sizes are recommended unless on a high-performance machine(which we do not have)

## IMPORTANT: THIS README IS NOT FINAL
