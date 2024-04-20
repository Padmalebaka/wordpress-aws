## Launching the Setup Server

<img width="820" alt="1" src="https://github.com/Padmalebaka/wordpress-aws/assets/164225494/b7be37e1-8b32-43be-ba7c-345bf62b2523">

I this section, we will launch the EC2 instance that will install WordPress in the public subnet AZ1. We are just creating this instance for temporary purposes, we need to install WordPress on EC2 and save the code to EFS, so we do it in the public subnet. We will deploy the Setup Server in the public subnet for ease of installation and file transfer to EFS.

In the AWS console, search for EC2. On the EC2 dashboard, in the "Resources" tab, select "Instances (running)" then "Launch Instances". Give it a name, one the "Application and OS Images (Amazon Machine Images), select "Amazon Linux 2 AMI (HVM)", this is the free tier AMI. Be careful not to choose "Amazon Linux 2023", even though this one is also free tier, it will not work when we try to install WordPress in it.

<img width="785" alt="2" src="https://github.com/Padmalebaka/wordpress-aws/assets/164225494/50a6d4e4-1889-4935-acaf-3cfceb328040">
<img width="786" alt="3" src="https://github.com/Padmalebaka/wordpress-aws/assets/164225494/011108c0-2b52-483d-8927-b2bdd879f4aa">

On "Instance type", make sure that "t2.micro" is selected to keep up with the free tier option. On the "Key pair", select the key pair that you created.

<img width="787" alt="4" src="https://github.com/Padmalebaka/wordpress-aws/assets/164225494/6915f3c5-5380-423f-bbb3-5b81e1b993ee">

On "Network settings", click "Edit". Select your custom Dev VPC and for the subnet, select the public subnet AZ1, this is where we will deploy our EC2 instance, on "Firewall (security groups)", "Select existing security group", this will be your SSH, ALB and Webserver SG. By adding the SSH SG to the setup server, we will be able to SSH into it. Adding the ALB SG will allow us to access the WordPress site from the internet, adding the Webserver SG will enable us to access the RDS database. You can now create the instance, wait a few minutes to make sure that your instance is fully deployed and has the "2/2 checks passed" in the "Status check".

<img width="792" alt="5" src="https://github.com/Padmalebaka/wordpress-aws/assets/164225494/ffc81aec-e150-48ae-80a2-00b525cef9da">