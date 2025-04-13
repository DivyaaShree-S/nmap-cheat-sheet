# nmap cheat sheet
## nmap

Nmap (Network Mapper) is a powerful open-source tool used for network discovery, security auditing, and vulnerability scanning. It helps ethical hackers and cybersecurity professionals map out networks, detect open ports, identify running services, and find potential security risks.

# Nmap Cheat Sheet - Top 200 Commands

This cheat sheet includes the top 200 Nmap commands organized into categories for quick reference. Use this guide to enhance your Nmap scanning and penetration testing efforts.

## Table of Contents
1. [Nmap Scan Types](#nmap-scan-types)
2. [Target Selection](#target-selection)
3. [Service and Version Detection](#service-and-version-detection)
4. [Port Scanning](#port-scanning)
5. [Advanced Scanning Techniques](#advanced-scanning-techniques)
6. [Scan Performance](#scan-performance)
7. [Output Formats](#output-formats)
8. [Scan Customization](#scan-customization)
9. [Nmap Scripting Engine (NSE)](#nmap-scripting-engine-nse)
10. [Host Discovery and Ping](#host-discovery-and-ping)
11. [Firewall and IDS/IPS Evasion](#firewall-and-idsips-evasion)
12. [Scan Output and Reporting](#scan-output-and-reporting)
13. [Nmap Miscellaneous Options](#nmap-miscellaneous-options)
14. [Nmap in Automation](#nmap-in-automation)

---

## Nmap Scan Types

### Basic Scans
- `nmap <target>`  
  Scans the target using default settings, including the most common 1000 TCP ports.

- `nmap -sS <target>`  
  Performs a TCP SYN scan, also known as a stealth scan. It only sends SYN packets to ports, not completing the handshake, which helps evade detection by some firewalls.

### Stealth Scans
- `nmap -sS <target>`  
  Same as above, this is the default stealth scan and is often used to avoid detection.

- `nmap -sF <target>`  
  FIN scan, used to evade some firewalls. It sends a FIN flag to a closed port, which may confuse firewalls into not detecting the scan.

- `nmap -sX <target>`  
  Xmas scan, which sets the FIN, URG, and PSH flags. This can sometimes bypass firewalls or filters that ignore unusual packets.

### OS Detection
- `nmap -O <target>`  
  Detects the operating system of the target by analyzing its responses and fingerprinting techniques.

- `nmap -A <target>`  
  Performs OS detection, version detection, script scanning, and traceroute all in one.

### Version Detection
- `nmap -sV <target>`  
  Attempts to identify the version of services running on open ports by sending probes and analyzing the responses.

- `nmap -A -sV <target>`  
  OS detection, version detection, script scanning, and traceroute together in one scan.

### Script Scans
- `nmap -sC <target>`  
  Runs the default set of Nmap scripts to gather more detailed information about the target.

- `nmap --script <script_name> <target>`  
  Allows you to specify a particular Nmap script for more targeted information gathering.

### Traceroute
- `nmap --traceroute <target>`  
  Traces the route packets take to reach the target, displaying each hop along the way.

### Ping Scans
- `nmap -sn <target>`  
  Performs a ping scan to see if a host is up without performing a full port scan.

---

## Target Selection

### Single Target
- `nmap 192.168.1.1`  
  Scans a single host at IP address `192.168.1.1`.

### Multiple Targets
- `nmap 192.168.1.1 192.168.1.2`  
  Scans two specific hosts.

### IP Range
- `nmap 192.168.1.1-10`  
  Scans IPs from `192.168.1.1` to `192.168.1.10`.

### Subnet Scan
- `nmap 192.168.1.0/24`  
  Scans all hosts in the `192.168.1.0` subnet (with a subnet mask of `255.255.255.0`).

### Host Discovery
- `nmap -Pn <target>`  
  Disables host discovery (assuming the target is up) and scans even if the target does not respond to pings.

---

## Service and Version Detection

### Service Detection
- `nmap -sV <target>`  
  Detects the version of services running on open ports by attempting to match them to known service banners.

### Version Detection
- `nmap -A -sV <target>`  
  Performs both OS detection and service version detection.

### Service Version Scanning
- `nmap -p <port> --version-all <target>`  
  Forces Nmap to attempt version detection on all services, even those on uncommon ports.

---

## Port Scanning

### TCP/UDP Scans
- `nmap -p 80,443 <target>`  
  Scans TCP ports `80` and `443` for web traffic.

- `nmap -p U:161,162 <target>`  
  Scans UDP ports `161` and `162` for SNMP traffic.

### Specific Port Scanning
- `nmap -p 22,80 <target>`  
  Scans ports `22` (SSH) and `80` (HTTP) on the target.

### Top Ports Scanning
- `nmap --top-ports 100 <target>`  
  Scans the top 100 most commonly open ports.

### Random Port Scanning
- `nmap -p R:1024-2048 <target>`  
  Scans a random selection of ports within the range `1024-2048`.

---

## Advanced Scanning Techniques

### OS Fingerprinting
- `nmap -O <target>`  
  Attempts to determine the target's operating system based on its network responses.

### TCP/IP Fingerprinting
- `nmap -sS -O <target>`  
  A SYN scan combined with OS detection for more accurate results.

### Scripting Engine
- `nmap --script vuln <target>`  
  Runs Nmap’s vulnerability scripts to check for known vulnerabilities.

### Firewall Evasion
- `nmap -f <target>`  
  Sends fragmented packets to evade packet filters and firewalls.

- `nmap --data-length 400 <target>`  
  Sends packets with a custom data length to evade some IDS/IPS systems.

### Proxy Usage
- `nmap --proxy socks5://127.0.0.1:1080 <target>`  
  Routes the scan through a proxy server.

---

## Scan Performance

### Speed Adjustments
- `nmap -T4 <target>`  
  Sets the scan speed to "Aggressive", reducing scan time while balancing accuracy.

- `nmap -T5 <target>`  
  Uses the highest scan speed, but this can trigger alerts on most networks.

### Timing Templates
- `nmap -T3 <target>`  
  Uses the default timing template, which is balanced in terms of speed and stealth.

### Scan Delay Options
- `nmap --scan-delay 1s <target>`  
  Adds a 1-second delay between probe packets to reduce the likelihood of detection.

### Parallelism and Performance Tuning
- `nmap -P0 -p 80 <target>`  
  Skips host discovery (pings) and directly scans port `80` to save time.

---

## Output Formats

### Normal Output
- `nmap -oN output.txt <target>`  
  Saves the scan results in a human-readable format.

### XML Output
- `nmap -oX output.xml <target>`  
  Outputs scan results in XML format, useful for parsing by scripts.

### Grepable Output
- `nmap -oG output.gnmap <target>`  
  Outputs scan results in a grepable format, which is useful for filtering specific results.

### Script Kiddie Output
- `nmap -oS output.nmap <target>`  
  A simplistic output format often used for quick scans.

### HTML Output
- `nmap -oA output.html <target>`  
  Generates HTML output for visual representation of the scan results.

---

## Scan Customization

### Port Ranges
- `nmap -p 1-1000 <target>`  
  Scans the first 1000 TCP ports on the target.

### Scan Delay and Timeout
- `nmap --host-timeout 10m <target>`  
  Sets a maximum timeout of 10 minutes for the entire host scan.

### Source Port and Packet Manipulation
- `nmap --source-port 53 <target>`  
  Sets the source port to `53` (DNS), often used to bypass filtering mechanisms.

### Fragmentation
- `nmap -f <target>`  
  Fragments packets into smaller pieces to avoid detection by firewalls.

### TTL (Time-to-Live) Adjustments
- `nmap --ttl 64 <target>`  
  Sets the TTL value for the scan packets to `64`, which may help avoid detection.

---

## Nmap Scripting Engine (NSE)

### Nmap Scripting Basics
- `nmap --script=<script_name> <target>`  
  Runs a specific Nmap script.

### Predefined Scripts
- `nmap --script=http-headers <target>`  
  Runs a script that retrieves HTTP headers from the target.

### Writing Custom Scripts
- `nmap --script ./my_custom_script.nse <target>`  
  Runs a user-defined script on the target.

### NSE Script Categories
- `nmap --script-args <category> <target>`  
  Runs scripts from a specific category, like "auth" or "vuln".

### Script Arguments
- `nmap --script-args <argument> <target>`  
  Passes custom arguments to a script.

---

## Host Discovery and Ping

### ICMP Ping
- `nmap -sn -PE <target>`  
  Uses ICMP echo requests to check if the target is alive.

### TCP Ping
- `nmap -sn -PS <target>`  
  Uses TCP SYN packets to determine if a host is online.

### UDP Ping
- `nmap -sn -PU <target>`  
  Uses UDP packets to check for a host.

### ARP Ping
- `nmap -sn -PR <target>`  
  Performs an ARP ping to find hosts on the local network.

---

## Firewall and IDS/IPS Evasion

### Firewall Evasion Techniques
- `nmap --source-port 53 -p 80 <target>`  
  Sends scans from port `53` (DNS) to bypass firewalls.

### IDS/IPS Evasion
- `nmap --badsum <target>`  
  Sends packets with bad checksums to evade intrusion detection systems (IDS).

### Spoofing Techniques
- `nmap --spoof-mac <mac_address> <target>`  
  Spoofs the MAC address of the source machine to avoid detection.

### Decoy Scans
- `nmap -D RND:10 <target>`  
  Uses 10 random decoys to disguise the source of the scan.

---

## Scan Output and Reporting

### Output File Customization
- `nmap -oN output.txt -oX output.xml <target>`  
  Saves the scan in both normal and XML output formats.

### Nmap and Report Generation
- `nmap -oA combined_scan <target1> <target2>`  
  Saves the scan results in all formats (normal, XML, and grepable).

### Combine Multiple Scan Results
- `nmap -oA combined_scan <target1> <target2>`  
  Combines multiple scan results into one file for easier management.

### Data Export and Parsing
- `nmap -oG output.gnmap <target>`  
  Exports the results in a grepable format for later parsing.

---

## Nmap Miscellaneous Options

### Verbosity and Debugging
- `nmap -v <target>`  
  Increases verbosity to show more detailed information during the scan.

- `nmap -d <target>`  
  Enables debugging mode for deeper insight into the scan process.

### Customizing Scan Results
- `nmap --open <target>`  
  Shows only open ports in the output, filtering out closed or filtered ports.

### Scan Timing Adjustments
- `nmap --min-rate 1000 <target>`  
  Sets the minimum packet send rate to `1000` packets per second for faster scans.

### Using Nmap with Proxy
- `nmap --proxy http://proxy_address:8080 <target>`  
  Routes the scan through a specified proxy server.

---

## Nmap in Automation

### Batch Scanning
- `nmap -iL targets.txt`  
  Scans multiple targets from a file (`targets.txt`).

### Nmap in Shell Scripts
- `#!/bin/bash\n nmap -T4 <target>`  
  Example of using Nmap within a shell script to automate scanning.

### Automating Nmap Reports
- `nmap -oX results.xml <target>`  
  Automates Nmap scanning and saves results in XML format for further processing.

---

> **Note**: For more detailed explanations of each command, refer to the Nmap documentation: [Nmap Manual](https://nmap.org/book/)
