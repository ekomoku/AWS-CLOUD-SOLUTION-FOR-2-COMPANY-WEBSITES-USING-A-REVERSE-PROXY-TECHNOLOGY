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






![Screenshot from 2024-01-01 13-08-59](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/bc2f0cdb-e393-4fc4-ba88-0986e575bd68)




![Screenshot from 2024-01-01 14-03-52](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/220d2ba0-1d27-4723-9902-70eae161c416)





![Screenshot from 2024-01-01 14-04-25](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/2bfd6ff8-ba9d-4a29-bd51-502c7fc3a7e4)





![Screenshot from 2024-01-01 14-06-23](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/486906b2-ee12-4b1b-888a-99584e572068)





![Screenshot from 2024-01-01 14-07-31](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/45cdf909-16a1-4aa4-857f-f079a6103ff3)








Create subnets as shown in the architecture - 3 subnets in each Availability zones i.e 10.0.1.0/24, 10.0.3.0/24, 10.0.5.0/24 in us-east-1a




![Screenshot from 2024-01-01 14-14-13](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/cc52125a-e610-47d1-9b22-bb48fdfc035b)






![Screenshot from 2024-01-01 14-14-37](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/0ac7e86f-fe4b-4de0-9ce8-3e5a572bcc11)






![Screenshot from 2024-01-01 14-21-56](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/259c42ac-6343-4385-9ff2-5edf93d4b244)







Also, add 3 subnets in Availability zone us-east-1b i.e 10.0.2.0/24, 10.0.4.0/24, 10.0.6.0/24





![Screenshot from 2024-01-01 14-29-33](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/172c3599-b092-4983-a310-6a36c0e9beae)











N/B: These subnets are neither private nor public at this point. The Internet gateway and NAT gateway associated with any of them identifies them private or public.


Create a route table and associate it with public subnets






![Screenshot from 2024-01-01 14-50-58](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/e8d42877-8a0f-44dc-ae47-e691d8817d93)





![Screenshot from 2024-01-01 14-52-00](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/1e53991a-78fd-4831-be0d-1189054c727f)




Goto> Submit associations > Edit subnet associations ....to add the public subnets







![Screenshot from 2024-01-01 14-54-20](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/d1f55047-2749-4577-84e1-f47c057f0f96)















![Screenshot from 2024-01-01 14-55-39](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/fb2120e4-8ae3-4d4c-8645-189a4c12e948)






Create a route table and associate it with private subnets





![Screenshot from 2024-01-01 14-58-39](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/d0f4b728-6551-4249-ab32-9eae28446753)





![Screenshot from 2024-01-01 15-00-07](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/a76866fb-51dc-459d-b48d-fdef75d7cba8)






![Screenshot from 2024-01-01 15-04-14](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/3a3e58f4-b605-4eb4-b5ac-44a965e58118)






Create an Internet Gateway for the public subnet






![Screenshot from 2024-01-03 08-40-58](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/66a07082-5a1e-455f-abdc-dd100065de9f)





Attach the internet gateway to the VPC by clicking " Attach to a VPC" on the top right corner or under "Actions"





![Screenshot from 2024-01-03 08-42-45](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/d281fd61-121e-4781-8841-82dd49292be2)






![Screenshot from 2024-01-03 08-45-02](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/976e2d37-57ee-4722-aa0d-e249511459d4)





![Screenshot from 2024-01-03 08-46-33](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/72ea5df7-7db1-4c46-8551-8f86501b825a)





Edit a route in public route table and associate it with the Internet Gateway. This allows the public subnet to be accessible from the Internet)




![Screenshot from 2024-01-03 08-50-26](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/3bb7c03a-de5b-47b9-8587-5ce19e1d211f)




Click Edit Routes and click on Add Route, select Internet Gateway and pick the IGw previously created,click Save Changes




![Screenshot from 2024-01-03 08-52-49](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/e3ded830-4500-4ace-bb2c-3b0bd5b524e0)





![Screenshot from 2024-01-03 08-54-42](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/bb5ac256-4046-4752-ad6b-6743bfe864cd)




Create 3 Elastic IPs - 1 Elastic IP will be used by the NAT gatewayw while the remaining 2 will be used by the Bastion host.





![Screenshot from 2024-01-03 09-28-17](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/96da8edd-2cab-40ba-b679-04a33c81ef68)





![Screenshot from 2024-01-03 09-31-59](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/fd2b8dfa-a88c-42a2-84cc-de442e9fa8bf)






Create a NAT Gateway and assign one of the Elastic IPs. The NAT gateway is created in the public subnet




![Screenshot from 2024-01-03 09-36-21](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/35244e0b-711c-4a13-b090-6448d4139cd3)




![Screenshot from 2024-01-03 09-44-46](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/ade11b2b-c33a-4b49-9220-3bf228ade1e9)





![Screenshot from 2024-01-03 09-47-00](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/81eb7358-03db-461e-9044-9f78711d1cfd)





Edit a route in private route table, and associate it with the NAT Gateway. This allows traffic to be sent to the internet but not from the internet.




![Screenshot from 2024-01-03 10-20-37](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/a43b3fd0-cc31-4d86-98cf-8e5b630767c7)





![Screenshot from 2024-01-03 10-22-17](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/152a05d4-040a-4d70-8aac-5113b929c515)





![Screenshot from 2024-01-03 10-25-56](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/dbea1ba3-2d40-4713-a80a-b3341f6add1a)






Create a Security Group for:


Nginx Servers: Access to Nginx should only be allowed from a Application Load balancer (ALB). At this point, we have not created a load balancer, therefore we will update the rules later. For now, just create it and put some dummy records as a place holder.



Bastion Servers: Access to the Bastion servers should be allowed only from workstations that need to SSH into the bastion servers. Hence, you can use your workstation public IP address. To get this information, simply go to your terminal and type curl www.canhazip.com



Application Load Balancer: ALB will be available from the Internet Webservers: Access to Webservers should only be allowed from the Nginx servers. Since we do not have the servers created yet, just put some dummy records as a place holder, we will update it later.



Data Layer: Access to the Data layer, which is comprised of Amazon Relational Database Service (RDS) and Amazon Elastic File System (EFS) must be carefully desinged – only webservers should be able to connect to RDS, while Nginx and Webservers will have access to EFS Mountpoint








Create a Security Group for Application Load Balancer - Access to ALB will be allowed from the internet






![Screenshot from 2024-01-03 10-33-38](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/8f35734f-59dc-4f49-8496-7ed824a52a1f)





![Screenshot from 2024-01-03 10-35-21](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/72198532-6ce2-45a3-b76c-791f245710f3)





Create security group for Bastion Servers - Access to the Bastion servers should be allowed only from workstations that need to SSH into the bastion servers. Hence, you can use your workstation public IP address.



We can get this by opening the CMD in our local workstation(computer) and run the command ipconfig ; or visit the link http://ipinfo.io/ip




![Screenshot from 2024-01-03 14-36-17](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/a4c21e9f-4941-4b6a-9a80-ba77cb882923)






![Screenshot from 2024-01-03 14-36-46](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/94aa41e6-1f4a-4e26-85e6-10f727638ed7)






![Screenshot from 2024-01-03 14-38-41](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/45592ad1-8b1b-4df9-8612-d59adf859cae)




Create security group from Application LB. Access will only be available from the Internet





![Screenshot from 2024-01-03 14-43-06](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/cf4ad20b-3fcf-4748-8c59-fad375055564)






Security Group for webservers - Access to Webservers should only be allowed from webserver ALB and bastion host.




![Screenshot from 2024-01-04 09-11-32](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/0e4711c0-461f-4a72-ad87-9d1fc7011135)





![Screenshot from 2024-01-04 09-17-40](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/c7dd5753-2509-4562-8be3-085ea337bf65)





N/B: We can choose to allow ssh only from the Bastion host's IP. This will mean that if the Ec2 instance is compromised them the access is lost.


This is not a good practice when applying auto scaling since it will scale out and scaling when needed and access to other Ec2 instances is denied to the bastion host.






![Screenshot from 2024-01-04 16-31-58](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/033ebc2d-0893-4b09-b684-296462e75cf3)






Create a Security Group for Nginx Servers - Access to Nginx should only be allowed from a Application Load balancer (ALB) and bastion host.




![Screenshot from 2024-01-04 16-47-27](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/e4e4c0cc-fdb8-401b-a7a0-ab80da28027a)





![Screenshot from 2024-01-04 16-49-38](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/3188c11a-9d2a-4cf2-a704-0730542cafab)




Create security group for the internal ALB - allow access to only nginx reverse proxy.




![Screenshot from 2024-01-04 16-52-48](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/fe2cdbec-c56f-4636-923b-18aaa8ea841c)




![Screenshot from 2024-01-04 16-54-19](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/6d15d4d6-ebfd-4be8-a8a4-0d639048f4ba)




Create security group for the backend services or datalayer to allow the websever access to the RDS and EFS in the backend security group. Allow access to the bastion host.




![Screenshot from 2024-01-04 20-03-49](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/e663cb11-4af1-4b7b-8fe6-ac794bba249a)



Add the webserver and bastion security groups



![Screenshot from 2024-01-04 20-09-52](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/49df2c00-8179-4477-b248-bfcc00c38b60)





### Proceed With Compute Resources




You will need to set up and configure compute resources inside your VPC. The recources related to compute are:

    EC2 Instances
    
    Launch Templates
    
    Target Groups
    
    Autoscaling Groups
    
    TLS Certificates
    
    Application Load Balancers (ALB)


### TLS Certificates From Amazon Certificate Manager (ACM)



You will need TLS certificates to handle secured connectivity to your Application Load Balancers (ALB).

    Navigate to AWS ACM
    
    Request a public wildcard certificate for the domain name you registered in Freenom
    
    Use DNS to validate the domain name
    
    Tag the resource
    
    Bind the ACM to the route53 hosted zone created earlier



### Setup



Purchase a domain name and Create an ACM certificate




![Screenshot from 2024-01-04 20-57-15](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/59169534-047b-49e2-8ede-8005db7c046d)




![Screenshot from 2024-01-04 20-58-18](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/8fafddc1-680c-4d37-aaaf-ca1c4d2923a1)




Create record for both tooling and wordpress


For tooling




![Screenshot from 2024-01-04 22-13-37](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/dea5a508-c3ec-439d-a9e6-042c296acc58)




![Screenshot from 2024-01-04 22-14-08](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/454924ef-f40a-4b01-9361-cf0dceaead2d)





![Screenshot from 2024-01-04 22-20-54](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/5ae9ac5f-b547-4272-af9c-27830a5c5890)





![Screenshot from 2024-01-04 22-22-21](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/080afa3f-b7f9-4ac8-864a-c67fa691d554)






### Configuring Target Groups


For Nginx Server

    
    Selecting Instances as the target type

    
    Ensuring the protocol HTTPS on secure TLS port 443

    
    Ensuring that the health check path is /healthstatus






![Screenshot from 2024-01-05 09-54-37](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/880f3261-e5f4-4fe6-8c0a-317a58ca4c82)







![Screenshot from 2024-01-05 09-56-02](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/6506fb8f-88b0-4674-a8fc-2fcd3e906eba)







![Screenshot from 2024-01-05 09-59-58](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/4ed68fe7-e99e-4811-bb1f-4c1463acb647)





For Wordpress

    
    Selecting Instances as the target type

    
    Ensuring the protocol HTTPS on secure TLS port 443

    
    Ensuring that the health check path is /healthstatus
    

For Tooling


    Selecting Instances as the target type

    
    Ensuring the protocol HTTPS on secure TLS port 443

    
    Ensuring that the health check path is /healthstatus





![Screenshot from 2024-01-05 10-19-35](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/054d3379-706c-4d80-a7eb-0021aac47d99)






### Configuring Application Load Balancer (ALB)

Create the ALB forwarding traffic to the Nginx reverse proxy




![Screenshot from 2024-01-04 22-40-49](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/b2eaeb62-5998-4e5b-8287-41ae8a9da56d)





![Screenshot from 2024-01-05 12-24-20](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/d94ac85e-3c6c-4348-ad62-048b4e71c99c)







![Screenshot from 2024-01-05 12-15-03](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/34c3f90a-cbeb-48f8-bbf8-79ad90f2dffc)





![Screenshot from 2024-01-05 12-15-51](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/9263775e-c12b-4d2f-ab5e-22f43fa00026)





Remember to select a certificate from the dropdown



![Screenshot from 2024-01-05 12-20-07](https://github.com/ekomoku/AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-A-REVERSE-PROXY-TECHNOLOGY/assets/66005935/21ad6289-72bb-4e28-9d88-08cefeaf90f1)

