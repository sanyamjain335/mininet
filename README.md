# SDN-Load Balancer using Dijkstra Algorithm

### About SDN:-

According to the classical definition, SDN is an approach to networking that enables the programmatic and dynamic control of a network. This kind of an approach is more congruent with cloud computing than traditional rigid networking. One of the drivers behind SDN is a desire to transition from an old networking mindset to the new agile and more flexible approach so often used in software development practices. When implementing SDN in practice, automation and flexibility are key concepts.

### Dijkstra Algorithm:-

Dijkstra's algorithm is an algorithm for finding the shortest paths between nodes in a graph, which may represent, for example, road networks. It was conceived by computer scientist Edsger W. Dijkstra in 1956 and published three years later.

The algorithm exists in many variants. Dijkstra's original algorithm found the shortest path between two given nodes, but a more common variant fixes a single node as the "source" node and finds shortest paths from the source to all other nodes in the graph, producing a shortest-path tree.

For a given source node in the graph, the algorithm finds the shortest path between that node and every other. It can also be used for finding the shortest paths from a single node to a single destination node by stopping the algorithm once the shortest path to the destination node has been determined. For example, if the nodes of the graph represent cities and edge path costs represent driving distances between pairs of cities connected by a direct road (for simplicity, ignore red lights, stop signs, toll roads and other obstructions), Dijkstra's algorithm can be used to find the shortest route between one city and all other cities. A widely used application of shortest path algorithms is network routing protocols, most notably IS-IS (Intermediate System to Intermediate System) and Open Shortest Path First (OSPF). It is also employed as a subroutine in other algorithms such as Johnson's.

### Tools used

#### 1. Floodlight

Floodlight Floodlight is a java-based OpenSDN controller, which works with the OpenFlow protocol to develop a SDN environment. OpenFlow defines a communication protocol using which the control plane (SDN controller) talks to the forwarding plane (switches and routers) to modify and push rules in the network.We have used the Floodlight controller due to its modular design and the ease to use REST APIs to develop an application. The floodlight website offers a variety of REST APIs, which are highly adaptable and gives access to a variety of modules used to make our load balancing application.

#### 2. Mininet

Mininet is a linux-based network emulator, which builds a virtualized software network.Mininet can be used to quickly create a virtual network running actual kernel in the devices and switches and, software application code on a personal computer. We use mininet for building our topology,which emulates a real scenario. Using this tool, we are able to create different custom topologies by running python scripts and choosing the most suitable topology. Moreover, mininet boots up quickly and has a small footprint. This helps in its efficiency.

#### 3. Iperf

In this project we use Iperf to create a data-stream of ICMP packets to analyze the bandwidth between the two selected hosts. Beyond this, one can create TCP or UDP packets to measure the throughput and losses using Iperf network testing tool. It allows the user to optimize or test a network by setting various parameters like datagram size.Typical Iperf output contains a time-stamped report of the amount of data transferred and the throughput measured. We have used Iperf to ping the data from one host to another and in return we obtain the bandwidth with a timestamp attached to it.

#### 4. Dijkstras Algorithm and Networkx Dijkstra’s Algorithm

Dijkstras Algorithm and Networkx Dijkstra’s Algorithm is used to find the shortest path between two nodes. In this application, a graph is formed with help of networkx library in python. The nodes represent the vertices of this graph and the links from nodes to other nodes represent the edges of the graph. Based on this graph, Dijkstra’s Algorithm is used to source node and find the shortest path to every node from this source. Thus, we obtain a shortest path tree for the entire network and we can hence choose the shortest path from one segment of the topology. This is used to calculate the transmission rates, which gives the link cost. With the help of all of these decisions, the best path is obtained.

### Implementation details

1. Start the floodlight controller using `cd floodlight` and `java -jar target/floodlight.jar`
2. Run custom mininet topology using and connect it to the remote controller `sudo mn --custom Topo.py --topo mytopo --controller=remote,ip=127.0.0.1,port=6653`
3. Run it until you get `0% packet loss` on `pingall` command
    ![img](https://github.com/sanyamjain335/mininet/blob/master/Screenshots/Screenshot%20from%202021-11-03%2006_02_44.png?raw=true)
4. Run `xterm h1 h1`
    ![img](https://github.com/sanyamjain335/mininet/blob/master/Screenshots/Screenshot%20from%202021-11-03%2006_03_47.png?raw=true)
5. On first h1 ping `10.0.0.3` and on second h1 ping `10.0.0.4` and check whether packets are reaching or not.
    ![img](https://github.com/sanyamjain335/mininet/blob/master/Screenshots/Screenshot%20from%202021-11-03%2006_04_11.png?raw=true)
6. On Terminal, open a new tab `Ctrl + Shift + T` and type `sudo wireshark`
7. Check the packets being captured on `ip.addr==10.0.0.3` and `ip.addr==10.0.0.4`
    ![img](https://github.com/sanyamjain335/mininet/blob/master/Screenshots/Screenshot%20from%202021-11-03%2006_17_40.png?raw=true)
    ![img](https://github.com/sanyamjain335/mininet/blob/master/Screenshots/Screenshot%20from%202021-11-03%2006_17_56.png?raw=true)
8. Now run load balancer script using `python3 FinalCode.py`
    ![img](https://github.com/sanyamjain335/mininet/blob/master/Screenshots/Screenshot%20from%202021-11-03%2006_07_26.png?raw=true)
    ![img](https://github.com/sanyamjain335/mininet/blob/master/Screenshots/Screenshot%20from%202021-11-03%2006_07_34.png?raw=true)
    ![img](https://github.com/sanyamjain335/mininet/blob/master/Screenshots/Screenshot%20from%202021-11-03%2006_07_55.png?raw=true)
    ![img](https://github.com/sanyamjain335/mininet/blob/master/Screenshots/Screenshot%20from%202021-11-03%2006_08_07.png?raw=true)
    ![img](https://github.com/sanyamjain335/mininet/blob/master/Screenshots/Screenshot%20from%202021-11-03%2006_08_16.png?raw=true)
9. The loadbalancer script performs REST requests, so initially the link costs will be 0. Re-run the script few times. This may range from 1-10 times. This is because statistics need to be enabled. After enabling statistics, it takes some time to gather information. Once it starts updating the transmission rates, you will get the best path and the flows for best path will be statically pushed to all the switches in the new best route. Here the best route is for h1->h4 and vice versa.
10. Finally you will be able to see Loadbalancer script fetching you the best possible path from source to destination.

