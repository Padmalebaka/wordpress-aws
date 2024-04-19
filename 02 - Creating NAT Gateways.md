## Creating NAT Gateways

![1](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/7597ad6f-3a61-40b2-bd33-76c36a7e117d)

*A NAT Gateway in AWS (Network Address Translation Gateway) is a managed service that allows resources within a private subnet in a Virtual Private Cloud (VPC) to access the internet or other AWS services, while keeping them hidden behind a single public IP address. The main use of a NAT Gateway is to provide internet connectivity to resources in private subnets that do not have a direct internet connection. It acts as a gateway and performs network address translation, allowing private subnet resources to communicate with the internet or other AWS services using the NAT Gateway's public IP address. This can be useful for Resources in private subnets, such as EC2 instances, can send requests to the internet (e.g., downloading updates or accessing external APIs) through the NAT Gateway.* 

In this section, we will create a NAT gateway and a private route table for each AZ so the private subnets can make their connections to the public internet.

### Creating the NAT Gateways

To create the NAT gateways, first make sure you are in the same region that you created the VPC, in this case it's the N. Virginia, then search for the VPC service. In the VPC dashboard, on the left side, select "NAT Gateways", then click "Create NAT gateway" We will create the first NAT gateway in the public subnet AZ1, give it a name. Select a subnet, in this case it will the the first one, the "Public Subnet AZ1", then click on "Allocate Elastic IP"

<img width="814" alt="2" src="https://github.com/Padmalebaka/wordpress-aws/assets/164225494/c26cec33-3fcb-43d1-a88f-13af68bd9142">
<img width="814" alt="3" src="https://github.com/Padmalebaka/wordpress-aws/assets/164225494/a07e6f55-4137-4640-962b-1109d109491e">

### Creating the Route Tables

The next thing we will do is create the "Private Route Table AZ1". On the VCP dashboard, on the left side of the screen, select "Route Tables", then click "Create route table". Give it a name and select your custom VPC.

<img width="811" alt="4" src="https://github.com/Padmalebaka/wordpress-aws/assets/164225494/4d645b2c-0a88-4fab-b78b-9c0aa4c2f5e2">

Next, we are going to add a route to the Private Route Table AZ1. On the "Routes" tab, select "Edit routes". Click "Add route", for the "Destination", type the generic internet traffic again "0.0.0.0/0", the "Target" is going to be our NAT Gateway.

<img width="1736" alt="5" src="https://github.com/Padmalebaka/wordpress-aws/assets/164225494/2fcb329e-ab2b-4cd9-84d6-95ecdf4186ca">

Next, we will associate this Route Table with Private Subnet AZ1 and AZ2, the app and data subnets. In the "Subnet associations" tab, click on "Edit subnet associations". On the "Available subnets", we will select the two subnets that we need.

<img width="1737" alt="6" src="https://github.com/Padmalebaka/wordpress-aws/assets/164225494/8d3ed6f0-f98a-4038-8c75-d81280a5ad41">

Do the same thing for the second AZ, follow the same steps above and obey the diagram.