# TCP_Comparsion

### Introduction
As a reliable end-to-end protocol, TCP provides a reliable data transfer service in the face of packet loss. In this project, we tend to analyze and compare the performance of  four TCP flavors(Reno, Cubic and Vegas), in order to study the advantages and disadvantages of their different implementation mechanisms.

### Tools
* use Geni, an open infrastructure for at-scale networking and distributed systems, to design our network
* use iperf to measure the achievable bandwidth on IP networks
* use Linux Traffic Control (tc) to manage and manipulate thetransmission of packets

### Command Line Usage
1. Set up Resources in Geni with the RSpec provided (Rsepc.txt)
2. log in to all hosts and install iperf using the command `sudo apt-get install iperf`
3. set network condition in delay server using tc
    1. set network delay, bandwidth bound and loss, for example `sudo tc qdisc add dev eth1 root netem rate 90mbit delay 5ms loss 0.01%`
    2. delete previous set: `sudo tc qdisc del dev eth1 root netem`
    3. view link condition: `sudo tc qdisc show`
5. set TCP version in hosts using `sysctl` 
    1. set sack: `sudo sysctl net.ipv4.tcp_sack=1`
    2. set tcp version, for example `sudo sysctl net.ipv4.tcp_congestion_control=reno`
6. measure throughput: 
    1. on PC1: `iperf -s`
    2. on PC2-PC5, for example `iperf -c pc2 -t 50`.

### Instructions - experimenal design
1. Basic performance - one flow - one server node & one client nodes `iperf -c pc2 -t 50`
    1. no loss no delay in the client node
    2. 0.1% loss in the client node
    3. 5ms delay in the client node
    4. 0.1% loss and 5ms delay in the client node

2. Fairness performance - two flows - one server node & two client nodes `iperf -c pc2 -t 50`
    1. no delay and no loss in both clients
    2. 5ms delay in client1 and 10ms delay in client2
    3. 0.1% loss in client1 and 1% loss in client2
    4. 5ms delay and 0.1% loss in client1

3. Competing traffic - two flows - one server node & two client nodes
    1. set client2 bandwidth to be 50Mbps
    2. set client1 banndwidth to be 30Mbps, 40Mbps and 50Mbps
    4. start client1 transmission for 300s `iperf -c pc2 -t 300`
    5. logon to client1 on a new terminal and start ping from client1 to the server `ping pc2 | tee out.txt`
    6. wait 100 seconds, start transmission of client2 for 120s `iperf -c pc2 -t 120`
 
4. Competing traffic - three flow - one server node & three client nodes `iperf -c pc2 -t 50`
    1. one client use Vegas, the other two use Reno
    2. one client use Cubic, the other two use Reno
    3. one use Cubic, one use Vegas, one use Reno
 



