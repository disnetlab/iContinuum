# iContinuum: An Emulation Toolkit for Intent-Based Computing Across the Edge-to-Cloud Continuum

Please find the full paper here: https://nzjohng.github.io/publications/papers/cloud2024.pdf
https://www.adelnadjarantoosi.info/pdf/CLOUD2024.pdf

In this tutorial, we propose an emulation toolkit for building intent-based edge-to-cloud computing testing and experimentation platforms called iContinuum. iContinuum allows developers or end-users to set up an emulated testbed for their applications and perform experimentation and performance evaluation. The proposed emulation toolkit comprises multiple distinct layers with a set of components to build all the layers from infrastructure to applications. We employ an advanced approach to emulate our network environment by leveraging Software-Defined Networking (SDN) as our "Network Controller". SDN decouples the data plane and control plane in IoT, enabling controllers to effectively regulate network flow while considering resource usage conditions of both cloud and edge servers. Additionally, we utilize containerization and orchestration technologies to efficiently manage the deployment and operation of applications across the emulated infrastructure. This ensures comprehensive consideration of both networking and computing parameters within the edge-to-cloud environment. Moreover, the toolkit supports users in specifying their desired requirements or intents for their applications, such as target response time or energy consumption, and maintains continuous alignment between the desired state of the applications and their actual performance. Please note that applications can include different microservices (containers). 

For the SDN Controller, we utilize ONOS controller and for the container orchestrator we use Kubernetes- In this project we use K3s which is a lightweight version of Kubernetes. To emulate the network infrastructure containing switches, we use Mininet which is a powerful Network Emulator supporting Open vSwitch (OVS). To have the hosts (servers) in the network topology, we configure a kubernetese cluster to connect to the Mininet topology by configuring GRE tunneling betwen the Kubernetes nodes and OVS switches in Mininet.

We employ a monitoring tool known as sFlow-RT which is providing a real-time monitoring solution designed to gather telemetry data from industry-standard sFlow Agents integrated into network devices or hosts. We also utilize the open-source Host sFlow agent for performance monitoring of hosts and servers. Moreover, we also utilize standard sFlow agents to monitor Open vSwitches within the Mininet network topology. Furthermore, to monitor the different microservices (Pods) of the application, we leverage sidecar containers with sFlow agents to operate alongside the main application container within the same Pod. We also utilize the Prometheus Exporter, an application integrated into the sFlow-RT analytics platform to convert real-time telemetry streams from sFlow agents on hosts and switches into metrics accessible via a REST API and a format compatible with Prometheus,enabling Prometheus to retrieve and utilize these metrics. And, We utilize Prometheus as a robust time series database and alerting system for persistent storage of real-time monitoring data collected by sFlow-RT. Additionally, we leverage Grafana, an open-source analytics and monitoring solution that seamlessly integrates with Prometheus for enhanced visualization. This integration allows us to efficiently collect, query, visualize, and alert on metrics data.

We have automated the entire setup of iContinuum including configuration, application deployment, and monitoring described above using the Ansible platform which is accessible here.

We have created three different sample topologies. The code for the first topology can be found in "Example1"- The code for the second topology can be found in "Example2"- The code for the third topology can be found in "Example3". 

Also, the sample topologies are shown below.


                                                     "Custom topology example1"

                                  An example of 2 switches and 2 K3s hosts along with 2 mininet hosts
                                  

                                                       k3s host    k3s host
                                                          |          |
                                                          |          |
                                                          |          |
                                    mininet host --- switch --- switch --- mininet host

-----------------------------------------------------------------------------------------------------------------
                                
                                                     "Custom topology example2"

                                  An example of 3 switch and 3 K3s hosts along with 2 mininet hosts


                                                       k3s host      k3s host
                                                          |            |
                                                          |            |
                                                          |            |
                                     mininet host --- switch ----- switch --- mininet host
                                                           \         /
                                                            \       /
                                                             \     /
                                                             switch
                                                               |
                                                               |
                                                               |
                                                           k3s host

 -----------------------------------------------------------------------------------------------------------------

                                                     "Custom topology example3"

                                  An example of 6 switch and 4 K3s hosts along with 2 mininet hosts
                          

                                                       k3s host      k3s host
                                                          |            |
                                                          |            |
                                                          |            |
                                   mininet host  ----- switch ----- switch ---- mininet host
                                                       / |  \       /  | \
                                                      /  |   \     /   |  \
                                                     /   |    \   /    |   \
                                                    /    |     \ /     |    \
                                               switch    |      /\     |    switch
                                                    \    |     /  \    |    /
                                                     \   |    /    \   |   /
                                                      \  |   /      \  |  /
                                                       \ |  /        \ | /
                                                      swicth ----- swicth
                                                        |             |
                                                        |             |
                                                        |             |
                                                   k3s host      k3s host
                                                   


# Please consider the below details regarding the available files in the master branch.

- It contains several yaml files named "onos.yml, mininet.yml, monitor.yml".

- "onos.yml" file has all the configuration for SDN Controller (Network Controller). We call this as onos machine during this tutorial.

- "mininet.yml" file has all the configuration for Network Emulator. We call this as mininet machine during this tutorial.

- "monitor.yml" file has all the configuration for Monitoring tools. We call this as sflow-rt machine during this tutorial.

    - Note1- "kubernetes.yml" file has all the configuration for Kubernetes cluster. This file is available in each Example folder due to different topologies for hosts and switches.
      
    - Note2- "example.py.j2" file creates a sample topology through Mininet. This file is available in each Example folder due to different topologies for switches.
     
    - Note3- "inventory.ini" file defines the IP address of all VMs and their specific interface{(tap0)- assigned to kubernetes nodes} and also the server names. This file is available in each Example folder due       to 
      different IP addresses and server name for VMs.
      
    - Note4- "topology.sh.j2" runs the topology defined on "example.py.j2" on mininet machine. This file is available in each Example folder due to different topologies and configuration.
      
    - Note5- "deployment.yml.j2" file creates the deployment of the application. This file is available in each Example folder due to different application.

- "main.yml" file has all the above mentioned file inside.

- "ansible.cfg" file disables SSH host key checking and suppresses command warnings in Ansible.

- "prometheus.yml.j2" file generates data in the prometheus format and sends data to sflow-rt.

- "host-sflow.yml.j2" file deploys on the kubernetes cluster to send data from each node to sflow-rt.

 -----------------------------------------------------------------------------------------------------------------
 Note that the emulator has been tested on "Ubuntu 20.04 LTS", so it is recommended to use this version.


 # ========= How To Use the tool =========

To use the emulator, please follow the steps below.


# Step1- Preparation

  1- Create Virtual Machines (VMs) to install onos, mininet, sflow-rt, and kubernetes cluster.
  
      - Note1- While creating VMs, it is highly recommended to configure the same key-pair for all the VMs.
      
      - Note2- While creating VMs, it is highly recommended to configure a unique hostname based on its difinition.Eg. onos, mininet, sflow-rt, etc.
      
      - Note3- You are able to use one VMs to install onos, mininet, and sflow-rt, or alternatively you can use 3 different VMs.
      
           - If you prefer to combine the VMs into one, you may need at least "8 vCPUs and 16GB RAM". 
           
           - If you prefer to have separate VMs for each, you can consider "4 vCPUs and 8GB RAM" for onos, and "2 vCPUs and 4 GB RAM" for mininet as well as sflow-rt.
           
      - Note4- Kubernetes cluster is a group of VMs acting as kubernetes nodes. 
      
           - You are required to configure a unique hostname for the kubernetes nodes. It is recommended to name the control-plane as master, and worker nodes as worker1, worker2, etc. 
           - For each worker node, you may consider "2 vCPUs and 4 GB RAM", and for the master node you may consider "4 vCPUs and 8GB RAM".
           
      - Note5- While creating VMs, configure required ports defined below (If the VMs are in the cloud).
          - TCP port 22 for SSH
          
          - TCP port 8008 for sflow-rt
          
          - UDP port 6343 for sflow-rt
          
          - TCP/UDP ports for K3s
          
              - Ref: https://docs.k3s.io/installation/requirements#:~:text=The%20K3s%20server%20needs%20port,listen%20on%20any%20other%20port.
              
              Note- You may need to open some other ports based on your requirements.
      
      

  2- Install Ansible on the local terminal.
  
           - You can install Ansible on you local terminal using the below commands:
    
               - Linux terminal (Ubuntu): sudo apt install ansible
      
               - mac OS: pip install ansible
      
               - If you have Windows terminal, enable Windows Subsystem for Linux (WSL) and then use "sudo apt install ansible"
      
  3- Create a new directory (folder) in the local terminal and pull all the available files and folders from master repository to that folder. To do that, please follow the below steps.
  
               - mkdir sample_folder
               - git clone https://github.com/disnetlab/iContinuum
               - cd iContinuum

# Step2- Modification

Based on the desired topology in each Example folder, edit the inventory file of that sample_folder.

      1- Eg. nano Example1/inventory.ini
      
      2- Replace IP address and the server name of the VMs with the actual one in inventory.ini. If you combine some VMs (based on step1- Note3), IP address of some nodes are the same. Then, you can just repeat IP 
          address in the specific section.
      
         - Note1- server name is the username that you can login to the remote VM via ssh. 
         
             - Eg. Considering this command "ssh ubuntu@192.168.56.123", the server name is "ubuntu".
         
         - Note2- No other changes need to be done in the inventory.ini.
         
             - If you wish to change the tap0 IP address of the kubernetes nodes, please make sure that you do not change the subnet range. However, you can keep the current configuration for that.
           
       3- Save changes.
          
# Step3- Installation

1- Run ansible playbook with one of the below commands based on the desired topology you previousely selected among the the Example folders and edited its inventory file.

          - sudo ansible-playbook -i Example1/inventory.ini main.yml -e "example_folder=Example1" --private-key=<Path-To-The-Private-Key>
          
          - sudo ansible-playbook -i Example2/inventory.ini main.yml -e "example_folder=Example2" --private-key=<Path-To-The-Private-Key>

          - sudo ansible-playbook -i Example3/inventory.ini main.yml -e "example_folder=Example3" --private-key=<Path-To-The-Private-Key>
          
                Note- Replace your actual private key with <Path-To-The-Private-Key>
            
2- Wait until the installation is completely done.

# Step4- Login to the Graphical User Interface (GUI) and Command Line Interface (CLI)

Note- If you install Ubuntu Server, you may need to install "VNC" to be able to have GUI. So make sure to install VNC on your server.

  1- Where onos, mininet, sflow-rt are installed, you can Login to the VM (VMs) and follow the belwo steps to access the GUI and CLI.
  
           1- Open this URL to login to the onos GUI: "http://localhost:8181/onos/ui/#/topo2"-  username: onos, password:rocks     // This is where onos is installed
           
           3- If you wish to have interaction with onos CLI use this command : "ssh -p 8101 karaf@<IP_Address_of_onos>"  -  password is "karaf"    // This is where onos is installed
           
                - Note- Replace the actual IP address of the onos machine with <IP_Address_of_onos> 
               
           2- Open this URL to login to Portainer to see the containers:  "https://localhost:9443"    #You will be asked to set a new password.     // This is where onos machine and also sflow-rt are installed
           
                - Note- Portainer is installed on both sflow-rt and also onos machine.
               
           3- Open this URL to login to sflow-rt: "http://localhost:8008"      // This is where sflow-rt is installed
           
           4- Open this URL to login to Prometheous: "http://localhost:9090"   // This is where sflow-rt is installed   
           
           4- Open this URL to login to grafana: "http://localhost:3000"    #You will be asked to set a new password.    // This is where sflow-rt is installed     
           
           3- If you wish to have interaction with mininet CLI, the running topology in onos GUI will be stopped and you can start it again based on the selected topology available in mininet machine known as "topology.sh" . // This is where mininet is installed
             
             # Note- You are not required
             to do this step . That is just FYI if the topolgy has been reseted.
              
               - Firstly, navigate to the home directory and find the file named "topology.sh" , then open the file.
               
               - secondly, open a terminal in mininet machine.
               
               - Thirdly, run the topology again. To run the topology, find the the line mentioning "Run mininet topology" in "topology.sh". Then, run the command which is specified between cotations.
               
                   - Eg. 'sudo mn --custom example.py,sflow-rt/extras/sflow.py --topo mytopo --mac --switch=ovs,protocols=OpenFlow13 --controller=remote,ip={{onos_ip}}'  
        
                        - Note1- Please make sure you remove the '' from the command before execution.
                        
                        - Note2- You will see the IP address of onos machine here "{{onos_ip}}" filled up automatically.
                        
                - Since the topolgy is reseted, you need to configure GRE for the new topology to connect to the Kubernetes nodes. To do that, find the line mentioning "Create gre tunnel between a mininet swicth and kubernetese master/worker node" in "topology.sh".
                        
                        - Run all the commands found in this section.   
                
                - Since the topolgy is reseted, you need to configure the topology to send data to sflow-rt again. To do that, find the line mentioning "Send data of the switch interface in the mininet to the monitoring tool (sFlow-RT)" in "topology.sh".
                        
                        - Run all the commands found in this section.
               
               
  2- Where master node is installed, you can login to the CLI and check if the nodes and pods(deployment) are ready with the below command.
  
               -  sudo kubectl get nodes,pods,deployment,service -o wide
                  - Now you are able to see all pods, nodes, services and deployment inside the K3s cluster
               

# ========= You're Done ========= 

                           ◝(ᵔᵕᵔ)◜
                                       
                ◝(ᵔᵕᵔ)◜              ◝(ᵔᵕᵔ)◜

 
