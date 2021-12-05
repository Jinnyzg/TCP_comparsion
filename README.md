# TCP_Comparsion

### Introduction
As a reliable end-to-end protocol, TCP provides a reliable data transfer service in the face of packet loss. In this project, we tend to analyze and compare the performance of  four TCP flavors(Reno, Cubic and Vegas), in order to study the advantages and disadvantages of their different implementation mechanisms.

### Tools
* use Geni, an open infrastructure for at-scale networking and distributed systems, to design our network
* use iperf to measure the achievable bandwidth on IP networks
* use Linux Traffic Control (tc) to manage and manipulate thetransmission of packets

### Intructions
1. Set up Resources in Geni with the RSpec provided(txt)
2. log in to all hosts and install iperf using the command `sudo apt-get install iperf`
3. set network condition in delay server using tc
    1. set network delay, bandwidth bound and loss: `sudo tc qdisc add dev eth1 root netem rate 90mbit delay 5ms loss 0.01%`
    2. delete previous set: `sudo tc qdisc del dev eth1 root netem'
    3. view link condition: `sudo tc qdisc show`
5. set TCP version in hosts using `sysctl` 
    1. set sack: `sudo sysctl net.ipv4.tcp_sack=1`
    2. set tcp version: `sudo sysctl net.ipv4.tcp_congestion_control=reno`
6. measure throughput: 
    1. on PC1: `iperf -s`
    2. on PC2-PC5: `iperf -c pc2 -t 50`

