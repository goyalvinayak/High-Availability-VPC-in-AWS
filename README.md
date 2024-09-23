# Setting Up a VPC(Virtual Private Cloud) with high availability in AWS
## Architecture
![Screenshot (26)](https://github.com/user-attachments/assets/4e81e33a-048d-4aca-920e-5a3136de3b7c)
### Details
* Region: us-wes-1
* VPC Range: 172.20.0.0/16
* Availability Zones: 2 (us-west-1a, us-west-1b)
* 4 Subnets: 2 public and 2 private
   * 172.20.1.0/24 => pub-sub us-west-1a
   * 172.20.2.0/24 => pub-sub us-west-1c
   * 172.20.3.0/24 => priv-sub us-west-1a
   * 172.20.4.0/24 => priv-sub us-west-1c
* Internet Gateway: 1
* NAT Gateway: 2
* Elastic IP: 1 for NAT Gateway
* Route Tables: 2 (1 for pub-subnet and 1 for priv-subnet)
* Bastion Host: 1 in Public Subnet

## Creating VPC and other components
### VPC
![image](https://github.com/user-attachments/assets/0e147cd4-b030-4894-b320-7f0319ae5977)

### Subnets
![image](https://github.com/user-attachments/assets/9c9b61e4-cab5-467e-a9e7-a6f1a5fbd868)
![image](https://github.com/user-attachments/assets/0b3bfa42-4e57-488b-a94e-339dde653a31)
![image](https://github.com/user-attachments/assets/3dab3c92-a3ab-49b6-bfb7-6b51e135f71d)
![image](https://github.com/user-attachments/assets/7fadbd01-93f6-4470-b5e0-0f6cee8915b3)
![image](https://github.com/user-attachments/assets/b7165f77-f34f-47fe-be97-ab25d124b3da)

### Internet Gateway 
Create an Internet Gateway and then go to Actions ,and attach it to our project VPC.
![image](https://github.com/user-attachments/assets/7afa96d4-8599-4f15-947c-b28f2524c531)

### Route Table for Public Subnets
Connecting Internet Gateway to public subnets through Route Tables. One route table will already be present because it automatically got created when we created VPC. We will make a route table for public subnet-
![image](https://github.com/user-attachments/assets/c5eb68dd-6c90-46a6-b087-667fa77f2a14)

Then we will associate our public subnets to this routing table in Subnet Association in Property Tab-
![image](https://github.com/user-attachments/assets/60670320-c863-4085-a4c1-ce20a4a3590d)
To make it actually public, we will add route in Edit Routes option-
![image](https://github.com/user-attachments/assets/5039a575-2f0c-4922-8348-0ea5e5c030a9)

#### Enabling Subnet to automatically allocate public IP
Select the public subnet ,go to Edit Subnet Settings and enble the option-
![image](https://github.com/user-attachments/assets/a5845b17-813f-4ad6-a440-4a9a56afd3ff)

### NAT Gateway for Private Subnets
#### Creating an Elastic IP for NAT Gateway
![image](https://github.com/user-attachments/assets/a2ebd3cc-4fe2-4a1f-b755-dd7a479f6fd6)

#### Creating NAT Gateway
![image](https://github.com/user-attachments/assets/d5c68bad-cf04-4216-bfd5-a724748ff294)
![image](https://github.com/user-attachments/assets/16d9c6a9-29e0-44de-abfe-f4b8158051d7)

#### Route Table for Private Subnets
![image](https://github.com/user-attachments/assets/add33274-711c-44e2-a692-235ca88add07)
![image](https://github.com/user-attachments/assets/cb24dd48-e8b4-4944-9463-0891f128294c)

### Enabling DNS Hostnames
By enabling this, our Instances will get DNS hostnames also.
![image](https://github.com/user-attachments/assets/2416be3c-e68d-4235-bab2-8ab0c5172ab2)

### Bastion Host
A Bastion host is a special-purpose server or an instance that is used to configure to work against the attacks or threats. It is also known as the ‘jump box’ that acts like a proxy server and allows the client machines to connect to the remote server. It is basically a gateway between the private subnet and the internet. It allows the user to connect private network from an external network and act as  proxy to other instances. 

Using this, we can access any resource in a private subnet. Why using Bastion Host- because we cannot access resources from Internet because they are in private subnet. So the bation host become a single entry point to access resources in private subnet.  

#### Making SG for our Bastion Host
















