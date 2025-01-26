


AWS PROJECT REPORT

























Submitted By P VIKAS

Under the guidance of Yasaswi Joseph Surabhi
 
What is AWS?
The AWS platform is a comprehensive cloud computing solution for project deployment and management. It includes several key services:


1.	EC2 (Elastic Compute Cloud): Provides resizable virtual servers, enabling easy deployment and scaling of applications on demand.


2.	S3 (Simple Storage Service): Offers scalable object storage for securely storing and retrieving data, such as documents, images, and backups.


3.	VPC (Virtual Private Cloud): Enables the creation of isolated virtual networks to enhance security and control over resources.


4.	CodePipeline: An automated continuous integration and continuous delivery (CI/CD) service that streamlines the software release process.


5.	IAM (Identity and Access Management): Manages user access and permissions to AWS resources, ensuring secure and controlled operations.


Utilizing AWS services like EC2 for computing power, S3 for data storage, VPC for network isolation, CodePipeline for automated deployments, and IAM for access control, project teams can benefit from scalability, reliability, and cost- effectiveness while deploying and managing applications in the cloud.


Advantages of AWS:
AWS (Amazon Web Services) cloud platform offers numerous advantages, some of which include:


1.	Scalability: AWS allows easy and rapid scaling of resources, ensuring that applications can handle varying workloads and traffic without interruptions.
 
2.	Cost-Effectiveness: With a pay-as-you-go pricing model, users only pay for the resources they use, reducing upfront costs and eliminating the need for investing in hardware.


3.	Flexibility: AWS offers a wide range of services and configurations, allowing users to choose the most suitable options for their specific needs.


4.	Global Reach: AWS has data centers in multiple regions worldwide, enabling global distribution of applications and low-latency access to users.


5.	Reliability and Availability: AWS provides high availability and redundancy, ensuring that applications remain accessible even in the event of hardware failures.


6.	Security: AWS implements robust security measures to protect data and applications, including encryption, IAM, DDoS protection, and compliance with various industry standards.


7.	Easy Deployment: AWS services like Elastic Beanstalk and AWS Lambda simplify the deployment process, making it faster and more efficient.


8.	Managed Services: Many AWS services are fully managed, meaning AWS handles administrative tasks like patching, backups, and updates, allowing users to focus on their applications.


9.	Innovation: AWS regularly introduces new services and features, keeping up with the latest technological advancements and enabling users to leverage cutting-edge tools.


10.	Integrations: AWS integrates with various third-party tools and services, allowing users to build comprehensive and customized solutions.
 
11.	Disaster Recovery: AWS offers disaster recovery solutions, enabling businesses to implement robust backup and recovery strategies.


Overall, AWS provides a powerful and reliable cloud infrastructure, empowering businesses and developers to build, deploy, and manage applications with ease, efficiency, and scalability.


Note: Main reason to make AWS popular is an option called Auto Scaling Groups.


Usage of AWS in Real Time:
Some of the most top companies depends upon AWS. Some of those companies are
•	NASA
•	NETFLIX
•	AMAZON
•	AIRBNB
•	NASA
•	SLACK
•	PINTREST
•	ADOBE SYSTEMS
 
PROJECT:
















We need a network to start this project in AWS.
We use our Private Network in AWS to start this project which is called VPC (Virtual Private Cloud).
So, firstly we create a VPC.
To create a VPC we need to login in AWS Console (https://aws.amazon.com/console/) and make sure we are working in Mumbai region as project needs to be done from Mumbai region (*mentioned).
Now in the search bar search for VPC and open it.
 
Now create a VPC
The given requirements to create a VPC are:
•	Name – My VPC
•	IPv4 CIDR – 10.0.0.0/16
After Creating the VPC, proceed to subnets and create two subnets with below configurations.
Note: When VPC is created
•	Route Table – Main
•	DHCP
•	NACL – Main , are created.
These subnets should Private + Public configuration because we host our database servers in Private Subnet which can be only accessible to some of company persons.
1st Subnet:
•	Name – Public
•	VPC – My VPC
•	Availability Zone – ap-south-1a
•	IPv4 CIDR – 10.0.1.0/24
2nd Subnet:
•	Name - Private
•	VPC – My VPC
•	Availability Zone – ap-south-1b
 
But,
When a subnet is created it is created in private nature.
To make a subnet public we need to do two things. They are:
•	Enable auto-assign public IPv4 address
•	Attaching Internet Gateway to Subnet


Now,
Go to internet gateways from EC2 Dashboard and create an internet gateway. After creating an internet gateway attach it to VPC (My VPC).
Note: An internet gateway can be attached to only one VPC.














Now,
Go to route tables which are used to connect to internet and subnet for secured internet connection.
Now, create a Route Table with a name called RT – Custom
 
Now do the following modifications, In the RT – Custom
Go to routes and edit routes and add route with
•	Destination – 0.0.0.0/0 (which is used for internet)
•	Target – Internet Gateway
Now, go to subnet associations and edit subnet associations and add Public subnet.







Now, go to NAT Gateways and create a NAT Gateway with the below configurations.
•	Name – NAT
•	Subnet – Public
•	Allocate Elastic IP












Now, again go back to route tables and edit RT - M which is created while creating the VPC.
 
In that route table go to edit routes and add a route
•	Destination – 0.0.0.0/0 (which is used for internet)
•	Target – NAT Gateway
And after the routes, go to subnet associations and edit subnet associations and add Private subnet.






We have created a network successfully. Now, go to EC2 Dashboard.
Go to, now create an ASG…
But, to create an ASG we need
•	Launch Template
•	Load Balancer
•	Scaling Policies
First create a Launch Template,
Go to launch templates and create a launch template with given configurations.
•	Name – Project
•	Description – AWS
•	Enable Auto Scaling Guidance
•	AMI – Linux 2023 (Free tier Eligible)
•	Instance Type – t2.micro (Free tier Eligible)
•	Key Pair – Create a new key pair named Project
•	Network Settings – Create a new security group with
	Name – Project
	VPC – My VPC
	Inbound Rules
 
Type	Port
Range	Source
Type	Source	Description
ssh	20	My IP	152.58.233.249/32	Admin
HTTP	80	Anywhere	0.0.0.0/0	Everyone
Custom
TCP	8000	Anywhere	0.0.0.0/0	Django

•	Now, go to advanced details and scroll down till user data box and enter some bin bash commands with EFS Attachment Url(nfs client).
Go to EFS in aws console in a new tab and create a file system.
•	Name – Project
•	Select VPC – My VPC
Now open Project and click on attach and copy the NFS Client

Now comeback to EC2 tab
Now, go to Advanced Details in Launch Template and add this command in bin bash commands.
Note: Bin bash commands are used to convert user data box into a terminal. These are used to create an auto server where we save time and doesn’t need to update these commands in the instance terminal using putty or etc.
Bin Bash Command:






 












•	Create a Launch Template















Now, create a Load Balancer with given configuration
•	Load Balancer Type – Classic Load Balancer
•	Name – Project
•	VPC – My VPC
•	Mappings – 1a
•	Security Groups – Project
•	Health Checks -> Advanced Health Check Settings -> Healthy Thresholds ->2
 
•	Create Load Balancer







Now go to Auto Scaling Groups, Create an auto scaling group with below configurations.
•	Name – ASG
•	Launch Template – Project -> next
•	Select VPC – My VPC
•	Availability Zones – 1a,1b -> next
•	Load Balancing -> attach an existing load balancer -> choose from classic load balancer -> Project
•	Health Checks – Turn on Elastic load balancing health checks. -> next
•	Group size
	Desired Capacity – 1
	Minimum Capacity – 1
	Maximum Capacity – 1
•	Scaling Policies (Not needed because we launch only one server)
	Target Scaling Policies
	Scaling Policy Name - Target Tracking Policy
	Metric Type – Average CPU Utilization
	Target Value – 90
	Instances Needed – 10 seconds ->next
	Next -> Next -> Create Auto Scaling Group






A web server is created now using ASG.
 
We create a
To create a jump box
•	Go to instance
•	Launch instance
 
server which used to connect to database securely
 
•	Name – Jump/Bastion Box
•	AMI – Amazon Linux 2023 (free tier eligible)
•	Instance type – t2.micro
•	Key Pair – Create a new key pair -> Jump
•	Network Settings -> edit
•	VPC – My VPC
•	Subnet – Public
•	Security Group -> Create a new security group

Type	Port	Source Type	Source	Description
ssh	22	My IP	152.58.236.64/32	Jump

•	Launch Instance











 
Now we need to create a To create database server:
•	Go to instances
•	Launch instance
Give the below configurations:
•	Name – Database
•	AMI – Amazon Linux 2023 (Free Tier Eligible)
•	Instance Type – t2.micro
•	Key Pair – Create a new key pair -> Name : Database
•	Network Settings -> edit
	VPC – My VPC
	Subnet – Private
	Security Group Rules

Type	Port	Source Type	Source	Description
ssh	22	Custom	Private Ip of
Jump box	Jump
My SQL	3306	Custom	IPv4 CIDR
range of Public
Subnet	Public Subnet

•	Here, My SQL port is opened for public subnet to pull data from database
•	ssh port is opened for jump box to connect safely to database server Launch Instance
Database Server Launched

 
Connect to database server with Jump box
•	Select jump box in instances and click on connect
•	Goto SSH Client and Copy the Public IP
•	Open Puttygen and conver the key pair file to .ppk
•	Now, open putty and paste the public ip in host
•	And go to connection -> SSH - > auth -> credentials
•	Upload your private key file there and connect to instance










Note: We need to connect to Database server from here, so we need database keypair in Jump box
Now, open winscp (which is used to transfer key pair from local pc to instance) Give the following credentials
•	Host: Public ip of jump box
•	Username: ec2-user
•	Advanced Settings -> SSH -> Authentication and Upload Private key
•	Login to Winscp
•	Now just drag the key file and paste it in the Jump Box
•	We have successfully transferred key file
•	Now terminate winscp
 










Now go to instances page in AWS Console
•	Select Database Server and click on connect
•	Go to ssh client and copy the example from it









Now go to jump putty instance and paste the code in the putty











We have successfully connected to Database using Jump Box
 
Now, go to VPC in AWS console Go to Security-> Network ACL’s
•	Create a NACL
•	Name – NACL – C
•	VPC -My VPC





•	Edit Subnet Associations and select Public Subnet





•	Go to inbound roules and add the inbound rules given to security groups in Webserver or ASG









•	Give the same outbound rules








Note: Here, we use epphimeral ports to prevent hacking at subnet level.
 
Now, connect the web server and connect to it using putty
•	Go to instances(running)
•	Select the web server and connect the web server
•	Go to ssh client and copy the Public IP
Use Puttygen to change the key file from .pem to .ppk
Open Puttygen
•	Click on Load
•	And load the Project.pem file
•	Now, click on save private key and save with same name. Now, open putty
•	Paste the IP Address (copied from instance) in the Host Name Box.
•	Now goto Connection -> ssh
-> auth -> credentials
•	And browse for the Project.ppk file
•	Click on connect
•	Username: ec2-user
Now, we create our project with Django
•	Check whether python is installed in your computer else install python.
•	Create a folder for Project in local disk and launch command prompt from the created folder





•	Now, install a virtual environment Command: pip install virtualenvwrapper-win
 
•	Install Django
Command: pip install django
•	Create a project
Command: django-admin startproject <projectname>
•	Now, a project is created now we should create an app
•	To create an app
Command: python manage.py startapp <appname>
•	Now, we should run the server properly Command: python manage.py runserver


























Now, to develop the app we use visual studio code. We should open root folder from Visual Studio Code.
Open Root Folder in Visual Studio Code.
 
Now, create two folders inside application folder using visual studio code
•	static: In which we store all Images, css and javascript files
•	template: In which we store all html files
•	Now, go to project file and edit urls.py

•	Now, go to app and create a file named urls.py and give the below code inside it.

•	Open views.py in app folder and edit the following


•	Now, create a folder named ‘templates’ in Project folder which contains the
source code













•	Go to settings.py in Project folder and edit Templates section
 

•	Go to settings and Change allowed hosts

•	Now run the server























The code process is over. Now we need to upload the project file to github. To upload the project open github from project file location.
And run the following commands:
•	git init
•	git status
•	git add .
•	git commit -m “Anyname”
•	git remote (repository address)
•	git push -u origin main/master
 






After this process now again connect to the instance using putty












•	Become root user Command: sudo su
•	Install git
Command: yum install git

•	Clone the github repository to instance Command: git clone <repo_Link>






•	Now change to the imported repo folder Command: cd reponame
•	Now install python in that repository Command: yum install python
 
•	Install pip
Command: yum install python3-pip

•	Install Django
Command: yum install django


•	Start the server
Command: python manage.py runserver 0.0.0.0:8000








Output:
 
Conclusion:
From this project I had learned to create a basic html page using Python-Django in EC2 instance using my private network which is called VPC. And I have learned how to protect a website from hacking and we use NACL there. I have learned acquired great knowledge and skill by doing this project and rectified many errors which increased confidence in me.


Firstly, I had created a Virtual Private Network to host my project and after that I had created a web server using ASG which helps to create server automatically if any server is crashed. To create ASG I used Launch Template, Load Balancer. Load balancer helps us to balance the load to server and it is the only way to route traffic to server. I had created a database in my private subnet and connected to it by using jump box for secure login. And after creating the Web server I had created a Django project in my local repository by using command prompt and after creating a project I had created an application and developed it in Visual Studio Code. After all that I have used git bash and uploaded my project into my Github Repository. Now, I had connected my Web server and clone my project into my web server instance by using some commands. And finally, I have installed Python and Django in cloned repository and run the server successfully.





Signature of Mentor
