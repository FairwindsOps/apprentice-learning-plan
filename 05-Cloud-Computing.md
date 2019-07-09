# Part V: Cloud Computing

## Objectives 
By the end of this module, the SRE will:
1. Describe the following cloud computing concepts: 
    - VMs
    - VPC/Subnets
    - Regions/Zones
    - Security Groups
    - LoadBalncing
    - Gateways
    - Routes

2. Setup a VPC in AWS 

## Deliverables 
Complete the project described below. Answer the questions, then schedule a time to meet with your mentor and discuss the what you've built.  

### Exercise 
- https://www.youtube.com/watch?v=aMSWlV4whSg
1. Log into the Fairwinds AWS cloud console
2. Note region
3. Create VPC with /16 CIDR (https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Scenarios.html)
    - Note why you chose the VPC CIDR 
    - Create a NAT gateway within your VPC for private traffic egress
    - Create a private and a public subnet within the VPC you created
      1. note the size you’ve chosen for your subnets and why
      2. note the availability zones you’ve chosen
      3. explore the route tables automatically created for each subnet
      4. ensure that the private subnet is using the NAT gateway as its default route
      5. ensure that the public subnet is using the IGW (internet gateway) as its default route
      6. note that the default ACLs created are permissive
4. Stand up a t2.micro instance in the private subnet you created
    - choose a basic Linux AMI (ubuntu or other)
    - ensure you chose the right availability zone and subnet as you create the instance
    - create a new security group and give it an identifiable name
      1. add an ingress rule to allow ssh traffic on port 22 (tcp) from your home IP address
    - when it asks you to generate a new SSH key, ensure you save/download the private key to access the instance later
5. Attempt to SSH into your instance using `ssh -i <private key> <default_user>@<ip address>`
    - What IP address do you use?
    - What might be the reason it can’t connect?
    - Whatever Linux AMI you chose has a default user (ubuntu's is `ubuntu`) 
6. Create an public/external classic ELB in the public subnet you created in your VPC
    - create a listener that exposes both port 22 tcp on the outside (load balancer) and 22 tcp on the inside (instance)
    - create a security group and give it an identifiable name
      1. add an rule to allows ssh traffic on port 22 (tcp) from your home IP address source
    - configure a healthcheck that will test that SSH is running on your instance
    - add your instance to the load balancer
7. Attempt to connect to your ELB via SSH
    - what IP address do you use?
    - What might be the reason the connection times out?
8. Adjust your ELB security group and instance security group to allow communication between the two on SSH port 22 (tcp)
9. SSH to your instance via the ELB

## Learning Resources 

- http://www.davidc.net/sites/default/subnets/subnets.html (Subnet calculator)
- https://tools.ietf.org/html/rfc1918 (Section 3, Private Address Space)
- https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenario2.html (First part of Overview and first diagram)
- Terraform info https://www.youtube.com/watch?v=h970ZBgKINg