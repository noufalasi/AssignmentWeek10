# AssignmentWeek10
![Image](https://github.com/user-attachments/assets/25f6d126-04ee-456c-9903-f15860f76847)


![Image](https://github.com/user-attachments/assets/6200a9f3-1971-4f90-975b-404d17562b89)


![Image](https://github.com/user-attachments/assets/69a30b1c-ba81-49a3-b19f-a799e107a144)


![Image](https://github.com/user-attachments/assets/484c2636-b90f-4c1d-bb0a-9c44b4487368)


![Image](https://github.com/user-attachments/assets/b74ce06a-7a23-46d3-9acb-0cc28ce746e6)


THESE ARE THE SOLUTION STEPS.

Frontend Hosting (S3)
1. Create bucket: nouf-blogapp-frontend (region: `eu-north-1`)  
2. Enable public access:  
   - Uncheck *"Block all public access"* → Confirm  
3. Configure static hosting:  
   - Default document: index.html  
4. Apply bucket policy:  
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::yourname-blogapp-frontend/*"
  }]
}



Media Uploads Bucket
1. Create bucket: nouf-blogapp-media  
2. Set CORS policy:  
[{
  "AllowedMethods": ["GET", "PUT", "POST"],
  "AllowedOrigins": ["*"]
}]


IAM User for S3 Access
1. Create user blog-app-user (programmatic access only)  
2. Attach custom policy:  
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": ["s3:PutObject", "s3:GetObject"],
    "Resource": ["arn:aws:s3:::nouf-blogapp-media/*"]
  }]
}


 Backend Deployment (EC2)
Instance Setup
- AMI: Ubuntu 22.04 LTS  
- Type: t3.micro (free tier eligible)  
- Security Group: Open ports 22 (SSH), 5000 (API)  
- User Data: Paste initialization script  

Configuration
 Clone repository
git clone https://github.com/cw-barry/blog-app-MERN.git ~/blog-app

Configure environment
nano ~/blog-app/backend/.env


Add your credentials:
MONGODB=mongodb+srv://user:pass@cluster.mongodb.net/blog-app
AWS_ACCESS_KEY_ID=AKIAXXXXXX
AWS_SECRET_ACCESS_KEY=XXXXXX
S3_BUCKET=nouf-blogapp-media

 Start Services
Install dependencies
npm install

 Launch with PM2
pm2 start index.js --name "blog-api"
pm2 save && pm2 startup


Frontend Deployment
 Build and deploy
pnpm install
pnpm build
aws s3 sync ~/blog-app/frontend/dist/ s3://nouf-blogapp-frontend/

Access bucket URL ( `http://nouf-blogapp-frontend.s3-website.eu-north-1.amazonaws.com`)
