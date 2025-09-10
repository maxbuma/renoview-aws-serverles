# RenovieW AWS Serverless Application

A serverless web application for construction site photo management built with AWS services.

## Architecture Overview

![Architecture Diagram](./images/architecture.png)

This application uses several AWS services:
- S3 for static website hosting and image storage
- Lambda for serverless backend logic
- API Gateway for RESTful API endpoints

## Features

- Static website hosting
- Secure image upload
- Image gallery with preview
- Serverless backend
- RESTful API endpoints

## Technical Implementation

### AWS Services Used

1. **S3 Buckets**
   - Static website hosting
   - Image storage
   - Public/private access configuration

2. **Lambda Functions**
   - Image processing
   - API handling
   - CORS support

3. **API Gateway**
   - RESTful endpoints
   - CORS configuration
   - Lambda integration

### Frontend Implementation

- HTML5/CSS3/JavaScript
- Async/await patterns
- Fetch API
- File upload handling
- Image preview functionality

## Code Examples

### Lambda Function
```javascript
import { S3Client, PutObjectCommand, ListObjectsV2Command, GetObjectCommand } from '@aws-sdk/client-s3';
import { getSignedUrl } from '@aws-sdk/s3-request-presigner';

const s3Client = new S3Client({ region: 'us-east-1' }); // Make sure this matches your region
const BUCKET_NAME = 'renoview-images'; // Your bucket name

export const handler = async (event) => {
    console.log('Event:', JSON.stringify(event, null, 2));
    
    const headers = {
        "Access-Control-Allow-Origin": "*",
        "Access-Control-Allow-Methods": "GET,POST,OPTIONS",
        "Access-Control-Allow-Headers": "Content-Type,X-Amz-Date,Authorization,X-Api-Key,X-Amz-Security-Token",
        "Content-Type": "application/json"
    };

    try {
        // Handle OPTIONS requests
        if (event.httpMethod === 'OPTIONS') {
            return {
                statusCode: 200,
                headers: headers,
                body: JSON.stringify({ message: 'CORS enabled' })
            };
        }

        // Handle different endpoints
        switch (event.path) {
            case '/projects':
                return {
                    statusCode: 200,
                    headers: headers,
                    body: JSON.stringify({
                        message: "Hello from Lambda!",
                        timestamp: new Date().toISOString()
                    })
                };

            case '/getUploadUrl':
                const imageKey = `images/${Date.now()}.jpg`;
                const putCommand = new PutObjectCommand({
                    Bucket: BUCKET_NAME,
                    Key: imageKey,
                    ContentType: 'image/*'
                });
                
                const uploadUrl = await getSignedUrl(s3Client, putCommand, { expiresIn: 300 });
                
                return {
                    statusCode: 200,
                    headers: headers,
                    body: JSON.stringify({
                        uploadUrl,
                        imageKey
                    })
                };

            case '/images':
                // List all images in the bucket
                const listCommand = new ListObjectsV2Command({
                    Bucket: BUCKET_NAME,
                    Prefix: 'images/'
                });
                
                const listResponse = await s3Client.send(listCommand);
                
                // Generate signed URLs for each image
                const images = await Promise.all((listResponse.Contents || []).map(async (item) => {
                    const getCommand = new GetObjectCommand({
                        Bucket: BUCKET_NAME,
                        Key: item.Key
                    });
                    const url = await getSignedUrl(s3Client, getCommand, { expiresIn: 3600 });
                    return {
                        url,
                        timestamp: item.LastModified,
                        key: item.Key
                    };
                }));

                return {
                    statusCode: 200,
                    headers: headers,
                    body: JSON.stringify(images)
                };

            default:
                return {
                    statusCode: 404,
                    headers: headers,
                    body: JSON.stringify({
                        message: "Route not found",
                        path: event.path
                    })
                };
        }
    } catch (error) {
        console.error('Error:', error);
        return {
            statusCode: 500,
            headers: headers,
            body: JSON.stringify({ 
                message: "Error in Lambda",
                error: error.message 
            })
        };
    }
};
