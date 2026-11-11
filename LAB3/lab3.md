
### Task 1 — 5G SA Connection Verification

| Stage | Successful? | Evidence from logs |
|---|---|---|
| CN5G initialization | Yes | CN5G containers are running and healthy. |
| gNB NG Setup | Yes | AMF shows gNB-OAI with status Connected. |
| UE synchronization | Yes | UE successfully decoded SIB1. |
| RRC connection establishment | Yes | UE entered NR_RRC_CONNECTED state. |
| UE Registration | Yes | AMF shows 5GMM-REGISTERED, IMSI 001010000000001. |
| PDU Session Establishment | Yes | SMF log shows "10.0.0.2 created", and UE tunnel is established. |
| UE tunnel creation | Yes | oaitun_ue1 is UP with IP address 10.0.0.2/24. |

<img width="887" height="567" alt="T1-1" src="https://github.com/user-attachments/assets/c1100a01-433d-4505-a4b8-c8f3dda4cad0" />
<img width="805" height="580" alt="T1_UE" src="https://github.com/user-attachments/assets/f62ceb00-2f60-420f-93b1-60bdd856f6e0" />
<img width="820" height="137" alt="T1_PDU" src="https://github.com/user-attachments/assets/5e057e45-09c5-464a-9bb7-e70c65291422" />

### Task 1 — Questions

**1. Does a successful NG Setup guarantee that a UE can register? Explain.**

No. NG Setup only confirms that the gNB has successfully connected to the AMF. UE registration still requires successful authentication and NAS signaling.

**2. Why can the UE establish an RRC connection but still fail Registration?**

RRC connection establishment only establishes signaling between the UE and gNB. Registration may still fail due to incorrect IMSI, authentication failure, or subscriber configuration errors.

**3. What additional procedure is required after Registration before user data can be exchanged?**

PDU Session Establishment is required. It establishes the user-plane connection and allows the UE to exchange IP packets through the 5G Core Network.

### Task 2 — RFsimulator Communication

| Item | Observed Value |
|---|---|
| gNB process ID | 53832, 53833 |
| nrUE process ID | 54710, 54711 |
| RFsimulator transport protocol | TCP |
| RFsimulator port | 4043 |
| TCP connection state | ESTABLISHED |
| Connected process(es) | nr-softmodem (PID 53832), nr-uesoftmodem (PID 54710) |
<img width="976" height="161" alt="T2-1" src="https://github.com/user-attachments/assets/7e3ee001-8ff6-428c-aec4-c3106c2c5a08" />

### Task 2 — Questions

**1. Why does RFsimulator use a TCP connection instead of a physical antenna?**

RFsimulator uses TCP to exchange IQ samples between the gNB and UE without physical RF hardware. This allows 5G communication to be tested entirely in software.

**2. Are the TCP packets exchanged by RFsimulator the same as the UE's application-layer IP packets?**

No. RFsimulator TCP packets carry IQ samples representing radio signals, while application-layer IP packets are user data transmitted through the 5G PDU Session.

**3. Does using RFsimulator mean that the UE and gNB are performing no physical-layer processing? Explain.**

No. The UE and gNB still perform 5G NR physical-layer processing, including OFDM modulation, channel coding, and decoding. RFsimulator only replaces the physical RF transmission interface.

### Task 3 — End-to-End Connectivity

| Measurement | Observed Value |
|---|---|
| UE IP address | 10.0.0.2 |
| Destination IP | 192.168.70.135 |
| Packets transmitted | 10 |
| Packets received | 10 |
| Packet loss (%) | 0% |
| Minimum RTT (ms) | 5.345 |
| Average RTT (ms) | 8.426 |
| Maximum RTT (ms) | 14.868 |
<img width="980" height="117" alt="T3-1 T1_UE" src="https://github.com/user-attachments/assets/678acc34-66e7-45f4-900e-e60cb06ad027" />
<img width="972" height="227" alt="T3-2ping" src="https://github.com/user-attachments/assets/a47dc0ed-2c3b-42f5-b828-dc425480c3d1" />
<img width="950" height="387" alt="T3UE" src="https://github.com/user-attachments/assets/a1b0027a-6490-4ebb-8505-434382adfc18" />

### Task 3 — Questions

**1. Why must Ping use the oaitun_ue1 interface in this experiment?**

The oaitun_ue1 interface represents the UE's 5G PDU Session. Using this interface ensures that ICMP packets are sent through the simulated 5G user-plane connection instead of the computer's normal network interface.

**2. Does successful Registration guarantee successful user-plane connectivity?**

No. Registration only confirms that the UE has successfully registered with the 5G Core Network. A PDU Session must also be established, and the user-plane routing must work correctly.

**3. What does a successful Ping prove, and what does it not prove about the simulated 5G system?**

A successful Ping proves that IP packets can travel between the UE and External DN through the simulated 5G network. However, it does not prove real wireless coverage, RF performance, or real-world 5G latency.

### Task 4 — Packet Size Comparison

| Scenario | ICMP Payload | Avg. RTT (ms) | Max. RTT (ms) | Packet Loss (%) |
|---|---|---|---|---|
| A | 56 bytes | 6.913 | 11.643 | 0% |
| B | 512 bytes | Pending | Pending | Pending |
| C | 1200 bytes | Pending | Pending | Pending |
<img width="1047" height="822" alt="T4-1" src="https://github.com/user-attachments/assets/09303430-d10e-4443-bb17-a9bf0c53d397" />
<img width="1067" height="815" alt="T4-2" src="https://github.com/user-attachments/assets/722d2a83-4e5e-4ff6-aa86-44fcd3750a08" />
<img width="1061" height="816" alt="T4-3" src="https://github.com/user-attachments/assets/3c3bd344-785a-4d1a-a220-9d737cc638c4" />

### Task 4 — Packet Size Comparison

| Scenario | ICMP Payload | Avg. RTT (ms) | Max. RTT (ms) | Packet Loss (%) |
|---|---|---|---|---|
| A | 56 bytes | 6.913 | 11.643 | 0% |
| B | 512 bytes | 9.175 | 14.919 | 0% |
| C | 1200 bytes | 10.402 | 15.461 | 0% |

### RTT Difference Calculation

RTT Difference = Average RTT(C) - Average RTT(A)

RTT Difference = 10.402 - 6.913 = 3.489 ms

The average RTT increased by 3.489 ms when the ICMP payload size increased from 56 bytes to 1200 bytes.

### Task 4 — Questions

**1. Which scenario has the highest average RTT?**

Scenario C (1200 bytes) has the highest average RTT of 10.402 ms.

**2. Does increasing packet size always increase RTT in your measurements?**

In our measurements, the average RTT increased from 6.913 ms to 9.175 ms and then to 10.402 ms as the packet size increased. However, this does not mean that RTT always increases with packet size in every experiment.

**3. Why might a larger packet require more transmission or processing resources?**

Larger packets contain more data and require more time for transmission and processing. They may also increase the workload of the protocol stack.

**4. What factors other than packet size could affect RTT in a software-based 5G network?**

RTT can be affected by CPU usage, operating system scheduling, virtualization overhead, network congestion, and software processing delays.

**5. Can these results be interpreted as real over-the-air 5G latency? Explain.**

No. RFsimulator replaces physical wireless transmission with TCP communication between the gNB and UE. Therefore, the measured RTT does not include real wireless propagation effects or RF hardware delays.

### Task 5 — Controlled UE Registration Failure

| Item | Observation |
|---|---|
| Modified parameter | IMSI changed from 001010000000001 to 001010000000099 |
| UE synchronized with gNB? | Yes, UE obtained RNTI 1ea9 and exchanged MAC traffic. |
| RRC connection established? | Not directly confirmed by the provided UE log screenshot. |
| Registration successful? | No |
| Error or rejection evidence | AUSF returned CONTEXT_NOT_FOUND (HTTP 404). AMF sent Registration Reject and UE entered 5GMM-DEREGISTERED. |
| Likely cause | The modified IMSI was not found in the subscriber database. |

### Task 5 — Controlled UE Registration Failure

| Item | Observation |
|---|---|
| Modified parameter | IMSI: 001010000000001 → 001010000000099 |
| UE synchronized with gNB? | Yes, UE obtained RNTI 1ea9. |
| RRC connection established? | Not directly confirmed by the captured UE logs. |
| Registration successful? | No |
| Error or rejection evidence | AUSF returned CONTEXT_NOT_FOUND (HTTP 404). AMF sent Registration Reject and UE entered 5GMM-DEREGISTERED. |
| Likely cause | The modified IMSI was not found in the subscriber database. |
<img width="1115" height="907" alt="T5-1" src="https://github.com/user-attachments/assets/3c7f4fdf-ec67-4c54-9b63-432fdded73e9" />
<img width="930" height="372" alt="T5-recover" src="https://github.com/user-attachments/assets/1b166455-859d-4db4-ad8e-eca389318f5f" />

### Recovery Verification

| Item | Observation |
|---|---|
| Restored IMSI | 001010000000001 |
| UE Registration | Successful |
| AMF State | 5GMM-REGISTERED |
| PDU Session ID | 10 |
| UE IP Address | 10.0.0.2 |
| Packets transmitted | 10 |
| Packets received | 10 |
| Packet loss | 0% |
| Average RTT | 7.379 ms |
| Recovery Result | Successful |

### Task 5 — Questions

**1. Why can the UE potentially synchronize with the gNB even when the IMSI is incorrect?**

UE synchronization uses physical-layer signals broadcast by the gNB and does not require IMSI authentication. Therefore, the UE can synchronize with the gNB even if its IMSI is incorrect.

**2. Which network function is involved in UE authentication?**

The AUSF (Authentication Server Function) is responsible for UE authentication, working with the AMF and UDM. In this experiment, the AUSF returned CONTEXT_NOT_FOUND because the modified IMSI was not found.

**3. Why does changing an IMSI affect Registration but not the RFsimulator TCP connection itself?**

The IMSI is used for subscriber identification and authentication during Registration. RFsimulator uses a TCP connection to exchange IQ samples between the gNB and UE, which is independent of IMSI authentication.

**4. What evidence proves that restoring the correct configuration solved the problem?**

After restoring IMSI 001010000000001, the AMF reported 5GMM-REGISTERED. The UE also successfully pinged the External DN with 10 packets received, 0% packet loss, and an average RTT of 7.379 ms.

### Task 6 — RFsimulator vs. Physical 5G

| Feature | OAI RFsimulator | Physical 5G System |
|---|---|---|
| gNB and UE protocol stack | Implemented in software | Implemented in network equipment and UE |
| OFDM signal processing | Performed in software | Performed using baseband processing hardware/software |
| Physical antennas | Not required | Required |
| RF hardware impairments | Not inherently included | Present |
| Wireless propagation effects | Not inherently included; can be modeled | Present in real environments |
| 5G Core connectivity | Supported | Supported |

### Task 6 — Questions

**1. Which parts of the 5G system remain functional when physical RF hardware is replaced with RFsimulator?**

The 5G protocol stack remains functional, including PHY, MAC, RLC, PDCP, RRC, and NAS. The system can still perform UE Registration, PDU Session Establishment, and IP data transmission.

**2. Why is RFsimulator useful for testing the 5G protocol stack?**

RFsimulator allows developers to test 5G communication without expensive RF equipment. It provides a controlled and repeatable environment for debugging protocol procedures.

**3. Why might latency and throughput differ between RFsimulator and a real 5G deployment?**

RFsimulator performance depends on CPU processing, operating system scheduling, and virtualization overhead. Real 5G networks also experience wireless interference, propagation loss, RF hardware delays, and changing channel conditions.

**4. What would need to change if you wanted to test a physical UE instead of OAI nrUE?**

The gNB would need compatible RF hardware and antennas instead of RFsimulator. A physical UE with a properly provisioned SIM or USIM would also be required, along with compatible radio configurations and an authorized RF test environment.

##  Final Analysis

### 1. Describe the complete process from starting the OAI 5GC to successful user-plane connectivity.

First, the OAI 5GC was started using Docker Compose, and all core network services were running successfully. Next, the gNB connected to the AMF through NG Setup, and the UE synchronized with the gNB and established an RRC connection. After successful UE Registration, a PDU Session was established, and the UE obtained the IP address 10.0.0.2. Finally, Ping testing confirmed successful connectivity to the External DN (192.168.70.135) with 0% packet loss.

### 2. Explain the difference between an RFsimulator TCP connection and a 5G PDU Session.

The RFsimulator TCP connection exchanges IQ samples between the gNB and UE without physical RF hardware. In contrast, a 5G PDU Session provides user-plane connectivity between the UE and an external data network. The RFsimulator connection operates as a software-based replacement for radio transmission, while the PDU Session carries user IP traffic. Both are required for end-to-end communication in this experiment.

### 3. Based on your measurements, how did packet size affect RTT and packet loss?

The average RTT increased as the ICMP payload size increased. Scenario A (56 bytes) had an average RTT of 6.913 ms, Scenario B (512 bytes) had 9.175 ms, and Scenario C (1200 bytes) had 10.402 ms. The RTT difference between Scenario C and Scenario A was 3.489 ms. All three scenarios had 0% packet loss, indicating that larger packets increased the observed RTT without affecting packet delivery in this experiment.

### 4. Describe the registration failure you introduced and explain how you diagnosed and recovered from it.

The UE IMSI was changed from 001010000000001 to 001010000000099 to introduce a registration failure. The AUSF returned a CONTEXT_NOT_FOUND error (HTTP 404), and the AMF sent a Registration Reject because the subscriber was not found. After restoring the original IMSI, the UE successfully registered again, as confirmed by the 5GMM-REGISTERED state. A Ping test confirmed recovery with 10 packets received, 0% packet loss, and an average RTT of 7.379 ms.

### 5. Identify two limitations of this simulated environment and explain how they might affect experimental results.

First, RFsimulator does not reproduce real wireless propagation effects such as interference, fading, and path loss by default. Therefore, the measured network performance may differ from real over-the-air 5G communication. Second, the system runs entirely in software on a virtual machine, making its performance dependent on CPU resources and operating system scheduling. These factors may affect RTT and throughput, limiting how accurately the results represent a physical 5G deployment.
<img width="887" height="567" alt="T1-1" src="https://github.com/user-attachments/assets/0202ef04-d8f0-4525-bcbf-21dcd02cda73" />
