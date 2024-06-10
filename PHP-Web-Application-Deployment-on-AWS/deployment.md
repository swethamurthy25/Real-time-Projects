# $\textcolor{red}{PHP\ Web\ Application\ Deployment\ on\ AWS}$

### $\textcolor{red}{Abstract}$

The primary objectives of the capstone project are to deploy the PHP application (web app) that runs on EC2 and to create an RDS database for the PHP application to query and interact with it. Then we need to secure the application to prevent public access to the backend database systems and update the necessary parameters in the AWS parameter store. For the web application to scale in response to user demand, we need to build an auto-scaling group and load balancer. We must utilize the four parameters listed in the AWS lab instructions to connect to the database. We are also provided with an SQL dump and the PHP files that can be used to build the solution.


### $\textcolor{red}{AWS\ Architectural\ Design}$

![final](https://github.com/swethamurthy25/Real-time-Projects/assets/112581595/c9c34174-4098-4498-a955-f89837c1bd78)



### $\textcolor{red}{Implementation\ Procedure}$

* A handful of the already-installed services, including VPC (Example VPC), subnets (two public subnets and two private subnets) security groups (Inventory-App, Bastion-SG, 
  ALBSG, Example-DBSG), and AMI, need to be inspected first before proceeding with the actual deployment process.
* Here, I have noted down the subnet IDs and IP addresses of the subnets so that we can utilize them in the upcoming steps.
* We may see that the AMI at this time has no images.

* To utilize the backend to connect with the PHP application, our first task is to set up the database instance and DB subnet groups.
* Due to its high availability and data redundancy properties, I have chosen to create my database using the standalone technique and deploy it using a multi-AZ DB instance. * Then, to constantly have access to the database, the credentials area needs to be kept updated with the username and password information.
* To prevent anyone other than the EC2 instance and devices connected to the VPC from accessing the database through RDS, the "No" checkbox for the public access option must 
  be enabled.
* I have also enabled the database's security group (inbound rules-MySQL/Aurora) to access the cloud9 application.
* After creating the database, I started to build the cloud9 environment using the direct access type and t2. micro (free tire) instance type on the Amazon Linux 2 platform 
* The LAMP web server was then installed on Amazon Linux 2 on the cloud9 instance, and I used this server to host a dynamic PHP application that reads and writes
  data to a database.
* The httpd MariaDB server and MariaDB are installed using a set of Sudo commands.
* After installing MariaDB, I set the HTTP server to open port 80 so that anyone with a web browser can view the website.
* Since the sample demo application (Example.zip) is already available to us, I simply unzipped it in the directory and moved the files to the /var/www/html target 
  directory.
* I then copied the cloud9 instance's public IP and attempted to access it using the Chrome web browser.
* I was able to access the web application, and I next checked the queries section to see if I could access the database tables like population, GDP, or mobile phones.
* Since there is no value entered in the data at this time, I was not initially able to query the web application.
* However, I could not be able to access the queries in the data until the httpd-MariaDB Maria server is linked to the appropriate parameters in the system manager with the 
  necessary username, password, endpoint, and DB.
* To accept traffic from the internet and forward it to the EC2 in the private subnet, I next configured an application load balancer using Example VPC and mapped it to the 
  public subnets 1 and 2.
* For autoscaling, I created an AMI image of the Cloud9 instance.
* When constructing load balancers and auto-scaling groups, I adhered to the two fundamental rules: using the private subnets for autoscaling and public subnets for load balancers. Using the load balancer's DNS name, I was able to quickly link to the EC2 instance, but I was unable to view my database from the domain name under queries.
* I then returned to my load balancer and modified the private subnet setting to public and I also made sure the AWS Manager was set up with the proper parameter settings. * * Finally, I executed the query again to view the GDP, mobile devices, and other tables that were in the database, and this time I was able to view them successfully.


