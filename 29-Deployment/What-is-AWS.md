# What is AWS?

## Definition

AWS (Amazon Web Services) is a **comprehensive cloud computing platform** offering computing power, storage, databases, and services for hosting, deploying, and scaling applications.

## Core AWS Services

| Service | Purpose |
|---------|---------|
| EC2 | Virtual servers (compute) |
| S3 | Object storage |
| Lambda | Serverless functions |
| RDS | Managed databases |
| CloudFront | CDN distribution |
| Route 53 | DNS management |
| IAM | Access management |

## Code Examples

### 1. AWS SDK Setup

```javascript
// Install AWS SDK
// npm install @aws-sdk/client-s3

import { S3Client, PutObjectCommand, GetObjectCommand } from '@aws-sdk/client-s3';

// Configure client
const s3Client = new S3Client({
  region: 'us-east-1',
  credentials: {
    accessKeyId: process.env.AWS_ACCESS_KEY_ID,
    secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY
  }
});

// Upload file
async function uploadFile(bucket, key, body) {
  const command = new PutObjectCommand({
    Bucket: bucket,
    Key: key,
    Body: body,
    ContentType: 'image/jpeg'
  });

  return s3Client.send(command);
}

// Download file
async function downloadFile(bucket, key) {
  const command = new GetObjectCommand({
    Bucket: bucket,
    Key: key
  });

  const response = await s3Client.send(command);
  return response.Body.transformToByteArray();
}
```

### 2. Lambda Function

```javascript
// lambda/handler.js
export const handler = async (event, context) => {
  console.log('Event:', JSON.stringify(event, null, 2));

  try {
    // Parse request
    const { httpMethod, path, body } = event;

    // Handle different routes
    switch (`${httpMethod} ${path}`) {
      case 'GET /users':
        return {
          statusCode: 200,
          headers: {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*'
          },
          body: JSON.stringify({
            users: ['Alice', 'Bob', 'Charlie']
          })
        };

      case 'POST /users':
        const newUser = JSON.parse(body);
        // Save to DynamoDB or other service
        return {
          statusCode: 201,
          body: JSON.stringify(newUser)
        };

      default:
        return {
          statusCode: 404,
          body: JSON.stringify({ error: 'Not found' })
        };
    }
  } catch (error) {
    console.error('Error:', error);
    return {
      statusCode: 500,
      body: JSON.stringify({ error: 'Internal server error' })
    };
  }
};
```

### 3. S3 Static Website Hosting

```javascript
// Deploy static site to S3
import { S3Client, PutObjectCommand, DeleteObjectCommand } from '@aws-sdk/client-s3';
import { CloudFrontClient, CreateInvalidationCommand } from '@aws-sdk/client-cloudfront';
import fs from 'fs';
import path from 'path';

const s3 = new S3Client({ region: 'us-east-1' });
const cloudfront = new CloudFrontClient({ region: 'us-east-1' });

async function deployToS3(distDir, bucketName) {
  const files = getAllFiles(distDir);

  for (const file of files) {
    const command = new PutObjectCommand({
      Bucket: bucketName,
      Key: path.relative(distDir, file),
      Body: fs.createReadStream(file),
      ContentType: getContentType(file)
    });

    await s3.send(command);
    console.log(`Uploaded: ${file}`);
  }

  // Invalidate CloudFront cache
  const invalidation = new CreateInvalidationCommand({
    DistributionId: process.env.CLOUDFRONT_DISTRIBUTION_ID,
    InvalidationBatch: {
      CallerReference: Date.now().toString(),
      Paths: {
        Quantity: 1,
        Items: ['/*']
      }
    }
  });

  await cloudfront.send(invalidation);
}
```

### 4. DynamoDB Operations

```javascript
import { DynamoDBClient } from '@aws-sdk/client-dynamodb';
import {
  DynamoDBDocumentClient,
  PutCommand,
  GetCommand,
  QueryCommand,
  UpdateCommand,
  DeleteCommand
} from '@aws-sdk/lib-dynamodb';

const client = new DynamoDBClient({ region: 'us-east-1' });
const docClient = DynamoDBDocumentClient.from(client);

// Create item
async function createUser(user) {
  const command = new PutCommand({
    TableName: 'Users',
    Item: {
      userId: user.id,
      name: user.name,
      email: user.email,
      createdAt: new Date().toISOString()
    }
  });

  return docClient.send(command);
}

// Get item
async function getUser(userId) {
  const command = new GetCommand({
    TableName: 'Users',
    Key: { userId }
  });

  const response = await docClient.send(command);
  return response.Item;
}

// Query items
async function getUsersByStatus(status) {
  const command = new QueryCommand({
    TableName: 'Users',
    IndexName: 'StatusIndex',
    KeyConditionExpression: 'status = :status',
    ExpressionAttributeValues: {
      ':status': status
    }
  });

  const response = await docClient.send(command);
  return response.Items;
}

// Update item
async function updateUser(userId, updates) {
  const command = new UpdateCommand({
    TableName: 'Users',
    Key: { userId },
    UpdateExpression: 'SET #name = :name, email = :email',
    ExpressionAttributeNames: {
      '#name': 'name'
    },
    ExpressionAttributeValues: {
      ':name': updates.name,
      ':email': updates.email
    }
  });

  return docClient.send(command);
}

// Delete item
async function deleteUser(userId) {
  const command = new DeleteCommand({
    TableName: 'Users',
    Key: { userId }
  });

  return docClient.send(command);
}
```

### 5. API Gateway with Lambda

```javascript
// serverless.yml (Serverless Framework)
service: my-api

provider:
  name: aws
  runtime: nodejs20.x
  region: us-east-1
  environment:
    TABLE_NAME: ${self:service}-users

functions:
  getUser:
    handler: handler.getUser
    events:
      - http:
          path: /users/{id}
          method: get

  createUser:
    handler: handler.createUser
    events:
      - http:
          path: /users
          method: post

  listUsers:
    handler: handler.listUsers
    events:
      - http:
          path: /users
          method: get

iam:
  role:
    statements:
      - Effect: Allow
        Action:
          - dynamodb:GetItem
          - dynamodb:PutItem
          - dynamodb:Query
        Resource:
          - !GetAtt UsersTable.Arn
```

### 6. S3 Presigned URLs

```javascript
import { S3Client, GetObjectCommand } from '@aws-sdk/client-s3';
import { getSignedUrl } from '@aws-sdk/s3-request-presigner';

const s3 = new S3Client({ region: 'us-east-1' });

// Generate presigned URL for download
async function getPresignedUrl(bucket, key, expiresIn = 3600) {
  const command = new GetObjectCommand({
    Bucket: bucket,
    Key: key
  });

  const url = await getSignedUrl(s3, command, { expiresIn });
  return url;
}

// Usage
const downloadUrl = await getPresignedUrl('my-bucket', 'files/document.pdf');
// Client can use this URL to download directly
```

### 7. CloudFront CDN

```javascript
// Deploy with CloudFront
import { CloudFrontClient, CreateInvalidationCommand } from '@aws-sdk/client-cloudfront';

const cloudfront = new CloudFrontClient({ region: 'us-east-1' });

// Invalidate cache after deployment
async function invalidateCache(distributionId, paths = ['/*']) {
  const command = new CreateInvalidationCommand({
    DistributionId: distributionId,
    InvalidationBatch: {
      CallerReference: Date.now().toString(),
      Paths: {
        Quantity: paths.length,
        Items: paths
      }
    }
  });

  return cloudfront.send(command);
}
```

### 8. Environment Variables and Secrets

```javascript
// Using AWS Systems Manager Parameter Store
import { SSMClient, GetParameterCommand } from '@aws-sdk/client-ssm';

const ssm = new SSMClient({ region: 'us-east-1' });

async function getParameter(name) {
  const command = new GetParameterCommand({
    Name: name,
    WithDecryption: true
  });

  const response = await ssm.send(command);
  return response.Parameter.Value;
}

// Or use AWS Secrets Manager
import { SecretsManagerClient, GetSecretValueCommand } from '@aws-sdk/client-secrets-manager';

const secrets = new SecretsManagerClient({ region: 'us-east-1' });

async function getSecret(secretName) {
  const command = new GetSecretValueCommand({
    SecretId: secretName
  });

  const response = await secrets.send(command);
  return JSON.parse(response.SecretString);
}
```

## Common Use Cases

```javascript
// Serverless API
exports.handler = async (event) => {
  // Process request
  return {
    statusCode: 200,
    body: JSON.stringify({ data: 'response' })
  };
};

// File upload to S3
const upload = async (file) => {
  const command = new PutObjectCommand({
    Bucket: 'my-uploads',
    Key: `uploads/${Date.now()}-${file.name}`,
    Body: file.buffer,
    ContentType: file.mimetype
  });

  return s3.send(command);
};

// Database operations
const saveData = async (data) => {
  const command = new PutCommand({
    TableName: 'MyTable',
    Item: data
  });

  return docClient.send(command);
};
```

## Common Mistakes

| Mistake | Risk |
|---------|------|
| Hardcoded credentials | Security breach |
| No IAM least privilege | Over-permissioned access |
| Ignoring costs | Unexpected bills |
| No monitoring | Undetected issues |
| Single region only | No disaster recovery |
| Not using IAM roles | Credential management |
| Ignoring S3 permissions | Data exposure |

## Quick Revision

- AWS offers comprehensive cloud services
- Lambda for serverless compute
- S3 for object storage and static hosting
- DynamoDB for NoSQL databases
- API Gateway for REST/GraphQL APIs
- CloudFront for CDN distribution
- IAM for access control
- Use AWS SDK for JavaScript
- Follow least privilege principle for IAM
- Monitor costs with AWS Cost Explorer

---

## Related Topics

- [[What-is-Deployment]] - Deployment concepts
- [[What-is-Docker]] - Container deployment
- [[Setup-CICD]] - CI/CD pipeline
- [[Deploy-Vercel]] - Vercel deployment
- [[Deploy-Netlify]] - Netlify deployment
- [[Store-Secrets]] - Secure secret storage
