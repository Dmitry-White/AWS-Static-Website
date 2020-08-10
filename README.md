# AWS-Static-Website

## Manual deployment

https://docs.aws.amazon.com/AmazonS3/latest/dev/HostingWebsiteOnS3Setup.html

https://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html

https://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-cloudfront-walkthrough.html

### S3 Part

1. Create `domain-name` S3 bucket and upload the static website files
2. Enable "Static website hosting" and specify "index.html" file
3. Turn off "Block public access" and add Bucket Policy to allow "s3:GetObject" from "\*" Principle
4. Optionally, create `logs.domain-name` S3 bucket and enable "Server access logging" for `domain-name` S3 bucket to send logs to the `logs.domain-name` S3 bucket
5. Create `www.domain-name` S3 bucket for www subdomain redirects
6. Enable "Static website hosting" and choose `domain-name` S3 bucket to refirect traffic to

This will enable pure S3 static website hosting with non-user-friendly S3 endpoints that point to `domain-name` S3 bucket

### Route53 Part

1. Optionally, register `domain-name` with Route53, `domain-name` HostedZone will be auto-created
2. Create `domain-name` Hosted zone and add NS and SOA records of the `domain-name` domain
3. Add alias "A" records for `domain-name` and `www.domain-name` that point to `domain-name` and `www.domain-name` S3 buckets respectively

This will enable Route53 static website hosting in a particular Region with user-friendly `domain-name`

### CloudFront Part

1. Create CloudFront distribution that point to `domain-name` S3 bucket endpoint
2. Optionally, point the distribution to `logs.domain-name` S3 bucket
3. Optionally, request an ACM certificate for `domain-name` and `www.domain-name` and update `domain-name` HostedZone with 2 CNAME records of this certificate.
4. During creation of the distribution specify the issued ACM certificate that holds validated `domain-name` and `www.domain-name` records in `domain-name` HostedZone
5. Update alias "A" records for `domain-name` and `www.domain-name` to point to `domain-name` and `www.domain-name` CloudFront endpoints respectivelly

This will enable CloudFront static website hosting in Edge Locations with user-friendly `domain-name` and user-friendly latency

## CloudFormation deployment

https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/getting-started-secure-static-website-cloudformation-template.html
