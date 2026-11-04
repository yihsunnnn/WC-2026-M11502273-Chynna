# Lab2

## Task 1 — Confirm the Traffic Direction

| Scenario | Reverse Value | Direction |
|---|---|---|
| A | 1 | Downlink |
| B | 0 | Uplink |
| C | 0 | Uplink |

**Answer:**

Yes. Scenario A uses downlink (`reverse = 1`),
while Scenarios B and C use uplink (`reverse = 0`).
All three match the scenario descriptions.

### Evidence

**Scenario A**

<img width="1062" height="852" alt="1-A" src="https://github.com/user-attachments/assets/ec3b61a6-9fdc-41c8-8bc2-d828da065024" />

**Scenario B**

<img width="1531" height="871" alt="1-B" src="https://github.com/user-attachments/assets/863b7de4-7761-4e98-b204-282eb90ef33b" />

**Scenario C**

<img width="1545" height="862" alt="1-C" src="https://github.com/user-attachments/assets/13deb94e-9025-40be-a480-e982de90de3e" />


## Task 2 — Compare Throughput

| Scenario | Direction | Throughput (Mbps) |
|---|---|---|
| A | Downlink | 90.65 |
| B | Uplink | 109.12 |
| C | Uplink | 124.42 |

### Calculation

Throughput (Mbps) = bits_per_second / 1,000,000

Improvement (%) = (C - B) / B × 100%

= (124.42 - 109.12) / 109.12 × 100%

= 14.02%

### Answers

1. The downlink throughput in Scenario A is 90.65 Mbps.
2. The uplink throughput in Scenario B is 109.12 Mbps.
3. Scenario C has higher uplink throughput than Scenario B.
4. Scenario C improves uplink throughput by approximately 14.02%.

### Evidence

**Scenario A**

<img width="1152" height="1022" alt="2-A" src="https://github.com/user-attachments/assets/b48d8107-2488-4982-9cac-aa23c7e05c47" />


**Scenario B**

<img width="1242" height="917" alt="2-B" src="https://github.com/user-attachments/assets/5135688f-b6fa-45e8-884d-c5e4dde9c0c6" />


**Scenario C**

<img width="1107" height="1027" alt="2-C" src="https://github.com/user-attachments/assets/694801a5-6868-42d5-a584-133631f93bfb" />


## Task 3 — Check the Trade-off

| Scenario | Throughput (Mbps) | Packet Loss (%) | Jitter (ms) |
|---|---|---|---|
| B | 109.12 | 0.2357 | 0.1162 |
| C | 124.42 | 2.1339 | 0.1620 |
<img width="492" height="705" alt="2-B,C" src="https://github.com/user-attachments/assets/3a659f45-c72c-42f6-aaf4-e25ea839318d" />

### Answers

1. **Yes.** Scenario C has higher uplink throughput (124.42 Mbps) than B (109.12 Mbps).

2. **No.** Scenario C has higher packet loss (2.1339%) than B (0.2357%).

3. Compared with Scenario B, Scenario C has higher throughput, higher packet loss, and higher jitter. These results suggest a trade-off between throughput and transmission quality.


## Task 4.3 — Record the TDD Parameters

The TDD parameters were obtained from SIB1 using Wireshark.

| Item | DL-heavy: A/B | Balanced: C |
|---|---|---|
| Configuration capture filename | `tdd-dl-heavy-sib1.pcapng` | `tdd-balanced-sib1.pcapng` |
| SIB1 packet number | 2 | 2 |
| `referenceSubcarrierSpacing` (kHz) | 30 | 30 |
| `dl-UL-TransmissionPeriodicity` (ms) | 5 | 2.5 |
| `nrofDownlinkSlots` | 7 | 2 |
| `nrofDownlinkSymbols` | 6 | 6 |
| `nrofUplinkSlots` | 2 | 2 |
| `nrofUplinkSymbols` | 4 | 4 |
| Is `pattern2` present? | No | No |

<img width="1386" height="902" alt="4-heavy" src="https://github.com/user-attachments/assets/71daee8f-2718-4650-b9c1-69878db70a4e" />


### Observation

Both configurations use a reference subcarrier spacing of 30 kHz. DL-heavy has a 5 ms TDD period with 7 full downlink slots, while Balanced has a 2.5 ms period with 2 full downlink slots. Both configurations have 2 full uplink slots per period.


## Task 4.4 — Draw the TDD Time Allocation

### 1. TDD Time Allocation Diagrams

Both configurations use a reference subcarrier spacing of 30 kHz. Each slot lasts 0.5 ms and contains 14 OFDM symbols.

D = Downlink, U = Uplink, F = Flexible.

**DL-heavy Configuration (A/B)**

<img width="2459" height="668" alt="image" src="https://github.com/user-attachments/assets/bf34f89a-da9d-4654-ad20-de5eff8f8fbc" />

**Balanced Configuration (C)**

<img width="2459" height="668" alt="image" src="https://github.com/user-attachments/assets/3f064350-a458-4215-ac4a-4020efd89612" />

### 2. Symbol Count Calculation

The total number of reference slots is calculated as:

Total Slots = TDD Period / Slot Duration

Configured DL Symbols = 14 × Full DL Slots + Partial DL Symbols

Configured UL Symbols = 14 × Full UL Slots + Partial UL Symbols

Flexible Symbols = Total Symbols - DL Symbols - UL Symbols

UL Symbol Share (%) = UL Symbols / Total Symbols × 100%

**DL-heavy (A/B):**

- Total Slots = 5 / 0.5 = 10
- Total Symbols = 10 × 14 = 140
- DL Symbols = 7 × 14 + 6 = 104
- UL Symbols = 2 × 14 + 4 = 32
- Flexible Symbols = 140 - 104 - 32 = 4
- UL Symbol Share = 32 / 140 × 100% = 22.86%

**Balanced (C):**

- Total Slots = 2.5 / 0.5 = 5
- Total Symbols = 5 × 14 = 70
- DL Symbols = 2 × 14 + 6 = 34
- UL Symbols = 2 × 14 + 4 = 32
- Flexible Symbols = 70 - 34 - 32 = 4
- UL Symbol Share = 32 / 70 × 100% = 45.71%

### 3. Symbol Count per Repeating Sequence

| TDD Setting | Repeating Duration (ms) | Total Reference Slots | DL Symbols | UL Symbols | Flexible Symbols | Configured UL Symbol Share (%) |
|---|---:|---:|---:|---:|---:|---:|
| DL-heavy | 5 | 10 | 104 | 32 | 4 | 22.86 |
| Balanced | 2.5 | 5 | 34 | 32 | 4 | 45.71 |

### 4. Symbol Count over the Same 5 ms Interval

| TDD Setting | Repeating Duration (ms) | Repetitions in 5 ms | DL Symbols in 5 ms | UL Symbols in 5 ms | Flexible Symbols in 5 ms |
|---|---:|---:|---:|---:|---:|
| DL-heavy | 5 | 1 | 104 | 32 | 4 |
| Balanced | 2.5 | 2 | 68 | 64 | 8 |

### 5. Question and Answer

**Question:** Can two patterns have the same UL symbol count per period but provide different numbers of UL symbols per unit time?

**Answer:**

Yes. Both configurations have 32 UL symbols per period, but their periods are different. DL-heavy uses a 5 ms period, while Balanced uses a 2.5 ms period. Therefore, within the same 5 ms interval, Balanced provides 64 UL symbols, twice as many as DL-heavy (32 symbols).



## Task 4.5 — Connect SIB1 to the Performance Results

### 1. Performance Comparison

| Scenario | Reference TDD Setting | JSON Traffic Direction | Configured UL Symbol Share (%) | Throughput (Mbps) | Packet Loss (%) |
|---|---|---|---:|---:|---:|
| A | DL-heavy | Downlink | 22.86 | 90.65 | 0.4430 |
| B | DL-heavy | Uplink | 22.86 | 109.12 | 0.2357 |
| C | Balanced | Uplink | 45.71 | 124.42 | 2.1339 |

### 2. Questions and Answers

**Q1. Which decoded TDD parameters differ between DL-heavy and Balanced?**

The TDD periodicity and number of full downlink slots differ. DL-heavy uses a 5 ms period with 7 DL slots, while Balanced uses a 2.5 ms period with 2 DL slots.

**Q2. Which configuration provides more configured UL symbols per unit time? Support your answer with the diagram and calculated share.**

Balanced provides more UL resources, with a UL symbol share of 45.71%, compared with 22.86% for DL-heavy. Within 5 ms, Balanced provides 64 UL symbols, while DL-heavy provides only 32.

**Q3. How does that difference help explain the B/C throughput comparison?**

Scenario C has more configured UL resources, which may help explain its higher uplink throughput (124.42 Mbps) compared with Scenario B (109.12 Mbps). However, other factors may also affect throughput.

**Q4. Why can A and B use the same SIB1 TDD configuration even though their traffic directions differ?**

Scenarios A and B use the same DL-heavy TDD configuration but test different traffic directions. Scenario A measures downlink performance, while Scenario B measures uplink performance. The TDD configuration supports both directions.


## Task 5 — Is the TDD Setting Suitable?

### 1. TDD Configuration Suitability

| Scenario | TDD Setting | Traffic Direction | Is the Setting Suitable for This Direction? Why? |
|---|---|---|---|
| A | DL-heavy | Downlink | Yes. More DL resources are available, making it suitable for downlink traffic. |
| B | DL-heavy | Uplink | Less suitable. Only 22.86% of symbols are configured for UL, which may limit uplink performance. |
| C | Balanced | Uplink | Yes. It provides 45.71% UL symbols, allowing more uplink transmission opportunities. |

### 2. Question and Answer

**Q: Why might Scenario C achieve better uplink throughput than Scenario B? Refer to your SIB1 diagrams and measured results.**

**Answer:**

Scenario C uses a Balanced TDD configuration with 45.71% UL symbols, while Scenario B uses DL-heavy with only 22.86%. This provides more uplink transmission opportunities in Scenario C, which may help explain its higher throughput (124.42 Mbps) compared with Scenario B (109.12 Mbps).

However, Scenario C also has higher packet loss (2.1339%) than Scenario B (0.2357%), indicating a trade-off between throughput and packet loss. Other factors may also influence the results.


## Task 6 — Find GTP-U Information

### 1. GTP-U Packet Information

| Item | Value |
|---|---|
| Scenario capture filename | `scenario-a-dl-heavy-downlink.pcapng` |
| Packet number | 1831 |
| Outer source IP | 192.168.70.129 |
| Outer destination IP | 192.168.70.134 |
| Inner source IP | 10.0.0.6 |
| Inner destination IP | 192.168.70.135 |
| TEID | 0x00000011 (17) |

### 2. Explanation

The outer IP addresses identify the GTP-U tunnel endpoints, while the inner IP addresses represent the original UE/application packet.

The TEID (0x00000011) identifies the GTP-U tunnel at the receiving endpoint.

### 3. Evidence

**Outer IP and TEID**

<img width="1261" height="1140" alt="6-1" src="https://github.com/user-attachments/assets/883dece3-3502-4e56-bcd3-2fd4aed97d97" />


**Inner IP**

<img width="1261" height="1137" alt="6-2" src="https://github.com/user-attachments/assets/fe1fc21e-c74d-4ab0-a53e-abbe13acdf18" />



## Task 7 — TCP Retransmissions

### 1. TCP Retransmission Results

Display Filter: `tcp.analysis.retransmission`

| Scenario | Displayed TCP Retransmissions |
|---|---:|
| A | 14 |
| B | 12 |
| C | 12 |

### 2. Question and Answer

**Q: Do these TCP retransmissions directly represent lost UDP performance packets?**

No. The iperf3 performance tests use UDP traffic, while TCP is used for the control connection. Therefore, TCP retransmissions do not directly represent UDP packet loss.

### 3. Evidence

**Scenario A**

<img width="1260" height="1141" alt="7-A" src="https://github.com/user-attachments/assets/6e06142e-1f18-4813-a5ca-8d54b35dbf14" />


**Scenario B**

<img width="1227" height="1135" alt="7-B" src="https://github.com/user-attachments/assets/16e79dd9-8507-4bc8-9baf-caba83085d7f" />

**Scenario C**

<img width="1227" height="1137" alt="7-C" src="https://github.com/user-attachments/assets/39a409db-6fdf-40a5-bd83-3ec07ecd3cbd" />



## Task 8 — RTT and Packet Interval

### 8.1 TCP RTT

Display Filter: `tcp.analysis.ack_rtt`

| Scenario | Packet Number | TCP ACK RTT |
|---|---:|---:|
| A | 1833 | 0.485 ms |
| B | 2068 | 0.331 ms |
| C | 1441 | 11.405 ms |

These values represent individual TCP ACK RTT samples from the iperf3 control connections, not UDP data packet RTTs.

### 8.2 Additional Observation — TCP Packet Interval

Display Filter: `tcp.analysis.ack_rtt`

| Scenario | Packet Number | Time Delta from Previous Displayed Frame |
|---|---:|---:|
| A | 1842 | 18.162 ms |
| B | 2076 | 16.742 ms |
| C | 1441 | 11.405 ms |

The measured intervals represent the time differences between consecutive displayed TCP packets under the selected Wireshark filter.

These values are not UDP data packet intervals and should not be interpreted as TDD slot durations.

### Evidence

**Scenario A**

<img width="1227" height="1136" alt="8-A" src="https://github.com/user-attachments/assets/d738dcab-c9fb-4f22-adf0-96dcff1553f2" />


**Scenario B**

<img width="1227" height="1136" alt="8-B" src="https://github.com/user-attachments/assets/ab915afd-d4cb-40c7-9e53-3e29f7d0941b" />


**Scenario C**

<img width="1230" height="1137" alt="8-C" src="https://github.com/user-attachments/assets/6aa5b17d-d100-4c88-888f-2342aa001245" />



## 11. Final Questions

### Q1.
Yes. Scenario A uses DL-heavy TDD, which is suitable for downlink traffic.

### Q2.
Scenario B has only 32 UL symbols out of 140 (22.86%), limiting uplink opportunities.

### Q3.
Throughput increases from 109.12 to 124.42 Mbps, an improvement of 14.02%.

### Q4.
Scenario C has higher packet loss (2.1339%) and jitter (0.1620 ms).

### Q5.
More UL resources provide more transmission opportunities, potentially increasing throughput.

### Q6.
No. Scenario C has higher throughput but also higher packet loss (2.1339%).

### Q7.
Test an intermediate TDD configuration to balance throughput and packet loss.

### Q8.
SIB1 captures show TDD configurations, while JSON shows performance results. Separately recorded captures cannot prove the exact settings used in earlier tests.
