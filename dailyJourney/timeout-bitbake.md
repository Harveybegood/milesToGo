Run into errors as like 
```
NOTE: Executing Tasks
NOTE: No reply from server in 30s
Timeout while waiting for a reply from the bitbake server (60s)
```
This error seldomly happens, as my experience, it will happen when I terminte a bitbake execution, then immediately run bitbake again.  
At this time,  there is same problem as above from another guy I see, but his build host is WSL v2 and with below BUild Configuration:   
```
Build Configuration:
BB_VERSION = "2.4.0"
BUILD_SYS = "x86_64-linux"
NATIVELSBSTRING = "ubuntu-22.04"
```
The causes to such issue probably like:   
1. the bandwidth to network and its stablity.
2. disk IO peroformance.
3. the ratio of memory and cpu cores, which should be a match of 4/1. memory 4GB against cpu 1 core.
4. processes related to bitbake not properly handled being in a pranoid mode.

To fix:
1. firtly should try to terminate all processes related to bitbake, to restart host machine is a lazy but effective way.
2. To config max tasks with BB_NUMBER_THREADS and PARALLEL_MAKE in conf/local.conf accordingly. as mentioned, the ration of memory sources and CPU cores.

extentions:
1. during researching, see some suggestions from internet, like 5.123 rm_workwhich will be put in local.conf. The disk space can be a possiblity for this error. In my opinion, it should be in high priority for troubelshooting. (https://docs.yoctoproject.org/ref-manual/classes.html#rm-work)[https://docs.yoctoproject.org/ref-manual/classes.html#rm-work]
2.  can take a glance to the log /tmp/log/../../*.log if there are some findings.

