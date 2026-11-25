## Index

- [Task 1: From Cell Search to Msg2 RAR](#task-1-from-cell-search-to-msg2-rar)
- [Task 2: Complete Random Access and RRC Connection Setup](#task-2-complete-random-access-and-rrc-connection-setup)
- [Task 3: Initial Access MSC Diagram](#task-3-initial-access-msc-diagram)

## Task 1: From Cell Search to Msg2 RAR

### Step 1: Cell Search and Initial Synchronization

| Item | Result |
|---|---|
| Starting cell search | Line 56 |
| Initial synchronization | Successful |
| Initial sync successful | Line 63 |
| PCI | 0 |

**Screenshot:**


**Explanation:**

The UE starts cell search at line 56 and successfully
synchronizes with the gNB at line 63. The detected PCI is 0.

**Question:**
After completing initial synchronization, what is the UE able to do?

**Answer:**
The UE can synchronize its timing and frequency with the
gNB, decode system information, and prepare for Random Access.

### Step 2: Find MIB and SIB1

| Message | Frame Number |
|---|---|
| MIB | 2 |
| SIB1 | 1 |

**MIB Screenshot:**


**Explanation:**

MIB provides basic cell information and helps the UE
locate and decode SIB1.

**SIB1 Screenshot:**


**Explanation:**

SIB1 provides cell access information and common
radio resource configurations, including Random
Access configuration.

The UE needs this information to prepare for
the Random Access procedure.

### Step 3: Find Msg1 PRACH

| Item | Result |
|---|---|
| Msg1 log line number | 81 |
| frame.slot | 553.19 |
| preambleIndex | 16 |
| Wireshark Frame Number | 489 |
| RAPID | 16 |

**UE Log Screenshot:**


**Explanation:**

The UE transmits the PRACH preamble at frame.slot
553.19 with preambleIndex 16.

**Wireshark Screenshot:**


**Explanation:**

Frame 489 shows a RACH preamble event with RAPID 16.

**Questions and Answers:**

1. Which Random Access message is this PRACH transmission?

   It is Msg1 of the four-step Random Access procedure.

2. At which frame.slot and with which preambleIndex
   does the UE log show transmission?

   The frame.slot is 553.19, and the preambleIndex is 16.

3. Does the Wireshark RAPID match the UE-log
   preambleIndex? What does this show?

   Yes. Both values are 16. This shows that the gNB-side
   trace records the same preamble index transmitted
   by the UE.
   ### Step 4: Find Msg2 RAR

| Item | Result |
|---|---|
| Frame Number | 490 |
| RAPID | 16 |
| Temporary C-RNTI | 0xf21e (61982) |
| Timing Advance | 0 |
| RA-RNTI | 271 |

**Screenshot:**


**Explanation:**

Frame 490 shows the Random Access Response (Msg2)
sent by the gNB.

The RAPID is 16, which matches the preambleIndex
in Msg1.

The gNB assigns a Temporary C-RNTI of 61982 and
provides an uplink grant for the UE to transmit Msg3.

## Task 2: Complete Random Access and RRC Connection Setup

### Step 1: Find RRCSetupRequest

| Item | Result |
|---|---|
| Frame Number | 491 |
| Context RNTI | 0xf21e (61982) |

**Screenshot:**


**Explanation:**

Frame 491 shows the RRCSetupRequest (Msg3)
sent from the UE to the gNB.

**Questions and Answers:**

1. Does the Context RNTI match the Temporary C-RNTI
   in the RAR? What does this show?

   Yes. Both values are 61982. This shows that the UE
   uses the Temporary C-RNTI assigned by the gNB
   during Random Access.

2. Can you claim that the RRC connection has been
   established after seeing only RRCSetupRequest?
   Why or why not?

   No. RRCSetupRequest only requests an RRC connection.
   The gNB must send RRCSetup, and the UE must
   respond with RRCSetupComplete.

   ### Step 2: Find RRCSetup

| Item | Result |
|---|---|
| Frame Number | 492 |
| Context RNTI | 0xf21e (61982) |
| rrc-TransactionIdentifier | 1 |
| Contention Resolution | Yes |

**Screenshot:**


**Explanation:**

Frame 492 shows the RRCSetup message sent from
the gNB to the UE. The transaction identifier is 1,
and Contention Resolution is present.

**Questions and Answers:**

1. Which message is this in four-step Random Access?

   It is Msg4 of the four-step Random Access procedure.

2. What does it mean that Contention Resolution and
   RRCSetup appear in the same downlink message?

   It means that the gNB resolves the Random Access
   contention and provides the RRC configuration
   to the selected UE in the same downlink message.
   ### Step 3: Find RRCSetupComplete

| Item | Result |
|---|---|
| Frame Number | 498 |
| Context RNTI | 0xf21e (61982) |
| rrc-TransactionIdentifier | 1 |

**Screenshot:**


**Explanation:**

Frame 498 shows the RRCSetupComplete message sent
from the UE to the gNB.

The Context RNTI is 61982, and the transaction
identifier is 1.

**Questions and Answers:**

1. Does the Context RNTI still match the Temporary
   C-RNTI in the RAR?

   Yes. Both values are 61982. The UE continues to
   use the same RNTI as its C-RNTI.

2. Does the rrc-TransactionIdentifier match the
   RRCSetup in Step 2? What is the purpose of this
   comparison?

   Yes. Both values are 1. This confirms that
   RRCSetupComplete corresponds to the RRCSetup
   message sent by the gNB.

   ### Step 4: Confirm the Final Random Access and RRC Connection Result

| Event | Line Number |
|---|---|
| 4-Step RA procedure succeeded | 87 |
| Received NR_RRCSetup | 88 |
| State = NR_RRC_CONNECTED | 90 |
| Generating RRCSetupComplete | 93 |

**Screenshot:**


**Explanation:**

The UE log confirms that the four-step Random Access
procedure succeeds and the UE enters the RRC_CONNECTED
state.

**Questions and Answers:**

1. In log order, what are the three events from
   successful Random Access to generating RRCSetupComplete?

   - The UE receives NR_RRCSetup.
   - The UE enters the NR_RRC_CONNECTED state.
   - The UE generates RRCSetupComplete.

2. What do "4-Step RA procedure succeeded" and
   "State = NR_RRC_CONNECTED" each prove?

   "4-Step RA procedure succeeded" proves that the
   Random Access and contention resolution are successful.

   "State = NR_RRC_CONNECTED" proves that the UE
   has entered the RRC connected state.

   However, this does not prove that 5GC registration
   is complete.

   ## Task 3: Initial Access MSC Diagram

```mermaid
sequenceDiagram
    participant UE
    participant gNB
    Note over UE: RRC_IDLE
    gNB->>UE: MIB / SIB1 - Broadcast system information
    UE->>gNB: Msg1 PRACH - Initiate Random Access
    gNB->>UE: Msg2 RAR - Assign RNTI and uplink grant
    UE->>gNB: Msg3 RRCSetupRequest - Request an RRC connection
    gNB->>UE: Msg4 RRCSetup - Resolve contention and configure RRC
    Note over UE: RRC_CONNECTED
    UE->>gNB: RRCSetupComplete - Confirm RRC setup
