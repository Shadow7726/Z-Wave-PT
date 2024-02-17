# Z-Wave Overview

Here is an overview of core Z-Wave concepts from basic to advanced:

**Basic Concepts**

- Z-Wave operates in the sub-GHz frequency range around 900MHz for longer range.

- It uses a low-power and low data rate wireless protocol designed for home automation. 

- Z-Wave networks consist of controllers and nodes - controllers manage the network while nodes are end devices.

- Nodes can be battery powered and support mesh networking with other nodes to route signals.

- Z-Wave uses ITU-T G.9959 specification with 9600bps data rate and FSK modulation.

**Networking** 

- Z-Wave networks have a 32-bit home ID which identifies the network. All devices share the home ID.

- Controllers assign a node ID from 1 to 232 to each node during inclusion.

- Nodes can be assigned to different groups for targeted control. Group information is shared across the network.

- Z-Wave uses source routing where the controller sets the route for a command through nodes.

**Security**

- Z-Wave uses AES-128 encryption. A network-wide security key encrypts broadcast traffic.

- Devices also have unique security keys to encrypt unicast traffic. Keys are exchanged securely during inclusion.  

- A 24-bit Network PIN is used during inclusion to authenticate new devices to the network.

- Inclusion involves both PIN authentication and key exchange to add nodes securely.

**Command Classes**

- Z-Wave uses command classes to define device functions like battery level, door lock, sensor alerts etc.

- Common command classes include BASIC, ASSOCIATION, SECURITY, TRANSPORT, APPLICATION.

- Each command class has a defined command set to control devices.

**Advanced Features** 

- Z-Wave Plus certification program that requires devices to implement security and smart routing.

- Support for OTA firmware updates, remote diagnostics, interference mitigation.

- Interoperability with IP networks via gateway controllers.

- Backward compatibility with legacy Z-Wave devices.



**Z-Wave Features**

| Specification          | Z-Wave Support                |
|------------------------|-------------------------------|
| Standard               | ITU-T G.9959 (PHY and MAC)   |
| RF Frequency Range     | 868.42 MHz in Europe, 908.42 MHz in US |
| Data Rate              | 9.6, 40, 100 Kbps            |
| Maximum Nodes          | 232                           |
| Architecture           | Master and slave in mesh mode |
| MAC layer              | CSMA/CA                       |
| RF PHY modulation      | FSK (for 9.6kbps and 40 kbps), GFSK with BT=0.6 (for 100 kbps) |
| Coding                 | Manchester (for 9.6kbps), NRZ (for 40 and 100 kbps) |
| Distance               | 30 meters indoors, 100 meters outdoors |

**Z-Wave Frequency Bands**

| Region          | RF Center Frequency (G.9959/MHz) | Data Rate   | Channel Width   |
|-----------------|----------------------------------|-------------|-----------------|
| Australia       | fANZ1/919.80, fANZ2/921.40      | 100/40/9.6 Kbps | 400/300/300 KHz |
| Brazil          | Same as Australia                | -           | -               |
| Canada          | Same as USA                      | -           | -               |
| Chile           | Same as USA                      | -           | -               |
| China           | fCN1/868.40                      | 100/40/9.6 Kbps | 400/300/300 KHz |
| European Union  | fEU1/869.85, fEU2/868.40        | 100/40/9.6 Kbps | 400/300/300 KHz |
| Hong Kong       | fHK1/919.80                      | 100/40/9.6 Kbps | 400/300/300 KHz |
| India           | fIN1/865.20                      | 100/40/9.6 Kbps | 400/300/300 KHz |
| Israel          | fIL1/916.00                      | 100/40/9.6 Kbps | 400/300/300 KHz |
| Japan           | fJP1/922.50, fJP2/923.90, fJP3/926.30 | 100 Kbps for all bands | 400 KHz for all bands |
| Korea           | fKR1/920.90, fKR2/921.70, fKR3/923.10 | 100 Kbps for all bands | 400 KHz for all bands |
| Malaysia        | fMY1/868.10                      | 100/40/9.6 Kbps | 400/300/300 KHz |
| Mexico          | Same as USA                      | -           | -               |
| New Zealand     | Same as Australia                | -           | -               |
| Russia          | fRU1/869.00                      | 100/40/9.6 Kbps | 400/300/300 KHz |
| Singapore       | Same as EU                       | -           | -               |
| South Africa    | Same as EU                       | -           | -               |
| Taiwan          | Same as Japan                    | -           | -               |
| UAE             | Same as EU                       | -           | -               |
| USA             | fUS1/916.00, fUS2/908.40        | 100/40/9.6 Kbps | 400/300/300 KHz |


# Z-Wave Protocol Stack

This page on Z-Wave protocol stack covers basics of Z-Wave protocol layers. The stack covers Z-Wave PHY, MAC, transport, network, and application layers.

The Z-Wave protocol layers' main function is to communicate very short messages of few bytes long from a control unit to one or more Z-Wave nodes. It is a low bandwidth and half duplex protocol to establish reliable wireless communication. Z-Wave protocol stack need not have to take care of large amount of data as well as any kind of time critical or streaming data.

As shown in the fig-1, Z-Wave protocol stack consists of 5 layers viz. PHY layer, MAC layer, Transport layer, network layer, and application layer. The security layer is not defined in Z-Wave open protocol specifications and hence it is implementation specific. Following are the major functions of these protocol layers.

- Physical layer takes care of modulation and RF channel assignment as well as preamble addition at the transmitter and synchronization at the receiver using preamble.
- MAC layer takes care of HomeID and NodeID, controls the medium between nodes based on collision avoidance algorithm and backoff algorithm.
- Transport layer takes care of transmission and reception of frames, takes care of retransmission, ACK frame transmission and insertion of checksum.
- Network layer takes care of frame routing, topology scan, and routing table updates.
- Application layer takes care of control of payloads in the frames received or to be transmitted.

## Z-Wave Protocol Stack

The fig-2 depicts fields at various protocol layers of Z-Wave stack. The figure mentions PHY/MAC frame, 4 types of frames at transport layer, and application layer frame. Before we move to understand different protocol layers go through the concept of Z-Wave network in Z-Wave Tutorial page.

### Z-Wave Frame Types

#### Z-Wave Physical Layer

The physical layer in Z-Wave does many functions. The important ones are modulation and coding as well as insertion of known pattern ('preamble') used for synchronization at receiver. It also takes care of RF channel allocation as desired. The input for configuring the Z-Wave PHY is data rate (9.6 or 40 or 100 Kbps).

#### Z-Wave MAC Layer

MAC layer, as the name suggests, takes care of medium access control among slave nodes based on collision avoidance and backoff algorithms. It takes care of network operation based on HomeID, NodeID, and other parameters in the Z-Wave frame.

#### Z-Wave Transport Layer

Z-Wave transport layer is mainly responsible for retransmission, packet acknowledgment, waking up low power network nodes, and packet origin authentication. The Z-Wave transport layer (or transfer layer) consists of four basic frame types. These are used for transferring commands in the network. All the frames use the format as mentioned below.

```
Transport Frame = { HomeID, Source NodeID, Header, length, Data byte(0 to X), Checksum }
```

The 4 frame types of the transport layer are explained below:

- Singlecast frame type: These type of frames are transmitted to one specific Z-Wave node. The frame is acknowledged so that transmitter will know whether the frame is received or not. If this frame or its ACK is lost or damaged then the singlecast frame is retransmitted.
- ACK frame type: It is a singlecast frame where the data payload part does not exist. This is explained above.
- Multicast frame type: These frames are transmitted to more than one node i.e. max. of 232 nodes. This type of frame does not support acknowledgment concept. Hence this type is not used for reliable communication.
- Broadcast frame type: These frames are received by all the nodes in a network and they are not ACKed by any nodes.

#### Z-Wave Network Layer

This Z-Wave network layer controls the frame routing from one node to the other node. Both the controllers as well as slave nodes participate in frame routing. The Z-Wave network layer is responsible for the following tasks:

- Transmission of a frame with correct repeater list
- Scanning of network topology
- Maintenance of routing table in the controller

This Z-Wave routing layer consists of two kinds of frames. These are used when repetition of frames becomes necessary.

- Routed Singlecast Frame Type: It is a one node destination frame with acknowledgment which contains repeater information. The frame is repeated from one repeater to another until it reaches the desired destination.
- Routed Acknowledge Frame Type: This acknowledgment frame is a routed singlecast frame without payload. This will inform the controller that the routed singlecast frame has reached the desired destination.


| 1 | 2 | 3 | 4 | 5 | 6 |
|---|---|---|---|---|---|
| 1 | 0 | 1 | 1 | 0 | 0 |
| 2 | 1 | 0 | 0 | 0 | 1 |
| 3 | 1 | 1 | 0 | 0 | 0 |
| 4 | 0 | 0 | 0 | 0 | 1 |
| 5 | 0 | 1 | 0 | 1 | 0 |
| 6 | 0 | 0 | 0 | 0 | 0 |

Table-1: Z-Wave routing table

Controller maintains the routing table. This table contains information about all the nodes in the network. This routing table is a bit field table as shown in the table-1. This routing table is built by primary controller based on information received from all the nodes in the Z-Wave network.

#### Z-Wave Application Layer

This layer is responsible for decoding and execution of commands in a Z-Wave network. The frame format used in the application layer consists of following fields.

```
Frame Format = { Single/Multi, broadcast frame header, Application command class, Application command, Command parameter1-to-X }
```

The application command class defines class of commands the command belong to:

- 00h-1Fh (This command class reserved for Z-Wave protocol)
- 20h-FFh (This command class reserved for Z-Wave application)

All the Z-Wave frame types except the acknowledgment frame contain an application command.
