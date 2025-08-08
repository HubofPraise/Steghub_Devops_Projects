# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS #

## WHAT IS TECHNOLOGY STACK ##
A technology stack (or tech stack) refers to the combination of tools, frameworks, programming languages, 
libraries, and services used to build and run a software application.

There are different web technology stacks, namely:
1. LAMP - {Linux, Apache, MySQL, PHP or Python or Perl}
2. LEMP - {Linux, Nginx, MySQL, PHP or Python or Perl}
3. MEAN - {MongoDB, ExpressJS, ReactJS, NodeJS}
4. MERN - {MongoDB, ExpressJS, AngularJS, NodeJS}

## What is the LAMP Stack? ##
The LAMP stack is a popular collection of open-source software used to build and run dynamic websites and web applications. It's one of the most widely used backend setups in the web development world, especially for hosting applications on servers.

Each letter in LAMP represents a layer in the stack:

L – Linux

This is the operating system that acts as the base for the entire stack.
- It manages hardware resources and system processes.
- Linux is free, secure, and widely used in server environments.

A – Apache

Apache is the web server software that delivers web content to users.
- It receives requests (like when you open a website) and sends back the correct web pages.
- Apache is reliable and customizable, making it ideal for handling traffic on websites.

M – MySQL

MySQL is a relational database system used to store and manage data.
- It organizes data in tables and allows websites to retrieve or update information as needed.
- For example, user profiles, login details, or product listings can all be stored in a MySQL database.

P – PHP or Perl or Python)

This is the programming language used to write the logic of your web application.
- It connects everything — handling user input, talking to the database, and generating the right content to display on a web page.

## Getting Ready to Deploy a LAMP Stack on AWS ##

Before jumping into the setup, it’s important to prepare your environment. Deploying a LAMP stack in the cloud (specifically on AWS) means you'll be using cloud computing resources to host your website or application. To make the process smooth, a few things need to be in place.

Below are the essential prerequisites you should have before starting

1. AWS Account
- Sign up at [AWS Account](https://aws.amazon.com )
- Ensure your account is activated and billing info is added for [Free Tier access](https://youtu.be/5MRTCayjzQk?si=rZ5Lab58q52w44GT).

### How to Create an EC2 Instance on AWS ###
An EC2 instance is a virtual server in Amazon’s cloud. You’ll use it to install your LAMP stack.

Step-by-Step Instructions
1. Log in to AWS Console
- Go to https://aws.amazon.com
- Click Sign In to the Console
- Use your root or IAM user credentials

![How to Lunch an Instance](./IMAGES/aws_login.PNG)


2. Open the EC2 Dashboard
- In the Services menu, search for and click EC2
- Click Launch Instance

3. Configure Instance Basics
- Name your instance: e.g., lamp-server
- Amazon Machine Image (AMI):
- Choose Amazon Linux 2 or Ubuntu Server 22.04 LTS

Instance type:
-Select t2.micro (Free Tier eligible)

4. Create or Choose a Key Pair
- Under Key pair (login):
- Choose “Create new key pair” or select an existing one

If creating new:

- Name it (e.g., lamp-key)
- Choose .pem format (for Linux/macOS) or .ppk (for Windows PuTTY users)

> Download and save it securely – you’ll need it to connect

5. Configure Network Settings
- VPC and Subnet: Leave as default (unless you’ve set up custom VPC)

Firewall (Security group):
- Create a new security group with these inbound rules:
- SSH – TCP – port 22 – from My IP
- HTTP – TCP – port 80 – Anywhere (0.0.0.0/0)
- HTTPS – TCP – port 443 – Anywhere (optional)

6. Storage Configuration
- Keep the default 8 GB (you can increase it if needed)

7. Review and Launch
- Double-check all configurations

Click Launch Instance

> Wait a few seconds → you’ll see “Your instances are now launching”

8. Access Your EC2 Instance
- Go to the Instances section
- Select your instance → click Connect
- Choose the SSH tab → follow the instructions provided using your downloaded .pem key






