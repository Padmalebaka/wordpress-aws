## Creating the Elastic File System (EFS)

![1](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/ceccb848-c21e-4eba-aef2-d567d984b8f1)

*Amazon Elastic File System (Amazon EFS) is a fully managed, scalable, and highly available file storage service provided by Amazon Web Services (AWS). It is designed to provide shared file storage for EC2 instances and other AWS services.*

In this section, we will create an Elastic File System (EFS) with mount targets in the private data subnets in each AZ. EFS will allow the webserver and the private app subnets to pull the application code and configuration files from the same location. With Amazon EFS, multiple Amazon EC2 instances running WordPress can share the same file system. This allows you to scale your WordPress deployment horizontally by adding more EC2 instances without worrying about file synchronization or data consistency issues. All instances can access and update the same files, ensuring that changes are immediately visible across the entire deployment.

### Creating EFS

Search for "EFS" in the AWS console, then "Create file system". A window will be shown in your browser, select "Customize". Give it a name for your file system, on the "Encryption", check the "Enable encryption of data at rest" unchecked so we don't get surprised by any bills. On the "Tags" field, fill "Name" in the "Tag key" and "Dev-EFS" in the "Tag value". You can click "Next".

![2](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/45b10f3e-0fac-421d-b581-d699db81fd39)
![3](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/9467767f-c164-4e8b-acb5-047502ba8a37)

Select your Dev VPC on the network. On "Mount targets", we will select the subnets in each AZ that we want to put our mount target in, since we are using two AZs (us-east-1a and 1b), we will use the private data subnet AZ1 and AZ2, on "Security groups", remove the default SG and select the EFS SG for both AZs.

![4](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/f76be2e2-ddaf-4ed6-9dbe-90d36d690827)

No need to make changes to the "File system policy", click "Next". On the "Review and create" you can click "Create".