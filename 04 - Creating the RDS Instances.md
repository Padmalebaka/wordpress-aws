## Creating the RDS Instances

![1](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/38522db4-3fff-418f-8322-afe1928a2902)

*Amazon RDS (Relational Database Service) is a managed database service provided by AWS. It offers a scalable and cost-effective solution for deploying and managing relational databases in the cloud. By implementing Amazon RDS in a WordPress deployment on AWS, you can leverage a scalable, managed, and highly available database service. It simplifies database administration, improves performance and scalability, enhances security, and integrates seamlessly with other AWS services, ultimately providing a reliable and efficient backend for your WordPress site.*

In this section, we will create the RDS database in the private data subnets. The main database will be in AZ2 and the standby replica will be in AZ1, making it a Multi-AZ instance.

### Subnet Groups

Search for RDS in the AWS console. Before we create the RDS instance in the private data subnets, the first thing we need to create is subnet groups, this allows us to specify in which subnets we want to create our RDS database. To do that, on the RDS dashboard, select "Subnet groups", then "Create DB subnet group", give it a name and this same name as its description, select your custom Dev VPC. On "Add subnets", in the "Availability Zones", select the AZs that we want to use for the subnet groups (us-east-1a and 1b), on "Subnets", we will select the subnets according to our architecture diagram above (10.0.4.0/24 and 10.0.5.0/24).

![2](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/9e9c4eb6-7fa2-4c8b-9ffb-eee01b1c8b2c)
![3](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/7b6da523-e723-4ec1-9321-124fa970edfa)

### Creating the Database

Now we can create the database, in the RDS dashboard, select "Databases", then "Create database". On "Choose a database creation method", choose "Standard create" so we can insert detailed information for its creation, and of course, learn more from it. On "Engine options", select "MySQL" and for the "Version", we have to select a compatible version for WordPress, select the latest 5.7.xx version of MySQL. For "Templates", select "Dev/Test" and in "Availability and durability", select "Multi-AZ DB instance", this is important because this option allows us to create a standby replica, the free tier can't do that. Of course, you can use the free tier if you want but be aware that the standby replica will not be available for this option. The presence of a standby replica is not paramount to the execution of this project but it's nice to implement it for a HA environment.

![4](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/8750b248-fd5f-4165-968f-7d59279eab5c)
![5](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/68325a9a-db8e-4df6-8445-7e3c8981158d)
![6](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/540f89be-4e59-40c2-8b02-45c5d0c2a817)
![7](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/b6f99901-4b94-4b13-a62d-7c0c4f80d2e1)

Proceeding with the creation, on "Settings", give your DB a name, a master username and a password. On "DB instance class", select the burstable classes and toogle "Include previous generation classes", AWS will give you the free instance. On "Connectivity", select your custom Dev VPC, make sure that you have your subnet groups selected for the "Subnet group" field, for the "VPC security group", make sure that you "Choose existing" SG, the existing one that we will use is the Database SG, delete the "default" option, on "Availability Zone", select "us-east-1b" according to the diagram above.

![8](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/8dbcfc10-e58f-48fd-9f94-de001018cc06)
![9](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/bc13903a-1afe-4a72-bd91-b79996d16b66)
![10](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/2aeba312-1bbc-441d-b08c-3106abbcd916)
![11](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/db3752b9-0d11-4855-a152-6efc69937e40)
![12](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/3d6ccc44-9110-4236-8b09-139e09def699)

Finally, on "Additional configuration", enter your database name on "Initial database name".

![13](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/d0c53e83-a20f-4e0d-a4c7-83e59cdd775f)

You can now create your database, wait a few minutes for it to be complete. Make sure you save your credentials by clicking on "View credential details".