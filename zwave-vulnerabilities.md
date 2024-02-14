---
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

Sure, here's the provided information formatted in Markdown:

### Steps to Perform an Insecure Inclusion Attack on a Z-Wave Network:

1. **Obtain Hardware:**
   - Obtain a Z-Wave transceiver device like the Yardstick One or a Software Defined Radio (SDR) capable of transmitting on the 908MHz frequency band.

2. **Identify Target Z-Wave Network:**
   - Use a sniffer like Wireshark with a Z-Wave sniffing plugin to detect nearby Z-Wave traffic.

3. **Put Target Z-Wave Network into Inclusion Mode:**
   - Put the network into inclusion mode by pressing a button on the main controller or gateway.

4. **Capture Z-Wave Packets:**
   - Capture Z-Wave packets being transmitted while the network is in inclusion mode using the transceiver.

5. **Analyze Traffic:**
   - Use Audacity or other SIGINT software to reverse engineer the inclusion packets and identify the command, frequency, modulation, etc.

6. **Craft Inclusion Packet:**
   - Craft your own inclusion packet using the source ID of the controller and a fake node ID, including an encrypted payload if necessary.

7. **Transmit Spoofed Inclusion Packet:**
   - Use RFcat or similar software to transmit your spoofed inclusion packet at the exact Z-Wave frequency.

8. **Verify Success:**
   - If successful, your rogue device will be added to the Z-Wave network under your control.

9. **Repeat if Necessary:**
   - Repeat the inclusion process multiple times for full access.

10. **Verify Device Inclusion:**
    - Verify your device is included by checking the controller or attempting to control other devices on the network.

### Intercepting and Decrypting Z-Wave Signals:

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

### Steps to Perform a Replay Attack on a Z-Wave Smart Lock:

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

### Extracting and Using Hardcoded Encryption Keys from Z-Wave Device Firmware:

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

4. **Capture Z-Wave Signals:**
   - Use RTL-SDR dongle with GQRX to capture Z-Wave signals.

5. **Analyze Captured Signals:**
   - Analyze packets in Audacity and decode commands.

6. **Decrypt Packets:**
   - Write Python script to decrypt captured payloads.

7. **Craft New Packets:**
   - Craft new packets with decrypted commands and encrypted with extracted key.

8. **Transmit Crafted Packets:**
   - Use Yardstick One to transmit crafted packets.

