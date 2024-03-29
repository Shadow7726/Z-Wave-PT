# Software and Hardware Hacking tools

| Category | Tool                   | Description                                                                                                         | Z-Wave Sniffing |
|----------|------------------------|---------------------------------------------------------------------------------------------------------------------|-----------------|
| Hardware | Yard Stick One         | USB transceiver for sub-GHz packet transmission/reception.                                                         | Yes             |
| Hardware | RTL-SDR                | USB software-defined radio dongle.                                                                                  | Yes (with additional software like Gqrx) |
| Hardware | HackRF                 | Open-source software-defined radio platform.                                                                        | Yes (with GNU Radio) |
| Hardware | Wireless Network Testers | Dedicated network testing tools like AirMagnet WiFi Analyzer or Fluke Networks Network Analyzers.                  | Sometimes (in promiscuous mode) |
| Hardware | Z-Wave Developer Kits | Official Silicon Labs Z-Wave Development Kits offering hardware and software for legitimate developers.           | Potentially (with caution and ethical considerations) |
| Hardware | Embedded Systems Debuggers | Tools like SEGGER J-Link or ST-LINK/V2 for advanced hardware access and debugging of Z-Wave devices.              | Potentially (with expertise and ethical considerations) |
| Software | Gqrx                   | Open-source SDR receiver software.                                                                                  | Yes, when used with an RTL-SDR. |
| Software | GNU Radio              | Toolkit for implementing software-defined radios and signal processing.                                             | Yes, with complex flowgraph designs and expertise. |
| Software | RFcat                  | Python library for Yardstick One.                                                                                   | Yes, for transmitting and receiving raw Z-Wave packets. |
| Software | Wireshark              | Popular network protocol analyzer capable of capturing and displaying raw Z-Wave packets.                           | Yes, with Z-Wave dissectors (user-created plugins). |
| Software | OpenZWave Libraries    | Variations like OpenZWave Control Panel or OpenZWave-Cpp offer various functionalities for interacting with Z-Wave devices. | Potentially, for interacting with Z-Wave devices. |
| Software | Custom Python Scripts  | Custom Python scripts utilizing libraries like Scapy or RadioTap for advanced Z-Wave packet manipulation and analysis. | Yes, for advanced packet manipulation and analysis. |
| Software | Zniffer                | Official tool from Silicon Labs for deep analysis and decryption of Z-Wave frames. Requires registration and approval for access. | Yes, for in-depth analysis and decryption (requires registration and approval). |
| Software | pywavez                | Python library for decoding and interpreting Z-Wave frames. Requires Python programming knowledge.                    | Yes, for open-source decoding and interpretation. |
| Software | OpenZWave Library      | Open-source library supporting Z-Wave communication and data parsing. Can be integrated into custom applications.      | Yes, for Z-Wave communication and data parsing. |
| Hardware | Aeotec Z-Stick Gen5    | USB stick with Z-Wave Plus and Gen5 technology for reliable connection. Supports Windows and Linux systems.           | Yes             |
| Hardware | Sigma Designs UZB700   | Z-Wave stick with built-in encryption/decryption capabilities. Compatible with Windows, Mac, and Linux.               | Yes             |
| Hardware | Zooz Z-Wave Plus S2 USB Stick | Cost-effective option with S2 encryption. Limited platform compatibility compared to others.                          | Yes             |
| Software | Sil Labs EmberZ (EZSP/EmberNet) | Powerful option for professional developers, providing deep access to Zigbee and Z-Wave networks. Requires technical expertise. | Potentially, for deep network access. |

# EZ-Wave
EZ-Wave: Tools for Evaluating and Exploiting Z-Wave Networks using Software-Defined Radios.

ezstumbler: passive Z-Wave network discovery and active network enumeration

ezrecon: Z-Wave device interrogation including:
	- Manufacturer and device name
	- Software/firmware versions
	- Supported Z-Wave command classes
	- Device configuration settings

ezfingerprint: determines device's Z-Wave module generation (3rd or 5th gen) using a PHY layer manipulation technique (preamble length manipulation).

# Requirements

**Tested on Ubuntu 14.04 only

Python 2.7

GNU Radio 3.7+ (recommend Pybombs: https://gnuradio.org/redmine/projects/pybombs/wiki/QuickStart)

Wireshark 1.12+ (https://code.wireshark.org/review/wireshark)

Mercurial (sudo apt-get install mercurial -y)

**Default configuration is for 2 HackRF One SDRs. Other SDRs can be use by modifying the GRC files accordingly post install ($HOME/.scapy/radio).

OsmocomSDR (http://sdr.osmocom.org/trac/wiki/GrOsmoSDR)

HackRF host software (https://github.com/mossmann/hackrf/tree/master/host)

# Installation

The setup script will clone Scapy-radio (https://bitbucket.org/cybertools/scapy-radio/) and modify installation files

```
./setup.sh
```

## Install Scapy-radio

```
cd $HOME/scapy-radio
./install.sh scapy
./install.sh blocks
```

Open [gnuradio prefix]/etc/gnuradio/conf.d in a text editor and append ":/usr/local/share/gnuradio/grc/blocks" to global_blocks_path

```
./install.sh grc
```

## Install Wireshark dissector

Copy all files in EZ-Wave/setup/wireshark to [wireshark]/epan/dissectors

```
cd [wireshark]
./autogen.sh
./configure
make
sudo make install
sudo ldconfig
```

# Usage

##ezstumbler

ezstumbler.py [-h, --help] [-p, --passive] [-t, --timeout] [-a, --active] [--homeid]
	-p, --passive		Conduct a passive scan for a set time (secs)
	-t, --timeout		Timeout (secs) for scans, default=60
	-a, --active		Conduct an active scan for a set time (secs)
	--homeid		4 byte HomeID to scan (ex: 0x1a2b3c4d)

30s passive followed by active scan:
```
ezstumbler.py --timeout=30
```

passive scan:
```
ezstumbler.py --passive
```

active scan:
```
ezstumbler.py --active --homeid=0x1a2b3d4e
```

##ezrecon

ezrecon.py [-h, --help] [-c, --config] [-t, --timeout] homeid nodeid
	homeid			4 byte HomeID of target network (ex: 0x1a2b3c4d)
	nodeid			Target device NodeID (in decimal, <233)
	-c, --config		Include scan of device configuration settings (takes a while)
	-t, --timeout		Stop scanning after a given time (secs, default=30)

```
ezrecon.py 0x1a2b3c4d 20
```

##ezfingerprint

ezfingerprint.py homeid nodeid
	homeid			4 byte HomeID of target network (ex: 0x1a2b3c4d)
	nodeid			Target device NodeID (in decimal, <233)

```
ezfingerprint.py 0x1a2b3c4d 20
```

Here are the complete steps for installing GNU Radio and Gqrx on different operating systems:

**Linux:**

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install gnuradio gnuradio-companion gr-osmosdr gqrx
```

**Arch Linux:**
```bash
sudo pacman -S gnuradio gnuradio-companion gr-osmosdr gqrx
```

**Other Linux distributions:** 
Consult your distribution's package manager or install from source (see below).

**macOS:**
1. Install Xcode Command Line Tools.
2. Download macOS Radioconda installer (x86_64 or aarch64) from gnuradio.org.
3. Run the installer and follow the instructions.
4. Launch "GNU Radio Companion" from the Start menu.

**Windows:**
1. Download Windows Radioconda installer from gnuradio.org.
2. Run the installer and follow the instructions.
3. Launch "GNU Radio Companion" from the Start menu.

Here are more details on installing GNU Radio and the gr-zwave module to decode Z-Wave with a HackRF One:

**Install GNU Radio**

- On Linux, install from your package manager, e.g:

```bash
sudo apt install gnuradio
```

- On Mac, install via Homebrew: 

```bash
brew install gnuradio
```

- On Windows, install via pre-built binaries or PyBOMBS

**Install gr-zwave module**

- Clone the gr-zwave repository:

```bash 
git clone https://github.com/bastibl/gr-zwave
```

- Build and install the module:

```bash
cd gr-zwave
mkdir build
cd build
cmake ..
make
sudo make install
sudo ldconfig
```

- This will install gr-zwave to your GNU Radio module path

**Verify Installation**

- Launch GNU Radio Companion 
- You should see the Z-Wave Decoder block under the gr-zwave category
- Try creating a flowgraph using the HackRF Source and Z-Wave Decoder

---

Here is an example of how to capture and analyze Z-Wave signals using the HackRF One with GNU Radio, along with some sample commands and output:

**Hardware Setup:**

- Connect HackRF One via USB and attach 900 MHz antenna

**Software Setup:**

- Install GNU Radio and gr-zwave module
- Open GNU Radio Companion and build flowgraph:

```
hackrf_source -> freq_xlating_fir_filter -> low_pass_filter -> zwave_decoder -> file_sink -> message_debug
```

- Set hackrf_source sample rate to 2 MS/s
- Set freq_xlating_fir_filter to translate to 908.42 MHz  
- Set low_pass_filter cutoff to 500 kHz

**Commands:**

- Click Generate to compile flowgraph 
- Click Play to start receiving
- Tune hackrf_source frequency offset to center on 908.42 MHz

**Sample Output:**

- Bring Z-Wave device near antenna
- Check file_sink to save capture to file
- Check message_debug which prints decoded packets:

```
ZL7 Address: 0x01 0x17 0x70 Node: 23 
ZL7 Command: 0x20 Seq: 0x05 
ZL7 Payload: 0x05 0xB2 0x4E 0x50 0x01 0xFF 0x00
```

- We can see a Z-Wave packet from node 23 was decoded 
- It has command 0x20 and payload with battery level (0xB2), firmware version etc.

This allows analyzing raw Z-Wave signals easily with HackRF One SDR and GNU Radio flowgraph. We can tune settings as needed for better captures.
