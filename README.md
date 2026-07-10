# Building a Secure Small Network
### TechCrush Capstone Project

## Project Overview
This project focuses on the practical design, deployment, and security hardening of a small network environment. Using industry-standard networking and penetration testing utilities, a baseline network layout was established, audited for structural vulnerabilities, and systematically hardened. The primary objective is to implement defensive technical controls—such as host-based firewalls and protocol deactivation—and empirically verify their effectiveness against common network risks.

## Objectives
1. **Architectural Design:** Map a functional small network topology incorporating key infrastructure devices (routers, switches, servers, workstations, and peripherals).
2. **Baseline Security Assessment:** Conduct passive packet analysis and active reconnaissance scanning to identify open ports, active services, and legacy protocols.
3. **Attack Surface Reduction:** Harden the operating environment by terminating unnecessary high-risk network services.
4. **Network Perimeter Control:** Enforce custom access control policies using host-based firewall structures to block unsolicited traffic.
5. **Defensive Validation:** Re-scan and simulate connection anomalies to empirically verify the operational success of the applied security controls.

## Project Methodology & Phases
The deployment followed a structured five-phase technical methodology:

*   **Phase 1: Architectural Design & Planning:** Constructing the logical topology and detailing IP allocation schemes using network simulation software.
*   **Phase 2: Environment Deployment & Baseline Assessment:** Provisioning the virtualization space and performing initial discovery scans to highlight pre-existing security gaps.
*   **Phase 3: Security Control Implementation:** Executing service configuration changes and engineering active firewall filters.
*   **Phase 4: Threat Simulation & Verification:** Conducting verification testing (post-hardening scans and connectivity stress checks) to ensure real-world threat mitigation.
*   **Phase 5: Documentation & Review:** Compiling operational logs, command strings, and comparative network telemetry into an administrative audit trail.

## Tech Stack & Tools Used
*   **Network Diagramming:** Cisco Packet Tracer (Topology planning and design verification)
*   **Virtualization Hypervisor:** VMware Workstation
*   **Operating System Platform:** Kali Linux 
*   **Network Reconnaissance / Port Scanner:** Nmap (Service version detection and state auditing)
*   **Network Protocol Analyzer:** Wireshark (Live packet-capture engine on interface `eth0`)
*   **Native Perimeter Control:** Netfilter Kernel Engine (`iptables` rule engineering)
*   **Service Manager System:** Systemd Core (`systemctl`)

## Implementation Details

### 1. Network Architecture Blueprint
The planned small network infrastructure contains segmented, distinct endpoints managed through a central distribution layer:
*   **Gateway Layer:** Edge Router coupled directly with a Perimeter Firewall mapping to the external internet.
*   **Switching Infrastructure:** Central Local Area Network (LAN) Switch distributing local traffic.
*   **Network Assets:** Local Server, Corporate Workstation, standard End-User PCs, a Wireless Access Point managing Laptop clients, and discrete internal peripherals (PDA, VoIP Phone).

### 2. Reconnaissance & Baseline Assessment
Before applying security controls, the active network space was analyzed to document the baseline exposure:
*   **Interface Inspection:** Interrogating local interfaces using `ifconfig` identified the active subnet structure on primary network device `eth0` ($192.168.236.10/24$).
*   **Initial Nmap Scan:** An active service detection scan (`nmap -sV 192.168.236.10`) revealed that **Port 22/tcp (SSH)** running OpenSSH 10.2p1 Debian 5 was completely open and exposed to potential brute-force or exploitation attempts.
*   **Passive Traffic Capture:** Initialized a live Wireshark session on `eth0` to monitor broadcast/multicast noise. The capture highlighted a high volume of background data flows, specifically Link-Local Multicast Name Resolution (LLMNR), Multicast DNS (mDNS) strings routing to $224.0.0.251$, and Simple Service Discovery Protocol (SSDP) discovery streams routing to destination address $239.255.255.250$.

### 3. Hardening & Security Controls Enforcement
To remediate the gaps discovered during assessment, two core defensive actions were taken:

#### Service Termination (Principle of Least Functionality)
To eliminate the open remote-access attack surface, the active SSH daemon was permanently terminated using the system controller:
```bash
sudo systemctl stop ssh
