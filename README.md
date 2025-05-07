# Assignment10-HalaT
Clarusway SDA Bootcamp Deployment Project
Week-10 Assignment | SDA2016 | Hala Tahlawi
MERN Stack Blog App Deployment on AWS
-------------------------------------------------------------------------------------------------------------------------------
Overview:
This assignment entails the deployment of a blog application utilizing the MERN (MongoDB, Express.js, React.js, Node.js) stack on AWS, incorporating multiple AWS services, including EC2, S3, IAM, and MongoDB Atlas. The work entails setting up the database, media storage, frontend, and backend before deploying everything on AWS with appropriate setup and safe access controls.

Technologies Used:
-Frontend: React.js, Vite, Tailwind CSS
-Backend: Node.js, Express.js
-Database: MongoDB Atlas (cloud-hosted)
-Deployment: AWS EC2, S3, IAM
-File Storage: AWS S3 for media file uploads
-Process Manager: PM2 for backend deployment management
--------------------------------------------------------------------------------------------------------
Part 1: Frontend S3 Bucket Configuration
S3 Bucket Creation:
I created a bucket named halatahlawi-blogapp-frontend in the eu-north-1 region.

Static Website Hosting:
I enabled static website hosting for the bucket and disabled public access restrictions.

Bucket Policy for Public Access:
I added the following bucket policy to allow public access to the files:
{ 
  "Version": "2012-10-17", 
  "Statement": [ 
    { 
      "Effect": "Allow", 
      "Principal": "*", 
      "Action": "s3:GetObject", 
      "Resource": "arn:aws:s3:::halatahlawi-blogapp-frontend/*" 
    } 
}
Deploy Frontend:

I deployed the React.js app to S3 after building the frontend with the following command:
-  aws s3 sync dist/ s3://halatahlawi-blogapp-frontend/

  ----------------------------------------------------------------------------------------------------------------------------

Part 2: Media S3 Bucket Configuration

-Second S3 Bucket Creation:
I created another bucket for media uploads, named halatahlawi-blogapp-media.

-ORS Configuration:
I configured the CORS policy to allow the frontend to interact with the media bucket for uploading files:

[ 
  { 
    "AllowedHeaders": ["*"], 
    "AllowedMethods": ["GET", "PUT", "POST", "DELETE", "HEAD"], 
    "AllowedOrigins": ["*"], 
    "ExposeHeaders": ["ETag"] 
  } 
]

Test File Upload:
I successfully tested uploading a file to the media bucket.
---------------------------------------------------------------------------------------------------------------------------------
part 3: IAM User and Policy for Media Bucket Access
-IAM User Creation:
I created a new IAM user (blog-app-user) with programmatic access and attached a custom policy to access the media S3 bucket.

-Custom Policy:
The custom policy allows actions like PutObject, GetObject, and DeleteObject on the media bucket.

{ 
  "Version": "2012-10-17", 
  "Statement": [ 
    { 
      "Effect": "Allow", 
      "Action": [ 
        "s3:PutObject", 
        "s3:GetObject", 
        "s3:DeleteObject", 
        "s3:ListBucket" 
      ], 
      "Resource": [ 
        "arn:aws:s3:::halatahlawi-blogapp-media", 
        "arn:aws:s3:::halatahlawi-blogapp-media/*" 
      ] 
    } 
  ] 
}

- Access Key and Secret Access Key:
I securely stored the Access Key ID and Secret Access Key for the IAM user.

------------------------------------------------------------------------------------------------------------------------------
part 4 Backend Deployment on AWS EC2
Steps:
-Launch EC2 Instance
-Region: eu-north-1
-Instance Type: t3.micro
-Security Group: Open ports 22 (SSH), 80 (HTTP), 443 (HTTPS), and 5000 (Custom TCP).
-Launched a new Ubuntu EC2 instance.
-Opened port 5000 in the security group.
-Installed Node, NPM, PM2, and cloned the backend repo.

-Created .env file with necessary variables:
PORT=5000
AWS_ACCESS_KEY_ID=********
AWS_SECRET_ACCESS_KEY=********
AWS_REGION=eu-north-1
S3_BUCKET_NAME=halatahlawi-blogapp-media
-Installed dependencies via npm install.
-Started the backend with pm2 start index.js.
-Saved PM2 config for persistence across rebo
------------------------------------------------------------------------------------------------------------------------------------
part 5 Frontend Deployment to S3
-Frontend Configuration:
I configured the .env file for the frontend to set the backend API base URL and media file URL.
-Build and Deploy Frontend:
I built the frontend using pnpm and deployed it to the S3 bucket:
pnpm run build
aws s3 sync dist/ s3://halatahlawi-blogapp-frontend/
-------------------------------------------------------------------------------------------------------------------------------------

part 6 Testing:
-Backend Testing:
I tested the backend API using tools like Postman to ensure the functionality of the endpoints, e.g., http://<EC2-IP>:5000/api/posts.
-Frontend Testing:
I tested the deployed frontend by visiting the S3 website URL: http://halatahlawi-blogapp-frontend.s3-website.eu-north-1.amazonaws.com.
-Media Upload Testing:
I verified that media files can be uploaded to the media bucket via the frontend.

------------------------------------------------------------------------------------------------------------------------------------------

Reflections & Lessons Learned:

-Obtained practical experience deploying a full-stack application using AWS services.

-Learned how to set up IAM users and policies for secure access to S3 buckets.

-Gained hands-on experience in backend deployment using EC2 with proper environment isolation.

-Learned to use PM2 for process management, including troubleshooting and monitoring deployment logs.

-This project helped me understand real-world infrastructure configuration and improved my skills in cloud deployment.
-------------------------------------------------------------------------------------------------------------------------------------------
Conclusion:
Through this project, I successfully deployed a full-stack MERN application using various AWS services. I configured media file storage, established secure access controls through IAM policies, and deployed both the frontend and backend components. The application is now fully functional, with secure backend operations, public frontend access, and complete MERN stack capabilities.
