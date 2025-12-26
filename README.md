# Nmap — Ultimate Complete Guide (0 to 100)

**Author:** NIGHTFURY0X01 (Arash)  

**Category:** Network Scanning / Reconnaissance  

**Use Cases:** Red Team, Pentesting, Bug Bounty, OSCP 


---

## Table of Contents
1. Introduction to Nmap  
2. Installation & Setup  
3. Nmap Architecture  
4. Basic Scan Types  
5. Host Discovery Techniques  
6. Port Scanning Deep Dive  
7. Service & Version Detection  
8. OS Detection & Fingerprinting  
9. Timing, Performance & Optimization  
10. Firewall / IDS / IPS Evasion  
11. Nmap Scripting Engine (NSE)  
12. Vulnerability Detection  
13. Authentication & Brute Force  
14. Output Formats & Reporting  
15. Real-World Scenarios  
16. Red Team & OSCP Usage  
17. Common Mistakes  
18. Cheat Sheet Tables  
19. Final Thoughts  

---

## 1. Introduction to Nmap

**Nmap (Network Mapper)** is the most important reconnaissance tool in offensive security.  
It allows attackers and defenders to map networks, discover hosts, enumerate services, and detect vulnerabilities.

---

## 2. Installation & Setup

### Linux (Kali / Debian / Ubuntu)
```bash
sudo apt update && sudo apt install nmap
```

### macOS
```bash
brew install nmap
```

### Windows
Download installer → add to PATH.

Verify:
```bash
nmap --version
```

---

## 3. Nmap Architecture

| Component | Description |
|--------|------------|
| Probe Engine | Sends packets |
| Port Scanner | Determines open/closed/filtered |
| Version Detector | Identifies services |
| OS Fingerprinter | TCP/IP stack analysis |
| NSE Engine | Script execution |

---

## 4. Basic Scan Types

| Scan | Command | Description |
|----|--------|------------|
| Default | `nmap target` | Basic scan |
| TCP Connect | `-sT` | Full TCP handshake |
| SYN Scan | `-sS` | Stealth scan |
| UDP Scan | `-sU` | UDP services |
| No Ping | `-Pn` | Bypass ICMP |


## Target Specification
- Single IP: `nmap <target>`
- Multiple IPs: `nmap <t1> <t2>`
- Range: `nmap 192.168.1.1-254`
- Domain: `nmap example.com`
- CIDR: `nmap 192.168.1.0/24`
- From file: `nmap -iL targets.txt`
- Random hosts: `nmap -iR 100`
- Exclude host: `nmap --exclude 192.168.1.1`

## Scan Types
- SYN scan: `-sS`
- TCP connect: `-sT`
- UDP scan: `-sU`
- FIN scan: `-sF`
- Xmas scan: `-sX`
- Null scan: `-sN`

## Host Discovery
- Ping scan: `-sn`
- List only: `-sL`
- TCP ACK: `-PA22,80`
- ICMP echo: `-PE`
- No ping: `-Pn`

## Port Scanning
- Single port: `-p 80`
- Port range: `-p 21-100`
- All ports: `-p-`
- Fast scan: `-F`
- Top ports: `--top-ports 1000`

## Service & Version
- Service detection: `-sV`
- Light: `--version-light`
- Aggressive: `--version-all`

## OS Detection
- OS detect: `-O`
- Aggressive guess: `--osscan-guess`

## Output
- Normal: `-oN out.txt`
- XML: `-oX out.xml`
- Open only: `--open`
- All formats: `-oA scan`

## Timing
- Paranoid: `-T0`
- Normal: `-T3`
- Aggressive: `-T4`
- Insane: `-T5`
- Rate limit: `--max-rate 100`

## Firewall Evasion
- Fragment: `-f`
- MTU: `--mtu 16`
- Random hosts: `--randomize-hosts`
- Decoys: `-D RND:10,ME`
- MAC spoof: `--spoof-mac random`
- Source port: `--source-port 53`
- TTL: `--ttl 128`
- Payload length: `--data-length 50`

## NSE Scripts
- http-enum
- dns-brute
- ssh-brute
- ftp-anon
- smb-enum-shares
- smb-enum-users
- vuln
- vulners


---

## 5. Host Discovery Techniques

```bash
nmap -sn 192.168.1.0/24
nmap -Pn target
nmap -PE target
nmap -PP target
```

| Option | Purpose |
|----|--------|
| -sn | Ping scan only |
| -PE | ICMP Echo |
| -PP | Timestamp |
| -Pn | Assume host is up |

---

## 6. Port Scanning Deep Dive

### Port States
| State | Meaning |
|----|-------|
| Open | Accepting connections |
| Closed | No service |
| Filtered | Firewall present |

### Scan All Ports
```bash
nmap -p- target
```

### Top Ports
```bash
nmap --top-ports 1000 target
```

---

## 7. Service & Version Detection

```bash
nmap -sV target
```

| Flag | Description |
|----|------------|
| --version-intensity | Accuracy level |
| --version-light | Faster |
| --version-all | Maximum detection |

---

## 8. OS Detection & Fingerprinting

```bash
sudo nmap -O target
```

| Result | Meaning |
|----|--------|
| Linux 5.x | Kernel fingerprint |
| Windows Server | TCP/IP behavior |

---

## 9. Timing & Performance

| Template | Speed |
|----|-----|
| -T0 | Extremely slow |
| -T3 | Normal |
| -T4 | Aggressive |
| -T5 | Insane |

---

## 10. Firewall / IDS / IPS Evasion

```bash
nmap -f target
nmap --mtu 24 target
nmap --spoof-mac random target
nmap --randomize-hosts target
```

| Technique | Effect |
|----|------|
| Fragmentation | Bypass filters |
| MTU | Packet evasion |
| MAC Spoof | Anonymity |

---

## 11. Nmap Scripting Engine (NSE)

### Script Categories
| Category | Purpose |
|----|-------|
| default | Safe checks |
| vuln | Vulnerabilities |
| exploit | Active exploitation |
| auth | Authentication |
| brute | Password attacks |

### Run Scripts
```bash
nmap --script vuln target
nmap --script http-* target
```

---

## 12. Vulnerability Detection

```bash
nmap --script vuln target
```

| Script | Vulnerability |
|----|--------------|
| smb-vuln-ms17-010 | EternalBlue |
| http-sql-injection | SQLi |
| ftp-anon | Anonymous FTP |

---

## 13. Authentication & Brute Force

```bash
nmap --script ssh-brute target
nmap --script ftp-brute target
```

⚠️ Use only in authorized environments.

---

## 14. Output Formats & Reporting

```bash
-oN normal.txt
-oX scan.xml
-oG grepable.gnmap
-oA all_formats
```

| Format | Use |
|----|----|
| Normal | Reading |
| XML | Tools |
| Grepable | Automation |

---

## 15. Real-World Scenarios

### Full Recon
```bash
sudo nmap -p- -sS -sV -O -A -T4 target
```
+ -p- → Scan all 65,535 TCP ports (1–65535)

+ -sS → TCP SYN (stealth) scan

+ -sV → Detect services and their versions

+ -O → Detect the target operating system

+ -A → Aggressive mode (OS + version + default scripts + traceroute)

+ -T4 → Aggressive timing (fast but still reliable)

+ target → IP address or domain to scan

### Stealth Scan
```bash
sudo nmap -sS -Pn -T2 target
```

---

## 16. Red Team & OSCP Usage

| Phase | Command |
|----|--------|
| Discovery | `-sn` |
| Enumeration | `-sV` |
| Exploit Prep | `--script vuln` |
| Reporting | `-oA` |

---

## 17. Common Mistakes

- Scanning without permission  
- Ignoring UDP  
- Not saving output  
- Using -T5 blindly  

---

## 18. Ultimate Cheat Sheet

| Goal | Command |
|----|--------|
| Discover Hosts | `nmap -sn subnet` |
| All Ports | `nmap -p- target` |
| Services | `nmap -sV target` |
| OS | `nmap -O target` |
| Vuln | `nmap --script vuln target` |
| Aggressive | `nmap -A target` |

---

## 19. Final Thoughts

Nmap is the **foundation of hacking**.  
If recon is weak, exploitation will fail.

Master Nmap =  
✔ Faster attacks  
✔ Cleaner reports  
✔ Professional mindset  

---

**Author:** NIGHTFURY0X01 (Arash)  
