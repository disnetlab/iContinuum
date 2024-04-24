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


