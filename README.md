# iContinuum: An Emulation Toolkit for Intent-Based Computing Across the Edge-to-Cloud Continuum

In this tutorial, we propose an emulation toolkit for building intent-based edge-to-cloud computing testing and experimentation platforms called iContinuum. iContinuum allows developers or end-users to set up an emulated testbed for their applications and perform experimentation and performance evaluation. The proposed emulation toolkit comprises multiple distinct layers with a set of components to build all the layers from infrastructure to applications. We employ an advanced approach to emulate our network environment by leveraging Software-Defined Networking (SDN) as our "Network Controller". SDN decouples the data plane and control plane in IoT, enabling controllers to effectively regulate network flow while considering resource usage conditions of both cloud and edge servers. Additionally, we utilize containerization and orchestration technologies to efficiently manage the deployment and operation of applications across the emulated infrastructure. This ensures comprehensive consideration of both networking and computing parameters within the edge-to-cloud environment. Moreover, the toolkit supports users in specifying their desired requirements or intents for their applications, such as target response time or energy consumption, and maintains continuous alignment between the desired state of the applications and their actual performance. Please note that applications can include different microservices (containers). 

For the SDN Controller, we utilize ONOS controller and for the container orchestrator we use Kubernetes- In this project we use K3s which is a lightweight of Kubernetes. To emulate the network infrastructure containing switches, we use Mininet which is a powerful Network Emulator supporting Open vSwitch (OVS). To have the hosts (servers) in the network topology, we configure a kubernetese cluster to connect to the Mininet topology by configuring GRE tunneling betwen the Kubernetes nodes and OVS switches in Mininet.

We employ a monitoring tool known as sFlow-RT which is providing a real-time monitoring solution designed to gather telemetry data from industry-standard sFlow Agents integrated into network devices or hosts. We also utilize the open-source Host sFlow agent for performance monitoring of hosts and servers. Moreover, we also utilize standard sFlow agents to monitor Open vSwitches within the Mininet network topology. At the end, to monitor the different microservices (Pods) of the application, we leverage sidecar containers with sFlow agents to operate alongside the main application container within the same Pod. We also utilize the Prometheus Exporter, an application integrated into the sFlow-RT analytics platform to convert real-time telemetry streams from sFlow agents on hosts and switches into metrics accessible via a REST API and a format compatible with Prometheus,enabling Prometheus to retrieve and utilize these metrics. Furthermore, We utilize Prometheus as a robust time series database and alerting system for persistent storage of real-time monitoring data collected by sFlow-RT. Additionally, we leverage Grafana, an open-source analytics and monitoring solution that seamlessly integrates with Prometheus for enhanced visualization. This integration allows us to efficiently collect, query, visualize, and alert on metrics data.

We have automated the entire setup of iContinuum including configuration, application deployment, and monitoring described above using the Ansible platform which is accessible here.

We have created three different sample topologies. The code for the first topology can be found in "Example1"- The code for the second topology can be found in "Example2"- The code for the third topology can be found in "Example3". 

# Please consider the below details regarding the available files in the master branch.

- It contains several yaml files named "onos.yml, mininet.yml, monitor.yml".

- "onos.yml" file has all the configuration for SDN Controller (Network Controller). We call this as onos machine during this tutorial.

- "mininet.yml" file has all the configuration for Network Emulator. We call this as mininet machine during this tutorial.

- "monitor.yml" file has all the configuration for Monitoring tools. We call this as sflow-rt machine during this tutorial.

    - Note1- "kubernetes.yml" file has all the configuration for Kubernetes cluster. This file is available in each Example folder due to different topologies for hosts and switches.
    - Note2- "example.py.j2" file creates a sample topology through Mininet. This file is available in each Example folder due to different topologies for switches.
    - Note3- "inventory.ini" file defines the IP address of all VMs and their specific interface{(tap0)- assigned to kubernetes nodes} and also the server names. This file is available in each Example folder due       to different IP addresses and server name for VMs.
    - Note4- "topology.sh.j2" runs the topology defined on "example.py.j2" on mininet machine. This file is available in each Example folder due to different topologies and configuration.
    - Note5- "deployment.yml.j2" file creates the deployment of the application. This file is available in each Example folder due to different application.

- "main.yml" file has all the above mentioned file inside.

- "ansible.cfg" file disables SSH host key checking and suppresses command warnings in Ansible.

- "prometheus.yml.j2" file generates data in the prometheus format and sends data to sflow-rt.

- "host-sflow.yml.j2" file deploys on the kubernetes cluster to send data from each node to sflow-rt.

=======updated to this part
Note- The emulator has been tested on Ubuntu 20 LTS, so it is recommended to use this version.



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


 -----------------------------------------------------------------------------------------------------------------



 
