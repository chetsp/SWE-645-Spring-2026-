
SWE-645-SPRING-2026: CAMPUSCONNECT DEPLOYMENT
============================================================
Student Name: Chetana Patel

Course:       SWE-645

Assignment:   Assignment 1 - S3 and EC2 Deployment

1. PROJECT URLs
------------------------------------------------------------
S3 Static Website: 
http://645-1-my-campus-connect-site.s3-website.us-east-2.amazonaws.com

EC2 Web Application: 
http://3.23.87.66


2. S3 BUCKET CONFIGURATION STEPS
------------------------------------------------------------
* Bucket Creation: Created a unique bucket named 
  '645-1-my-campus-connect-site' in the us-east-2 region.
* Public Access: Disabled "Block all public access" settings 
  to allow the website to be viewable.
* Static Hosting: Enabled "Static website hosting" in 
  Properties, setting index.html as the index document.
* Bucket Policy: Applied a JSON bucket policy to grant 
  s3:GetObject permissions to all users ("Principal": "*").
* Artifacts: Uploaded index.html and campus.jpg to the root.


3. EC2 INSTANCE CONFIGURATION STEPS
------------------------------------------------------------
* Launch: Launched an Ubuntu 24.04 LTS t2.micro instance.
* Security Group: Configured inbound rules for:
    - SSH (Port 22): Administrative terminal access.
    - HTTP (Port 80): Public web traffic access.
* Installation: Installed Apache2 using the apt package manager.
* Deployment: Moved source files to /var/www/html and pulled 
  the campus image from the S3 bucket using wget.
* Custom 404: Modified the Apache virtual host configuration 
  to redirect invalid requests to error.html.


4. IMPLEMENTATION COMMANDS (LINUX TERMINAL)
------------------------------------------------------------
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


5. SOURCE FILE DESCRIPTIONS
------------------------------------------------------------
* index.html: Main landing page with campus info and links.
* survey.html: Data collection form with raffle entry field.
* error.html: Custom 404 page for non-existent URLs.
* campus.jpg: Image asset used across S3 and EC2 deployments.

============================================================
