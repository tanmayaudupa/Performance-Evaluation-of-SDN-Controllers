# Performance-Evaluation-of-SDN-Controllers

## Introduction 

Traditional networks suffer from scalability issues. Internet Service Providers and network backbones are experiencing issues like traffic congestion, lengthy installation periods, and prolonged network convergence times as a result of the exponential growth in internet traffic. With these problems in mind, Software Defined Networks have recently gained ground and appeal. Software Defined Networks allow us to separate the control plane and the data plane. The control plane is then utilized as a centralized entity that controls the underlying network resources. With this approach, network administrators may simply change network resources and policies in response to shifting traffic demands and priorities and have greater control over network operations. After understanding the flexibility offered by SDNs in traffic management and flow management of networks, the networking industry and academia have started using SDNs for building large and complex networking systems. 

An SDN controller is the fundamental and core element of SDN architecture. They are called the brains of the network since they have a complete view of the underlying network topology, network devices, and how the traffic flows in the network. It is the medium that connects the application to the underlying network. SDN controllers allow us to define policies that help in automated network management. Based on the policies defined by the network operators, the data plane can be programmed to perform a particular function through protocols like OpenFlow.  As new requirements emerged for various applications, there was an increase in the development of controllers to meet these requirements. This made it necessary to compare these controllers on both a qualitative and quantitative level. Engineers and researchers would be able to choose controllers more effectively depending on the requirements at hand with the help of a clear comparison between the various controllers. 

This study compares five distinct SDN controllers—RYU, POX, Opendaylight, Floodlight, and ONOS—in depth on both a qualitative and quantitative level. Using benchmarking tools like Cbench and Iperf, these controllers are compared in terms of metrics like controller latency and throughput, node-to-node latency, throughput, and round-trip timings. The qualitative comparisons were made based on the programming language used, the architecture of the controller, the ability to support multithreading, and the platforms that are supported by the controllers. There are other numerous controllers present, but the scope of this paper will be limited to the above-mentioned controllers.

## SDN CONTROLLERS

![SDN_controller](https://user-images.githubusercontent.com/30312503/206814229-e917ed08-6883-49ce-8958-fa69c71109a0.png)

The above figure shows a simple architecture of Software Defined Networks. The controller is like the medium between the user applications and the underlying network devices. The architecture shown here is general architecture. The architecture of each of the SDN controllers may differ from the others. SDN Controllers have 2 main components, the controller core, and the controller interfaces. The core contains different modules like topology manager, link discovery manager, flow manager, etc. These modules are used for managing traffic and maintaining the topology-related information of the underlying network.  The topology manager has the topology map of the network and uses algorithms to find optimal paths between nodes and find routes for efficient traffic management. In some controllers, it also helps in visualizing the topology map. This helps network administrators to understand and get a view of how the network is built. The link discovery module helps in rebuilding the topology map when a network device is connected or disconnected from the network. The flow manager interacts with the network devices flows and the flow tables. The controller interfaces are used to communicate with the applications and the underlying network devices. The southbound interfaces of the controller are used as a communication channel to send packets and receive packets to and from the network devices. Openflow is the most commonly used Southbound interface (SBI). The East Bound interfaces and West Bound interfaces are used for inter controller communications and the Northbound Interfaces are used to communicate with the user applications. We considered the top 5 most popular controllers in the market for our project namely Ryu, Floodlight, ONOS, POX and OpenDayLight.

## Benchmarking Tools
We have used Cbench, iperf and MTCbench for analysis. We measured Latency, throughput and other parameters using them.

Cbench is a tool for measuring the performance of network controllers. It was developed by the Open Networking Foundation to help network administrators evaluate the capabilities of different OpenFlow switches and identify potential bottlenecks in their networks. Cbench can simulate various traffic patterns and network scenarios, allowing administrators to test the performance of a network controller under different conditions. This can help them optimize their network for specific applications and workloads.

Iperf is a network performance testing tool that is commonly used to measure the bandwidth and throughput of a network connection. It works by transmitting a specified amount of data over the network and measuring the time it takes to complete the transfer, allowing users to calculate the network's throughput. iperf can be run in either client or server mode, allowing users to test network performance between two points on a network. It is a useful tool for network administrators and engineers who need to evaluate the performance of a network and identify potential bottlenecks or other issues.

In addition to measuring bandwidth and throughput, iperf can also be used to test other aspects of network performance, such as jitter and packet loss. It supports a range of options and configuration settings, allowing users to customize their tests to simulate specific network conditions and scenarios. iperf is available for a wide range of operating systems, including Windows, Linux, and macOS, and it is free and open-source software. It is commonly used in conjunction with other network performance tools, such as ping and traceroute, to diagnose and troubleshoot network issues.

## EXPERIMENTAL SETUP

In our study, we have used mininet for building network topologies. Mininet is basically a network emulator which emulates network devices such as switches, hosts and the links between them. Mininet allows us to create different network topologies such as linear, single, reversed, tree, torus and also custom topologies. The hosts mininet creates are standard Linux based hosts and the switches have support for OpenFlow allowing controllers to program these switches. The controllers were run on Amazon EC2 t2.large instance with intel Xeon processor and had 2 CPU cores. We used l2 learning switches for our experiments. Benchmarking tools like Cbench and MtCbench (for multithreading experiments) were installed on the EC2 instance and interacted with the controller for determining the performance metrics.

## RESULTS AND DISCUSSION

insert table here

We present the observations using graphical representation of our
results and further it with some inferences we obtained. Table above
shows the qualitative comparisons between the controllers.

### Cbench Latency Mode

Latency of the controller denotes the time it takes to process a single packet. We tested the controller for latency performance. For this metric, we ran Cbench in latency mode which sends a packets to the controller and waits for response before sending another packet. We evaluated the controller swith different number of virtual OpenFlow switches emulated by Cbench. Each latency test was run for 10 iterations.

![Cbench Latency Mode (3)](https://user-images.githubusercontent.com/30312503/206818071-774b67cd-3daa-4523-b858-382695d5d1eb.png)

Figure above shows the results obtained using Cbench tool's latency mode. Cbench latency identifies the communication delay between the controller and the switch. Here every switch is directly connected to the controller. In Cbench latency mode all the switches send packets to the controller in a serial manner and wait for its reply before sending the next packet. The reciprocal of the average number of flows processed by the controller gives the Cbench latency metric. For this particular experiment, we kept the number of test runs within the Cbench latency mode constant at 10 across all the controllers.

Following were our observations regarding different controllers based on Cbench latency mode results:
* We observed that the POX controller shows a minimal change in latency as the number of switches increases. However, less latency does not translate to an outright winner as the capabilities of the controller itself must be considered.
* For OpenDayLight, latency increases fairly and linearly with the number of switches.
* Latency was barely affected for the Ryu controller. It remained constant throughout the experiment.
* Floodlight and ONOS controllers showed fluctuating performance where after 45 switches, Floodlight showed a linear increase in latency whereas ONOS showed a linear decrease in latency.

### Cbench Throughput Mode

In throughput mode, each switch sends as many packets as the switches' buffer allows to the controller without waiting for a reply. This is done because we essentially want to measure how many packets can be handled/processed by the controller at once per switch. Thus, the throughput mode measures the maximum flow rate that a controller can handle. In Cbench Throughput mode there are 10 thousand hosts connected to each switch. The figure below shows the result for the same.

![Cbench - throughput (1)](https://user-images.githubusercontent.com/30312503/206818086-7e654f29-4bec-4fab-98d7-2f5140ecbdbc.png)


We present our results from the Cbench throughput mode in Fig \ref{cthroughmode} and list the observations.
* Ryu and OpenDayLight were consistently the lowest performers. This was contrary to the results we expected, both Ryu and OpenDaylight because both of these controllers support multithreading. 
* Floodlight showed increased throughput with an increased number of switches indicating that it might be suitable for tasks that would want to maintain their throughput rates for large-scale networks.
* Pox surprisingly showed fairly constant throughput rates. This might be true for small networks however we might need additional experiments to verify POX's performance in huge networks.

### Node to Node Latency Mode
Node-to-node latency measures the amount of time required to send a packet and receive a reply between two hosts that are connected to L2-learning switches which are in turn connected to the controller. In our graphs, we present statistics gathered using the \textbf{Iperf} benchmarking tool as well as Ping statistics.
In this experiment we tried to ping the farthest hosts within a network to get the worst possible ping time in that network, and tested this setting on different network topologies. We used TCP packets to ping the hosts. 

We conducted 15 test rounds of pings between hosts, where the packet size was fixed to the default ping packet size. The results presented here include the ping time averaged over these 15 tests. Please note that the first ping between any two hosts for any of the controllers showed the highest ping time. Below figure shows the node to node latency of different controllers.

![Node to Node Latency (3)](https://user-images.githubusercontent.com/30312503/206818118-a331609d-3f83-4423-b2c4-a59efcaba232.png)

   
* Lower latency means better performance.
* ONOS gives the least latency, however, its performance oscillates between different topologies.
* Floodlight and POX have stable latency times across all the topologies for a fixed number of hosts. This can be attributed to the simpler designs of these controllers however a simpler design does not necessarily translate to better controller capabilities.
* We believe commenting on OpenDayLight and Ryu performances for these topologies wouldn't be apt at this time since we might need additional experimental analysis with huge networks to actually evaluate their performance. Both of these controllers support multi-threading and are hence better suited in distributed networks.
* Single and Minimal topologies show similar performance trends.

### Node to Node Throughput Mode

In Node to Node throughput mode, we measure the maximum bandwidth utilized between 2 hosts using the Iperf tool. Iperf provides results in the form of "Interval", "Transfer Size" and "Bandwidth". Interval represents the time duration for which data was transferred between two hosts. Transfer Size specifies the amount of data to be transfered in Mega bytes and the Bandwidth specifies the number of Mega bits transfered per second.
For the node-to-node throughput, we did our Iperf tests on TCP flows. 
We capture our results and visualize them in the below Figure.

![Node to Node throughput (2)](https://user-images.githubusercontent.com/30312503/206818140-9ad54a82-1e9e-4318-9f8b-20983b66a7a0.png)


Our observations are as follows:
* Ryu was consistently the best performer out of all the controllers across all topologies.
* Following Ryu, ONOS, POX and Floodlight gave a stable throughput and did not show high variation.
* Opendaylight had the worst throughput out of all the controllers.
* For tree topology all the controllers performed fairly the same.
*  Single and Linear topologies displayed similar performance trends for all the controllers.
* OpenDayLight had the worst performance in Torus topology.
 
 ### Jitter
 
For understanding what jitter is we need to understand the formal definition of Delay. \textit{Delay} is the difference between the transmission time from the sender and the reception time on the receiver. \textit{Jitter} on the other hand expresses the variation in Delay on packet flow between two To evaluate jitter, we used Iperf tests across topologies in the UDP mode. 

![Jitter (1)](https://user-images.githubusercontent.com/30312503/206818170-e7e8b527-3d6e-4a16-bb34-bc3cc3d604c6.png)

Figure above summarizes our results for Jitter analysis
* Ryu has the least jitter out of all the controllers under consideration, aka it has a fairly consistent delay.
* ODL has fluctuating jitter while ONOS and POX have minimal jitter variation across topologies.

 ### Throughput for Multithreaded Controllers
 
We also extended our research by testing the effects of multithreading on controller throughput. We observed that Ryu and Opendaylight have better throughput with increasing number of threads. However, Floodlight's performance drastically decreases with increasing thread. This result is contrary to the general behavior of this controller hence there is a  need for more experimental analysis since this observation deviates from our original hypothesis. The below figure shows the performance of the controllers when multithreading is enabled.

![Throughput - Multithreaded (1)](https://user-images.githubusercontent.com/30312503/206818240-de2bc84b-ebcd-424a-a4fd-56e8f58ecff5.png)


### Inferences

* Multi-threaded controllers perform significantly better than single-threaded controllers like (POX and RYU).
* Topology has an impact on the performance metrics and, efficient handling of switches is characteristic of every controller.
* Placement of the controller in physical topologies greatly affects the evaluation of performance parameters. We plan to include our studies on different data center topologies like Fat and Short trees with many devices.
* Limitations of evaluation tools directly affect benchmarking results. Changes in underlying physical resources or compilers may have a noticeable impact on the results collected.
* Isolating the performance of a controller from its results is not possible since the features of the tools available greatly impact the performance of the controller in a given topology.
* Simple controllers are better suited for lightweight tasks. Feature-based controllers are better suited for heavy performance tasks.
* Comparing controllers is use-case and application-dependent, simply comparing them on performance metrics would not be enough.
* Extending topologies with many hosts is necessary.
*  All controllers should be installed directly onto the machine, using docker containers or emulated controllers can directly affect the results that are collected.
 
 ### Conclusions
 
From our project we have concluded that overall, all the controllers
performed differently for different topologies. We have inferred
that Multi-threaded controllers perform significantly better than
single-threaded controllers like (POX and RYU). Different topolo-
gies also has an impact on the performance metrics and, efficient
handling of switches is characteristic to every controller. And our
inference is that different controllers can be used in different use
cases depending on the task at hand such as simple controllers are
better suited for lightweight tasks and Feature based controllers
are better suited for heavy performance tasks
