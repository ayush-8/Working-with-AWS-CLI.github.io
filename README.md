# Working-with-AWS-CLI.github.io
Presuming AWS-CLI is configured in PC.
## Creating a Key-pair
We might have a need to create a new key-pair for launching a new instance, which is quite easy in WebUI but it might seem to be difficult in CLI but it's not.
Just 1 command and we are done, Although we have to take care of a few things. 

`aws ec2 create-key-pair --key-name <name>`

![image](https://user-images.githubusercontent.com/55775311/95861695-716e6500-0d7f-11eb-81a2-c243a746ec97.png)

Here, keep in mind to replace <name> with your desired name in double quotes.
You will get an output like this.
 
![image](https://user-images.githubusercontent.com/55775311/95861778-8b0fac80-0d7f-11eb-8a5b-e851d023478e.png)

Keep a reference of this key material, as you will need this to access the Instance.

## Creating a Security-group
We will also need to define a new security group.

`aws ec2 create-ecurity-group --description " "   --group-name <name>`



Here also, keep in mind to replace <name> with your desired name in double quotes.
You will get an output like this.

![image](https://user-images.githubusercontent.com/55775311/95861815-98c53200-0d7f-11eb-93d8-e3f12cb69dd7.png)

Keep a reference of this security group id, as you will need this to launch or configure an Instance.
 
## Launching an instance
Now, Let's launch an instance using this above configuration.
But before that we'll have to look up for some information- the easiest way for which is WebUI.
The info required is:

1. AMI id
2. Instance type
3. Subnet Id - Obtained from next page
4. Security Group ID - Obtained
5. Key - Obtained

![image](https://user-images.githubusercontent.com/55775311/95861870-b09cb600-0d7f-11eb-9c85-530094f5f045.png)

![image](https://user-images.githubusercontent.com/55775311/95861903-bc887800-0d7f-11eb-8b73-2194c189b4e5.png)



Then,

`aws ec2 run-instances --image-id ami-0e306788ff2473ccb --instance-type t2.micro --key-name aws-cli --security-group-ids sg-0505a52e46f87f332 --count 1 --subnet-id subnet-a5dfa0e9`

![image](https://user-images.githubusercontent.com/55775311/95862041-ee99da00-0d7f-11eb-8e99-4d4fcd2b5b24.png)

Alert! your key name, security group ids will be different. Also, your subnet can be different depending on the region and subnet where you are launching your instance.

You'll see this when you see the running instances by executing this command:

`aws ec2 describe-instances`

![image](https://user-images.githubusercontent.com/55775311/95861966-d45ffc00-0d7f-11eb-9d51-36a0970656e5.png)
![image](https://user-images.githubusercontent.com/55775311/95861989-dd50cd80-0d7f-11eb-9d20-ba9608f121a8.png)



First, the status of the instance created is pending, but soon it changes to running.
We can see that this is the same instance as the key attached, security group and all the other parameters are same as above.



## Attaching an EBS volume of 4GB in the same.
### Creating an EBS volume of 4GB
We can create an Elastic Block Storage volume of any size in AWS 

`aws ec2 create-volume --size 4 --availability-zone ap-south-1b`

![image](https://user-images.githubusercontent.com/55775311/95862129-112bf300-0d80-11eb-9fe2-402a0ea004cc.png)

Take note of the volume id.
The only thing that is to be taken care of is that the availability zone should be SAME as that of the instance. Here, the size is defined in GiB.

### Attach
Now, We have to attach it to the instance.

Alert! The availability zone should be SAME.

`aws ec2 attach-volume --instance-id i-04f2279aa7d668424 --device "/dev/sdf"  --volume-id vol-03b69e73bcd8d2684`

![image](https://user-images.githubusercontent.com/55775311/95862158-1ab55b00-0d80-11eb-93cc-c3227dc482f0.png)
![image](https://user-images.githubusercontent.com/55775311/95862203-286ae080-0d80-11eb-81f3-f9d6053e2856.png)

NOTE- The device name /dev/xvda is for the primary volume but when attaching this secondary volume the device name is /dev/sdf in double quotes.

Hurray! We are Done. We can verify that instance is launched and the volume is attached from WebUI portal.

![image](https://user-images.githubusercontent.com/55775311/95864109-9a442980-0d82-11eb-88c6-2e1824597d45.png)
![image](https://user-images.githubusercontent.com/55775311/95864841-92d15000-0d83-11eb-86e0-5875e5e9b536.png)
Now, We can login in the instance using the key and create partition in this attached volume and then mount it and use it.



