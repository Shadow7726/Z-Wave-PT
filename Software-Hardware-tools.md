# Software and Hardware Hacking tools

| Category | Tool                   | Description                                                                                                         | Z-Wave Sniffing |
|----------|------------------------|---------------------------------------------------------------------------------------------------------------------|-----------------|
| Hardware | Yard Stick One         | USB transceiver for sub-GHz packet transmission/reception.                                                         | Yes             |
| Hardware | RTL-SDR                | USB software-defined radio dongle.                                                                                  | Yes (with additional software like Gqrx) |
| Hardware | HackRF                 | Open-source software-defined radio platform.                                                                        | Yes (with GNU Radio) |
| Hardware | Ubertooth              | Open-source wireless development board.                                                                             | No, not directly. Ubertooth is primarily designed for Bluetooth and classic RF protocols. While it can be used to detect and capture raw Z-Wave signals, it lacks the ability to decode and interpret the Z-Wave protocol itself. |
| Hardware | BANANA                 | Z-Wave network jamming tool.                                                                                        | N/A             |
| Hardware | Wireless Network Testers | Dedicated network testing tools like AirMagnet WiFi Analyzer or Fluke Networks Network Analyzers.                  | Sometimes (in promiscuous mode) |
| Hardware | Z-Wave Developer Kits | Official Silicon Labs Z-Wave Development Kits offering hardware and software for legitimate developers.           | Potentially (with caution and ethical considerations) |
| Hardware | Embedded Systems Debuggers | Tools like SEGGER J-Link or ST-LINK/V2 for advanced hardware access and debugging of Z-Wave devices.              | Potentially (with expertise and ethical considerations) |
| Software | Gqrx                   | Open-source SDR receiver software.                                                                                  | Yes, when used with an RTL-SDR. |
| Software | GNU Radio              | Toolkit for implementing software-defined radios and signal processing.                                             | Yes, with complex flowgraph designs and expertise. |
| Software | RFcat                  | Python library for Yardstick One.                                                                                   | Yes, for transmitting and receiving raw Z-Wave packets. |
| Software | Audacity               | Open-source digital audio editor.                                                                                   | While Audacity can process and visualize captured Z-Wave signal data, it cannot decode or interpret the Z-Wave protocol itself. It requires additional tools for meaningful analysis. |
| Software | Wireshark              | Popular network protocol analyzer capable of capturing and displaying raw Z-Wave packets.                           | Yes, with Z-Wave dissectors (user-created plugins). |
| Software | OpenZWave Libraries    | Variations like OpenZWave Control Panel or OpenZWave-Cpp offer various functionalities for interacting with Z-Wave devices. | Potentially, for interacting with Z-Wave devices. |
| Software | Custom Python Scripts  | Custom Python scripts utilizing libraries like Scapy or RadioTap for advanced Z-Wave packet manipulation and analysis. | Yes, for advanced packet manipulation and analysis. |

# EZ-Wave, a Z-Wave hacking tool:

**Overview**

- EZ-Wave is an open source Python-based tool for analyzing and attacking Z-Wave networks. 

- It allows intercepting, decrypting, and replaying Z-Wave traffic using software defined radios.

- EZ-Wave supports multiple SDR hardware like HackRF, BladeRF, RTL-SDR.

**Configuration**

- Install prerequisites like Python 2.7, PyCrypto, Numpy, Scipy

- Install Pyserial and required SDR drivers

- Clone EZ-Wave github repository:

```
git clone https://github.com/probonopd/EZ-Wave.git
```

- Edit ezwave.cfg to specify SDR parameters

**Usage** 

- Plug in supported SDR hardware and start EZ-Wave:

```
python ezwave.py
```

- Scan for Z-Wave networks with the 'scan' command

- Sniff traffic on a network using the 'sniff' command

- Capture and replay packets with 'get' and 'send'

- Brute force captured network PINs with 'bfp'

- Crack encryption and decrypt packets using 'crack' and 'decrypt'

**SDR Connection**

- EZ-Wave supports RTL-SDR, HackRF, BladeRF.

- USB connection from SDR to computer running EZ-Wave.

- May require USB hub for multiple SDRs.

- Edit ezwave.cfg to specify correct SDR hardware.

So in summary, EZ-Wave provides an automated tool for Z-Wave hacks using cheap SDR hardware and various analysis features accessible via CLI.

Here are some of the key features and usage examples of EZ-Wave:

**Features**

- Traffic sniffing - Intercept Z-Wave traffic using an SDR and decrypt if encryption keys are known.

- Packet capture and replay - Capture Z-Wave packets and retransmit to mimic the controller or other nodes. 

- Brute force cracking - Crack network PINs by brute forcing all possible combinations.

- Encryption cracking - Extract encryption keys from captured packets and use them to decrypt payload.

- Network scanning - Scan for nearby Z-Wave networks by sweeping frequency range.

- Decapsulation - Remove network encapsulation to obtain raw Z-Wave packets. 

- Jamming - Transmit noise on Z-Wave frequencies to disrupt the network.

**Usage Examples**

- Sniff traffic:

```
ezwave> sniff 90 868.42
```

- Capture on/off command: 

```
ezwave> get on
ezwave> get off
```

- Replay captured on command:

``` 
ezwave> send on
```

- Brute force network PIN:

```
ezwave> bfp 90 868.42
``` 

- Crack encryption with known plaintext:

```
ezwave> crack 90 868.42
```

- Decrypt captured packets: 

```
ezwave> decrypt packets.pcap
```

- Scan for Z-Wave networks:

```
ezwave> scan 868.0 868.6
```


