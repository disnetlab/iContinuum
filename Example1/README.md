#To use the topology for example1, please follow the below steps.

# ========= Explanation ========= 
#As you can see from the codes, the tool contains 4 main yaml files named onos.yml, mininet.yml, monitor.yml and kubernetes.yml as the "Network Controller" "Network Emulator", "Monitoring Tools", and "Cluster Manager", respectively.

#For "Network Controller", we use ONOS controller and you can find "Network Controller" configuration on "onos.yml". We call it onos machine.

#For "Network Emulator", we use Mininet and you can find "Network Emulator" configuration on "mininet.yml". We call it mininet machine.

#For "Monitoring Tools", we use sFlow-RT and you can find "Monitoring Tools" configuration on "monitor.yml". We call it sflow-rt machine including other monitoring tools such as prometheus and grafana.

#You can find "Cluster Manager" configuration on "kubernetes.yml". We call it kubernetes cluster since it has at least one master node and several worker nodes.

#You can see "main.yml" file which includes all the above mentioned.

#You can see "ansible.cfg" which disables SSH host key checking and suppresses command warnings in Ansible.

#You can see "example.py.j2" which creates the above topology.

#You can see "inventory.ini" which defines the IP address of all VMs and their specific interface{(tap0)- assigned to kubernetes nodes} and the server names.

#You can se " prometheus.yml.j2" which generates data in the prometheus format and sends data to sflow-rt.

#You can see "deployment.yml.j2" which creates an nginx container alongwidth a sidecar container deployed in the kubernetes cluster.

#You can see "host-sflow.yml.j2" which deploys on the kubernetes cluster to send data from each node to sflow-rt.

#You can see "topology.sh.j2" which runs the above topology defined on "example.py.j2" on mininet machine.

Note- The emulator has been tested on Ubuntu 20 LTS, so it is recommended to use this version.

# ========= How To Use ========= 
To use this emulator with the above topology, please follow the steps below.

# Step1- Preparation

  1- Create Virtual Machines (VMs) to install onos, mininet, sflow-rt, and kubernetes cluster.
  
      # Note1- While creating VMs, it is highly recommended to configure a unique key-pair for all the VMs.
      
      # Note2- While creating VMs, it is recommended to configure a hostname based on the difinition above- Eg. onos, mininet, etc.
      
      # Note3- You are able to use one VMs to install onos, mininet, and sflow-rt, or alternatively you can use 3 different VMs.
      
           - If you prefer to combine the VMs into one, you may need at least "8 vCPUs and 16GB RAM". 
           
           - If you prefer to have separate VMs for each, you can consider "4 vCPUs and 8GB RAM" for onos, and "2 vCPUs and 4 GB RAM" for mininet as well as sflow-rt.
           
      # Note4- Kubernetes cluster is a group of VMs acting as kubernetes nodes. 
      
           - It is recommended to configure a hostname for the control-plane as the master, and worker nodes as worker1, worker2, etc. 
           
      # Note5- While creating VMs, configure ssh (TCP port 22) for all the VMs.  
      
      # Note6- For the Kubernetes cluster, we use K3s version.
      
           - For each worker node, you may consider "2 vCPUs and 4 GB RAM", and for the master node you may consider "4 vCPUs and 8GB RAM".
           
      # Note4- Open ports for all the kubernetes nodes. ref: https://docs.k3s.io/installation/requirements#:~:text=The%20K3s%20server%20needs%20port,listen%20on%20any%20other%20port.

  2- Ansible installation
  
    Install Ansible on you local terminal using the below commands:
    
      # Linux terminal (Ubuntu): sudo apt install ansible
      
      # mac OS: pip install ansible
      
      # If you have Windows terminal, enable Windows Subsystem for Linux (WSL) and then use "sudo apt install ansible"
      
  3- Create a folder in the local terminal and pull all the available files from this repository to that folder.

# Step2- Modification

Navigate to the folder that all the files are available and follwo the below steps.

      1- Open "inventory.ini" with an editor such as vim or nano.
      
      2- Replace IP address and the server name of the VMs with the actual one in inventory.ini. If you combine some VMs (based on step1-Note3), IP address of some nodes are the same. Then, you can just repeat IP address in the specific section.
      
         # Note- server name is the username that you can login to the remote VM via ssh. Eg. in this command "ssh ubuntu@192.168.56.123" server name is ubuntu
         
         # Note- No other changes need to be done in the inventory.ini.
         
           -  If you wish to change the tap0 IP address of the kubernetes nodes, please make sure that you do not change the subnet range.
           
       3- Save changes.
          
# Step3- Installation

1- Run ansible playbook with the below command:

          # sudo ansible-playbook -i inventory.ini main.yml --private-key=<Path-To-The-Private-Key>
          
            Note- Replace your actual private key with <Path-To-The-Private-Key>
            
2- Wait until the installation is completely done.

# Step4- Login

  1- Where onos, mininet, sflow-rt are installed, you can Login to that VM and follow the belwo steps to access the graphical User Interface.
  
           1- Open this URL to login to the onos GUI: "http://localhost:8181/onos/ui/#/topo2"-  username: onos, password:rocks     // This is where onos is installed
           
           3- If you wish to have interaction with onos CLI use this command : "ssh -p 8101 karaf@<IP_Address_of_onos>"  -  password is "karaf"    // This is where onos is installed
           
               # Note- Replace the actual IP address of the onos machine with <IP_Address_of_onos> 
               
           2- Open this URL to login to Portainer to see the containers:  "https://localhost:9443"    #You will be asked to set a new password.     // This is where onos machine and also sflow-rt are installed
           
               # Note- Portainer is installed on sflow-rt and also onos machine.
               
           3- Open this URL to login to sflow-rt: "https://localhost:8008"      // This is where sflow-rt is installed
           
           4- Open this URL to login to Prometheous: "https://localhost:9090"   // This is where sflow-rt is installed   
           
           4- Open this URL to login to grafana: "https://localhost:3000"      // This is where sflow-rt is installed     
           
           3- If you wish to have interaction with mininet CLI, the running topology in onos GUI will be stopped and you can start it again with the belwo steps. // This is where mininet is installed
           
               -  sudo mn --custom example.py,sflow-rt/extras/sflow.py --topo mytopo --mac --switch=ovs,protocols=OpenFlow13 --controller=remote,ip=<IP_Address_of_onos>  //Replace the actual IP address of the onos machine with <IP_Address_of_onos>
               
               -  sudo ovs-vsctl add-port s1 int1 -- set Interface int1 type=gre options:remote_ip=<Master-IP-Address>    //Replace the actual IP address of the master node with <Master-IP-Address>
               
               -  sudo ovs-vsctl add-port s1 int1 -- set Interface int1 type=gre options:remote_ip=<Worker1-IP-Address>    //Replace the actual IP address of the master node with <Worker1-IP-Address>
               
               -  sudo ovs-vsctl -- --id=@sflow create sflow agent=eno1 target="<sflowrt-IP-Address>\:6343" header=128 sampling=64 polling=10 -- set bridge s1 sflow=@sflow    //Replace the actual IP address of the sflow-rt with <sflowrt-IP-Address>
               
               -  sudo ovs-vsctl -- --id=@sflow create sflow agent=eno1 target="<sflowrt-IP-Address>\:6343" header=128 sampling=64 polling=10 -- set bridge s2 sflow=@sflow    //Replace the actual IP address of the sflow-rt with <sflowrt-IP-Address>
               
               # Note- You do not need to do step3. That is just FYI.
               
  2- Where master node is installed, you can login and check if the nodes and pods(deployment) are ready.
  
               -  sudo kubectl get nodes, pods, deployment, service
               

# ========= You're Done ========= 

                           ◝(ᵔᵕᵔ)◜
                                       
                ◝(ᵔᵕᵔ)◜              ◝(ᵔᵕᵔ)◜

