# TCP_Comparsion

### Introduction
As a reliable end-to-end protocol, TCP provides a reliable data transfer service in the face of packet loss. In this project, we tend to analyze and compare the performance of  four TCP flavors(Reno, Cubic and Vegas), in order to study the advantages and disadvantages of their different implementation mechanisms.

### Tools
* use Geni, an open infrastructure for at-scale networking and distributed systems, to design our network
* use iperf to measure the achievable bandwidth on IP networks
* use Linux Traffic Control (tc) to manage and manipulate thetransmission of packets

### Intructions
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
    Basic performance - one flow - one server node & one client nodes
    1. no loss no delay in the client node
    2. 0.1% loss in the client node
    3. 5ms delay in the client node
    4. 0.1% loss and 5ms delay in the client node

    Fairness performance - two flows - one server node & two client nodes
    1. no delay and no loss in both clients
    2. 5ms delay in client1 and 10ms delay in client2
    3. 0.1% loss in client1 and 1% loss in client2
    4. 5ms delay and 0.1% loss in client1

(c) Competing traffic - two flows
Three of the nodes are employed: one as the server and the other two as competing clients. The whole link bandwidth serving all clients is set to be 90Mbps. Client 2 is set to be receiving at 50Mbps. Client 1 is constantly receiving at a certain rate (30Mbps, 40Mbps, and 50Mbps). After 100 seconds, client 2 joins the network and starts to transmit more files. We want to look at if the second client is going to affect the packet delays of client 1.

(d) Competing traffic - three flow
In order to study fairness among three TCP flavors, we observe how clients using different TCP versions share the same link.


