## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.



THE NETWORK DIAGRAM
![Screenshot (266)](https://user-images.githubusercontent.com/89820505/156865266-7fab235a-8ecb-4ff0-a561-7ba34814e0c4.png)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml and config file may be used to install only certain pieces of it, such as Filebeat.

  - Install-elk.yml
  - filebeat_playbook.yml
  - install_docker_playbook

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly Functional & available in addition to restricting traffic to the network.
- Load balancers add resilience by rerouting live traffic from one server to another if a server falls prey to a DDoS attack or otherwise becomes unreachable
A Jumpbox Provisioner is also important as it prevents Azure VMs from being exposed via public IP Address. This allows us to do monitoring and logging on a single box. We can also restrict the ip addresses ability to communicate w/ the jump box

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the Network and system Logs.
- _TODO: What does Filebeat watch for?_
- FileBeat monitors the log files or locations that you specify, collects log events & forwards them either to Elasticsearch or Logstash for indexing
- _TODO: What does Metricbeat record?_
 Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name      | Function      | IP Address             | Operating System |
|-----------|---------------|------------------------|------------------|
| JumpBox   | Gateway       | 10.0.0.9 20.121.184.10 | LINUX            |
| WEB-1     | Ubuntu-server | 10.0.0.7 52.170.148.41 | LINUX            |
| WEB-2     | Ubuntu-server | 10.0.0.6 52.170.148.41 | LINUX            |
| WEB-3DWVA | Ubuntu-server | 10.0.0.10              | LINUX            |
| ELKserver | Ubuntu-server | 10.1.0.4 40.78.100.118 | LINUX            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JUMP-BOX machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO:Workstation MY public IP thru TCP 5601

Machines within the network can only be accessed by Workstation and Jump-Box thru SSH JUMP-BOX.
-JUMP-BOX IP: 10.1.0.4 via SSH Jump-Box to access my ELK VM
The Ip Address was the Workstation of MY public IP via Port TCP 5601

A summary of the access policies in place can be found in the table below.

| Name      | Publicly Accessible | IP Address                                |
|-----------|---------------------|-------------------------------------------|
| JumpBox   | YES                 | 20.121.184.10  (workstation IP on SSH 22) |
| WEB-1     | NO                  | 10.0.0.7 on SSH 22                        |
| WEB-2     | NO                  | 10.0.0.9 on SSH 22                        |
| WEB-3DWVA | NO                  | 10.0.0.9 on SSH 22                        |
| ELKserver | NO                  | Workstation MY Public IP using  TCP 5601  |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?
- There are multiple advantages, Ansible lets you quickly and easily deploy multiter applications through a YAML playbook
- You arent required to write custom code to automate your systems
- Ansible will also figure out how to get your systems to the state you wanjt them to be in

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- name: config elk VM with Docker
- hosts: elk
- become: true
- tasks:
- 
- .Install docker.io
  - name: Install docker.io
    apt:
      update_cache: yes
      force_apt_get: yes
      name: docker.io
      state: present
- Install python-pip
-   - name: Install python3-pip
    apt:
      force_apt_get: yes
      name: python3-pip
      state: present

    # Use pip module (It will default to pip3)
  - name: Install Docker module
    pip:
      name: docker
      state: present
      `docker`, which is the Docker Python pip module.
      
- Install docker module
 - name: Download and launch a docker elk container
   docker_container:
     name: elk
     image: sebp/elk:761
     state: started
     restart_policy: always

- Increase Virtual Memory
-  - name: Use more memory
   sysctl:
     name: vm.max_map_count
     value: '262144'
     state: present
     reload: yes
- -     published_ports:
       -  5601:5601
       -  9200:9200
       -  5044:5044   

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Screenshot (265)](https://user-images.githubusercontent.com/89820505/156863919-99c05ed3-2ec5-4383-83e0-7a4d2313ad1c.png)


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _WEB-1: 10.0.0.7
- WEB-2: 10.0.0.6
- WEB-3DVWA: 10.0.0.10
We have installed the following Beats on these macines:
![Filebeat_data_successful](https://user-images.githubusercontent.com/89820505/156864449-4cf80742-c7c1-422b-9a55-e0d1925c1a63.png)
![Metricbeat_data_successful](https://user-images.githubusercontent.com/89820505/156864451-b0daeaac-75d3-452c-b130-9eb661b60d39.png)


Filebeat will be used to collect log files from specfic files. i.e Apache, Microsoft Azure tools and web servers, etc
Metericbeat will be used to monitor VM stats per CPU core stats, per filesystem stats, memory or network stats

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the yml file to ansible folder_.
- Update the config file to include remote users and ports
- Run the playbook, and navigate to kibana (ELK IP address):5601 to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
-copy the ifilebeat_Playbook.yml to the /etc/ansible/roles/files/directory
- Update the file to include the host IP for the ELK server (10.1.0.4) to port 9200 which is the API over HTTP that communicates w/ Elasticsearch
-Run the playbook, and navigate to the ELK-VM by running SSH to the Private IP (10.1.0.4) of the elk server & navigate to http://[0.0.0.0]:5601/app/kibana to check that installation works 
-Create a new playbook in the /etc/ansible/roles directory that will install, drop in the updated config file, enable and confid system module, run the filebeat setup, and start the filebeat service
-

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
ssh Orochi@29.121.184.10
sudo docker container list -a (to locate ansible container)
sudo docker start container (the assigned name of the container)
sudo docker attack container ( of the assigned name of container)
cd /etc/ansible
ansible-playbook install-elk.yml (configs Elk Server & starts the Elk container on the ELK-server) wait a moment for the implementation of the ELK-server
cd /etc/ansible/roles
ansible-playbook filebeat_playbook.yml (installs filebeat & metricbeat)
Open a new web broswer session and navigate to Elk-server 40.78.100.188:5601/App/kibana to bring up the kibana web portal.
