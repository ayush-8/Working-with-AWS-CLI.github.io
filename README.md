# Working-with-AWS-CLI.github.io
Presuming AWS-CLI is configured in PC.
## Creating a Key-pair
We might have a need to create a new key-pair for launching a new instance, which is quite easy in WebUI but it might seem to be difficult in CLI but it's not.
Just 1 command and we are done, Although we have to take care of a few things. 

`aws ec2 create-key-pair --key-name <name>`

Here, keep in mind to replace <name> with your desired name in double quotes.
You will get an output like this.

Keep a reference of this key material, as you will need this to access the Instance.
 

## Creating a Security-group
We will also need to define a new security group.

`aws ec2 create-ecurity-group --description " "   --group-name <name>`

Here also, keep in mind to replace <name> with your desired name in double quotes.
You will get an output like this.



Keep a reference of this security group id, as you will need this to launch or configure an Instance.
 



## Launching an instance
Now, Let's launch an instance using this above configuration.
But before that we'll have to look up for some information- the easiest way for which is WebUI.
The info required is:

1. AMI id
2. Instance type
3. Subnet Id
4. Security Group ID - Obtained
5. Key - Obtained

Then,

`aws ec2 run-instances --image-id ami-0e306788ff2473ccb --instance-type t2.micro --key-name aws-cli --security-group-ids sg-0505a52e46f87f332 --count 1 --subnet-id subnet-a5dfa0e9`

Alert! your key name, security group ids will be different. Also, your subnet can be different depending on the region and subnet where you are launching your instance.

You'll see this when you see the running instances by executing this command:

`aws ec2 describe-instances`

First, the status of the instance created is pending, but soon it changes to running.
We can see that this is the same instance as the key attached, security group and all the other parameters are same as above.

## Attaching an EBS volume of 4GB in the same.
### Creating an EBS volume of 4GB
We can create an Elastic Block Storage volume of any size in AWS 

`ws ec2 create-volume --size 4 --availability-zone ap-south-1b`

Take note of the volume id.
The only thing that is to be taken care of is that the availability zone should be SAME as that of the instance. Here, the size is defined in GiB.

### Attach
Now, We have to attach it to the instance.

Alert! The availability zone should be SAME.

`aws ec2 attach-volume --instance-id i-04f2279aa7d668424 --device "/dev/sdf"  --volume-id vol-03b69e73bcd8d2684`

NOTE- The device name /dev/xvda is for the primary volume but when attaching this secondary volume the device name is /dev/sdf in double quotes.

Hurray! We are Done.
Now, We can login in the instance using the key and create partition in this attached volume and then mount it and use it.



