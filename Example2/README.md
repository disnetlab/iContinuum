"""Custom topology example2

An example of 3 switch and 3 K3s hosts along with 2 mininet hosts:




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
