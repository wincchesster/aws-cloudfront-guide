# AWS-CloudFront-Guide
Easy tips on how to configure AWS CloudFront

## What is AWS CloudFront?
If you read this guide, you probably already know what AWS CloudFront is. But if you don't, here is a brief explanation. AWS CloudFront is a content delivery network (CDN) service that delivers static and dynamic web content, video streams, and APIs around the world. It integrates with other Amazon Web Services products to give developers and businesses an easy way to distribute content to end users with low latency, high data transfer speeds, and no minimum usage commitments.

## How to configure AWS CloudFront?

## Prerequisites:

In my case, the web application **Booble.space** runs on nginx on two instances and it is traffic controlled by the LoadBalancer.
So it is: 
- 2 EC2 instances
- Target Group
- Load Balancer
- Security Group for Load Balancer
- Security Group for EC2 instances

## Step 1: Route 53

Obviously, you need to have a domain name. And you need to have a hosted zone in Route 53.
Also you need Request a certificate from ACM.
> **ATENTION:** For working with CloudFront, you need to have a certificate in the us-east-1 region. So, if you have a certificate in another region, you need to request a new one in the us-east-1 region.
This how looks like my hosted zone in Route 53:

<img src="https://i.imgur.com/2sYvGkJ.png" alt="drawing" width="95%"/></img>

* 1 - A record for the domain name (booble.space) and point it to CloudFront (you will get the CloudFront domain name after creating it)
* 2, 3 - default NS and SOA records
* 4 - CNAME record for both certificate validation 


## Step 2: Create and configure CloudFront

To configure AWS CloudFront, you need to follow these steps:

1. Open the AWS Management Console
2. In the navigation pane, choose **Create Distribution**.
3. On the first page of the Create Distribution Wizard, choose **Get Started** in the Web section.
4. On the next page, specify the following settings:
**Origin**
   - **Origin Domain Name**: The domain name of the Amazon S3 bucket or the HTTP server from which you want CloudFront to get your content.
   > In my case, it is the domain name of the Load Balancer and for some reason just HTTP works, not HTTPS
   - **Origin Path**: The directory path in the Amazon S3 bucket or the HTTP server from which you want CloudFront to get your content, or the name of the custom origin.
   (In my case, I leave it empty because the content is in the nginx root directory)
   - **Origin Name**: A unique identifier for the origin. This value helps you to identify the origin in CloudFront logs.
   
   <img src="https://i.imgur.com/pt1quiI.png" alt="drawing" width="95%"/></img>

**Default cache behavior**
    (I leave it as default)

**Viewer**
   - **Viewer Protocol Policy**: Choose **Redirect HTTP to HTTPS** if you want CloudFront to redirect HTTP requests to HTTPS.
   - **Allowed HTTP Methods**: Choose the HTTP methods that you want to allow CloudFront to process.

<img src="https://i.imgur.com/yqcgOpa.png" alt="drawing" width="95%"/></img>

**Cache key and origin requests**
   - **Cache Policy**: The cache policy to associate with the cache behavior. This value determines the values that CloudFront includes in the cache key.
   (I choose the Chaching Optimized for the best performance)
   - **Origin Request Policy**: The origin request policy to associate with the cache behavior. This value determines the values that CloudFront includes in requests that it sends to the origin.
   (I leave it as default blank)

<img src="https://i.imgur.com/a1v3fNR.png" alt="drawing" width="95%"/></img>

**Function associations**
   (I leave it as default)

**Web Application Firewall (WAF)**
   WAF is a web application firewall that helps protect your web applications from common web exploits that could affect application availability, compromise security, or consume excessive resources.
   (I choose Do not enable WAF)

<img src="https://i.imgur.com/v97hKsu.png" alt="drawing" width="95%"/></img>

**Settings**
   **Price class**: Choose the price class that corresponds to the features that you want to use.
   (I choose the Price Class All Edge Locations for testing purposes)

**Alternate domain name (CNAME) - optional**
   - **CNAMEs**: The CNAMEs (alternate domain names) that you want to associate with this distribution.
   (I put here the domain name of my web application **booble.space**)
   - **Custom SSL certificate - optional** - Choose the custom SSL certificate that you want to use for this distribution.

<img src="https://i.imgur.com/7mSqq6m.png" alt="drawing" width="95%"/></img>

After that push the **Create Distribution** button and wait for the distribution to be deployed.