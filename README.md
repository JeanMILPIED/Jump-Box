# Jump-Box
Jump Box  notes from AWS class

** First
1. create your VPC: mine is named Jean_VPC and is a 192.168.0.0/16
2. create an Internet Gateway assocated to the VPC named JEAN_IGW that is open to the internet
3. associate the IGW and the VPC using public route table
4. create a Public Subnet: named JEAN_PUBLIC and is 192.168.1.0/24
5. create a public route from the public subnet to the gateway and the VPC
6. create a Private Subnet : named JM_PRIVATE and is 192.168.255.0/24 (it musn't be connected to the IGW via the routing table)
7. you need to create a new route table named "private route" where the private subnet is only connected to the VPC
8. create one private Security group and a Public Security group (connects to internet inbound and outbound)
9. then go to EC2 to create the instances
10. Instance1: JB (for "JumpBox") - AmazonLinux2 machine free tier
11. create instance / on VPC / On Public SUbnet / with associated Security Group
12. Create an elastic IP for the JumpBox instance and associate it
13. Associate the instance to the JM_Public Subnet (Services / VPC / Subnets / ...)
14. Instance2: create instance 2: FI (for "Final Instance") - AmazonLinux2 machine free tier
15. Create a new security group with only SSH inbound from Jumpbox instance with its IP address
16. Instance3 NAT: create a security group "NAT_SG" for the NAT instance (see: https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html#NATSG)
17. copy the keyfile of the FI machine onto the JumpBox machine: scp -i "JM_Keypair.pem" "JM_Keypair_2.pem" ec2-user@52.70.164.164:/home/ec2-user/JM_Keypair_2.pem
18. apply chmod on the key file pasted
19. connect to the Jumpbox machine: ssh -i "JM_Keypair.pem" ec2-user@52.70.164.164
20. connect to the FI machine from the Jumpbox machine: [ec2-user@ip-192-168-1-79 ~]$ ssh -i "JM_Keypair_2.pem" ec2-user@192.168.255.207
21. !! route of private subnet should have a route to the NAT instance open to all Internet 0.0.0.0/0
22. then you can ping and have fun from the Instance 2 "FI" through the Instance1 "Jump Box" via the Instance 3 "NAT"
