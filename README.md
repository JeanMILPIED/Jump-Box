# AWS TUTORIAL
# Jump-Box
## How to create a JumpBox architecture on AWS - Instructions

JUMPBOX (Public) => FINAL INSTANCE (Private) => NAT INSTANCE (PUBLIC) => Public Internet

![alt text](https://github.com/JeanMILPIED/Jump-Box/blob/master/JumpBox_img.png)

## create VPC and Subnets
1. create your VPC: mine is named Jean_VPC and is a 192.168.0.0/16
2. create an Internet Gateway assocated to the VPC named JEAN_IGW that is open to the internet (0.0.0.0/0)
3. associate the IGW and the VPC using public route table
4. create a Public Subnet: named JEAN_PUBLIC and is 192.168.1.0/24
5. create a public route from the public subnet to the gateway and the VPC
6. create a Private Subnet : named JM_PRIVATE and is 192.168.255.0/24 (it musn't be connected to the IGW via the routing table)
7. you need to create a new route table named "private route" where the private subnet is only connected to the VPC
8. create one private Security group and a Public Security group (connects to internet inbound and outbound)

## create instances in Subnets
9. go to EC2 to create the instances
10. Instance1: JB (for "JumpBox") - AmazonLinux2 machine free tier
- create instance / on VPC / On Public SUbnet / with associated Security Group
- Create an elastic IP for the JumpBox instance and associate it
- Associate the instance to the JM_Public Subnet (Services / VPC / Subnets / ...)

11. Instance2: FI (for "Final Instance") - AmazonLinux2 machine free tier
- create instance / on VPC / on Private Subnet
- Create a new security group with only SSH inbound from Jumpbox instance with its IP address

12. Instance3 NAT: create a security group "NAT_SG" for the NAT instance
- you can follow instructions on : https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html#NATSG

## preparing the way to connect to FI through JB
11. copy the keyfile of the FI machine ("JM_Keypair_2.pem") onto the JumpBox machine: 
- use the command line on windows 10
``scp -i "JM_Keypair.pem" "JM_Keypair_2.pem" ec2-user@52.70.164.164:/home/ec2-user/JM_Keypair_2.pem``

12. connect to the JB machine via ssh
- connect to the Jumpbox machine:
``ssh -i "JM_Keypair.pem" ec2-user@52.70.164.164``
- apply chmod on the key file "JM_Keypair_2.pem" pasted

## connection to the FI machine
20. connect to the FI machine from the Jumpbox machine:
``[ec2-user@ip-192-168-1-79 ~]$ ssh -i "JM_Keypair_2.pem" ec2-user@192.168.255.207``

## beware note: ensure the outbound connection of the private subnet goes through NAT
21. !! route of private subnet should have a route to the NAT instance open to all Internet 0.0.0.0/0

## it is then ready!
22. then you can ping and have fun from the Instance 2 "FI" through the Instance1 "Jump Box" via the Instance 3 "NAT"
