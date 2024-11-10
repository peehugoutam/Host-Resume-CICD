# Host-Resume-CICD

Host Your Resume on AWS EC2 with a CI/CD Setup Using GitHub Actions

# GITHUB setup

1.  Create git repo
2.  Generate PAT(personal access token) - with the selected option (workflow, repo)
3.  Add .readme file
4.  Take git clone on your local machine
5.  git clone https://github.com/peehugoutam/Host-Resume-CICD.git
6.  git init
7.  To set git config locally on current directory :-

    git config user.name "Your Personal Name"
    git config user.email "yourpersonalemail@example.com"
    git config --list --local
    git remote add origin https://github.com/yourusername/your-personal-repo.git
    git add .
    git commit -m "Initial commit"
    git push -u origin main

    # Issue :- remote: Permission to peehugoutam/Host-Resume-CICD.git denied to VarshaGoutamLMS.

         fatal: unable to access 'https://github.com/peehugoutam/Host-Resume-CICD.git/': The requested URL returned error: 403

         1. Confirm Local Git Credentials:
         git config user.name
         git config user.email

         2. If you’re using HTTPS to push code, Git may cache your professional credentials. To update it:
         git credential-cache exit

         3. Set Up Personal Access Token (If Required):

         4. Force Use of Local Configuration:
         git remote set-url origin https://<your_username>:<your_personal_access_token>@github.com/peehugoutam/Host-Resume-CICD.git

         5. After this, try pushing the code again. If issues persist, consider removing the cached credentials using:
         git credential-manager erase

8.  After this create .github/workflows/deploy.yml
    workflows directory can contain different yml file that contain different setting to deploy automatically code on EC2
9.  Go to setting -> secret & variables -> action -> Add repository secret(mentioned in yml file) according to current repo setting option
10. Whenever push taken place on mail branch then code will automatically deploy on EC2
11. All this deployment that is going on can we see on action tab where deploy.yml workflow will be shown

## Ec2 setup

1. Create EC2 instance by selecting ubunut as virtual machine, and free tier select
2. Create add key-pair by .pem file

## Connect EC2 using SSH

1. Give permission to .pem file then connect
   chmod 400 contact-CICD.pem
   ssh -i contact-CICD.pem ubuntu@ec2-16-171-154-110.eu-north-1.compute.amazonaws.com
2. Create directory over there
   ls /var/www/html/
   sudo mkdir -p /var/www/html
   sudo chown -R ubuntu:ubuntu /var/www/html
   sudo chmod 755 /var/www/html
   ls /var/www/html/
3. Check file content by this command - cat /var/www/html/index.html
4. Access the File via Web Browser - http://<EC2_PUBLIC_IP>/index.html

Note :- Need to setup your EC2 ubuntu VM

1.  Install Apache
    sudo apt update
    sudo apt install apache2 -y # for Ubuntu/Debian
2.  Start and Enable the Web Server:
    sudo systemctl start apache2 # or nginx
    sudo systemctl enable apache2 # or nginx
3.  Place index.html in the Correct Web Server Directory
    ls /var/www/html/
4.  Check File and Directory Permissions
    sudo chmod 644 /var/www/html/index.html
    sudo chmod 755 /var/www/html
5.  Verify EC2 Security Group Settings
    a. Go to the EC2 Console.
    b. Select Security Groups in the left sidebar.
    c. Choose the security group associated with your EC2 instance.
    d. Check the Inbound rules and ensure there is a rule that allows HTTP traffic on port 80 from 0.0.0.0/0 (or from specific IP ranges as needed).
6.  Test in a Browser
    http://<EC2_PUBLIC_IP>/index.html
7.  Check the Web Server Status
    sudo systemctl status apache2
8.  View error logs for Apache:
    sudo tail -f /var/log/apache2/error.log

# Same EC2 instance can be connect on FTP to see code

1. Host = Public ipv4 address
2. Logon type = key file
3. Protocol = SFTP
4. User = ubuntu/ec2-user
5. Key file = .pem file
6. Path = /var/www/html
