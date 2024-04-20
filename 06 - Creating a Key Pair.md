## Creating a Key Pair

In this section, we will create a key pair that we will use to SSH into our EC2 instance. In the AWS console, search for "EC2". On the EC2 dashboard, select "Key Pairs" then "Create key pair". Give it a name and select ".pem" for Mac/Linux users and ".ppk" for Windows users. 

![1](https://github.com/Padmalebaka/wordpress-aws/assets/164225494/0e36b1b1-563b-4630-8886-bcb232d007bb)

When you create a key pair, two keys will be generated for you. The first key is called a public and the second is called a private key. The key that you see in the AWS management console is the public key, this is the key that you will put on your EC2 instance when you launch it. The key that it's been downloaded to your computer is the private key. So anytime you need to SSH into your EC2 instance, you will put the public key on that EC2 instance, then you will use the private key that was downloaded into your computer to SSH into that EC2 instance.

### For Mac/Linux Users

Select the ".pem" format for the key when creating it. Make sure to move the key that was downloaded to your home folder. Open up your terminal and type "chmod 400 myec2key.pem" to enable the permissions for the key.

### For Windows Users

Select the ".ppk" format for the key when creating it. We will also have to download [PuTTY](https://www.putty.org/) to make the connection with the EC2 instance.