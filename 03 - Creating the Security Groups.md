## Creating the Security Groups

<img width="1044" alt="1" src="https://github.com/Padmalebaka/wordpress-aws/assets/164225494/81085612-c359-4576-ad29-b6731b1b4d1a">

*In AWS, a Security Group (SG) is a virtual firewall that controls inbound and outbound traffic for EC2 instances, RDS instances, and other resources within a VPC. It acts as a first line of defense for your resources, allowing you to define inbound and outbound rules to control network access.*

In this section we will create multiple Security Groups:

- ALB SG
- SSH SG
- Webserver SG
- Database SG
- EFS SG

### ALB SG

![2](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/df10ddb6-fa1e-4457-9ec3-ea25daa573d1)

The first Security Group we will create is the Application Load Balancer (ALB) SG. We will open port 80 (for HTTP connection) and 443 (for HTTPS connection) and the source is going to come from anywhere on the internet (0.0.0.0/0).

Search for VPC on the AWS console. On the VPC dashboard, select "Security groups", click "Create security group". Give it a name and the same name for the description, select your custom Dev VPC that you created. On "Inbound rules", click "Add rule", we will add two ports:

1. Type: HTTP (port 80). Source: Anywhere-IPv4 (0.0.0.0/0)
2. Type: HTTPS (port 443). Source: Anywhere-IPv4 (0.0.0.0/0)

It's worth noting that, since SGs are stateful (when you allow inbound traffic, the corresponding outbound traffic is automatically allowed) you do not need to add an explicit outbound rule to allow connection traffic when you have already added an inbound rule in your security group.

![3](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/41d1f7ef-fddb-417e-8bf9-7073441de235)
![4](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/002d3c69-e9e8-46a1-b4c4-b3d947b11a45)

### SSH SG

The next Security Group we will create is called the SSH (Secure Shell) SG. This is the SG we will use for the SSH connection on the EC2 instances by opening port 22 (for SSH connection). The source is going to come from your IP address. Every time you create an SSH SG, you should always limit the source of the traffic to your IP address.

On VPC dashboard that is on the left side, select "Security groups", click "Create security group". Give it a name and the same name for the description, select your custom Dev VPC that you created. On "Inbound rules", click "Add rule", we will add one port:

1. Type: SSH (port 22). Source: My IP

![5](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/d6325185-c5dc-44d7-8a23-4b2792af446b)
![6](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/4b81bcf2-413c-42e7-82b7-59357d406cd5)

### Webserver SG

![7](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/040d2a60-5a37-49de-be9e-75c7203106d4)

The next Security Group is the Webserver SG. This is the SG we will add to the webservers in the private app subnets. We will open port 80 (for HTTP connection) and 443 (for HTTPS connection) and the source is going to come from the ALB SG. We will also open port 22 (for SSH connection) and the source is going to come from the SSH SG.

On VPC dashboard that is on the left side, select "Security groups", click "Create security group". Give it a name and the same name for the description, select your custom Dev VPC that you created. On "Inbound rules", click "Add rule", we will add three ports:

1. Type: HTTP (port 80). Source: Custom and select ALB SG
2. Type: HTTPS (port 443). Source: Custom and select ALB SG
3. Type: HTTP (port 22). Source: Custom and select SSH SG

![8](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/e7dbd237-e7f3-425c-869b-94011b0a45f9)
![9](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/0fd63cf7-ee4e-409f-a865-2a11996fc99a)

### Database SG

![10](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/86d2c763-fa87-484d-a3b9-f249a777b4c4)

The next Security Group is the Database SG. This is the SG we will add to the RDS istance in the private data subnet. We will open port 3306 (for MySQL connection) and the source is going to come from the Webserver SG.

On VPC dashboard that is on the left side, select "Security groups", click "Create security group". Give it a name and the same name for the description, select your custom Dev VPC that you created. On "Inbound rules", click "Add rule", we will add one port:

1. Type: MYSQL/Aurora (port 3306). Source: Custom and select Webserver SG

![11](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/42bfa764-ed9a-4bf6-9089-b559dfe82ca8)

### EFS SG

![12](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/5203a3f8-2f8c-4ea9-80dd-bc8a55c2b435)

The last Security Group is the EFS SG. This is the SG we will add to the EFS resource. We will open port 2049 (for NFS (Network File System) protocol connection) and the source is going to come from the Webserver SG and the EFS SG. We are also going to open port 22 and the source is going to come from the SSH SG.

On VPC dashboard that is on the left side, select "Security groups", click "Create security group". Give it a name and the same name for the description, select your custom Dev VPC that you created. On "Inbound rules", click "Add rule", we will add two ports:

1. Type: NFS (port 2049). Source: Custom and select Webserver SG
2. Type: SSH (port 22). Source: Custom and select SSH SG

![13](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/fbadefb3-12fc-4150-8b36-fdc7576d2a0e)
![14](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/e862f20a-8c62-4a60-ba94-9aa7d1e32838)

After you created the SG, on the "Inbound rules" tab, select "Edit inbound rules". Add one more port:

1. Type: NFS (port 2049). Source: Custom and select EFS SG

![15](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/376776c7-5e2b-499d-8299-0c96885e040e)

The reason for adding a rule to an EFS security group to allow it to contact itself is to allow the various instances or resources that are associated with the security group to access the EFS file system. Without this rule, the instances or resources would not be able to communicate with the EFS file system and would not be able to access or store files on it. By allowing the SG to contact itself, you are essentially creating a permission for the resources within the security group to communicate with the EFS file system. This is an important step in the process of setting up and using an EFS file system.