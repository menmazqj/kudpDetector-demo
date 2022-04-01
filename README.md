# kudpDetector-Demo

### INTRODUCTION

This project origins from an issue in openEuler community (link below). We develop a tool during 2021 Open source Summer Supply Chain.

https://gitee.com/openeuler-competition/summer-2021/issues/I3ESBZ

kudpDetector aims to detect Linux kernel UDP packet loss and delay in real-time to help developers locate problematic kernel functions quickly. For packet loss detection, kudpDetector mainly outputs names and return values of kernel functions where UDP packet loss happens. For UDP packet lelay detection, kudpDetector mainly outputs names and execution time of kernel functions with an abnormally high execution time.

### Prerequisites

All the steps below are performed on openEuler 20.03 LST (linux kernel version 5.10.0) as root user.

kudpDetector leverages *netstat* to show the number of dropped UDP packets, so *net-tools* is required. 

kudpDetecor uses *iperf* as UDP packets sender and receiver.

```shell
dnf -y install net-tools
wget https://iperf.fr/download/fedora/iperf-2.0.8-2.fc23.x86_64.rpm
rpm -ivh iperf-2.0.8-2.fc23.x86_64.rpm
```

### USAGE

To start use kudpDetector, executes script
```shell
./kudpDetector.sh -h
```
Executes *kudpDetector.sh* with *-l* parameter to start packet loss detection

```shell
./kudpDetector.sh -l
```

Executes *kudpDetector.sh* with *-d* parameter to start function delay detection. Default thresh of kernel execution time is 10000(us)

```shell
./kudpDetector.sh -d
```

### SIMULATIONS

##### UDP packet loss scenario 1 - default firewall rules

Default firewall policy will block UDP traffic 

##### UDP packet loss scenario 2 - fulfill receive buffer

Setup a UDP server using *iperf*

```shell
iperf -s -u
```

Setup a UDP client using *iperf* (parameter *-c* specifies UDP server IP address, if you want to test on your machine, modify it to your UDP server IP address)

```
iperf -c 192.168.226.128 -u -b 10000M -P 4
```

Open a new terminal, execute *kudpDetector.sh* to see the results

```shell
./kudpDetector.sh -l
```

##### UDP packet loss scenario 3 - reverse path filtering

Reverse path filtering is a firewall behavior to drop packets routed back to their incoming network interface. See [*rp_filter and LPIC-3 Linux Security*](https://www.theurbanpenguin.com/rp_filter-and-lpic-3-linux-security/) to setup the packet loss scenario.(or [Chinese version](https://www.tinymema.cn))

##### UDP function delay scenario 1 - tc

In *scripts* directory, execute *tc_delay.sh* to setup UDP function delay scenario(Default delay is 10000us, if your want to test on your machine, modify *IF* and *DST_CIDR* variables to make them corresponds to your machine's configurations)

```
tc_delay.sh
```

