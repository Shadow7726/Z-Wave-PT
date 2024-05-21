# Z-Wave Downgrade Attack Checklist

## Introduction
This checklist guides you through exploiting the vulnerability in Z-Wave's S2 pairing process, allowing it to be downgraded to the less secure S0 protocol. This exposes smart devices to compromise. 

## Requirements
1. **Hardware and Tools**
    - Z-Wave PC Controller (Sigma UZB EU)
    - Z-Wave Zniffer
    - RF Cat or similar jamming device
    - Battery-powered drop-box (optional for advanced attacks)

2. **Software**
    - Sigma PC Controller software
    - Sniffing and packet modification tools

## Steps

### Step 1: Setup
1. **Obtain Home ID**
    - **Optional Method:** Insert battery into the node or place it in the vicinity to capture the node info.
    - Ensure your Zniffer is set up to capture packets.

### Step 2: Sniffing and Intercepting Packets
1. **Initiate Pairing Process**
    - Put the controller into "add" mode.
    - Press the button on the device to send out a "node info" packet.

2. **Capture Node Info**
    - Use Zniffer to capture the node info packet. This packet is unencrypted and includes the command class.

### Step 3: Modifying Packets
1. **Spoof Node Info**
    - Modify the node info packet to remove the `COMMAND_CLASS_SECURITY_2 (0x9F)`.

    **Original Packet:**
    ```
    FB 2E E0 F2 00 01 4B 15 FF 01 01 53 DC 01 40 03 5E 55 98 9F 59
    ```

    **Modified Packet:**
    ```
    FB 2E E0 F2 00 01 41 14 FF 01 01 53 DC 01 40 03 5E 55 98 CD
    ```
    - Recalculate the length and checksum.

### Step 4: Execute the Attack
1. **Inject Modified Packet**
    - Send the modified packet to the controller.
    - Ensure the controller pairs the device using S0 instead of S2.

### Step 5: Verify Attack Success
1. **Monitor Controller UI**
    - Check if the controller shows the device as paired using S0 security.
    - **S0 Paired Device:** Confirm that the network key was intercepted during the S0 key exchange.

### Step 6: Control the Device
1. **Access Network Key**
    - Use the intercepted network key to send commands to the Z-Wave devices.

## Advanced Attack Methods
1. **Passive Sniffing**
    - **Capture Node Info During Setup:** When the battery is inserted.
  
2. **Active Jamming**
    - **Continuous Listening:** Use RFCat to continuously listen and jam packets until the node info is captured.
    - **Time-Critical Jamming:** Develop a tool to jam the packet mid-way through transmission to prevent genuine node info from reaching the controller.

## Disclosure and Ethical Considerations
1. **Vendor Disclosure**
    - Report the findings to the device manufacturers and the Z-Wave Alliance.
    - Follow responsible disclosure practices to avoid misuse of the vulnerability.

## References
- [Pen Test Partners Blog](https://www.pentestpartners.com/security-blog/z-shave-exploiting-z-wave-downgrade-attacks/)
- [Z-Wave Specification Documentation](http://zwavepublic.com/sites/default/files/command_class_specs_2017A/SDS11846-20%20Z-Wave%20Plus%20Role%20Type%20Specification.pdf)
