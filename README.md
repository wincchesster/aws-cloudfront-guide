# aws-cloudfront-guide
Easy tips on how to configure AWS CloudFront

## What is AWS CloudFront?
If you read this guide, you probably already know what AWS CloudFront is. But if you don't, here is a brief explanation. AWS CloudFront is a content delivery network (CDN) service that delivers static and dynamic web content, video streams, and APIs around the world. It integrates with other Amazon Web Services products to give developers and businesses an easy way to distribute content to end users with low latency, high data transfer speeds, and no minimum usage commitments.

## How to configure AWS CloudFront?

**Prerequisites:**

In my case, the web application runs on nginx on two instances and it is traffic controlled by the loadbalancer.
So it is: 
- 2 EC2 instances
- Target Group
- Load Balancer with HTTP and HTTPS listener (after you have SSL certificate from ACM)
- Security Group for Load Balancer
- Security Group for EC2 instances

**Step 1: Route 53**

Obviously, you need to have a domain name. And you need to have a hosted zone in Route 53. After you have a hosted zone, you need to create an A record for your domain name (if it without subdomains) and point it to the load balancer.
Also you need Request a certificate from ACM.

> **ATENTION:** **For working with CloudFront, you need to have a certificate in the us-east-1 region. So, if you have a certificate in another region, you need to request a new one in the us-east-1 region.










To configure AWS CloudFront, you need to follow these steps:

1. Open the AWS Management Console at https://console.aws.amazon.com/cloudfront/.
2. In the navigation pane, choose **Create Distribution**.
3. On the first page of the Create Distribution Wizard, choose **Get Started** in the Web section.
4. On the next page, specify the following settings:
   - **Origin Domain Name**: The domain name of the Amazon S3 bucket or the HTTP server from which you want CloudFront to get your content.
   - **Origin Path**: The directory path in the Amazon S3 bucket or the HTTP server from which you want CloudFront to get your content, or the name of the custom origin.
   - **Origin ID**: A unique identifier for the origin. This value helps you to identify the origin in CloudFront logs.
   - **Restrict Bucket Access**: Choose **Yes** if you want to require users to access your content using CloudFront URLs instead of Amazon S3 URLs.
   - **Origin Access Identity**: The CloudFront origin access identity to associate with the origin. This value helps to ensure that end users can only access objects in an Amazon S3 bucket through CloudFront URLs.
   - **Grant Read Permissions on Bucket**: Choose **Yes** to give CloudFront permission to read the objects in your origin bucket.
   - **Viewer Protocol Policy**: Choose **Redirect HTTP to HTTPS** if you want CloudFront to redirect HTTP requests to HTTPS.
   - **Allowed HTTP Methods**: Choose the HTTP methods that you want to allow CloudFront to process.
   - **Cache Key and Origin Requests**: Choose **Use a cache policy and origin request policy** to use the default cache and origin request policies, or choose **Create a new cache policy and origin request policy** to create a new policy.
   - **Cache Policy**: The cache policy to associate with the cache behavior. This value determines the values that CloudFront includes in the cache key.
   - **Origin Request Policy**: The origin request policy to associate with the cache behavior. This value determines the values that CloudFront includes in requests that it sends to the origin.
   - **Smooth Streaming**: Choose **Yes** if you want CloudFront to use smooth streaming for media files.
   - **Restrict Viewer Access (Use Signed URLs or Signed Cookies)**: Choose **Yes** if you want to require viewers to access your content using CloudFront URLs that are signed.
   - **Compress Objects Automatically**: Choose **Yes** if you want CloudFront to automatically compress files for viewers.
   - **Lambda Function Associations**: Choose **Yes** if you want to associate a Lambda function with the cache behavior.
   - **Field-level Encryption Config**: Choose **Yes** if you want to configure field-level
