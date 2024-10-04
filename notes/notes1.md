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
