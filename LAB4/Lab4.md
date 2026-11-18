# LAB 4
## Table of Contents

- [Task 1 — Inspect the Split Configurations](#task-1)
- [Task 2 — Deploy the Split 5G SA Network](#task-2)
- [Task 3 — Capture F1-C and F1-U](#task-3)
- [Task 4 — Analyze F1-C (F1AP / SCTP)](#task-4)
- [Task 5 — Analyze F1-U (GTP-U / UDP)](#task-5)
- [Task 6 — Compare Monolithic and Split gNB](#task-6)
## Task 1 — Inspect the Split Configurations

### Configuration Summary

| Parameter | Observed Value |
|---|---|
| OAI Version | v2.4.0 |
| CU F1 IP Address | 127.0.0.3 |
| DU F1 IP Address | 127.0.0.4 |
| DU SSB NR-ARFCN | 630048 |
| DU SSB Frequency | 3450.72 MHz |
| PRB Count | 106 |
| Physical Cell ID | 0 |
| AMF IP Address | 192.168.70.132 |

### Questions

**Q1. Which protocol layers mainly reside in the CU, and which reside in the DU?**

The CU handles RRC and PDCP. The DU handles RLC, MAC, and PHY.

**Q2. What is the purpose of the F1 interface?**

The F1 interface connects the CU and DU. F1-C carries control messages, while F1-U carries user data.

**Q3. Why must the UE radio frequency be compatible with the DU rather than taken blindly from the previous lab?**

The UE must use a frequency compatible with the DU to synchronize and communicate. A wrong frequency may cause connection failure.

### Evidence

<img width="878" height="766" alt="task1_configuration" src="https://github.com/user-attachments/assets/1bd038b7-f537-49ea-a78f-3ffbbb362e80" />


## Task 2 — Deploy the Split 5G SA Network

### Experimental Results

| Parameter | Result |
|---|---|
| CN5G Status | 10 containers healthy |
| CU NG Setup | Successful |
| DU Startup | Successful |
| UE IP Address | 10.0.0.2/24 |
| Destination IP | 192.168.70.135 |
| Packets Transmitted | 10 |
| Packets Received | 10 |
| Packet Loss | 0% |
| Average RTT | 10.067 ms |

### Analysis

The CU successfully connected to the 5G Core Network.

The DU and UE were started successfully. The UE obtained the IP address 10.0.0.2.

The Ping test showed 0% packet loss and an average RTT of 10.067 ms.

This confirms successful end-to-end connectivity through the split CU/DU network.

### Evidence

- Screenshot 1: CU startup.
- <img width="1047" height="577" alt="task2_cu_startup" src="https://github.com/user-attachments/assets/588ce2f5-df6a-4eac-9d04-6582fe696346" />
- <img width="802" height="741" alt="task2_cu_startup2" src="https://github.com/user-attachments/assets/f4d3d031-02be-4b1c-9bc3-b70a29234fd0" />

- Screenshot 2: DU startup.
- <img width="996" height="657" alt="task2_du_startup" src="https://github.com/user-attachments/assets/f1ec9ffe-22d3-474a-9917-c770d9153552" />

- Screenshot 3: UE IP address and Ping results.
- <img width="807" height="552" alt="task2_ue_ip" src="https://github.com/user-attachments/assets/a516ee75-26fa-453a-ac58-79907a92d6c1" />



## Task 3 — Capture F1-C and F1-U

### Capture Results

| Parameter | Result |
|---|---|
| Capture Interface | lo |
| PCAP File | lab4-f1-complete.pcap |
| File Size | 26 KB |
| CU F1 IP | 127.0.0.3 |
| DU F1 IP | 127.0.0.4 |
| SCTP Packets | Captured |
| UDP Port 2152 Packets | Captured |

### Analysis

The packet capture successfully recorded SCTP and UDP traffic between the CU and DU.

The SCTP packets included INIT, INIT ACK, COOKIE ECHO, and COOKIE ACK. These messages confirm that the SCTP association was established.

UDP packets were observed from the DU (127.0.0.4) to the CU (127.0.0.3).

The UDP packets will be analyzed in Wireshark to verify whether they contain GTP-U user-plane data.

### Evidence

- Screenshot: task3_packet_capture.png
- <img width="972" height="817" alt="T3ping_30" src="https://github.com/user-attachments/assets/e2fabed1-0f88-4577-a51c-bea21d00d635" />


## Task 4 — Analyze F1-C (F1AP / SCTP)

### F1 Setup Analysis

| Packet No. | Source | Destination | Message |
|---|---|---|---|
| 5 | 127.0.0.4 (DU) | 127.0.0.3 (CU) | F1 Setup Request |
| 7 | 127.0.0.3 (CU) | 127.0.0.4 (DU) | F1 Setup Response |
| 9 | 127.0.0.4 (DU) | 127.0.0.3 (CU) | GNBDUConfigurationUpdate |

The DU sends an F1 Setup Request to the CU.

The CU sends an F1 Setup Response to accept the connection.

The DU then sends a configuration update.

This confirms that F1 Setup was successful.


### UE Context Modification

| Packet No. | Source | Destination | Message |
|---|---|---|---|
| 51 | 127.0.0.3 (CU) | 127.0.0.4 (DU) | UE Context Modification Request |

### Analysis

The CU sends a UE Context Modification Request to the DU.

This message is used to update UE-specific configuration.

The packet contains the RRC Reconfiguration Complete Indicator.

This shows that the CU and DU can update UE context information through F1-C.

### Evidence

<img width="952" height="637" alt="task4_f1_setup" src="https://github.com/user-attachments/assets/8714f033-bd96-4408-bd24-602c8e409ba4" />



### GTP-U Packet Analysis

| Parameter | Result |
|---|---|
| Packet Number | 54 |
| Source IP | 127.0.0.4 (DU) |
| Destination IP | 127.0.0.3 (CU) |
| Source Port | 2152 |
| Destination Port | 2152 |
| Protocol | GTP-U over UDP |
| Message Type | T-PDU (0xff) |
| TEID | 0x4d1fe40d |
| TEID (Decimal) | 1293935629 |
| Packet Length | 101 bytes |
| Direction | Uplink (DU to CU) |


## Task 4 — Questions

**Q1. Which node initiates F1 Setup, and which acknowledges it?**

The DU initiates F1 Setup by sending an F1 Setup Request. The CU acknowledges it by sending an F1 Setup Response.

**Q2. Explain the difference between SCTP association establishment and F1 Setup.**

SCTP establishes a reliable transport connection between the CU and DU. F1 Setup exchanges configuration information to establish the F1 interface.

**Q3. How can a NAS Registration Request be transported over F1-C?**

The UE sends a NAS Registration Request inside an RRC message. The DU forwards the RRC message to the CU using F1AP over SCTP.

**Q4. What is the role of UE Context Setup?**

UE Context Setup establishes UE-specific resources and configuration in the DU so the UE can communicate through the network.

**Q5. Does F1 Setup alone prove successful UE Registration? Explain.**

No. F1 Setup only confirms that the CU and DU have established the F1 interface. The UE must still complete RRC connection and NAS registration procedures, including authentication.


### Evidence

<img width="1036" height="931" alt="T4-2" src="https://github.com/user-attachments/assets/dc849e3f-9228-465c-8433-67f696e758ba" />




## Task 5 — Analyze F1-U (GTP-U / UDP)

### UL / DL Packet Count

| Direction | Source | Destination | Packet Count |
|---|---|---|---|
| UL | 127.0.0.4 (DU) | 127.0.0.3 (CU) | 47 |
| DL | 127.0.0.3 (CU) | 127.0.0.4 (DU) | 40 |
| Total | CU ↔ DU | F1-U | 87 |

### GTP-U Tunnel Information

| Parameter | Uplink | Downlink |
|---|---|---|
| Packet Number | 54 | 76 |
| Source IP | 127.0.0.4 | 127.0.0.3 |
| Destination IP | 127.0.0.3 | 127.0.0.4 |
| Protocol | GTP-U | GTP-U |
| UDP Port | 2152 | 2152 |
| Message Type | T-PDU | T-PDU |
| TEID | 0x4d1fe40d | 0x8c2ac7bb |

### Analysis

The capture contains 87 GTP-U packets.

There are 47 uplink packets and 40 downlink packets.

Uplink traffic is sent from the DU to the CU. Downlink traffic is sent from the CU to the DU.

The two directions use different TEIDs to identify their GTP-U tunnel endpoints.

These results confirm successful bidirectional user-plane communication over F1-U.

### Evidence

<img width="1036" height="956" alt="T5_ul" src="https://github.com/user-attachments/assets/6471ca9b-ec8d-492c-934e-e84b264b2bf5" />

<img width="1042" height="961" alt="T5_dl" src="https://github.com/user-attachments/assets/b2e1cdb2-9cef-48bd-8ad4-b41459cffb56" />


### Questions

**Q1. Which transport protocol carries F1-U?**

F1-U uses GTP-U over UDP to carry user-plane data.

**Q2. What does a GTP-U TEID identify?**

A TEID identifies a GTP-U tunnel endpoint. It helps the receiver identify the correct user data tunnel.

**Q3. Which direction is 127.0.0.4 → 127.0.0.3 in this setup?**

It is the uplink direction, from the DU to the CU.

**Q4. How would you distinguish F1-U from N3 in a packet capture?**

F1-U carries GTP-U packets between the DU and CU.

N3 carries GTP-U packets between the CU and the 5G Core UPF.

We can distinguish them by checking the source and destination IP addresses.

**Q5. Why do F1-C and F1-U use different transport protocols?**

F1-C uses SCTP for reliable control signaling.

F1-U uses GTP-U over UDP for efficient user data transmission.


## Task 6 — Compare Monolithic and Split gNB

### Comparison Table

| Feature | Lab 3: Monolithic gNB | Lab 4: CU/DU Split |
|---|---|---|
| Number of RAN Processes | 1 gNB process | 2 processes (CU + DU) |
| Location of RRC/PDCP | Inside gNB | CU |
| Location of RLC/MAC/PHY | Inside gNB | DU |
| F1-C Interface Present? | No | Yes |
| F1-U Interface Present? | No | Yes |
| UE Connected to the Same 5GC? | Yes | Yes |
| End-to-End Ping Works? | Yes | Yes |

### Explanation

In Lab 3, all RAN protocol layers run in one gNB process.

In Lab 4, the CU handles RRC and PDCP, while the DU handles RLC, MAC, and PHY.

The F1-C and F1-U interfaces connect the CU and DU.

However, the UE still needs radio synchronization, registration, and PDU session establishment.

Both architectures allow the UE to connect to the 5G Core Network and communicate with the external data network.

### Conclusion

The CU/DU split separates control functions and lower-layer radio processing into different processes.

F1-C uses F1AP over SCTP for control signaling, while F1-U uses GTP-U over UDP for user data.

The experiment successfully captured F1AP messages and bidirectional GTP-U packets.

The UE also obtained an IP address and completed the Ping test with 0% packet loss.

This confirms that the split gNB architecture supports successful end-to-end 5G communication.
