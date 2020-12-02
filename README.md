
# RIoTMAN2020
RIoTMAN: A Systematic Analysis of IoT Malware Behavior


## Requesting Data
To request data please contact Ahmad Darki at adark001[at]ucr[dot]edu and include your answer to the following questions:

 1. Number of malware execution samples
 2. Duration of each execution (This can be between 1-20 minutes)
 3. Number and type of C&C commands or DDoS attacks

For a reference sample of traffic, you can download [this](https://www.cs.ucr.edu/~adark001/riotman/sample_ddos_traffic.tar.gz) which contains data from executing one malware and collecting its traffic performing 4 different DDoS attacks.

If you wish to use the dataset or this project please consider citing our paper below:

```
@article{darki2020riotman,
  title={RIoTMAN: A Systematic Analysis of IoT Malware Behavior},
  author={Darki,Ahmad and Faloutsos, Michalis},
  journal={Proceedings of the 16th International Conference on emerging Networking EXperiments and Technologies},
  year={2020}
}
```

## Introduction
RIoTMAN is an IoT malware analysis that iteratively adapts to a given IoT malware's requirement and executes it as well as impersonating its C&C server. This is an academic project and aim to help the community to have access to malware dataset.



## Installation

Due to size limitation in committing files to Github we instead made a tar file that includes the core of RIoTMAN and is available [here](https://www.cs.ucr.edu/~adark001/riotman/riotman.tar.gz).
Before executing the code please follow the following steps.


### Configuration
In our experimental setup we used Debian8 as the host and installed all `Qemu`'s requirement. In this repository, you can find our version of Qemu for MIPS CPU architectures emulation (other architectures are available upon request). In addition to that under kernel you can find our version of Openwrt Linux kernel as well as a 2GB filesystem which will host configuration files and monitoring capabilities.


### Network configuration
Make sure the host is disconnected from any network that may impose threat to any of your devices or other servers. In our experimental setup we have a dedicated server disconnected from the rest of the network.
Install the `dnsmasq` and add the following to its configuration using `/etc/dnsmasq.conf`  file:

```
interface=br-wan
dhcp-range=192.168.0.2,192.168.255.254,255.255.0.0,30m
```

This is required as it will enable the instances to get an IP address assigned to them using the DHCP service.


Install cowrie from https://github.com/cowrie/cowrie as well as nginx on the host machine.

**Note:** Please note that RIoTMAN adds and removes many `iptable` rules which may include redirecting traffic based on different port. This may interfere with services such as HTTP:80 or SSH:22 for accessing the server.

### Experiment
Place the malware samples under `malware/malware` folder. In file `run_riotman.py` the following variables define the execution time for each experiment as well as the number of instances to run in parallel:

```
PARALLEL_INSTANCES = 24
EXPERIMENT_TIME = 1300
```

In addition, in file `manager/Manager.py` maximum number of iterations can be determined:

```
ALLOWED_MAX_ITERATION = 8
```


### Firmware configuration
In folder `firmware` there are two folders MIPS and ARM which hold the configuration files collected from different firmware blobs and categorized based on the firmware name and their architecture. The database `firmware_mips.db` is used to help RIoTMAN locate these files.


## Output
The analysis results will be available in folder `analysis/result` which will include a folder by the name of the binary and numbered subfolders indicating the iteration number. Each folder includes network traffic collected from the execution of the malware.
