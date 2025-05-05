# AWS VPC Peering and Testing Connectivity

# Overview
In modern cloud architectures, it's common for organizations to separate workloads across multiple Virtual Private Clouds (VPCs) for reasons like <!--more-->security, billing, or organizational structure. To allow communication between these VPCs, VPC Peering can be used.

In this project, I demonstrate how to create two VPCs in AWS, set up a VPC Peering Connection, and test private IP communication between EC2 instances in each VPC

# Objective
To create two isolated VPCs in AWS.

To peer the VPCs using AWS VPC Peering.

To deploy EC2 instances in each VPC and test connectivity using private IP addresses.

To showcase practical networking skills in AWS that employers value.

<img class="alignnone size-full wp-image-104" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/aws_vpc_peering.png" alt="" width="751" height="571" />

# Components:
<ul>
 	<li>VPC A: 10.0.0.0/16 with Subnet 10.0.1.0/24</li>
 	<li>VPC B: 192.168.0.0/16 with Subnet 192.168.1.0/24</li>
 	<li>EC2 instance in each subnet (Amazon Linux 2, t2.micro)</li>
 	<li>VPC Peering connection between VPC A and VPC B</li>
</ul>

# Tools and Services Used
<ul>
 	<li>AWS Management Console</li>
 	<li>VPC</li>
 	<li>EC2</li>
 	<li>Security Groups</li>
 	<li>Route Table</li>
 	<li>Internet Gateway</li>
 	<li>VPC Peering</li>
</ul>

# Step-by-Step Implementation

## Step 1: Create Two VPCs
I configured the first VPC:
<ul>
 	<li>Name: VPC-A</li>
 	<li>IPv4 CIDR Block: 10.0.0.0/16</li>
</ul>
<img class="alignnone size-full wp-image-105" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-277.png" alt="" width="1177" height="296" />


I repeat for the second VPC:
<ul>
 	<li>Name: VPC-B</li>
 	<li>IPv4 CIDR Block: 192.168.0.0/16</li>
</ul>
<img class="alignnone size-full wp-image-106" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-278.png" alt="" width="1143" height="304" />


## Step 2: Create Subnets in Each VPC
Navigate to Subnets.
<ul>
 	<li>I created a subnet for VPC A:</li>
 	<li>Name: Public Subnet A</li>
</ul>
<img class="alignnone size-full wp-image-107" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-279.png" alt="" width="1336" height="250" />

I create another subnet for VPC B in a different Availability zone:
<ul>
 	<li>Name: Public Subnet B</li>
</ul>
<img class="alignnone size-full wp-image-108" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-280.png" alt="" width="1354" height="278" />

## Step 3: Launch EC2 Instances in Each Subnet
<ul>
 	<li>Go to EC2 Dashboard → Launch Instance.</li>
 	<li>Select Amazon Linux 2 AMI, instance type t2.micro.</li>
</ul>
Configure:
<ul>
 	<li>Network: Instance VPC A</li>
 	<li>Subnet: Public Subnet A</li>
 	<li>Auto-assign Public IP: Enable</li>
</ul>
<img class="alignnone size-full wp-image-110" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-281-1.png" alt="" width="1148" height="240" />

I repeated the same steps for <strong>VPC B</strong>, placing the <strong>EC2 in Public Subnet B</strong>, I named it <strong>Instance VPC B</strong>, and I enabled <strong>Auto-assign Public IP</strong>

<img class="alignnone size-full wp-image-111" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-282.png" alt="" width="1148" height="213" />

## Step 4: Modify Security Groups for Private Communication
Navigate to Security Groups.

For Instance VPC A:

Add an inbound rule:
<ul>
 	<li>Type: ICMP - Echo Request</li>
 	<li>Source: 192.168.0.0/16 (CIDR of VPC-B)</li>
</ul>
<img class="alignnone size-full wp-image-112" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-283.png" alt="" width="1307" height="179" />



For Instance VPC B:

Add an inbound rule:
<ul>
 	<li>Type: ICMP - Echo Request</li>
 	<li>Source: 10.0.0.0/16 (CIDR of VPC-A)</li>
</ul>
<img class="alignnone size-full wp-image-113" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-284.png" alt="" width="1204" height="101" />

## Step 5: Create VPC Peering Connection
Set:
<ul>
 	<li>Name: Peer-A-B</li>
 	<li>Requester: VPC A</li>
 	<li>Accepter: VPC B</li>
</ul>
<img class="alignnone size-full wp-image-114" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-285.png" alt="" width="1235" height="433" />

After creation, I accepted the request <strong data-start="3623" data-end="3653">“Actions &gt; Accept Request”</strong>

<img class="alignnone size-full wp-image-115" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-286.png" alt="" width="831" height="337" />

## Step 6:  Route Tables to Enable Peering
Go to Route Tables.

Identify the route table associated with Subnet-A:
<ul>
 	<li>Click Edit Routes - Add Route</li>
 	<li>Destination: 192.168.0.0/16</li>
</ul>
<img class="alignnone size-full wp-image-116" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-287.png" alt="" width="1304" height="546" />

I repeated the same thing for the route table of Subnet-B:
<ul>
 	<li>Destination: 10.0.0.0/16</li>
</ul>
<img class="alignnone size-full wp-image-117" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-288.png" alt="" width="1316" height="555" />

## Step 7: Create an Internet Gateway
I created two internet gateways for VPC A and VPC B respectively

<img class="alignnone size-full wp-image-118" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-293.png" alt="" width="1270" height="364" />

## Step 8: Modify each Route Table for each VPCs and include the Internet Gateway to each VPC
I clicked <strong>Add Route,</strong> and added as <strong>0.0.0.0/0</strong> for <strong>Destination </strong>and the <strong>Target</strong> as the new Internet Gateway I just created for that particular VPC

<img class="alignnone size-full wp-image-119" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-290.png" alt="" width="1289" height="234" />

## Step 9: Associate each Route Table to each Subnet
<img class="alignnone size-full wp-image-120" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-294.png" alt="" width="1068" height="214" />

## Step 10: Modify the Security Group Inbound Rule attached to Instance
To be able to test connectivity I ensured the <strong>Security Group Inbound Rule</strong> attached to my Instances could allow ICMP from anywhere. (PS. This is not a good practice but for the sake of this project, I did just to test connectivity)

<img class="alignnone size-full wp-image-121" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-296.png" alt="" width="1536" height="149" />

## Step 11: Test Connectivity
To Test connectivity, I copied the <strong>public IP address</strong> of <strong>Instace VPC B</strong>



<img class="alignnone size-full wp-image-122" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-297.png" alt="" width="1133" height="279" />

Checked the box of <strong>Instance VPC A</strong> and clicked on <strong>Connect</strong>

<img class="alignnone size-full wp-image-123" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-298.png" alt="" width="1158" height="228" />

In the terminal I typed <strong>ping </strong>and entered the <strong>Instnace</strong> <strong>VPC B </strong>public IP address I copied

<img class="alignnone size-full wp-image-124" src="https://eyemekacyberportfolio.name.ng/wp-content/uploads/2025/04/Screenshot-292.png" alt="" width="1308" height="345" />

The pings were successfully sent, no request time out.

This proves that I have successfully connected both VPCs with VPC Peering and both Instances in each VPC can communicate to each other.

# Conclusion
This project taught me how to set up secure communication between two AWS VPCs using VPC Peering. I gained hands-on experience with configuring subnets, route tables, security groups, and troubleshooting real networking issues.

This project boosted my confidence in cloud networking and helped me understand how different AWS components work together
