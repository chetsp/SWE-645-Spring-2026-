# SWE-645-Spring-2026-
CampusConnect Deployment
Student Name: Chetana Patel

Course: SWE-645

Assignment 1 - S3 and EC2 Deployment

1. Project URLs
S3 Static Website: http://645-1-my-campus-connect-site.s3-website.us-east-2.amazonaws.com

EC2 Web Application: http://3.23.87.66

2. S3 Bucket Configuration Steps
Bucket Creation: Created a unique bucket named 645-1-my-campus-connect-site in the us-east-2 region.

Public Access: Disabled "Block all public access" settings to allow the website to be viewable.

Static Hosting: Navigated to Properties > Static website hosting and enabled it, setting index.html as the index document.

Bucket Policy: Applied a JSON bucket policy to grant s3:GetObject permissions to all users ("Principal": "*") so the site is publicly reachable.

Artifacts: Uploaded index.html and campus.jpg to the root of the bucket.

3. EC2 Instance Configuration Steps
Launch: Launched an Ubuntu 24.04 LTS t2.micro instance.

Security Group: Configured inbound rules to allow:

SSH (Port 22): For administrative access.

HTTP (Port 80): For public web traffic.

Software Installation: Installed the Apache2 web server using the apt package manager.

Deployment: Moved HTML source files to the /var/www/html directory and pulled the campus image from S3.

Custom 404: Modified the Apache virtual host configuration to redirect invalid requests to a custom error.html page.

4. Implementation Commands (Linux Terminal)
The following commands were executed on the Ubuntu server:

Bash
# System Update & Apache Installation
sudo apt update -y
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2

# Directory Setup & Image Retrieval
sudo rm /var/www/html/index.html
sudo wget -O /var/www/html/campus.jpg http://645-1-my-campus-connect-site.s3-website.us-east-2.amazonaws.com/campus.jpg

# Permissions Configuration
sudo chown -R ubuntu:ubuntu /var/www/html
sudo chmod 644 /var/www/html/*.html
sudo chmod 644 /var/www/html/campus.jpg

# Custom Error Page Configuration
# Edited /etc/apache2/sites-available/000-default.conf to add:
# ErrorDocument 404 /error.html
sudo systemctl restart apache2

5. Source File Descriptions
index.html: The homepage of the application featuring campus information and navigation links.

survey.html: A data collection form for students, including text inputs, radio buttons, checkboxes, and a raffle entry.

error.html: A custom-designed error page that displays when a user attempts to access a non-existent URL on the server.

campus.jpg: The image asset used across both S3 and EC2 deployments.
