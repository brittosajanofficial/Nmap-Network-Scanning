# Task 1: Basic Network Scanning with Nmap

## Objective
Perform a network scan to identify open ports and services on a local machine using Nmap.

---

## Tools Used
- **Nmap 7.99** (via Zenmap GUI)
- **Operating System:** Microsoft Windows 11
- **Target:** localhost (127.0.0.1)

---

## Scan Command
```bash
nmap -sV -T4 -A -v 127.0.0.1
```

### Command Flags Explained
| Flag | Meaning |
|------|---------|
| `-sV` | Detect service versions on open ports |
| `-T4` | Aggressive timing (faster scan) |
| `-A`  | Enable OS detection, version detection, script scanning |
| `-v`  | Verbose output (show more details) |

---

## Scan Results Summary

- **Host:** localhost (127.0.0.1)
- **Status:** Up (0.00033s latency)
- **OS Detected:** Microsoft Windows 11 (24H2 – 25H2)
- **Open Ports Found:** 2
- **Closed Ports:** 998
- **Scan Duration:** 21.95 seconds

---

## Open Ports & Findings

### Port 135/tcp — Microsoft Windows RPC
| Field | Detail |
|-------|--------|
| Port | 135 |
| Protocol | TCP |
| State | Open |
| Service | msrpc |
| Version | Microsoft Windows RPC |

**What is it?**
Port 135 is used by Microsoft Remote Procedure Call (RPC). It acts as an endpoint mapper — it helps Windows services communicate with each other locally and across a network.

**Security Significance:**
- This port is a common target for attackers on Windows systems.
- Historically exploited by worms like **Blaster (MS03-026)**.
- On a local machine, it is normal and expected to be open.
- Should be **blocked at the firewall** to prevent external access.

---

### Port 445/tcp — Microsoft-DS (SMB)
| Field | Detail |
|-------|--------|
| Port | 445 |
| Protocol | TCP |
| State | Open |
| Service | microsoft-ds |
| Version | SMB (Server Message Block) |

**What is it?**
Port 445 is used by **SMB (Server Message Block)** — Windows file and printer sharing protocol. It allows computers to share files, folders, and printers over a network.

**Security Significance:**
- One of the most critical ports to monitor on Windows systems.
- Exploited by **EternalBlue** — the vulnerability behind the **WannaCry ransomware** attack (2017).
- SMB signing is **enabled and required** on this machine (good security practice ✅).
- Should **never be exposed to the internet**.

---

## SMB Security Details (from Script Scan)

```
smb2-security-mode:
  3.1.1:
    Message signing enabled and required ✅
```
SMB message signing being enabled means all SMB communications are digitally signed, protecting against **man-in-the-middle (MITM) attacks**.

---

## OS Detection Results

| Field | Value |
|-------|-------|
| OS | Microsoft Windows 11 |
| Version | 24H2 – 25H2 |
| CPE | cpe:/o:microsoft:windows_11 |
| TCP Sequence Prediction | Difficulty=260 (Very Hard) |
| IP ID Sequence | Incremental |

A TCP Sequence Prediction difficulty of **260** means it is very hard for an attacker to predict TCP sequence numbers, which is a good security indicator.

---

## Key Security Recommendations

1. **Block Port 135 and 445** at your firewall for external traffic.
2. **Keep Windows updated** to patch RPC and SMB vulnerabilities.
3. **Disable SMB v1** if not needed (already using SMB 3.1.1 ✅).
4. **Monitor these ports** regularly for unauthorized access attempts.
5. **Never expose these ports** to the public internet.

---

## Screenshots
![Nmap Scan Output](nmap_Screenshot-1.png)

---

## Files in This Repository

| File | Description |
|------|-------------|
| `nmap_scan_results.txt` | Raw Nmap scan output |
| `README.md` | Full explanation of scan and findings |
| `nmap_Screenshot-1.png` | Screenshot of Zenmap scan output |

---

## Conclusion
This basic Nmap scan revealed that the localhost is running **Windows 11** with two open ports: **135 (RPC)** and **445 (SMB)**. Both are standard Windows services but represent potential attack vectors if exposed externally. The SMB signing configuration found is a positive security measure already in place.
