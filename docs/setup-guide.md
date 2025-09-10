# Setup Guide

## Prerequisites
- AWS Account
- Basic understanding of AWS services
- GitHub account

## AWS Configuration Steps

### 1. S3 Setup
#### Website Bucket
1. Create bucket `renoview-app-storage`
   - Enable static website hosting
   - Configure public access
   - Add bucket policy for public read

#### Image Storage Bucket
1. Create bucket `renoview-images`
   - Configure CORS
   - Set appropriate permissions

### 2. Lambda Function Setup
1. Create new Lambda function
   - Runtime: Node.js 20.x
   - Handler: index.handler
   - Configure IAM role with S3 access

### 3. API Gateway Setup
1. Create REST API
2. Configure endpoints:
   - /projects
   - /images
   - /getUploadUrl
3. Enable CORS
4. Deploy API

## Frontend Deployment
1. Upload website files to S3
2. Configure static website hosting
3. Update API endpoint in code

## Testing
1. Access website through S3 endpoint
2. Test image upload functionality
3. Verify image gallery display
