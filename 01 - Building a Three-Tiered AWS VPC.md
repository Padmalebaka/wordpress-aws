## Building a Three-Tiered AWS VPC

![image](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/41a9b926-7e8f-41fe-9df1-35d9d664c563)

*Creating VPCs and subnets provides you with flexibility, control, and security when deploying and managing your resources in the AWS cloud. It allows you to build a customized network infrastructure tailored to your specific needs while maintaining a secure and isolated environment.*

We will start by creating a custom VPC, we will build a three-tiered VPC using the architecture shown above.
In this architecture, your infrastructure is divided into three tiers:

- **Tier 1 - Public Subnet**: This subnet will have resources like the NAT gateway and the load balancer
- **Tier 2 - Private App Subnet**: This is the subnet that will hold our web servers (EC2 instances), managed by an auto scaling group.
- **Tier 3 - Private Data Subnet**: This subnet will hold our database.

We will duplicate these subnets across another Availability Zone (AZ) for High Availability (HA) and fault tolerance. Next, an Internet Gateway (IG) will be created and a Route Table to allow the resources in our VPC to have access to the public internet.

### Creating the VPC
Fist, make sure you select N. Virginia as your region on the AWS console. Search for the VPC service in the search bar and on the VPC page, click on "Create VPC". We have to give a name to the VPC and its IPv4 CIDR, which is the one shown in the diagram above (10.0.0.0/16). Leave the rest as default.

![2](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/1909efc3-07ce-455b-8907-89f36a2f513a)
![3](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/664af5e8-9499-47a8-8b30-229938eb798f)

Now enable DNS hostname choosing your VPC and select "Action" > "Edit VPC settings". Check the box that says "Enable DNS hostnames".

![4](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/b47e7592-aac2-4873-bcbc-5ad060d8fe04)

### Creating the Internet Gateway

In the VPC service, select the "Internet gateways" option on the left side, then click "Create Internet Gateway". Just give your Internet Gateway a name for now.

![5](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/92db1d06-39a2-437b-b42b-92b873c08037)

Attach the Internet Gateway to the VPC that we previously created, this way we can allow the VPC to communicate with the internet. Select "Actions" > "Attach to VPC". Select the VPC you created. You can only attach one Internet Gateway to one VPC.

![6](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/aa04522c-b2cd-429d-85f9-9b6eb155b831)

### Creating the public subnets

In the VPC service, select "Subnets" on the left side, then click "Create subnet". Select the VPC you created, give the subnet the name "Public Subnet AZ1", select "us-east-1a" as the AZ and "10.0.0.0/24" as the CIDR block.

![7](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/6a446a33-2769-47dd-8e8a-75699febdb0d)
<img width="810" alt="Screenshot 2023-07-22 at 21 18 10" src="https://github.com/Padmalebaka/wordpress-aws/assets/164225494/00196574-fcfb-4605-8804-7de8226283af">


Now we have to create the second public subnet, to do that, click on "Create Subnet" again and follow the same steps we used previously to create our first public subnet, this time, give it the name "Public Subnet AZ2", select "us-east-1b" as the AZ and give the CIDR block the number "10.0.1.0/24", according to the diagram above.

Next, we will enable the "Auto assign IP settings". This means that every time you launch an EC2 instance in these public subnets, those EC2 instances will be assigned a public IPv4 address. To do that, select one of the public subnet, click "Actions" > "Edit subnet settings". Click on the checkbox that says "Enable auto-assign public IPv4 address".

![9](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/69062885-7927-466b-8779-de1087cb1a92)

Do the same thing to the other public subnet.

### Creating the Route Table

In the VPC service, select "Route tables" on the left side. You can see an already existing route table in the list, this route table is created by default when you create the VPC. Another thing you should know is that this one is called the "main" route table and it's private by default. We have to create a public route table. To do that, click on "Create route table", give it a name and choose our custom Dev VPC that we just created. On "Tags", give the "Key" a "Name" and for "Value", you can it the same name as the route table.

![10](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/45a03399-f478-4003-b9ea-2e025f9d7bd6)

We have to add a route to the public route table. On the "Routes" tab, click "Edit routes". Click "Add route" and in the "Destination", type "0.0.0.0/0". On "Target", select "Internet Gateway" and select your Internet Gateway. This will allow traffic to the public internet.

![11](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/a8e4d738-1260-4dd4-98f6-91b7eed98c47)

Now we associate the public subnets to the public route table that we created. On the "Subnet associations" tab, click "Edit subnets associations". Select the two public subnets that we created.

![12](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/9eb02a61-d9d7-4007-86ad-bd94edbd2c42)

Finally, we will create our last four private subnets, two for the application and two for the database. We just have to repeat the steps when we created the public subnets, but following the diagram above. The difference between a public and private subnet is that you attach a public route table to the public subnet. A route table is public when you add "0.0.0.0/0" to the destination to your Internet Gateway, allowing traffic to all the internet, without restrictions. The other four private subnets will be implicitly associated with the main route table, the one that is created when you create the VPC, but we will create a private route table for those private subnets in the next section.
