# Layer 7 DNS TTL vs. Layer 3 IP TTL Hands-On Lab

## Project Overview
Practical verification of Time to Live (TTL) mechanisms at both the Application Layer (DNS Record Caching) and Network Layer (Packet Loop Termination) as part of CompTIA Network+ (N10-009) Objective 1.2.

---

## Lab Execution & Findings

### 1. Layer 7: DNS TTL Caching (`nslookup` & `dig`)
- **Execution:** Issued `nslookup -debug www.professormesser.com` on Windows and `dig www.professormesser.com` on Kali Linux.
- **Finding:** The authoritative DNS response included `ttl = 300 (5 mins)`. Sequential queries showed the local DNS resolver decrementing the counter live (e.g., down to `14` seconds) before flushing the cached IP address.
- **Wireshark Verification:** Filtered `dns || icmp` traffic to observe DNS Standard Queries over UDP Port 53 originating from `192.168.1.14` to default gateway `192.168.1.1`.

### 2. Layer 3: IP TTL & Loop Prevention (`ping -i`)
- **Execution:** Issued `ping -i 1 104.20.22.204` on Windows host command prompt to restrict packet hop limit to 1.
- **Finding:** Default gateway `192.168.1.1` received the packet, decremented the TTL field from `1` to `0`, dropped the packet, and returned an ICMP `TTL expired in transit` response.
- **Takeaway:** Confirmed that network layer gateways strictly enforce TTL decrements to prevent rogue packets from endlessly looping and exhausting bandwidth.

---

## Tools Used
- Wireshark
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/67541e41-37d0-4d8f-a860-4b7b7b73a9c3" />

- Windows Command Prompt (`nslookup`, `ping`)
<img width="855" height="961" alt="image" src="https://github.com/user-attachments/assets/b124c523-2657-437e-9845-344382c6475e" />
<img width="639" height="338" alt="image" src="https://github.com/user-attachments/assets/4c0dce4a-2802-4a1f-bd01-3078a30602bc" />

- Kali Linux Terminal (`dig`, `traceroute`)
<img width="1599" height="1599" alt="image" src="https://github.com/user-attachments/assets/e5520a32-a5ac-499e-a9aa-a9b3e4e40a8c" />
<img width="1599" height="1599" alt="image" src="https://github.com/user-attachments/assets/187690bb-dc55-4885-8331-e6af715ecac9" />
