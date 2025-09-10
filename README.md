# Renoview AWS Serverless Application

## Project Overview
A serverless web application for construction site photo management built using AWS services. This project demonstrates the implementation of cloud architecture using:
- S3 for static website hosting and image storage
- Lambda for serverless backend processing
- API Gateway for RESTful endpoints

## Features
- Static website hosting on S3
- Secure image upload functionality
- Image gallery with preview
- Secure API endpoints
- Serverless architecture

## AWS Architecture
![Renoview Architecture](Renoview%20Architecture.drawio.png)

### S3 Buckets:
- `renoview-app-storage`: Hosts the static website
- `renoview-images`: Stores uploaded images

### Lambda Functions:
- Image processing and API handling
- CORS support
- Secure file uploads

### API Gateway Endpoints:
- GET /projects
- GET /images
- GET /getUploadUrl

## Implementation Details

### Frontend
- HTML5/CSS3/JavaScript
- Drag-and-drop image upload
- Real-time image preview
- Responsive design

### Backend
- Serverless architecture
- RESTful API design
- Secure file handling
- CORS configuration

## Live Demo
Website URL: [(http://renoview-app-storage.s3-website-us-east-1.amazonaws.com/)]

## Screenshots
[Screenshots to be added]

## Setup Instructions
Detailed setup instructions can be found in [docs/setup-guide.md](docs/setup-guide.md)

## Author
Max Buma
