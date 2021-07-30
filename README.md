## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[Network Diagram:](./images/Network%20Diagram%20(1).pdf)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook yml file may be used to install only certain pieces of it, such as Filebeat.

- [filebeat Playbook:](filebeat-playbook.yml)
- [Pentest Playbook for DVWA setup:](pentest..yml)

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application.

Load balancing ensures that the application will be highly available and reliable, in addition to restricting access to the network.  It also helps protect production servers by redirecting traffic in case of DDOS attacks.  
Administrative Access to these load balanced virtual machines is provided through the Jump Box Provisioner.  Using the Jump Box Provisioner provides the advantage of Network Segmentation and Access Control as well as central launching point for any administrative tasks needed on the network servers.


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

- Much of this monitoring is simplified by utilizing a software called [Filebeat](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-overview.html)
  - Filebeat does much of the heavy lifting when it comes to retrieving the log data needed.  It's configured to search for logs in locations you specified and then forwards any found logs to the harvester where all the logs are aggreated.


- Another software that also helps collect metric data from the system and services running on the servers is called [Metricbeat](https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-overview.html)
  - Metricbeat functionality is very similar to that of filebeat, but its focus is what's running within the server.
  - Services that Metricbeat may include Apache, MySQL or MongoDB to name a few.  
    - [Here's the complete list of modules Metricbeat supports](https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-overview.html)

The configuration details of each machine may be found below.

| Name                 | Function       | IP Address | External IP   | Operating System     |
|----------------------|----------------|------------|---------------|----------------------|
| Jump Box Provisioner | Gateway        | 10.0.0.1   | 52.183.59.195 | Linux (ubuntu 20.04) |
| Web-1                | Web Server     | 10.0.0.5   |      None     | Linux (ubuntu 20.04) |
| Web-2                | Web Server     | 10.0.0.6   |      None     | Linux (ubuntu 20.04) |
| Elk-Server           | Server Monitor | 10.1.0.5   | 20.150.141.225| Linux (ubuntu 20.04) |

<details>
  <summary>Prepare Virtual machines and related Azure objects</summary>
  
  ## CreateVMS
  1. [The Virtual Machines can be provisioned in the same manner.  ](./images/CreateVM)
  2. [Once the VMs are provisioned the Load Balancer can be created. ](./images/CreateLoadBalancer.PNG)
  3. [Once the Load Balancer is created add Web-1 and Web-2 to a newly created Backend pool. ](./images/BackEndPool.PNG)
  4. [A load balancing rules is then created to manage the flow of traffic. ](./images/LoadBalanceRule.PNG)
  5. In order to get the machines to be able to communicate with each other add an SSH key to all your VMs
     * To generate the SSH Key run your can run the commands below or if you have one load that key:
       - ~/.ssh# ssh-keygen
       - ~/.ssh# cat id_rsa.pub
     * Once you have that key, go to each VM and select [Reset Password.](./images/ResetVMPassword.PNG)
       - Mode : Reset SSH Public Key
       - Username: Whatever username you setup the VMs with.
       - SSH public key: Copied key from generated code or old key you already had.
  6. [Modify the host file on the ansile container to include a reference to the Web Servers and to the Elk Server.](./images/HostChanges.png)
  6. Next you install the necessary software by loading ansible docker container into the Web servers via the Jump Box Provisioner.
  7. To test and see if everything worked out go to the IP address for the load balancer's setup.php page.  My page looks url is: [http://13.66.162.18/setup.php](./images/DVWATest.PNG), yours will be different.
</details>

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

<details>
  <summary>Only the Elk Server machine can accept connections from the Internet. Access to this machine is only allowed from the following IP whatever IP addresses were white listed in the Azure inbound rules:</summary>
    
  ## Creating Outbound rule on Elk Server
  1. Locate you IP address on https://whatismyipaddress.com
  2. [Once your IP address is located create an inbound rule allowing SSH access to your machine.](./images/InboundRuleElkServer.PNG)
</details>
    
    
Machines within the network can only be accessed by whatever IP address was setup witthin the outbound rule on the load balancer.

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Addresses                 |
|----------------------|---------------------|--------------------------------------|
| Jump Box Provisioner | No                  | IP setup on Inbound Rule             |
| Web-1                | No                  | 10.0.0.4 on ssh 22                   |
| Web-2                | No                  | 10.0.0.4 on ssh 22                   |
| ELK Server           | No                  | IP Setup in Outbound rule on TCP 5601|
| Load Balancer        | No                  | IP Setup in Outbound rule on HTTP 80 |


### Elk Configuration  

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_
_
The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ...
- ...

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
