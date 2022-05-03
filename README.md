# Automated-Elk-Stack-Deployment-Project-1
Marcus Cooper Cyber Bootcamp Project 05/03/2022

The files in this repository were used to configure the network depicted below.

![Elk Stack Network Diagram](https://github.com/CyberMarcus0/Automated-Elk-Stack-Deployment-Project-1/blob/main/Diagrams/ELK%20Stack%20Network%20Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YML files may be used to install only certain pieces of it, such as Filebeat.

  - [Install Elk](https://github.com/CyberMarcus0/Automated-Elk-Stack-Deployment-Project-1/blob/main/Ansible/install-elk.yml.txt)
  - [Filebeat Playbook](https://github.com/CyberMarcus0/Automated-Elk-Stack-Deployment-Project-1/blob/main/Ansible/filebeat-playbook.yml.txt)
  - [Metricbeat Playbook](https://github.com/CyberMarcus0/Automated-Elk-Stack-Deployment-Project-1/blob/main/Ansible/metricbeat_playbook.yml.txt)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant, in addition to restricting access to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the services and system logs.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  |(Private)10.0.0.4(Public) 20.216.43.66  | Linux Ubuntu 18.04           |
| Elk-Stack     |     Elk Server     |(Private)10.1.0.4(Public)20.234.102.40    |  Linux Ubuntu 18.04   |
| Web-1     |    DVWA Server      | (Private) 10.0.0.5       |       Linux Ubuntu 18.04           |
| Web-2     |    DVWA Server      | (Private)10.0.0.10      |       Linux Ubuntu 18.04           |
| Web-3     | DVWA Server| (Private) 10.0.0.6| Linux Ubuntu 18.04 |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 73.172.189.229

Machines within the network can only be accessed by the Jumpbox VM.
- Jumpbox (Private)10.0.0.4 (Public)20.216.43.66

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box |   Yes               |   73.172.189.229     |
|Elk-Stack |   No                |   10.0.0.4           |
| Web-1    |   No                |   10.0.0.4           |
| Web-2    |   No                |   10.0.0.4           |
| Web-3    |   No                |   10.0.0.4           |


### Elk Configuration

Ansible was used to automate the configuration of the ELK machine. No configuration was performed manually,  which is an advantage because it uses SSH to communicate with the remote host and run all commands (task). Using ansible, you won’t need to write custom code, you list the tasks required to be done by writing a playbook, and Ansible will get your system to the state desired. 

The playbook implements the following tasks:
- Install docker.io
- Install python3-pip
- Install Docker module
- Increase memory " sysctl vm.max_map_count 262144"
- Download and launch a docker elk container
- Enable service on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

!['docker ps' Command Output](https://github.com/CyberMarcus0/Automated-Elk-Stack-Deployment-Project-1/blob/main/Images/Docker%20ps.PNG?raw=true)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 (10.0.0.5)
- Web-2 (10.0.0.10)
- Web-3 (10.0.0.6)

We have installed the following Beats on these machines:

-[Filebeat](https://github.com/CyberMarcus0/Automated-Elk-Stack-Deployment-Project-1/blob/main/Ansible/filebeat-playbook.yml.txt)

-[Metricbeat](https://github.com/CyberMarcus0/Automated-Elk-Stack-Deployment-Project-1/blob/main/Ansible/metricbeat_playbook.yml.txt)

These Beats allow us to collect the following information from each machine:
- You can use Filebeat to monitor the Elasticsearch log files, collect log events, and ship them to the monitoring cluster. Your recent logs are visible on the Monitoring page in Kibana.

![Kibana Monitoring Page](https://github.com/CyberMarcus0/Automated-Elk-Stack-Deployment-Project-1/blob/main/Images/Filebeat%20Syslog%20dashboard.PNG)


-Metricbeat helps you monitor your servers by collecting metrics from the system and services running on the server. The benefit of using Metricbeat instead of internal collection is that the monitoring agent remains active even if the Metricbeat instance dies.

![Metricbeat Example](https://github.com/CyberMarcus0/Automated-Elk-Stack-Deployment-Project-1/blob/main/Images/Metricbeat%20Docker%20Overview.PNG)


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- Copy the configuration.yml and playbook.yml files to your /etc/ansible directory.
- Update the configuration files to include the ELK private IP Address.
- Run the playbook, and navigate to http://20.234.102.40:5601/ to check that the installation worked as expected.

# Commands the user will need to run to download the playbook, update the files, etc._ 



			$ docker container ls -a  (this will give you a list of all you docker containers)

			$ sudo docker start [container name]

			$ sudo docker attach [container name] (this will su to root user)
            
			- Copy and paste the the filebeat-configuration file, and input the correct IP addresses. Repeat step for all yml files:

			# nano /etc/ansible/filebeat-config.yml

			- Copy and paste the playbook file for filebeat. Repeat step for all config files:

			# nano /etc/ansible/filebeat-playbook.yml 

			- Be sure to update the ansible host file with correct IP address:

			# nano /etc/ansible/host 



			  [filebeat_playbook.yml]
			---
			- name: installing and launching filebeat
			  hosts: webservers
			  become: yes
			  tasks:

			  - name: download filebeat deb
				command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

			  - name: install filebeat deb
				command: dpkg -i filebeat-7.6.1-amd64.deb

			  - name: drop in filebeat.yml
				copy:
				  src: /etc/ansible/filebeat-config.yml
				  dest: /etc/filebeat/filebeat.yml

			  - name: enable and configure system module
				command: filebeat modules enable system

			  - name: setup filebeat
				command: filebeat setup

			  - name: start filebeat service
				command: service filebeat start

			  - name: enable service filebeat on boot
				systemd:
				  name: filebeat
				  enabled: yes
				  

			[metricbeat_playbook.yml]
			---
			- name: Install metric beat
			  hosts: webservers
			  become: true
			  tasks:
				# Use command module
			  - name: Download metricbeat
				command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb

				# Use command module
			  - name: install metricbeat
				command: dpkg -i metricbeat-7.6.1-amd64.deb

				# Use copy module
			  - name: drop in metricbeat config
				copy:
				  src: /etc/ansible/metricbeat-config.yml
				  dest: /etc/metricbeat/metricbeat.yml

				# Use command module
			  - name: enable and configure docker module for metric beat
				command: metricbeat modules enable docker

				# Use command module
			  - name: setup metric beat
				command: metricbeat setup

				# Use command module
			  - name: start metric beat
				command: service metricbeat start

				# Use systemd module
			  - name: enable service metricbeat on boot
				systemd:
				  name: metricbeat
				  enabled: yes




			- Run this command to make sure all machines can be reached:

			# ansible all -m ping 

			You should see if host file is correct:

			10.0.0.10 | SUCCESS => {
				"changed": false,
				"ping": "pong"
			}
			10.0.0.5 | SUCCESS => {
				"changed": false,
				"ping": "pong"
			}
			10.0.0.6 | SUCCESS => {
				"changed": false,
				"ping": "pong"
			}
			10.1.0.4 | SUCCESS => {
				"changed": false,
				"ping": "pong"

			# ansible-playbook /etc/ansible/install-elk.yml
			
			- After Elk is installed test by SSH into Elk VM and run:
			
			# sudo docker ps 
			
			- Once you see that you have successfully installed ELK instance you can now install Filebeat and Metricbeat: 
			
			# ansible-playbook /etc/ansible/filebeat-playbook.yml (repeat for metricbeat)

			- If ran correctly you should see:


			[WARNING]: ansible.utils.display.initialize_locale has not been called, this may result in incorrectly calculated text
			widths that can cause Display to print incorrect line lengths

			PLAY [installing and launching filebeat] *******************************************************************************
			TASK [Gathering Facts] *************************************************************************************************ok: [10.0.0.6]
			ok: [10.0.0.10]
			ok: [10.0.0.5]

			TASK [download filebeat deb] *******************************************************************************************changed: [10.0.0.6]
			changed: [10.0.0.5]
			changed: [10.0.0.10]

			TASK [install filebeat deb] ********************************************************************************************changed: [10.0.0.10]
			changed: [10.0.0.6]
			changed: [10.0.0.5]

			TASK [drop in filebeat.yml] ********************************************************************************************changed: [10.0.0.6]
			changed: [10.0.0.5]
			changed: [10.0.0.10]

			TASK [enable and configure system module] ******************************************************************************changed: [10.0.0.5]
			changed: [10.0.0.6]
			changed: [10.0.0.10]

			TASK [setup filebeat] **************************************************************************************************
			changed: [10.0.0.5]
			changed: [10.0.0.6]
			changed: [10.0.0.10]

			TASK [start filebeat service] ******************************************************************************************
			changed: [10.0.0.5]
			changed: [10.0.0.10]
			changed: [10.0.0.6]

			TASK [enable service filebeat on boot] *********************************************************************************
			ok: [10.0.0.5]
			ok: [10.0.0.6]
			ok: [10.0.0.10]

			PLAY RECAP *************************************************************************************************************
			10.0.0.10                  : ok=8    changed=6    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
			10.0.0.5                   : ok=8    changed=6    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
			10.0.0.6                   : ok=8    changed=6    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

			 
			- To verify that the Elk-Stack is configured correctly log into your Kibana http://[ElkPublicIP]:5601/
