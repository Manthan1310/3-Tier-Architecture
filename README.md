# 3  tier Architecture AWS



Services used 

1. EC2
2. ASG
3. ALB
4. IAM
5. S3(CAN ADD EFS)
6. RDS
7. VPC
8. CLOUDWATCH
9. SNS(3 TOPICS)
10. CLOUDTRAIL
11. ROUTE53(OVERVIEW)
12. CLOUDFRONT(OVERVIEW)




## **STEP 1**: Downloading the code in local system


## **STEP 2:** Create a s3 bucket & Upload the code to it 


![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/ozbc_a2WOGgKbRKE7Gt_Z.png?ixlib=js-3.7.0 "image.png")

Give a name , scroll down give a tag .

Click on create 

After creating click on the bucket and upload the entire application code file , which you have downloaded before 



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/WimPrQE9egAzpKS2FJRbm.png?ixlib=js-3.7.0 "image.png")



## **Step 3: Create bucket for VPC flowlogs**
![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/p7tnmINdne17NPnXzAHFV.png?ixlib=js-3.7.0 "image.png")



## **Step 4 : Create an IAM role **
- s3 read only access
- ssm managed instance core
(in order to keep it highly secure not giving administrator access)

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/FJv0VwvK4JYPWh7Cw77ex.png?ixlib=js-3.7.0 "image.png")



## **STEP 5 : Create VPC, Subnets, IGW,NAT-GW,RT**




![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/fOT7iWNwAvDFeEd_S8MLG.png?ixlib=js-3.7.0 "image.png")





#### Create 6 subnets 
![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/8PAjojKvgKJ8Jt82se3eW.png?ixlib=js-3.7.0 "image.png")

---

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/vUkugtX2xBA5cqkLUw_9B.png?ixlib=js-3.7.0 "image.png")



#### Only the WEB-TIER subnet swill be private 
#### Go to web-tier subnet ,-> Actions -> Edit subnet setting ->Enable auto assign IPv4 address
![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/ayej1ytv7ewKJ7fD6P7jh.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/PpH3U5HIDnL_35k67UdlH.png?ixlib=js-3.7.0 "image.png")



### DO this for WEB-TIER-SUBNET-1 AND WEB-TIER-SUBNET-2






#### Create a VPC Flow logs
![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/OsxHhJwxmd9DVfOYRgwKT.png?ixlib=js-3.7.0 "image.png")



Give a name and Chose destination S3 bucket ->Give ARN of the bucket which we created above->Click Create

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/elbZ19awnEo9PTr6k8YBt.png?ixlib=js-3.7.0 "image.png")



This is to store the logs of VPC into S3 bucket 



#### Now  , Create Internet gateway 
![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/JQK75_GaNo1JmzQXtEKG-.png?ixlib=js-3.7.0 "image.png")

Then attach to the VPC we are working on 



Now create a route table for public internet gateway



Create Route table -> Web-Tier-Route-Table-> Edit routes 

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/wUftYWzuUTOJYO0Oy84YL.png?ixlib=js-3.7.0 "image.png")



then in Subnet association add this 2 web tier subnets

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/XZi60Sb28sHzSmtD-oGkI.png?ixlib=js-3.7.0 "image.png")







#### Now, we will create NAT gateway (you can create a single NAT gateway and connect to 2 instances but we will go with 2 different for **High Availability**)


Create NAT gateway-> Subnet web tier 1 -> Allocate Elastic IP

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/sNGXCe0tkF65eBjNsGeXI.png?ixlib=js-3.7.0 "image.png")



Similarly we will create NAT-GW-2.



#### Now we will configure route tables for NAT-GW-1. and NAT-GW-2.


#### Create  route table -> App-tier-rt-1->Edit routes ->subnet associations
![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/OpAcUANmUtDAwnWH0oTxZ.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/MdQmEGHBXRODS8OL8nlAX.png?ixlib=js-3.7.0 "image.png")



Similarly Create  route table -> App-tier-rt-2->Edit routes(NAT-2) ->subnet associations(APP tier 2)





## STEP 6 : Create security groups
1.EXTERNAL-LOAD-BALANCER-SG --> HTTP(80) : 0.0.0.0/0

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/mbTZ4J8gOjFPQ8E1K7R7A.png?ixlib=js-3.7.0 "image.png")

2.WEB-TIER --> HTTP --> EXTERNAL-LOAD-BALANCER-SG



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/GQ1UbPsXdZBKYFWXAXIYK.png?ixlib=js-3.7.0 "image.png")

3.INTERNAL-LOAD-BALANCER-SG--> HTTP --> WEB-TIER

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/MILnDZOmQwwZYkrdBxBGz.png?ixlib=js-3.7.0 "image.png")



4.APP-TIER-SG --> PORT 400 --> INTERNAL-LOAD-BALANCER-SG



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/wZmxT1rgyKKdeuJTi8aIx.png?ixlib=js-3.7.0 "image.png")



5.DB-TIER-SG -->MYSQL 3306 -->APP-TIER-SG



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/ehZ-MtqIJO2xROdYWfElp.png?ixlib=js-3.7.0 "image.png")



## STEP 7  Create DB Subnet group and RDS 


#### To use MultiAZ we will use Databse subnet groups , so we not need to use baston 
#### 
#### Creating Database Subnet group ->RDS -> Subnet groups
![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/FJCMTf7e276w6U0wvtF7o.png?ixlib=js-3.7.0 "image.png")



Now we will create Database 

RDS->Dashboard->Create Database 

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/oPMGu4s4_NqJw-ulcqx5L.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/CCBe01puDWyAL6lydcFmQ.png?ixlib=js-3.7.0 "image.png")



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/UqFAw3aQhbjjAgVedwi-k.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/TS7TSm-0kwA0i3WHpYHHW.png?ixlib=js-3.7.0 "image.png")



Click on **create**

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/VGdvrgG3JsHLLK4aRcYJ7.png?ixlib=js-3.7.0 "image.png")





## Step 8 : Create APP server , Install Packages ,Test Connections 
create AMI

Create Launch template using AMI

Create Target group 

Create Internal Load Balancer 

Create Autoscalling Group





Create EC2 server

(TO make this Secure we wont use key pair )



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/Vxe_jmapqxrSSKinwzPKI.png?ixlib=js-3.7.0 "image.png")





```
`﻿`﻿# =========================================
# COMMANDS TO RUN IN THE APPLICATION SERVER
# =========================================

# ======================================
# INSTALLING MYSQL IN AMAZON LINUX 2023
# ======================================
# (REF: https://dev.to/aws-builders/installing-mysql-on-amazon-linux-2023-1512)

#!/bin/bash
sudo -su ec2-user
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm
sudo dnf install mysql80-community-release-el9-1.noarch.rpm -y
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023
sudo dnf install mysql-community-client -y
mysql --version

# TO TEST CONNECTION BETWEEN APP-SERVER & DATABASE SERVER
mysql -h <RDS-Endpoint> -u <username> -p <Hit Enter & provide your password>

#===============================
# COPYING CONTENT FROM S3 BUCKET
#===============================
cd /home/ec2-user

# !!! IMP !!!
# MODIFY BELOW CODE WITH YOUR S3 BUCKET NAME
sudo aws s3 cp s3://<YOUR-S3-BUCKET-NAME>/application-code/app-tier app-tier --recursive
cd app-tier
sudo chown -R ec2-user:ec2-user /home/ec2-user/app-tier
sudo chmod -R 755 /home/ec2-user/app-tier

#===============================
# INSTALLING NODEJS
#===============================
# (REF: https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html)

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install 16
nvm use 16
npm install -g pm2
npm install
npm audit fix

#===============================
# STARTING INDEX.JS FILE
#===============================
pm2 start index.js 	#(Start Application with PM2, PM2 is process manager for NodeJS)
pm2 logs            #(To see logs, run Ctrl+C to exit)
pm2 startup 			  #(Set PM2 to Start on Boot)
sudo env PATH=$PATH:/home/ec2-user/.nvm/versions/node/v16.20.2/bin /home/ec2-user/.nvm/versions/node/v16.20.2/lib/node_modules/pm2/bin/pm2 startup systemd -u ec2-user --hp /home/ec2-user
pm2 save			      #(Save the current configuration)
curl http://localhost:4000/health #(To do the health check) 
```
Run this command one by one in session manager 





### Create AMI
![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/_JO6XyXZo_Y0vgQdAo-u9.png?ixlib=js-3.7.0 "image.png")



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/x6QiG__A6MXBVs9Sdlw8j.png?ixlib=js-3.7.0 "image.png")







Now create Target Group

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/9ua2fP8NmvVB0rHZ35iRH.png?ixlib=js-3.7.0 "image.png")

Select your VPC and click on create 







### Now create Launch Template 


![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/aSp26KAX2_szeKt7kmpu2.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/w2WQgtupx0lDJe8rOLm6M.png?ixlib=js-3.7.0 "image.png")



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/53e5v9e_dF-mgPx7ou73x.png?ixlib=js-3.7.0 "image.png")



Click on Create 





### Now Create load balancer 
![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/1HilzNL4XZ3XMZNv5Mi9w.png?ixlib=js-3.7.0 "image.png")



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/nGXmlrTVMhCBHsjIxXu0I.png?ixlib=js-3.7.0 "image.png")



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/wEU3tEqa6tgOYvid_yPqF.png?ixlib=js-3.7.0 "image.png")



# SNS
create 3 sns topics and add email subscription



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/GhLKP2_xys0-bQThPDIaO.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/SoNymUwpAX9Ad_1OtnoxH.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/AwmuMffhteLUUl1DJLdp6.png?ixlib=js-3.7.0 "image.png")



### Create Auto Scaling Group 


![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/g8DgVOmdW5C0c8_hOwhda.png?ixlib=js-3.7.0 "image.png")



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/w0H7Y4qTE_YLldWMh2FMj.png?ixlib=js-3.7.0 "image.png")



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/pgLhNfs66CeA8Isp69Zwp.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/YUIyuRN0JitoHgwo5Mt8B.png?ixlib=js-3.7.0 "image.png")

#### Then select the app-server-notification and click on create 


## Step 8: Update file in S3
## 
## 
![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/RP8FmPBALPpppQABBL_2q.png?ixlib=js-3.7.0 "image.png")

#### Edit the ngnix config file --> upload the DNS name of internal load balancer in the Proxy_pass ->upload the fille again in S3 bucket














## Step 9:    Create Test Web Server , install packages (Ngnix and Node js) Test connections 


### Create a instance 


![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/p5iPYoEtWB_55g5X-Lgce.png?ixlib=js-3.7.0 "image.png")







Connect to session manager 



```
# =========================================
# COMMANDS TO RUN IN THE WEB SERVER
# =========================================

# =========================================
# COPY WEB-TIER CODE FROM S3
# =========================================
#!/bin/bash
sudo -su ec2-user
cd /home/ec2-user

# !!! IMP !!!
# MODIFY BELOW CODE WITH YOUR S3 BUCKET NAME
sudo aws s3 cp s3://<YOUR-S3-BUCKET-NAME>/application-code/web-tier web-tier --recursive
sudo chown -R ec2-user:ec2-user /home/ec2-user
sudo chmod -R 755 /home/ec2-user

# =========================================
# INSTALLING NODEJS (FOR USING REACT LIBRARY)
# =========================================
# (REF: https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html)	
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install 16
nvm use 16
cd /home/ec2-user/web-tier
npm install
#npm audit fix

# =========================================
# BUILDING THE APP FOR PRODUCTION
# =========================================
# Below command is used to build the code which can be served by the webserver (Nginx)
cd /home/ec2-user/web-tier
npm run build

# =========================================
# INSTALLING NGINX (WEBSERVER)
# =========================================
# (REF: https://dev.to/0xfedev/how-to-install-nginx-as-reverse-proxy-and-configure-certbot-on-amazon-linux-2023-2cc9)
# NOTE: Before using the nginx.conf file in below code, ensure to add the Internal-Load-Balancer-DNS in the nginx.conf file & upload it to S3

sudo yum install nginx -y	
cd /etc/nginx
sudo mv nginx.conf nginx-backup.conf

# !!! IMP !!!
# MODIFY BELOW CODE WITH YOUR S3 BUCKET NAME
sudo aws s3 cp s3://<YOUR-S3-BUCKET-NAME>/application-code/nginx.conf . 
sudo chmod -R 755 /home/ec2-user
sudo service nginx restart
sudo chkconfig nginx on
```


after installing for testing purpose edit the inbound rules :

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/qZ0B1OIkbeT7pnC0ub3uc.png?ixlib=js-3.7.0 "image.png")



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/erBGRMqycLKffW8jH-XU2.png?ixlib=js-3.7.0 "image.png")





### NOW creating ami  of it 


![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/5x91gBB4YjPeO_QbirwF9.png?ixlib=js-3.7.0 "image.png")



### Creating Target Groups 
![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/NmYFxmTfQhHxiKyKpV3sq.png?ixlib=js-3.7.0 "image.png")



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/iXwRLipPoi7OXICYNELCN.png?ixlib=js-3.7.0 "image.png")







### Creating  Launch template 
### 


![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/d533myuSmpgRWWSlDxL_g.png?ixlib=js-3.7.0 "image.png")





### Creating Load Balancer


![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/uaex8SmUepbCT8ILd8vBp.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/O5S4WdAuKdP1cVLg34Dh0.png?ixlib=js-3.7.0 "image.png")



NOW Create Auto scalling group



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/K8YcuOT74orejdAsTPV7x.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/IHaBgWk2ueOd-7Yi2MhdJ.png?ixlib=js-3.7.0 "image.png")



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/FqJ4c5VVwFu7ahnxxuvdB.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/n9pzzbJuh5spuFsBLxuf7.png?ixlib=js-3.7.0 "image.png")



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/GjWRoGEYMEuVO7Pf3z6ER.png?ixlib=js-3.7.0 "image.png")







Now you can see 4 instance are automaticaly creating with help of ASG 

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/-9W6ra9tKcHTHA8kUTAk1.png?ixlib=js-3.7.0 "image.png")







### We can access the website through it External Load balancer end point 


![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/bht_sP6wyrKhvDWBFA0xV.png?ixlib=js-3.7.0 "image.png")







### Open CLoudwatch -> New alarm -> Select metrics
![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/n89F4BsbiEs_gLGlEa3aV.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/AqoeEx7GOS-B-QGciluu9.png?ixlib=js-3.7.0 "image.png")



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/8dRjkhzGkP1kXLCkotzH1.png?ixlib=js-3.7.0 "image.png")

![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/FFdtn1a8xeMrFU99ggvrV.png?ixlib=js-3.7.0 "image.png")



![image.png](https://eraser.imgix.net/workspaces/8brmqATzO8lhk9dXIDIx/4hCfQr0B1VTjDXjuDnR8mUtnais1/GM8-GgplvKbyU4rXz16Ch.png?ixlib=js-3.7.0 "image.png")



