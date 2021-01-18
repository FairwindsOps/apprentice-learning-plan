# Part VIII: Google Cloud VPC

## Objectives 
This learning session builds on network concepts from [05-Cloud-Computing](05-Cloud-Computing.md) and uses them on Google Cloud instead of AWS. This is to primarily show the similarities and differences in VPC network structure between AWS and Google Cloud. 
By the end of this module, the SRE will:
1. Describe the following concepts:  
    - Scope of VPC in Google Cloud versus the scope of VPC in AWS
    - Subnets in GCP vs subnets in AWS
    - Regions/Zones in GCP vs AWS
    - GCP Firewall rules and targets versus AWS security groups
    - GCP Cloud NAT vs AWS NAT gateway
    - Google Cloud Load Balancers
2. Setup a VPC in GCP 

## Deliverables 
Complete the project described below. Answer the questions, then schedule a time to meet with your mentor and discuss the what you've built.  

### Exercise 

1. Log into GCP console
2. Create a VPC network
    1. Select Custom subnet creation mode.
        - Create 3 subnets in any region you'd like and with any names you'd like. Note that the subnets can be in any region and still talk to each other. 
        - For each subnet, specify an IP address range of /24 size within the blocks defined in [RFC1918](https://tools.ietf.org/html/rfc1918). Set "Private Google Access" to On for each. 
    2. Leave "Dynamic routing mode" to regional for this exercise -- this is only relevant when we link this VPC to an on-premises network or another VPC hosted elsewhere via a VPN. 
    3. Create a Cloud NAT. This is analagous to a NAT Gateway in AWS. 
        - Name the Cloud NAT whatever you'd like, and place it in one of the regions you put a subnet. 
        - For the "Cloud Router" setting, select "Create a Cloud Router", and name it whatever you'd like. 
        - Leave the NAT mapping settings at their defaults for this exercise. These define how the private IP addresses for VMs are allocated. 
3. Stand up a Compute Engine g1.small instance in one of the subnets you created. 
    1. On the "Create an instance" page, click the "1 vCPU" button to view the instance options and find g1.small. 
    2. Under "Boot disk", select the Ubuntu 18.04 LTS image and set the size to 15 GB. 
    3. Pick any availability zone you'd like, given that it's within a region where one of your subnets is located. 
    4. Open the "Management, security, disks, networking.." menu at the bottom of the page. 
    5. From this menu, select the "Networking" tab. Under "Network interfaces", select the name of your network and the appropriate subnetwork.  
    6. For External IP, select "None". 
    7. Click "Done" on the network menu, then click Create. 
    8. GCP will return you to the Compute Engine VM instances list. Note that the instance is up, but you cannot SSH into it, for the same reasons as AWS exercise -- it doesn't have a public IP address.  
    9. To resolve this, edit your instance to have an ephemeral external IP. 
    10. Create a firewall rule in your VPC that allows access to tcp port 22 from 0.0.0.0/0 on all instances in the network. 
    11. Go back over to the Compute Engine VM instances page and click the SSH button for your instance -- note how GCP allows you to automatically connect from the browser, once you have the appropriate network configuration. 
    12. Try connecting to the instance from your local terminal as well if you'd like.  
4. Install nginx and create a load balancer. 
    1. SSH onto the VM and install nginx. 
    2. On the GCP console, find Load Balancers. From the "Create a load balancer" page, choose TCP Load Balancing.
    3. Set the load balancer to be from the internet to your VMs, and as single region only.
    4. In "Backend configuration", select the region where your VM exists and select it by name as an existing instance. Create a health check on port 80.
    5. In "Frontend configuration", configure port 80 to be exposed.
5. Attempt to browse to your load balancer's IP address after it comes up.
    1. what IP address do you use?
    2. What might be the reason the connection times out?
6. Go and create another firewall rule that allows HTTP access on port 80, similar to how you had to allow SSH access. 
7. Successfully load your load balancer's test page. 

## Learning Resources 
- https://cloud.google.com/vpc/docs/vpc
- http://www.subnet-calculator.com/subnet.php?net_class=C