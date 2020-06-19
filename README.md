Cloudflare's IP address list > AWS Security Group and S3 Policies
===================================

This Lambda function to retrieve Cloudflare's IP address list and
update an AWS Security Group and S3 Policies.

It was originally written by John McCracken (johnmccuk@gmail.com), 
updated by Ryan Gibbons (rtgibbons) and Endrigo Antonini (antonini).

We have made a fork that we are expanding and optimizing for our use cases and hopefully for more generic use cases. It could at some point be a version two written in go. Hans Øyvind Laderud (afreakk) and Christian Sakshaug (csakshaug) supporting this fork on behalf of dx.

Instructions
------------

#### Serverless
Serverless will automatically create and manage a security group in the specified vpc.
And add cloudflare ips to its inbound rules on ports specified.
(S3 permissions is not set up in serverless.yml because i dont need it, but you can uncomment the permissions in serverless.yml)
```
serverless deploy  --vpcid "vpc-12345678912345678" --ports "80,443"
```

#### Manual
Use the content of the file `cf-security-group-update.py` in your lambda ou upload it.

It is also required that you upload or create the `package` file as is available on this repository.

The Lambda uses the Python 2.7 runtime and requires the following
enviroment variables:

* `SECURITY_GROUP_IDS_LIST` - security group ID(s) to update `sg-12345678912345678,s3-11122223333344444`
* `PORTS_LIST` - port(s) to update `80,443` or `80` (defaults to 80 if none specified)
* `S3_CLOUDFLARE_SID` - Sid that stores all the CloudFlare configurataion. That Sid is stored on the Stament policy.
* `S3_BUCKET_IDS_LIST` - S3 buckets ID(s) to update

You need to allow the Lambda to execute those actions (example on the file `allow-lambda-ingress-role`:

* ec2:AuthorizeSecurityGroupIngress
* ec2:RevokeSecurityGroupIngress
* ec2:DescribeSecurityGroup
* s3:GetBucketPolicyStatus
* s3:PutBucketPolicy
* s3:GetBucketPolicy



To update Security Groups
------------

You need to define `SECURITY_GROUP_IDS_LIST`.
The parameter `PORTS_LIST` is also used to update an AWS Security Group.

To update S3 Policy
------------

You need to define the parameters `S3_CLOUDFLARE_SID` and `S3_BUCKET_IDS_LIST`.

Contributors
-----------

* John McCracken ([@johnmccuk](https://www.github.com/johnmccuk))
* Ryan Gibbons ([@rtgibbons](https://www.github.com/rtgibbons)) 
* Ben Steinberg ([@bensteinberg](https://www.github.com/bensteinberg))
* Endrigo Antonini ([@antonini](https://www.github.com/antonini))
