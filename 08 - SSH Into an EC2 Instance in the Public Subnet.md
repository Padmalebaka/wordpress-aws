## SSH Into an EC2 Instance in the Public Subnet

In this section, we will learn how to SSH into an EC2 instance in the public subnet. There is a slight difference in installing it for Mac/Linux and Windows users.

In the EC2 dashboard, select "Instances" and then select the instance you created. Copy the public IPv4 address provided.

<img width="860" alt="1" src="https://github.com/Padmalebaka/wordpress-aws/assets/164225494/60771dee-bf41-4848-947e-e6bae5a5201b">

### For Mac/Linux Users

Open your terminal and type "ssh -i myec2key.pem ec2-user@<yourIpv4Here>". Paste your IPv4 address after "ec2-user@", on \<yourIpv4Here\>. Remember to store your key pair on your home directory, otherwise you will have to specify the directory that the key pair is stored in the command. For the first time using this command, you will get a warning message and it will be asking if you are sure you want to continue connecting, just type "yes" and you will be fine. Be also aware that, if by any means you change your private IP (continue with your project in a different location, like your friend's house), the connection will not be possible, you will then have to select your new IP on the SSH SG.

<img width="579" alt="2" src="https://github.com/Padmalebaka/wordpress-aws/assets/164225494/6ac207d1-9671-4765-a7ed-496495d99901">

### For Windows Users

Open PuTTY, select "Session" on its dashboard, the "Host Name (or IP address)" will be "ec2-user@<yourIPaddress>".

![3](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/f13a16af-555b-4100-8a38-66f844452874)

On the dashboard again, select the "+" on "SSH", then select the "+" on the "Auth", select "Credentials".

![4](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/b7cd37c8-f0f3-4a6f-b7df-e6404222a81e)

The first time you access your EC2 instance, you will get a "PuTTY Security Alert", just accept it. Now you have your instance open on Windows via SSH.

<img width="894" alt="5" src="(https://github.com/Padmalebaka/wordpress-aws/assets/164225494/0fc48f96-320b-42df-a4fc-ef29f670ee9e">