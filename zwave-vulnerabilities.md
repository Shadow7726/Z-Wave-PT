
# Z-wave Vulnerabilities


| Vulnerability         | Description                                                                                                      | Scenario                                                                                                               | Attack Steps                                                                                                                                    |
|-----------------------|------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| Insecure Inclusion    | Z-Wave networks allow new devices to be included without authentication.                                           | Attacker sets up RFcat and transmits spoofed inclusion packets during inclusion mode to add their device.            | - Capture inclusion packets using SDR. - Analyze packets and craft spoofed inclusion packet. - Transmit spoofed packet to add rogue node.        |
| Insecure Transmission | Z-Wave uses weak encryption (AES-128) and some devices have no encryption.                                         | Attacker uses RTL-SDR dongle to intercept encrypted Z-Wave signals. They capture the encryption key leaked in one frame and decrypt all communications. | - Set up RTL-SDR dongle and Kali Linux for packet sniffing. - Decrypt captured traffic using extracted AES encryption key.                        |
| Replay Attacks        | Unencrypted or broken encryption Z-Wave commands can be captured and retransmitted to control devices.           | Attacker records command sending "unlock" to a smart lock node. They replay it later to unlock the door without authorization. | - Capture command packets. - Replay captured packets to perform unauthorized actions.                                                           |
| Weak Keys             | Some Z-Wave devices have hardcoded encryption keys that are extracted and used to decrypt communications.         | Attacker extracts hardcoded AES key from a smart bulb's firmware. They use it to decrypt Z-Wave traffic and replay commands. | - Extract hardcoded AES keys from device firmware. - Use keys to decrypt Z-Wave traffic and replay commands.                                     |
| Forced Inclusion      | Tools like BANANA can jam the controller and force nodes into inclusion mode, allowing capture and replay attacks. | Attacker uses BANANA device to jam the controller. Nodes enter inclusion mode. Attacker replays captured inclusion packets to add their device. | - Continuously jam Z-Wave controller. - Capture inclusion packets. - Replay packets to add malicious nodes.                                      |
| Brute Force Attacks   | The 24-bit Network PIN can be brute forced as the key space is relatively small.                                   | Attacker brute forces weak 24-bit network PIN to gain access to Z-Wave network and control devices.                   | - Capture network key exchange frame during inclusion. - Brute force 24-bit PIN. - Gain access to Z-Wave network.                                |
| De-encapsulation Attacks | Z-Wave uses encapsulated frames which can be decapsulated into raw Z-Wave commands and replayed if encryption is broken. | Attacker strips the Z-Wave routing encapsulation to obtain raw frames. They replay commands after decrypting payload.  | - Analyze captured packets to understand encapsulation. - Write code to strip encapsulation and decrypt payload. - Replay commands.               |
| Firmware Downgrade    | Some devices allow firmware downgrades to older vulnerable versions without authentication.                        | Attacker downgrades smart lock firmware via unauthenticated OTA update. Older firmware has auth bypass bug they exploit. | - Reverse engineer device OTA protocol. - Craft fake firmware image. - Exploit vulnerabilities in older firmware.                              |
| Command Injection     | Sending malformed input to handler functions could cause command injection and execution of arbitrary code.       | Attacker sends malformed packet that injects OS commands into smart bulb's command handler. Executes code on the device. | - Fuzz packet parameters. - Identify vulnerable input parameters. - Craft input strings with OS commands. - Execute arbitrary code.         |
| Weaknesses in Protocol | Design weaknesses like lack of source authentication and integrity checking enable attacks like signal replay.    | Attacker replays captured "on" command due to lack of source authentication in Z-Wave protocol.                       | - Capture and analyze packets. - Replay commands without proper source validation.                                                               |


### 1.Steps to Perform an Insecure Inclusion Attack on a Z-Wave Network:

Hardware Required:
- RTL-SDR Software Defined Radio 
- Yardstick One Transceiver

Software Required: 
- Kali Linux OS
- GQRX (for signal analysis)
- GNU Radio (for signal decoding)
- RFcat (for packet transmission)

Steps:

1. Put the Z-Wave network into inclusion mode.

2. Use the RTL-SDR to capture inclusion traffic:

```
gqrx #Open GQRX 
Set frequency to 908.42MHz
Capture traffic to file
```

3. Analyze the captured traffic in GNU Radio:

```
gnuradio-companion #Open flowgraph
Decode FSK modulation
Decode Manchester encoding
Analyze packets
```

4. Identify the inclusion command packet. 

5. Craft an inclusion packet with RFcat:

```
rfcat
d.setFreq(908420000)  
d.makePktFLEN(100)
pkt = "0x01..." # Spoofed packet 
d.RFxmit(pkt)
```

6. Set the Yardstick One to transmit on 908.42MHz.

7. Transmit the spoofed inclusion packet with RFcat using the Yardstick One.

8. Verify the attack worked by checking the controller or attempting to control other devices.

This outlines the specific steps using the SDR hardware and open source SIGINT tools to intercept, analyze, and transmit a spoofed inclusion packet to add an unauthorized device to the target Z-Wave network.

### 2.Intercepting and Decrypting Z-Wave Signals:

#### Hardware Used:
- RTL-SDR USB dongle 
- Laptop running Kali Linux

#### Software Used:
1. **Set up RTL-SDR Dongle:**
   ```shell
   sudo apt install librtlsdr-dev rtl-sdr
   ```

2. **Tune to Z-Wave Frequency:**
   - Open GQRX:
     ```shell
     gqrx
     ```
   - Set frequency to 908MHz and capture traffic.

3. **Analyze Captured Data:**
   - Open in Audacity and look for known Z-Wave preamble.

4. **Decode Packets:**
   - Use GNU Radio companion and decode blocks to output decoded packets.

5. **Decrypt Packets:**
   - Extract encryption key from captured packet.
   - Perform AES 128 decryption in Python:
     ```python
     from Crypto.Cipher import AES
     aes = AES.new(key, AES.MODE_ECB)
     plaintext = aes.decrypt(encrypted_payload)
     ```

6. **Replay Decrypted Commands:**
   - Use RFcat with Yardstick One to transmit decrypted commands:
     ```shell
     d.setFreq(908400000)
     d.RFxmit(decrypted_command)
     ```

### 3.Steps to Perform a Replay Attack on a Z-Wave Smart Lock:

#### Hardware Required:
- Yardstick One transceiver
- Laptop with Kali Linux OS

#### Software Required:
- RFcat software for Yardstick One
- Audacity for signal analysis

1. **Put Z-Wave Network into Learning Mode:**
   - Initiate learning mode on the Z-Wave network.

2. **Capture Traffic:**
   - Use RTL-SDR dongle to capture Z-Wave traffic.

3. **Analyze Captured Signals:**
   - Use Audacity to analyze captured signals and identify the "Unlock door" command.

4. **Craft Replay Packet:**
   - Convert the "Unlock door" command bits to a hex string.

5. **Transmit Replay Packet:**
   - Use RFcat with Yardstick One to transmit the replayed packet:
     ```shell
     d.setFreq(908400000)
     d.RFxmit(hex_string)
     ```

### 4.Extracting and Using Hardcoded Encryption Keys from Z-Wave Device Firmware:

#### Hardware Required:
- Z-Wave device
- RTL-SDR dongle
- Laptop with Kali Linux

#### Software Required:
- binwalk
- python-crypto
- gqrx
- rfcat

1. **Obtain Firmware:**
   - Obtain firmware image file for the Z-Wave device.

2. **Extract Firmware:**
   ```shell
   binwalk -e lightbulb-firmware.bin
   ```

3. **Search for Encryption Keys:**
   ```shell
   grep -R "AES" _lightbulb-firmware.bin.extracted
   ```
Sure, here's the information formatted in Markdown:

### 5.Forced Inclusion Attack on a Z-Wave Network using the BANANA Device:

#### Hardware Required:
- BANANA device
- Yardstick One transceiver
- RTL-SDR dongle
- Laptop with Kali Linux

#### Software Required:
- BANANA control software
- Gqrx
- RFcat
- Audacity

1. **Connect BANANA Device:**
   - Connect the BANANA device to the Kali laptop via USB.

2. **Start BANANA Control Software:**
   ```shell
   $ banana-control
   ```

3. **Configure BANANA for Jamming:**
   - Configure BANANA for Z-Wave network jamming on 908MHz.

4. **Activate Jamming:**
   - Activate jamming to block controller communications.

5. **Enter Inclusion Mode:**
   - Z-Wave nodes will enter inclusion mode after timeout due to disrupted communication.

6. **Capture Inclusion Packets:**
   - Use RTL-SDR and Gqrx to capture inclusion packets.

7. **Analyze Traffic:**
   - Analyze traffic in Audacity to decode the inclusion command.

8. **Craft Spoofed Inclusion Packet:**
   - Craft a spoofed inclusion packet with the controller and fake node ID.

9. **Replay Spoofed Packet:**
   - Replay the spoofed packet using Yardstick One and RFcat:
     ```shell
     $ rfcat
     d.setFreq(908400000)
     d.RFxmit(spoofed_packet)
     ```

10. **Successful Inclusion:**
    - The fake node will get included into the Z-Wave network.

### 6.Brute Force Attack on a Z-Wave Network PIN:

#### Hardware Required:
- Z-Wave transceiver like Yardstick One
- Laptop with Kali Linux

#### Software Required: 
- RFcat for Yardstick One
- Python script for brute forcing

1. **Capture Network Key Exchange Frame:**
   - Capture the Z-Wave network key exchange frame during inclusion using RTL-SDR dongle.

2. **Extract PIN:**
   - Extract the 24-bit network PIN from the captured inclusion packet.

3. **Brute Force PIN:**
   - Write a Python script to brute force the 24-bit PIN.

4. **Use RFcat to Transmit Inclusion Command:**
   - Transmit the inclusion command with guessed PIN using RFcat and Yardstick One.

5. **Repeat Brute Force:**
   - Repeat the brute force process, iterating through all 24-bit PIN combinations.

6. **Successful Inclusion:**
    - Once the correct PIN is found, the inclusion will succeed, granting access to the Z-Wave network.
  
Here are the steps to perform a brute force PIN attack against a Z-Wave network during the inclusion process:

1. Put the Z-Wave network into inclusion mode to trigger a key exchange.

2. Use an RTL-SDR dongle and GNU Radio to capture the network key exchange frame.

3. Extract the 24-bit PIN value from the captured packet.

4. Write a Python script to brute force the PIN:

```python
import rfcat

# Open connection to Yardstick One 
d = rfcat.RFcat()

# Set transmission parameters
d.setFreq(908420000)
d.setMdmModulation(MOD_2FSK)

# Iterate through all pin combinations
for pin in range(0, 16777215):

  # Construct inclusion command with pin   
  pkt = create_inclusion_pkt(pin)

  # Transmit packet
  d.RFxmit(pkt)

  # Check if inclusion succeeded
  if inclusion_succeeded():
    print "[+] PIN found:", pin
    break
```

5. Run the brute force script to iterate through all 24-bit PIN combinations.

6. Once the correct PIN is found, the inclusion command will succeed and grant access to the Z-Wave network.

This outlines a brute force attack targeting the PIN exchanged during the inclusion process to gain unauthorized access. The SDR and rfcat library enable automating transmission of all pin combinations.

### 7.De-encapsulation Attack to Replay Z-Wave Commands:

#### Hardware Required:
- RTL-SDR dongle 
- Yardstick One transceiver
- Laptop with Kali Linux

#### Software Required:
- Gqrx
- Python
- RFcat

1. **Capture Z-Wave Packets:**
   - Use RTL-SDR and Gqrx to capture Z-Wave packets between controller and nodes.

2. **Analyze Packets:**
   - Analyze packets in Audacity to understand Z-Wave framing.

3. **Strip Encapsulation:**
   - Write a Python script to strip the routing encapsulation and extract the command payload.

4. **Decrypt Payload:**
   - If the payload is encrypted, attempt to decrypt it using captured encryption keys.

5. **Craft Raw Z-Wave Command Frame:**
   - Craft a raw Z-Wave command frame with the decrypted payload.

6. **Replay Raw Frame:**
   - Use RFcat with Yardstick One to replay the raw frame.

7. **Successful Replay:**
    - The target node will process the raw Z-Wave command without encapsulation.

By de-encapsulating packets, an attacker removes routing information to obtain raw command frames that can be replayed directly to nodes after decrypting payloads.


4. **Capture Z-Wave Signals:**


### 8.**Z-Wave Lock Firmware Downgrade Attack:**

#### **Hardware Required:**
- RTL-SDR dongle
- Yardstick One transceiver
- Laptop with Kali Linux

#### **Software Required:**
- Gqrx
- KillerBee framework
- Old vulnerable firmware binary
- Exploit code

#### **Steps:**
1. Analyze Z-Wave API commands for firmware update using KillerBee and an SDR.
2. Craft a raw Z-Wave command frame for outdated firmware version OTA update.
3. Spoof the controller node ID in the firmware update command.
4. Transmit firmware update command to the lock using Yardstick One.
5. Lock receives update and reboots with vulnerable firmware.
6. Use the SDR to capture encrypted Z-Wave traffic.
7. Extract encryption keys from captured packets.
8. Decrypt command payload and analyze protocol.
9. Identify authentication bypass bug in old firmware.
10. Craft raw Z-Wave command to exploit the bug and unlock door.
11. Transmit crafted command to unlock the door.

---

### 9.**Command Injection Attack on a Z-Wave Smart Bulb:**

#### **Hardware Required:**
- Yardstick One transceiver
- Laptop with Kali Linux

#### **Software Required:**
- rfcat for Yardstick One
- Audacity for analyzing captured packets
- Python for packet crafting

#### **Steps:**
1. Capture normal Z-Wave packets sent to smart bulb using RTL-SDR dongle.
2. Analyze captured packets in Audacity to understand packet structure.
3. Identify command fields in packet that are processed by the device firmware.
4. Craft malicious packets in Python with `;` and other chars to break out of command parsing.
5. Attempt to inject OS commands into the command fields:

```python
mal_packet = "SET Color =RED; cat /etc/passwd > /tmp/out" 
```

6. Transmit the crafted packet using Yardstick One:

```python
rfcat
d.RFxmit(mal_packet)
```

7. If command injection is successful, output of the OS command will be written to file.
8. Other impacts like code execution, DoS can also be achieved depending on the exploited device and injection point.
9. Repeat by fuzzing different packet fields to identify other injection points.

---

### 10.**Replay Attack Exploiting Lack of Source Authentication in Z-Wave Protocol:**

#### **Hardware Required:**
- RTL-SDR dongle
- Yardstick One transceiver
- Laptop with Kali Linux

#### **Software Required:**
- Gqrx
- RFcat
- Audacity

#### **Steps:**
1. Use the RTL-SDR and Gqrx to capture normal traffic between the Z-Wave controller and devices.
2. Analyze the captured traffic in Audacity to identify the packet structure.
3. Note that there are no source or authentication fields in the Z-Wave packet.
4. Identify a command packet like "Turn light ON".
5. Select the encoded signal bits for this command packet in Audacity.
6. Export the selected bits as a raw audio file.
7. Open RFcat and set Yardstick One to the desired Z-Wave frequency:

```python
rfcat
d.setFreq(908400000)
```

8. Convert the captured command bits to a hex string.
9. Transmit the captured command packet using RFcat:

```python
d.RFxmit(hex_string)
```

10. The replayed packet will turn on the light without any authentication.

