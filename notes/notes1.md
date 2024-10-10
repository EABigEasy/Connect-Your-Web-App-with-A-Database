### AWS SERVICES
1.Amazon Aurora
2.Relational databases
3.EC2
4. Command Line Interface

## Summary
In this project, you'll be recreating one of THE most common use cases of cloud computing.

There are countless web apps on the Internet today, and engineers use databases all the time to store the web app's data e.g. login credentials, e-commerce product information, movie reviews... you name it!

Let's build a mini version of this pattern, by setting up a web app from scratch and connecting it with a relational database (Amazon Aurora) to store things that users input.

### Step1 
Login with your IAM user

### Step 2
Launch EC2 Instance
***Why do we need an EC2 instance to run a web app?*** 
A web app needs to run on a computer. But are we going to go buy a physical computer to do this?
No! We're going to use a virtual computer that we rent through AWS. This is exactly what EC2 is here for. 
Think of an EC2 instance as a rented virtual computer that you use to run your web app. It's the place where all the processing, data handling, and user interactions happen.

- name of instance nextwork-ec2-instance-web-server
- Choose the Amazon Linux 2023 AMI.
- Create a key pair
   What is a key pair? Why are we creating one?
A key pair in EC2 is quite literally the keys to access our EC2 instance.
We need keys to our EC2 instance if we want to add, change, or update how our EC2 instance is running.
For your Key pair name, enter NextWorkAuroraApp
Leave your Key pair type as RSA.
Leave your Private key file format as .pem since we're using SSH later on to access our EC2 instance.
A new file will automatically download to your computer - don't be alarmed!
This is your new key pair. We'll need it for later in the project to access our EC2 instance.
Back in our EC2 creation, under Network settings, set these values and keep the other values as their defaults:
For Allow SSH traffic from, choose your IP address if it's correct. Otherwise select Anywhere.
Check the boxes for Allow HTTP traffic from the internet.
When you're ready, choose Launch instance.

Navigate back to your list of EC2 instances, and then select the checkbox next to your new instance.
In the Details tab, note the following important details:
In Instance summary, note the Public IPv4 DNS.
Note the value for Key pair assigned at launch.

### Step 3
Create an Aurora MySQL Database
Aurora MySQL DB is a relational database
 What IS a relational database? How is it different from a normal database?
A relational database is a type of database that organizes data into tables, which are collections of rows and columns. Kind of like a spreadsheet! We call it "relational" because the rows relate to the columns and vice-versa.

When a database is relational we can query it using a special language called SQL (Structured Query Language)


Let's create your Aurora database

Head to RDS console - search for rds
 in search bar at the top of the screen.
Notice that even if you search for Aurora
, the same result shows up!
-In the left navigation bar, select Databases.
-In the Database section, select Create Database.
-On the Create database page, choose Standard Create.
-In the Configuration section, make the following changes:
-For Engine type, choose Aurora (MySQL Compatible).

Strangely enough, there isn't really such a thing as a "normal" database. We have relational databases and non-relational databases (also known as NoSQL)

- Make sure you're in the same region ( your Aurora DB)  as your web server EC2 instance.

***What is engine type?***
The engine type is like the core software that powers our database. Imagine it as the operating system of a computer, but for databases.


***How is Aurora different to other databases?***
AWS Aurora is a type of relational database. As you can tell from the first few steps of creating a relational database, there are plenty of options to choose from! 

We'd use AWS Aurora if we needed something large-scale, with peak performance and uptime. This is because Aurora databases use clusters (more on that later!). Ordinary relational databases, like MySQL and Oracle are more generic and cost-effective. They suit smaller databases and less demanding workloads.

### In summary; Aurora is for the big jobs. Fun fact: Coca-Cola uses Amazon Aurora to store their consumer data globally!


For Engine Version, choose Aurora MySQL 3.05.2 (compatible with MySQL 8.0.32) - default for major version 8.0
For Templates, choose Dev/Test

***What are Templates?***
Templates are pre-set settings that help you quickly set up your database environment according to your needs. It's basically AWS helping you make selections for the rest of this set-up page! The Dev/Test template is designed for development and testing environments, helping you pick lower cost options.***


***In the Settings section, set these values:***

DB cluster identifier: nextwork-db-cluster

Master username: admin

For Credentials management select Self managed.

Master password: Type a password (eg. n3xtw0rk)

Confirm password: Retype the password.

Make sure you save your database login details somewhere safe! You'll need them later on.


***Leave the Cluster storage configuration settings as default..***
In the Instance configuration section,
set these values:. Burstable classes (includes t classes). db.t3.medium


***What are Burstable classes?***
The Burstable classes (includes t classess) are cost-effective types of database instances that are best when you have a consistent baseline level of traffic with occassional, random spikes in demand...a sudden "burst" of traffic. 
By choosing burstable classes, our database can perform well under high traffic, but also save us costs when things are quiet.Extra for Experts: 
The other two DB instance classes are Serverless V2, which let you only pay for what you use (scales automatically from zero traffic), and memory optimized classes, which are designed for high-performance databases that need to do heavy, memory-intensive tasks.



That's it! We've created our web app's server in the previous step.
Let's connect that instance to our database now.. 
For Compute resource, choose Connect to an EC2 compute resource..
Now select the drop down for EC2 instance, and choose ***nextwork-ec2-instance-web-server.***

Scroll down and open the Additional configuration section.. 
Enter sample for Initial database name.

Keep the default settings for the other options..
Select Create database.

Close any pop-ups that appear.. 
Your new DB cluster will show in the Databases list with the status Creating.


Do you notice that your database has the name nextwork-db-cluster and that there's 2 of them? What's with that?.
***Note***: It may take five or more minutes for the database to be created.
***What's this 'database cluster' business?***
Remember how we said that Aurora is really good for the big jobs? The reason for this is ****clusters****.
****A database cluster**** in Aurora is a group of database copies that work together so your data is always available.Each cluster consists of a primary instance (where all write operations occur) and multiple read replicas as back-ups. If your database's primary instance fails, one of the replicas can be promoted to primary automatically.. 

Wait for the Status of your new DB cluster to show as Available.. 
Select the DB identifier of your top database to take a look at the details..
Notice that there are two Endpoints in our Database. Cool! This is our cluster in action.. 

#### Understanding Reader vs Writer instances####

Our writer database instance is our primary instance that handles all the "write" operations like INSERT, UPDATE, and DELETE.Our reader database instance is our backup instance that can do very basic operations like SELECT. This is used to get data, but not to add or change data.Why separate read and write?We only want one writer instance at a time so that things stay focused and controlled...but we want multiple reader instances so that we can share the workload for read requests as our database grows and have a backup if our writer instance fails.What is an endpoint?In general, endpoints are like contact points where data flows in and out. A super popular example is a website URL! When you enter a website's address (like nextwork.org), your browser uses that endpoint to receive data and load the page. For databases, an endpoint is how your app finds a database to ask for data or update it.. Yay! We've created a new relational database and have a waiting EC2 instance for our beautiful new web app. . 

This is what we created in this step:. 

![architecture-diagram aurora](https://github.com/user-attachments/assets/4efde979-00d3-400a-a018-0ff0b14d052a)

### Step 4
Create your web app. 
In this step, you're going to:
Connect to your EC2 instance through SSH.. 
Install a basic Web App that runs on your EC2 instance.

. ![architecture-diagram6](https://github.com/user-attachments/assets/06740ebd-a0f8-49e2-985c-0359815d0320)





Open your local Terminal. 
On a Mac:. Press Cmd + Space to open Spotlight.. Type Terminal and press Enter..
On a Windows/Linux:. Press Windows + R to open the Run dialog.. Type cmd or powershell and press Enter.


Connect to your EC2 instance. You need to access your .pem file in order to login successfully to your EC2 instance - remember, the .pem file is like your keys to your EC2 instance!. Find your .pem file on your local computer (it's probably in your downloads folder!) and put it in a new folder on your Desktop labelled nextwork. Nice! Now we need to navigate to that folder from your terminal, so we can use it.. Run the command ls in your terminal - this shows you all the folders that you can see from your current terminal position.
