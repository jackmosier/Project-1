## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://drive.google.com/file/d/1sgZrZe4M4M3n_2zFSt1QmfVOfzGOKBy3/view?usp=sharing

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  - https://github.com/jackmosier/Playbooks/blob/main/install-elk.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundent, in addition to restricting traffic to the network.
- The Load Balancer adds additional layers of security from emerging threats such as DDoS attacks and authenticates user access. In addition to the added layer of security provided by the Load Balancer, the Jump Box acts as a "bridge" between two trusted networks and is treated as a single entryway to a server group.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- Filebeat: Watches and records log files from locations specified and forwards them to Logstash for indexing.
- Metricbeat: Is used to monitor and collect data such as system CPU, memory and load in a docker environment.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| Web-VM1  | Web VM   | 10.0.0.18  | Linux            |
| WebVM-2  | Web VM   | 10.0.0.19  | Linux            |
| WebVM-3  | Web VM   | 10.0.0.23  | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 20.69.91.74

Machines within the network can only be accessed by JumpBox_provisioner.
- Onle the JumpBox has access to the ELK-VM
- 10.0.0.15

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 10.00.0.15           |  
| ELK-serv | No                  |                      |
| WebVM-1  | No                  |                      |
| WebVM-2  | No                  |                      |
| WebVM-3  | No                  |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Then we dont need to take alot of time doing the operations manuely

The playbook implements the following tasks:
- Install Docker.io
- Install Python3-pip
- Install Docker module
- Increase virtual memory
- Use more memory
- Download and launch a docker elk container
- Enable service docker on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/jackmosier/Project-1/issues/1

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 (10.0.0.18), Web-2 (10.0.0.19), Web-3 (10.0.0.23)

We have installed the following Beats on these machines:
- Filebeat
- metricbeat

These Beats allow us to collect the following information from each machine:
- Logbeats allows us to monitor the lof files or locations that are specified to collect log events to be fowarded for indexing.
- Metricbeats helps us monitor our servers metrics and statistics such as inbound and outbound traffic that can be output into Logstash.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- nano into /etc/ansible/hosts to edit the hosts file in order for the playbooks to run on the specified machines. There you are going to locate #[webservers], when you do you are going to update it by entering the Web VM's IP addresses as follows:

10.0.0.6 ansible_python_interpreter=/usr/bin/python3'

10.0.0.7 ansible_python_interpreter=/usr/bin/python3

10.0.0.9 ansible_python_interpreter=/usr/bin/python3

'ansible_python_interpreter=/usr/bin/python3' specifies the container that it will be running

Make sure to un-comment [webservers].

Next you will create a new group called '[elk]' underneath '[webservers]', update that section by entering the ELK-Server's private IP as follows:

10.1.0.4 ansible_python_interpreter=/usr/bin/python3

Copy the filebeat-config.yml file to the /etc/ansible/ directory inside the ansible container.

Update the filebeat-config.yml file to include the Elk Server's Private IP on the following lines:

line 1105 'hosts: ["10.1.0.4:9200"]'

line 1806 'host: "10.1.0.4:5601"'

Run the playbook filebeat.yml located in the /etc/ansible/roles/ directory inside the ansible container, then navigate to Kibana using your web browser by inputing the URL http://20.115.166.166:/app/kibana#/ which would be the ELK-Server public IP, specifying port '5601' to check that the installation worked as expected.

Copy the metricbeat-config.yml file to /etc/ansible/ directory inside the ansible container.

Update the metricbeat-config.yml file in the /etc/ansible/ to include the Elk-Server's private IP on lines 62 and 95 as follows:

line 62 'host: "10.1.0.4:5601"'

line 95 'hosts: ["10.1.0.4:9200"]'

Run the playbook, metricbeat-playbook.yml inside the /etc/ansible/roles directory, and navigate to Kibana on your web browser using the URL http://20.115.166.166:/app/kibana#/ ELK-Server public IP, specifying port '5601' to check that the installation worked as expected.
