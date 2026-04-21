# CPDLC Procedures – Manila FIR

## Overview

Automatic Dependent Surveillance – Contract / Controller–Pilot Data Link Communications (ADS-C/CPDLC) services are available to **FANS 1/A capable aircraft** operating on designated air routes within the Manila FIR.

## Data Link Airspace and Routes

| Sector | Routes | Primary Comms | Secondary Comms |
|--------|--------|---------------|-----------------|
| East | A582, A590, G578, M501, G467 | CPDLC | HF |
| North | A583 | VHF | HF |
| South | P515, R337, A339, B462, B473 | HF | — |
| West | N892, L625, L628, M772, M765, W17 | HF | — |

> In airspace where both VHF voice and CPDLC are available, flight crew **should use VHF** as the primary means of communication.
!!! warning "Warning"

    There are areas within the FIR where Controllers and Pilots will not hear each other due to radio wave propagation

## Log On Procedures

### When to Log On

An AFN (ATS Facilities Notification) log on is required when:

- The aircraft is **preparing for departure**
- The aircraft will **enter a data link area** from a non-data link area
- **Instructed by ATC** (e.g. following a failed data link transfer)

### Manila ACC Log On Details

- **Log On Address:** `RPHI`,`RPHX`,`RPHC`,`RPHN` or `RPHS`
- Log on **prior to departing RPLL**, or **10–25 minutes before entering Manila FIR**

### Log On Requirements

- Ensure all **flight-specific information** in the aircraft system matches the filed flight plan exactly to avoid automatic rejection.
- Pilots unable to establish a data link connection shall **inform ATC on VHF**.

## ADS-C / CPDLC Operating Procedures

- Aircraft with an ADS-C/CPDLC connection to RPHI shall **send an ADS position report upon entering the FIR boundary**.
- **Response medium must match the initiating medium:** respond to CPDLC via CPDLC; respond to voice via voice.
- If there is any doubt about message intent or ambiguity, **seek clarification via voice communication**.
- **Read back for ATC clearances/instructions issued via CPDLC is not required.**

## Message Handling

### Downlink Message Responses

| Response | Meaning |
|----------|---------|
| **WILCO** | Pilot will fully comply with the clearance/instruction |
| **UNABLE** | Negative response to a clearance request (applies to all elements of a multi-element request) |
| **AFFIRM** | Appropriate response to an uplinked negotiation request (e.g. *CAN YOU ACCEPT [altitude] AT [time]*) |

> **ROGER** and **AFFIRM** are **not** appropriate responses to a clearance request. The controller must approve by sending an uplink containing the actual clearance.

### Message Content Rules

- A CPDLC downlink message should **not contain more than one clearance request** to avoid ambiguity.
- Clearances and clearance requests shall use **pre-formatted message elements**.
- **Free text** shall only be used when no appropriate pre-formatted element exists; standard ICAO phraseology and abbreviations apply.

## CPDLC Connection Transfer and End of Service

### Transfer from RPHI to Next Data Authority

1. RPHI sends an uplink — *CONTACT [unit name] [frequency]* — **10 minutes before the FIR boundary**.
2. After receiving **WILCO** from the pilot, RPHI sends *CPDLC TERMINATED*.
3. Avionics downlinks a **DISCONNECT** message.

## Data Link Service Failure

### CPDLC Connection Failure

If a CPDLC dialogue is interrupted, the controller shall **recommence the entire dialogue by voice**.

**Phraseology:**

| Speaker | Message |
|---------|---------|
| Controller / Radio Operator | *CPDLC FAILURE. DISCONNECT CPDLC THEN LOG ON TO [facility designator]* |
| Flight Crew | *ROGER* |

### Unplanned Data Link Shutdown

**Phraseology to all affected aircraft:**

| Speaker | Message |
|---------|---------|
| Controller / Radio Operator | *ALL STATIONS CPDLC FAILURE. DISCONNECT CPDLC. CONTINUE ON VOICE* |
| Flight Crew | *ROGER* |

### Planned Data Link Shutdown

**Pre-shutdown CPDLC message / voice phraseology:**

| Speaker | Message |
|---------|---------|
| Controller / Radio Operator | *CPDLC WILL BE SHUT DOWN. DISCONNECT CPDLC. CONTINUE ON VOICE.* |
| Flight Crew | *ROGER* |

> The controller may also provide the voice frequency operationally.

### Resumption of Data Link Operations

| Speaker | Message |
|---------|---------|
| Controller / Radio Operator | *[ALL STATIONS] RESUME NORMAL CPDLC and ADS-C OPERATIONS.* |
| Flight Crew | *LOG ON [facility designation]* |
