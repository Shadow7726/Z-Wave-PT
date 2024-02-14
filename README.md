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

This table provides a clear overview of the Z-Wave specifications, including frequency bands supported and data rates across different regions.
