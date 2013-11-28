.. Copyright |copy| 2013 by Justin Vellemans, Florentin Rochet, David Lebrun, Juan Antonio Cordero, Olivier Bonaventure
.. This file is licensed under a `creative commons licence <http://creativecommons.org/licenses/by/3.0/>`_

Local Area Networks: The Spanning Tree Protocol and Virtual LANs
=================================================================

.. warning:: 

   This is an unpolished draft of the second edition of this ebook. If you find any error or have suggestions to improve the text, please create an issue via https://github.com/obonaventure/cnp3/issues?milestone=6

Exercises
---------

1. Consider the switched network shown in Fig. 1. What is the spanning tree that will be computed by 802.1d in this network assuming that all links have a unit cost ? Indicate the state of each port.

  .. figure:: fig/ex-stp-switches.png
     :align: center
     :scale: 100
    
     Fig. 1. A small network composed of Ethernet switches

2. Consider the switched network shown in Fig. 1. In this network, assume that the LAN between switches S3 and S12 fails. How should the switches update their port/address tables after the link failure ?


3. Many enterprise networks are organized with a set of backbone devices interconnected by using a full mesh of links as shown in Fig.2. In this network, what are the benefits and drawbacks of using Ethernet switches and IP routers running OSPF ?

  .. figure:: fig/ex-stp-backbone.png
     :align: center
     :scale: 100

     Fig. 2. A typical enterprise backbone network

4. In the network depicted in Fig. 3, the host `H0` performs a traceroute toward its peer `H1` (designated by its name) through a network composed of switches and routers. Explain precisely the frames, packets, and segments exchanged since the network was turned on. You may assign addresses if you need to.

  .. figure:: fig/ex-stp-switches_vs_routers.png
     :align: center
     :scale: 100

     Fig. 3. Host `H0` performs a traceroute towards its peer `H1` through a network composed of switches and routers

5. In the network represented in Fig. 4, can the host `H0` communicate with `H1` and vice-versa? Explain. Add whatever you need in the network to allow them to communicate.

  .. figure:: fig/ex-stp-routing_across_VLANs.png
     :align: center
     :scale: 100

     Fig. 4. Can `H0` and `H1` communicate ?

6. Consider the network depicted in Fig.5. Both of the hosts `H0` and `H1` have two interfaces: one connected to the switch `S0` and the other one to the switch `S1`. Will the link between `S0` and `S1` ever be used? If so, under which assumptions? Provide a comprehensive answer.

  .. figure:: fig/ex-stp-switches_wo_STP.png
     :align: center
     :scale: 100

7. Most commercial Ethernet switches are able to run the Spanning tree protocol independently on each VLAN. What are the benefits of using per-VLAN spanning trees ?

Netkit STP lab
--------------

In this lab, you can explore the behavior of a network with switches that use the Spanning Tree Protocol (STP). This protocol allows switches to automatically disable ports on Ethernet switches to ensure that the network does not contain any cycle that could cause frames to loop forever.

Here is the topology of the network:

  .. figure:: fig/stp-topology.png
     :align: center
     :scale: 100


To use STP, these switches uses ``brctl``, a tool that allows to make bridges and build the spanning tree.

For this lab, you can use the "Wireshark" tool. It's a packet sniffer (like tcpdump) but it is more convenient to use.

To launch the lab you have to go in the directory of the lab and launch it with netkit using command ``lstart`` (you can add the option -f for a quick launch). 

You can see that the 6 machines are launched. For the moment, no one of them runs STP. You will run it (via the ``brctl`` daemon) on two routers and activate wireshark on one of them as explained above. To activate the STP on one switch type in his terminal:

 .. code:: console

    brctl stp br0 on
    ifconfig br0 up

With these two routers you can see what messages are exchanged for the root bridge election.
You can see the state of a bridge by typing :

 .. code:: console

    brctl showstp br0

This command brings information about the designated root of the tree, the root port of the switch and the cost to the root switch.

Now, we will launch some other switches. By doing that we change the topology. With wireshark you can observe the packets of the spanning tree protocol that are exchanged. The switches already launched will generate a "topology change notification", then others switches will acknowlegde these changes.

When all switches are launched, you can look at the bridge state of each switches: 

 .. code:: console

    brctl showstp br0

You can see wich ports are in blocking state, wich are in forwarding state.

You can also look at the port-station table by entering :

 .. code:: console

    brctl showmacs br0

You can make some links fail and observe what is happening. You can do that by stoping one interface on a switch or the entire bridge (if=br0) :

 .. code:: console

    ifconfig IF down

where IF is the name of your interface.


.. include:: /links.rst
