# SWE-645: CampusConnect Deployment
**Student:** Chetana Patel  
**Course:** SWE-645
**Github Link:** https://github.com/chetsp/SWE-645-Spring-2026- 
**Assignment:** Assignment 1 - S3 and EC2 Deployment

---

## 1. Project URLs
* **S3 Static Website:** [View Site](http://645-1-my-campus-connect-site.s3-website.us-east-2.amazonaws.com)
* **EC2 Web Application:** [View App](http://3.23.87.66)

## 2. S3 Bucket Configuration
* **Bucket Creation:** Created `645-1-my-campus-connect-site` in `us-east-2`.
* **Permissions:** Disabled *Block all public access* and applied a JSON policy for `s3:GetObject`.
* **Hosting:** Enabled *Static website hosting* with `index.html` as the entry point.
* **Artifacts:** Uploaded `index.html` and `campus.jpg`.

## 3. EC2 Instance Configuration
* **Instance:** Ubuntu 24.04 LTS (`t3.micro`).
* **Security Group:**
  * Port 22 (SSH) for admin access.
  * Port 80 (HTTP) for public traffic.
* **Web Server:** Apache2 installed via `apt`.
* **Customization:** Configured a custom 404 redirect to `error.html` in the virtual host file.

## 4. Implementation Commands
```bash
# System Update & Apache Installation
sudo apt update -y
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2

# Directory Setup & Image Retrieval
sudo rm /var/www/html/index.html
sudo wget -O /var/www/html/campus.jpg [http://645-1-my-campus-connect-site.s3-website.us-east-2.amazonaws.com/campus.jpg](http://645-1-my-campus-connect-site.s3-website.us-east-2.amazonaws.com/campus.jpg)

# Permissions Configuration
sudo chown -R ubuntu:ubuntu /var/www/html
sudo chmod 644 /var/www/html/*.html
sudo chmod 644 /var/www/html/campus.jpg

# Custom Error Page (Edit /etc/apache2/sites-available/000-default.conf)
# Add: ErrorDocument 404 /error.html
sudo systemctl restart apache2

```
## 5. Source File Descriptions
| File Name | Description |
| :--- | :--- |
| `index.html` | Main landing page with campus info and links. |
| `survey.html` | Data collection form with raffle entry field. |
| `error.html` | Custom 404 page for non-existent URLs. |
| `campus.jpg` | Image asset used across S3 and EC2 deployments. |

