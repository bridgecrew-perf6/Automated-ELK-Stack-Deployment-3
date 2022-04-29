## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.



These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YML file may be used to install only certain pieces of it, such as Filebeat.

  - [ELK Install](Ansible/install-elk.yml)
  - [Metricbeat Playbook](Ansible/metricbeat-playbook.yml)
  - [Filebeat Playbook](Ansible/filebeat-playbook.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network. Load balancers help ensure environment availability through distribution of incoming data to web servers. The advantage of having a jump box allows for more easy administration of multiple systems and provides an additional protection layer between the outside and assets  

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system metrics.
- FileBeat watches for log directories or specific log files.
- MetricBeat helps you monitor your servers by collecting metrics from the system and services running on the server.

The configuration details of each machine may be found below.
| Name               | Function    | IP Address | Operating System |
|--------------------|-------------|------------|------------------|
| JumpBoxProvisioner | Gateway     | 10.0.0.7   | Linux (Ubuntu)   |
| Web1               | DVWA Server | 10.0.0.8   | Linux (Ubuntu)   |
| Web2               | DVWA Server | 10.0.0.9   | Linux (Ubuntu)   |
| CentralVM-Alpha    | ELK Server  | 10.1.0.5   | Linux (Ubuntu)   |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBoxProvisioner and CentralVM-Aplha machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Personal IP address

Machines within the network can only be accessed by JumpBoxProvisioner.

A summary of the access policies in place can be found in the table below.

| Name               | Publicly Accessible | Allowed IP Addresses |
|--------------------|---------------------|----------------------|
| JumpBoxProvisioner | Yes                 | Personal             |
| Web1               | No                  | 10.0.0.7             |
| Web2               | No                  | 10.0.0.7             |
| CentralVM-Alpha    | Yes                 | Personal             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows consistent and predictable configuration. 

The playbook implements the following tasks:
- Installs docker.io, pip3, docker python module
![alt text](https://github.com/juan-desu/Automated-ELK-Stack-Deplyment/blob/385bd483ce51459d6875681cbe65ba68cdac6777/Images/Install%20docker.png)
- Increases Virtual memory
![alt text](https://github.com/juan-desu/Automated-ELK-Stack-Deplyment/blob/385bd483ce51459d6875681cbe65ba68cdac6777/Images/Increase%20Memory.png)
- Enables systemd docker service
![alt text](https://github.com/juan-desu/Automated-ELK-Stack-Deplyment/blob/385bd483ce51459d6875681cbe65ba68cdac6777/Images/Enable%20systemd%20module.png) 
- Downloads and launches the docker container for elk server
![alt text](https://github.com/juan-desu/Automated-ELK-Stack-Deplyment/blob/385bd483ce51459d6875681cbe65ba68cdac6777/Images/Download%20and%20lauch%20docker.png)


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![alt text](https://github.com/juan-desu/Automated-ELK-Stack-Deplyment/blob/385bd483ce51459d6875681cbe65ba68cdac6777/Images/docker-sudo-ps.png
)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1 (10.0.0.7)
- Web 2 (10.0.0.8)

We have installed the following Beats on these machines:
- FileBeat
- MetricBeat 

These Beats allow us to collect the following information from each machine:
- FileBeat analyses and forwards system logs from the VMs to the ELK Stack. FileBeat generates the log files, tailing them, and forwarding the data to either Logstash for more advanced processing or directly into Elasticsearch for indexing.
- MetricBeat collects and reports system metrics and statics about the Web VMs to the ELK Stack VM.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the instal-elk.yml file to /etc/ansible/roles.
- Update the /etc/ansible/hosts file to include the ELK stack VM IP address.
- Run the playbook, and navigate to http://[local.elk.ip]:5601/app/kibana to check that the installation worked as expected.
- Which file is the playbook? The FileBeat-configuration 
- Which file do you update to make Ansible run the playbook on a specific machine? Update filebeat-config.yml 
- How do I specify which machine to install the ELK server on versus which to install Filebeat on? Specify which machine to install by updating the host files with the web and elk servers IP addresses and selecting which group to run on in ansible.
- Which URL do you navigate to in order to check that the ELK server is running? http://[external.elk.ip]:5601/app/kibana

## Using the MetricBeatand FileBeat Playbooks
FileBeat
- In the ansible container navigate to /etc/ansible/files/filebeat-config.yml and make the desired changes to include the ELK Stack IP address and the default login credentials.
- To run the playbook run: ansible-playbook /etc/ansible/role/filebeat-playbook.yml

MetricBeat
- In the ansible container navigate to /etc/ansible/files/metric-config.yml and make the desired changes to include the ELK Stack IP address and the default login credentials.
- To run the playbook run: ansible-playbook /etc/ansible/role/metribeat-playbook.yml
