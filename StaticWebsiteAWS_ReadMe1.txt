Static Website Hosting on AWS 
==================================================================

 Project Overview
-------------------
This project demonstrates secure static website hosting on AWS using the Free Tier.
It’s designed as a real-world demo for small businesses in South Africa that want
to get online without high hosting costs.

Key AWS Services Used:
- Amazon S3 (private) → Store website files securely
- Amazon CloudFront (CDN) → Global content delivery + HTTPS
- Origin Access Control (OAC) → Secure connection between CloudFront & S3
- AWS IAM → Role-based access and least-privilege security
- Amazon CloudWatch → Monitoring & logs
- (Optional) Route 53 → Domain name setup

With this architecture, businesses can scale instantly, stay secure, and reach global customers.

------------------------------------------------------------------
 Architecture
------------------------------------------------------------------
User Request → CloudFront (CDN, HTTPS)
                  ↓
            Origin Access Control (OAC)
                  ↓
              S3 Bucket (Private)

- S3 stays private
- Only CloudFront can read objects
- No public access to bucket

------------------------------------------------------------------
Security Setup (JSON Policy)
------------------------------------------------------------------
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowCloudFrontServicePrincipal",
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudfront.amazonaws.com"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-static-website-demo/*",
      "Condition": {
        "StringEquals": {
          "AWS:SourceArn": "arn:aws:cloudfront::<ACCOUNT_ID>:distribution/<DISTRIBUTION_ID>"
        }
      }
    }
  ]
}

Replace <ACCOUNT_ID> and <DISTRIBUTION_ID> with your actual values.

------------------------------------------------------------------
 Deployment Steps
------------------------------------------------------------------
1. Create private S3 bucket → upload website files
2. Set up CloudFront distribution → link to S3 (via OAC)
3. Apply JSON bucket policy → allow CloudFront only
4. Enable CloudWatch logs → monitor traffic and errors
5. (Optional) Connect Route 53 domain → e.g., mybusiness.co.za

------------------------------------------------------------------
 Benefits for Small Businesses
------------------------------------------------------------------
-  Free Tier friendly → keep costs low
-  Fast global delivery → CloudFront edge locations
-  Secure by design → IAM + OAC, no public buckets
-  Scalable automatically → no servers to manage
-  Always available → 99.9% uptime

------------------------------------------------------------------
