## AWS CLOUD SOLUTION FOR 2 COMPANY WEBSITES USING A REVERSE PROXY TECHNOLOGY


We will build a secure infrastructure inside AWS VPC (Virtual Private Cloud) network for narbyd Company that uses WordPress CMS for its main business website, and a Tooling website for the DevOps team.

As part of the company’s desire for improved security and performance, a decision has been made to use a reverse proxy technology from NGINX to achieve this.

PURPOSE:

Reduced Cost, increase Security and Scalability are the major requirements for this project. Hence, implementing the architecture designed below, ensure that infrastructure for both websites (WordPress and Tooling) are resilient to Web Server failures, can accomodate increased traffic and at the same time, has reasonable cost.

AWS resources Required for the Design:

    North Virginia Region (us-east-1)
    Availibility zones (3 subnets in us-east-1a) and (3 subnets in us-east-1b)
    VPC Network Range - 10.0.0.0/16
    subnets - 10.0.1.0/24, 10.0.2.0/24, 10.0.3.0/24, 10.0.4.0/24, 10.0.5.0/24 and 10.0.6.0/24
    6 subnets (4 private subnets and 2 public subnets)
    internet gateway
    2 nginx for reverse proxy
    2 bastion hosts/jump servers
    2 application Load balancers(ALB)
    Auto scaling Groups to manage the scaling of the Ec2 instances
    2 NAT gateways for the resources in the private subnet to communicate with the internet gateway.
    N/B: The NAT gateway only allows traffic to the internet and does not allow from the internet.

    Route DNS
    RDS for the database
    Amazon Elastic Files System for the file management



### AWS MULTIPLE WEBSITE PROJECT





![Screenshot from 2023-12-29 11-17-57](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/757f5269-a459-46cf-8044-eb0c09fd9204)
![Screenshot from 2023-12-29 11-19-20](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/6a941ab5-be39-45d1-847e-e883cee234bc)





### SET UP A VIRTUAL PRIVATE NETWORK (VPC)



Create VPC




