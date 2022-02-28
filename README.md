The files in this repository were used to configure the network depicted below.

Images/ElkStackDiagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the below files may be used to install only certain pieces of it, such as Filebeat.

-	-

This document contains the following details:

-	Description of the Topology
-	Access Policies
-	ELK Configuration
-	Beats in Use
-	Machines Being Monitored
-	How to Use the Ansible Build


Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application remains highly available to users, in addition to restricting unwanted traffic to the network. They mitigate attacks in the event that a server goes down. Advantages of using a jump box is that it only communicates with authorized IP addresses. It is the starting point for implementing administrative tasks by requiring all administrators to connect to the jump box before they can perform tasks. 


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.

-	Filebeat collects logs events and watches for files and locations. 
-	Metricbeat records metrics and static data from running services and the operating system. It is a great tool for monitoring your system. 

The configuration details of each machine may be found below.

| Name       | Function | Public IP     | Private IP | OS    |
|------------|----------|---------------|------------|-------|
| Jumpbox    | Gateway  | 20.124.24.142 | 10.0.0.4   | Linux |
| Elk-Server | Server   | 52.159.86.187 | 10.1.0.4   | Linux |
| Web1       | Server   | N/A           | 10.0.0.7   | Linux |
| Web2       | Server   | N/A           | 10.0.0.6   | Linux |


Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

-	User’sworkstation Public IP Address

Machines within the network can only be accessed via the Jumpbox and user workstation.

-	Machine permitted to access ELK VM: Jumpbox Provisioner – 20.124.24.142 via SSH(22). 

A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible | Whitelisted IP's                |
|------------|---------------------|---------------------------------|
| Jumpbox    | Yes                 | user workstation                |
| Elk-Server | No                  | Jumpbox: 20.124.24.142/10.0.0.4 |
| Web1       | No                  | Jumpbox: 20.124.24.142/10.0.0.4 |
| Web2       | No                  | Jumpbox: 20.124.24.142/10.0.0.4 |

Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because: 

-	It allows you to automate the creation of multiple servers using a single playbook. This saves time and eliminates repeated syntax errors in the deployment process.

The playbook implements the following tasks:

-	Defines the hosts
-	Installs docker.io and python3-pip
-	Installs docker via pip
-	Increases memory of the container(sysctl -w vm.max_map_count=262144)
-	Downloads and launches the Elk Docker container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

 

 Images/ runningdockerps.png

### Target Machines & Beats

This ELK server is configured to monitor the following machines:

| Machine | IP       |
|---------|----------|
| Web1    | 10.0.0.7 |
| Web2    | 10.0.0.6 |

We have installed the following Beats on these machines
-	Filebeat
-	Metricbeat

These Beats allow us to collect the following information from each machine:

-	Filebeat collects log files from specified files, tools and webservers, and MySQL databases.
-	Metricbeat collects metrics and statistics such as system, memory, and network and places them in specified outputs.

Using the Playbook
To use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

-	Copy the yml file to the /etc/ansible directory
-	Update the config file to include the new Elk-Server IP
-	Run the playbook, and navigate to Kibana using the Elk-Server IP (http://52.159.86.187:5601) to check that the installation worked as expected.

-	Playbook files: 
o	Ansible: 
o	Filebeat: 
o	Metricbeat:
Copy files to /etc/ansible directory

-	The /etc/ansible/hosts file must be updated to include the private IP Addresses of Web1, Web2, and Elk-Server.
-	 Specify Web1 and Web2 as [webservers], and Elk-Server as [elk-server] in the etc/ansible/hosts file.
-	Navigate to http://52.159.86.187:5601/app/kibana in order to check that the ELK server is running.
