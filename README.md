# Cracking WiFi WPA2 Handshake Guide

This guide provides a step-by-step approach on how to crack a WiFi WPA2 handshake using various tools on Kali Linux. It is intended for educational purposes only. Ensure you adhere to ethical hacking practices such as testing only on networks you own or have explicit permission to analyze.

## Required Technologies and Tools

### Kali Linux
- **Description**: A Linux distribution designed for digital forensics and penetration testing. It includes numerous tools for hacking and security testing.
- **Usage**: Running in a VM (Virtual Machine) environment to isolate testing activities from the main operating system.

### Network Adapter
- **Description**: A device that connects your computer to a network. For hacking WiFi, it must support monitor mode, which allows it to monitor all traffic in the air.
- **Specific Model Used**: ALFA AWUS036NHA Wireless B/G/N USB Adaptor
  - **Features**: 802.11n, 150 Mbps, 2.4 GHz, 5 dBi Antenna, Long Range
  - **Chipset**: Atheros
  - **Compatibility**: Windows XP/Vista 64-Bit/128-Bit, Windows 7, TAA Compliant
  - **Purchase Link**: [Buy here](https://www.amazon.com/gp/product/B004Y6MIXS)

### WiFi Network
- **Description**: The target of the attack, can be a router, modem, or network provided by a network provider.
- **Ethical Considerations**:
  - Ensure adherence to ethical hacking practices.
  - Attack only networks you own or for which you have explicit permission.

## Tools Used in the Process

### Monitoring and Capturing Tools

#### Airmon-ng
- **Description**: Manages wireless interfaces in monitor mode.
- **Usage**: Killing conflicting processes and toggling between monitor and managed modes.

#### Airodump-ng
- **Description**: Captures packet data from the network, useful for identifying the target network and capturing the handshake necessary for cracking.
- **Usage**: Discovering and monitoring wireless networks, capturing WPA2 four-way handshake.

### Network Disruption Tool

#### Aireplay-ng
- **Description**: Injects frames into the network to create traffic that can be used for attack purposes, such as deauthentication.
- **Usage**: Deauthenticating clients from a network to force reconnections, facilitating the capture of the four-way handshake.

### Analysis Tools

#### Wireshark
- **Description**: Analyzes the details of the network traffic, including the contents of captured packets.
- **Usage**: Examining the captured four-way handshake to ensure it has the necessary components for cracking.

#### Aircrack-ng
- **Description**: Cracks network security using the captured information, primarily through the use of dictionary attacks.
- **Usage**: Decrypting the WiFi password using the captured handshake and a password dictionary.

## Setup

1. Download and install Kali Linux on VM Ware Fusion.
2. Purchase and connect a network adapter that supports monitor mode.

## Initial Steps

1. Boot up your Kali Linux VM and connect the network adapter.
2. Open a terminal window and type `iwconfig` to check IP addresses and interfaces. Ensure your wireless LAN adapter (wlan0) is listed.
3. Execute `sudo airmon-ng check kill` to kill any conflicting processes.
4. Switch your network interface to monitor mode with `sudo airmon-ng start wlan0`, then verify with `iwconfig`.

## Network Discovery and Monitoring

1. Discover wireless networks by running `sudo airodump-ng wlan0mon`.
2. To focus on a specific network, execute:
   ```bash
   sudo airodump-ng -w hack1 -c [channel] --bssid [BSSID] wlan0mon
   ```
   Replace `[channel]` and `[BSSID]` with the appropriate network channel and BSSID.

## Network Disruption

1. Open a new terminal window and execute:
   ```bash
   sudo aireplay-ng --deauth 0 -a [BSSID] wlan0mon
   ```
   This will deauthenticate all clients from the network. Use Ctrl+C to stop the process.

## Handshake Capture

1. Monitor the previous terminal window to capture the four-way handshake when a device tries to reconnect.
2. Check for the handshake capture by running `ls` in the terminal.

## Password Cracking

1. Open the captured file in Wireshark and search for the four-way handshake.
2. Perform a dictionary attack using the Rockyou wordlist:
   ```bash
   sudo aircrack-ng [capture-file] -w /usr/share/wordlists/rockyou.txt
   ```
   Replace `[capture-file]` with your capture file path.

## Conclusion

The password, once identified, can illustrate the importance of using strong, unpredictable passwords. For example, even a password like `p@ssw0rd!` with letters, special characters, and numbers can be vulnerable if it follows a predictable pattern.

## Preventing Dictionary Attacks

Dictionary attacks exploit weak passwords by using large lists of common passwords and phrases. To protect against these attacks, it's crucial to adopt robust password policies and security measures.

### Strong Password Policies

1. **Length Matters**: The longer the password, the more combinations a hacker needs to try to crack it. Aim for passwords that are at least 12 characters long.

2. **Complexity**: Use a mix of upper and lower case letters, numbers, and symbols. This diversity significantly increases the number of possible combinations.

3. **Use Random Memorable Phrases**: Instead of single words, consider using random phrases that are easy for you to remember but hard for others to guess. These are less likely to appear in dictionaries.

### Additional Security Strategies

1. **Regular Password Changes**: Encourage or enforce regular password updates. Changing passwords periodically reduces the window of opportunity for a password to be cracked and misused.

2. **Use Multi-Factor Authentication (MFA)**: Adding an additional layer of security by requiring a second form of identification drastically reduces the risk of unauthorized access, even if the password is compromised.

3. **Account Lockout Policies**: Set up account lockouts after a certain number of unsuccessful login attempts. This can discourage brute-force attacks by making them time-consuming and easily detectable.

4. **Educate Users**: Regularly educate users about the importance of security practices, especially regarding how to create strong passwords and recognize phishing attempts.

5. **Avoid Common Pitfalls**: Advise against the use of easily accessible personal information in passwords, such as birthdays or names, as these can often be guessed or found through social engineering.

6. **Use Password Managers**: Encourage the use of password managers. These tools can generate and store complex passwords, reducing the temptation to reuse passwords across multiple sites.

Implementing these strategies can significantly enhance your network's security and resistance to dictionary attacks, protecting sensitive data from unauthorized access.

## Disclaimer

This tutorial is for educational purposes only. Do not attempt to access networks without explicit permission from the rightful owners.

