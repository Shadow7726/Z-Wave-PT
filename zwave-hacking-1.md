
# hardware and software components used for pentesting Z-Wave devices

### Hardware:
- **RTL-SDR Radio Dongle**:
  - An inexpensive software-defined radio that can receive radio signals from 24MHz to 1.766GHz. Used to intercept Z-Wave signals.
  
- **Yardstick One**:
  - A sub-1GHz radio designed for transmission and reception. Can be used to transmit custom Z-Wave packets.
  
- **Samsung Galaxy Tab S**:
  - An Android tablet used in conjunction with the RTL-SDR to capture Z-Wave signals using the RF Analyzer app.

### Software:
- **Kali Linux**:
  - A Linux distro with many preinstalled hacking/pentesting tools. Used as the main OS.

- **GQRX**:
  - An open-source SDR receiver application used to visualize and attempt to capture Z-Wave signals.

- **GNU Radio**:
  - An open-source software toolkit for implementing software radios. Used to build custom flow graphs for capturing and decoding signals.

- **Indigo**:
  - Commercial home automation software that can directly control Z-Wave devices via a connected Z-Stick controller. Used for sending commands.

- **Baudline**:
  - Software for time-frequency analysis of signals through Fourier transforms. Used for in-depth signal analysis.

- **RFcat**:
  - Python library used in conjunction with the Yardstick One to transmit custom-crafted Z-Wave packets.

- **Audacity**:
  - Open-source audio editing software. Used for visualizing and analyzing captured signals.

- **RF Analyzer**:
  - Android app that can use a connected SDR to capture radio signals. Used as an alternative to GQRX for capturing Z-Wave signals.

The combination of the SDR hardware and open-source software provides a complete toolkit for intercepting, analyzing, and transmitting Z-Wave signals to test vulnerabilities.

Here are the detailed steps to perform the Z-Wave replay attack, based on the information provided in the research paper:

1. **Obtain Z-Wave devices**:
   - A controller and node device (e.g., smart switch)

2. **Look up FCC IDs**:
   - To determine hardware details like chipsets.

3. **Configure software and hardware**:

   - Set up Kali Linux VM on Windows host.
   - Install GQRX, GNU Radio, Baudline, RFcat on Kali VM. Install Audacity on separate analysis machine.
   - Test RTL-SDR dongle reception with GQRX.
   - Configure Z-Wave controller in Indigo software on the analysis machine.
   - Include node device with the controller.
   - Test sending on/off commands from Indigo to the node.

4. **Use Indigo to send on/off commands**:
   - To node device while capturing with RTL-SDR:

   ```
   gqrx #Launch GQRX 
   Set frequency to 908.42MHz
   Capture I/Q data to file when commands sent
   ```

5. **Analyze captured signals**:
   - In Audacity and Baudline to identify Z-Wave signals.

6. **Build custom GNU Radio flow graph**:
   - For capturing Z-Wave signal:

   ```
   osmocom_source #Generate signal source
   Throttle #Downsample       
   Frequency Xlating FIR Filter #Tune offset
   Quadrature Demod #FM demod
   File Sink #Output to file
   ``` 

7. **Capture signal using GNU Radio flow graph**:
   - While sending on/off commands.

8. **Analyze captured signal**:
   - Convert binary signal to hex with xxd.
   - Identify preamble/sync words.
   - Decode Manchester encoding.
   - Align bytes to packet structure.

9. **Construct replay packet in hex**.

10. **Use Yardstick One and RFcat**:
    - To transmit replay packet:

    ``` 
    d.setFreq(908420000) 
    d.setMdmModulation(MOD_2FSK)
    d.setMdmDRate(9600) 
    d.RFxmit("<hex packet>")
    ```

This outlines the key steps from configuring the software-defined radios to capturing, decoding, and retransmitting a Z-Wave command to attack the system.

Here are installation and usage examples for the key software tools used in the Z-Wave replay attack:

### Kali Linux

- Download Kali ISO image and create a bootable USB drive with Rufus/Etcher.
- Boot Kali from USB and install to VM or dedicated pentesting machine.
- Can dual boot alongside the existing OS like Windows.

```shell
# Start Kali VM/machine 
```

### GQRX

- Install dependencies:

```shell
sudo apt install git cmake libqt4-dev libqt4-opengl-dev libusb-1.0-0-dev 
```

- Clone source and compile: 

```shell
git clone https://github.com/csete/gqrx.git
cd gqrx
mkdir build
cd build
cmake ..
make
sudo make install
sudo ldconfig
```

- Run GQRX:

```shell
gqrx
# Set frequency, sample rate, capture file
```

### GNU Radio

- Add PPA and install:

```shell
sudo add-apt-repository ppa:gnuradio/gnuradio-releases
sudo apt update 
sudo apt install gnuradio
```

- Build flow graph in GUI or create a Python script.
- Run flow graph: 

```shell
sudo gnuradio-companion #GUI
python my_flow_graph.py
```

### Audacity

- Install with apt on Linux:

```shell
sudo apt install audacity
```

- Import raw IQ files for analysis

### RFcat/Yardstick One

- Install dependencies:

```shell
sudo apt install python-pip python-setuptools python-usb 
sudo pip install pyusb
```

- Install rfcat:

```shell
git clone https://github.com/atlas0fd00m/rfcat
cd rfcat
sudo python setup.py install
``` 

- Plug in Yardstick One
- Transmit packets:

```shell
rfcat -r
d.setFreq(908420000)
d.makePktFLEN(100)
d.RFxmit("0x01 0x02...") 
```
