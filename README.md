# This is an Emulation toolkit suitable for edge-to-cloud environment automated by Ansible.

#It has three different sample topologies which are shown below. The code for the first topology can be found in "Example1"- The code for the second topology can be found in "Example2"- The code for the third topology can be found in "Example3".

#Based on the selected topology, please select the desired folder among the three folders and follwo the "README.mn" file in each folder.

#The emulator is developed by two main technologies: SDN Controller, Container Orchestrator.

#Mininet is used to emulate the below network topologies.

#In the topologies, some of the hosts are from Mininet emulator in order to make sure we have the connectivity, and the rest of the hosts are part of the Kubernetes cluster acting as the container orchestrator tool.

#k3s host in the topologies are the Kubernetes nodes attached to the topology by developing GRE Tunneling. 

Note- K3s is a lightweight of Kubernetes.



                                                     "Custom topology example1"

                                  An example of 2 switches and 2 K3s hosts along with 2 mininet hosts
                                  

                                                       k3s host    k3s host
                                                           |          |
                                                           |          |
                                                           |          |
                                       mininet host --- switch --- switch --- mininet host


-----------------------------------------------------------------------------------------------------------------

                                
                                                     """Custom topology example2"

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


                                                     """Custom topology example3

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



 
