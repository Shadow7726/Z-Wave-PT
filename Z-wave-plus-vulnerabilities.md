
| Z-Wave Plus Feature | Potential Vulnerabilities |
|----------------------|---------------------------|
| Long Range (LR) Mode | Increased signal range: Expands attack surface. <br> Potential security downgrade for backward compatibility. |
| SmartStart | Simplified inclusion process: Weakens authentication. <br> Limited key exchange validation: Allows injection of malicious keys. |
| Secure Neighbor Discovery (SND) | Trust-based network formation: Exploitable by attackers with initial access. <br> Limited vulnerability disclosure: Harder to assess and address risks. |
| Z-Wave Plus Long Frame Format (ZPFF) | Increased data packet size: Potential for buffer overflows. <br> Complex protocol interactions: May introduce unforeseen vulnerabilities. |
| Z-Wave Security 2 (S2) Key Derivation Function (KDF) | Potential implementation weaknesses: Insecure implementations or weak random number generation. <br> Limited public scrutiny: Proprietary mechanism with less public analysis. |


| Vulnerability                     | Description                                                                                                                                                   |
|----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Insecure Inclusion**                | Older devices relying on the standard security class remain vulnerable to spoofed inclusion attacks despite the introduction of the S2 security class.          |
| **Insecure Transmission**            | Some older devices or implementations might still have weak encryption or none at all, making them susceptible to interception and decryption.              |
| **Replay Attacks**                   | Unauthenticated commands are vulnerable to capture and replay, even with encryption. Implementation of message authentication codes (MACs) varies.         |
| **Weak Keys**                        | Despite improved key management in S2, older devices or implementations might still use hardcoded or predictable keys, facilitating extraction and misuse.   |
| **Forced Inclusion**                 | Z-Wave Plus does not directly address forced inclusion vulnerabilities, leaving networks vulnerable unless specific measures are implemented by manufacturers. |
| **Brute Force Attacks**              | While S2 has a larger key space, dedicated brute-forcing efforts might still be successful, particularly against weak implementations.                        |
| **De-encapsulation Attacks**         | Vulnerable S2 implementations might allow stripping encapsulation and manipulating raw commands if encryption is bypassed.                                    |
| **Firmware Downgrade**               | Z-Wave Plus lacks specific measures against firmware downgrades, leaving vulnerabilities in downgrade mechanisms or older firmware versions exploitable.   |
| **Command Injection**                | Secure implementations are essential to prevent command injection vulnerabilities, as insecure input handling could lead to code execution.                  |
| **Weaknesses in Protocol Design**    | Despite improvements, limitations such as the lack of source authentication in specific message types can leave S2 networks vulnerable to replay attacks.     |


