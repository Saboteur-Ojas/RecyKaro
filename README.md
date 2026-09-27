# RecyKaro

### Offline-First E-Waste Collection and Recycler Connectivity Platform

> **Collect. Connect. Circularize.**

RecyKaro is a platform for connecting informal e-waste collectors with authorized recyclers.

The idea is simple: a collector should be able to record the scrap they have, check its expected value, find a suitable recycler, and complete the handover without depending on a continuous internet connection.

The platform also provides a voice-based interface for collectors who do not use smartphones. Through a phone call, they can access basic services such as checking prices, finding recyclers, and managing collection-related transactions.

The system is built around four parts:

* Collector application
* Authorized recycler interface
* Administrator dashboard
* Voice interface for non-smartphone collectors

The proposed system also uses image classification, value estimation, recycler matching, and digital lot management to support the collection process.

---

## Problem Statement

Informal e-waste collectors often work outside the formal recycling chain.

A collector may have access to scrap but still face problems such as:

* Limited information about current prices
* Difficulty finding authorized recyclers
* Dependence on local intermediaries
* Lack of documented handovers
* Limited access to digital platforms
* Poor connectivity in some working areas

This makes it difficult to maintain a clear record from collection to recycling.

RecyKaro addresses this gap by connecting collectors with recyclers and maintaining a digital record of the material as it moves through the chain.

---

## How RecyKaro Works

The basic workflow is:

```text
Collector
   │
   ▼
Capture Scrap
(Photo / Details)
   │
   ▼
Classification & Value Estimation
   │
   ▼
Find Suitable Recycler
   │
   ▼
Create Digital Lot
   │
   ▼
Recycler Accepts Lot
   │
   ▼
Handover
   │
   ▼
Payment
```

The important part is that the collector does not have to be online for every step.

If there is no network connection, the collection data is stored on the device. Once connectivity is restored, the pending records are uploaded to the server.

---

# Collector Application

The collector application is the main entry point for smartphone users.

A collector can:

* Record collected e-waste
* Take or upload photographs
* Enter item details
* Check estimated value
* Find nearby recyclers
* Create collection lots
* Track lot status
* Record handover
* View payment information

### Offline operation

The application does not immediately depend on the server for every action.

When the collector is offline:

```text
Collector enters data
        │
        ▼
Local Storage
        │
        ▼
Pending Sync Queue
```

The record remains on the device until a network connection becomes available.

When the application comes online:

```text
Internet Available
        │
        ▼
Read Pending Records
        │
        ▼
Upload to Server
        │
        ▼
Server Confirms Record
        │
        ▼
Mark Record as Synced
```

This prevents a temporary network problem from stopping the collection process.

The SIH architecture specifically proposes local storage with a synchronization queue and automatic upload when connectivity returns.

---

# Authorized Recycler

The recycler side of the platform is used by authorized recycling partners.

A recycler can:

* View incoming lots
* Check material details
* Review estimated quantity and value
* Accept or reject a lot
* Confirm the handover
* Record transaction details
* Maintain records of received material

The recycler does not directly modify the collector's original collection record.

Instead, the system keeps the lot and transaction history so that the movement of material can be followed.

---

# Administrator

The administrator manages the platform and its participants.

Typical administrative functions include:

* Managing collectors
* Managing recycler accounts
* Verifying recycler information
* Monitoring collection activity
* Managing platform records
* Reviewing transactions
* Handling operational issues

Administrative access is separate from collector and recycler access.

---

# Voice Access for Non-Smartphone Collectors

Not every collector will have a smartphone or be comfortable using an application.

RecyKaro therefore includes a Hindi/Marathi voice-based interface that can be accessed through a normal phone call.

The flow is:

```text
Incoming Call
      │
      ▼
Speech Recognition
      │
      ▼
Intent & Entity Extraction
      │
      ▼
Backend API
      │
      ▼
Required Information
      │
      ▼
Voice Response
```

For example, a collector may call and ask about the price of a particular type of scrap.

The voice system converts the speech into text, identifies what the collector is asking for, sends the request to the backend, and returns the result through voice.

The same interface can be used for supported actions such as finding recyclers and checking transaction information.

---

# Intent and Entity Extraction

The voice system needs to understand two things from a collector's request:

### Intent

The intent tells the system **what the collector wants to do**.

Examples:

```text
"Mobile ka rate kya hai?"
        ↓
Intent: CHECK_PRICE
```

```text
"Mere paas e-waste hai, recycler kaha milega?"
        ↓
Intent: FIND_RECYCLER
```

### Entity

An entity is the specific information inside the request that the backend needs.

For example:

```text
"Mobile ka rate kya hai?"
```

can be interpreted as:

```text
Intent  → CHECK_PRICE
Item    → MOBILE
```

Another request:

```text
"Prayagraj ke paas recycler chahiye"
```

can become:

```text
Intent      → FIND_RECYCLER
Location    → Prayagraj
```

The extracted intent and entities are then passed to the backend API.

This keeps the voice layer separate from the actual business logic.

---

# AI and ML Components

AI is used where it provides a practical benefit to the collection process.

## 1. Image Classification

A collector can provide an image of the scrap.

The classification model can identify the likely category of the material.

For example:

```text
Image
  ↓
Image Classification
  ↓
E-Waste Category
```

The result can then be used as one of the inputs for valuation and recycler matching.

---

## 2. Value Estimation

The platform can estimate the expected value of a collection based on available information such as:

* Material category
* Estimated quantity
* Item condition
* Current pricing information

The estimate is meant to assist the collector and recycler. It is not a replacement for the final transaction value agreed during the actual handover.

---

## 3. Recycler Matching

Once the material details are available, RecyKaro can identify suitable recyclers.

The matching process can consider factors such as:

* Material handled by the recycler
* Location
* Availability
* Collection requirements
* Transaction history

The purpose is to reduce the effort required for a collector to find an appropriate recycler.

---

# Digital Lot

The digital lot is the main record used to track a collection.

A lot can contain:

```text
Lot ID
Collector
Material Type
Quantity
Estimated Value
Recycler
Status
Collection Details
Handover Details
Payment Details
```

A typical lot moves through states such as:

```text
CREATED
   ↓
MATCHED
   ↓
ACCEPTED
   ↓
HANDED_OVER
   ↓
COMPLETED
```

Keeping the collection as a digital lot makes it possible to maintain a record of what was collected, who accepted it, and what happened during the handover.

---

# Collector → Aggregator → Recycler

The platform can support a three-stage digital chain:

```text
Collector
    │
    ▼
Aggregator
    │
    ▼
Authorized Recycler
```

The collector records the material and creates the lot.

The aggregator can handle collection or consolidation where required.

The authorized recycler receives the material and completes the recycling-side transaction.

This structure allows the platform to connect the informal collection layer with the formal recycling network.

---

# System Architecture

```text
                         ┌─────────────────────┐
                         │   Collector App     │
                         └──────────┬──────────┘
                                    │
                                    │
                         ┌──────────▼──────────┐
                         │   Local Storage      │
                         │   + Sync Queue       │
                         └──────────┬──────────┘
                                    │
                              Internet
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │    Backend API      │
                         │ Authentication       │
                         │ Lot Management       │
                         │ Matching             │
                         │ Transactions         │
                         └───────┬─────┬───────┘
                                 │     │
                   ┌─────────────┘     └─────────────┐
                   ▼                                 ▼
          ┌──────────────────┐             ┌──────────────────┐
          │    Database      │             │    AI / ML       │
          │                  │             │                  │
          │ Users            │             │ Classification   │
          │ Lots             │             │ Value Estimation │
          │ Transactions     │             │ Matching         │
          └──────────────────┘             └──────────────────┘
                                      
                   ┌───────────────────────────┐
                   │   Recycler / Admin Apps   │
                   └───────────────────────────┘

                         Voice Interface
                              │
                              ▼
                       Voice AI Backend
                              │
                              ▼
                         Backend APIs
```

The architecture separates the user interfaces, backend services, database, and AI components.

The voice interface uses the same backend APIs instead of maintaining a separate transaction system.

---

# Offline-First Design

Offline support is not an optional feature in RecyKaro. It is part of how the collector application is expected to work.

### When online

```text
User Input
   ↓
Local Storage
   ↓
Backend
   ↓
Database
```

### When offline

```text
User Input
   ↓
Local Storage
   ↓
Pending Queue
```

Nothing is lost simply because the network disappears.

### When connection returns

```text
Pending Queue
   ↓
Sync Worker
   ↓
Backend
   ↓
Server Confirmation
   ↓
Local Record Marked Synced
```

The application should not delete a pending record before receiving confirmation from the server.

This also makes it possible to retry failed uploads instead of creating duplicate records manually.

---

# Data Flow

A normal collection can be represented as:

```text
1. Collector records e-waste
              ↓
2. Data saved locally
              ↓
3. Image / details processed
              ↓
4. Value estimated
              ↓
5. Suitable recycler identified
              ↓
6. Digital lot created
              ↓
7. Recycler accepts the lot
              ↓
8. Material is handed over
              ↓
9. Payment is recorded
              ↓
10. Lot is marked completed
```

If the collector is offline, steps involving the server are queued until the connection becomes available.

---

# Security and Access Control

The platform handles user accounts, collection records, recycler information and transaction data.

Access should therefore be controlled by role.

### Collector

Can access:

* Their own collection records
* Their lots
* Their transactions
* Available recycler information

### Recycler

Can access:

* Lots available to them
* Lots they accepted
* Relevant collector and material information
* Their transaction records

### Administrator

Can access platform-level management information according to administrative permissions.

The backend should enforce these permissions. Hiding a button in the frontend is not access control.

---

# Notifications and External Services

The architecture allows RecyKaro to connect with external services where required.

These include:

* Maps for nearby recyclers
* SMS / WhatsApp notifications
* UPI or payment gateways
* Voice communication services
* Government and EPR-related systems

These integrations are kept separate from the core collection workflow so that the main application can continue to operate even when a particular external service is unavailable.

---

# Project Structure

A possible repository structure is:

```text
recykaro/
│
├── frontend/
│   ├── collector-app/
│   ├── recycler-app/
│   └── admin-dashboard/
│
├── backend/
│   ├── controllers/
│   ├── routes/
│   ├── models/
│   ├── middleware/
│   ├── services/
│   └── utils/
│
├── ai/
│   ├── classification/
│   ├── valuation/
│   └── matching/
│
├── voice/
│   ├── speech-to-text/
│   ├── intent/
│   └── text-to-speech/
│
├── integrations/
│   ├── maps/
│   ├── notifications/
│   └── payments/
│
├── docs/
│
└── README.md
```

The exact structure can change as the implementation develops. The important separation is between the user interfaces, backend services, AI modules, and external integrations.

---

# Technology Stack

The final stack depends on the implementation used by the team.

The architecture requires the following types of components:

| Layer               | Purpose                                |
| ------------------- | -------------------------------------- |
| Collector App       | Capture and manage collection data     |
| Recycler Interface  | Receive and manage lots                |
| Admin Dashboard     | Platform management                    |
| Backend API         | Business logic and APIs                |
| Database            | Users, lots, transactions and records  |
| Local Storage       | Offline collector data                 |
| AI/ML               | Classification, valuation and matching |
| Voice Services      | Speech recognition and voice responses |
| Maps                | Nearby recycler discovery              |
| Payment Integration | Digital payments                       |

The SIH proposal specifically identifies the mobile collector app, authorized recycler app/web, offline-first data layer, AI/ML engine, voice AI, maps, notifications and payment integrations as parts of the technical approach.

---

# Getting Started

Clone the repository:

```bash
git clone <repository-url>
cd recykaro
```

Install the dependencies for the required applications and services.

```bash
npm install
```

Start the development environment according to the individual frontend and backend projects.

```bash
npm run dev
```

The exact commands may differ depending on the final project structure.

---

# Environment Variables

Keep credentials and service configuration outside the source code.

Example:

```env
DATABASE_URL=
JWT_SECRET=

MAPS_API_KEY=

VOICE_API_KEY=
VOICE_PHONE_NUMBER=

PAYMENT_API_KEY=
PAYMENT_SECRET=
```

Do not commit `.env` files containing real credentials.

---

# Collection Lifecycle

The complete lifecycle of a collection is:

### 1. Capture

The collector records the e-waste using the application or supported voice interface.

### 2. Store

The record is saved locally first when required.

### 3. Sync

Once connectivity is available, pending records are sent to the backend.

### 4. Classify

The system identifies the likely category of the material.

### 5. Estimate

An estimated value is generated from the available information.

### 6. Match

The system identifies suitable recycler options.

### 7. Create Lot

The collection becomes a digital lot containing the relevant item and transaction information.

### 8. Accept

A recycler reviews and accepts the lot.

### 9. Handover

The physical material is transferred and the handover is recorded.

### 10. Payment

The agreed transaction amount is recorded and the lot is completed.

---

# Why Offline-First Matters

Many digital systems assume that a stable internet connection is always available.

That assumption does not work well for a field-based collection workflow.

A collector may be working in an area with poor connectivity or may lose the connection while recording a collection.

RecyKaro therefore treats the device as the temporary source of truth for unsynced records.

The sequence is:

```text
Work Offline
    ↓
Save Locally
    ↓
Continue Collection
    ↓
Connection Returns
    ↓
Synchronize
    ↓
Server Confirmation
```

The collector does not need to stop working simply because the network is unavailable.

---

# Business Model

The proposed platform can operate through multiple channels.

The SIH proposal identifies:

* Transaction or platform fees
* B2B partnerships

The three-stage Collector → Aggregator → Recycler model also provides a structure for connecting collection activity with formal recycling operations.

---

# Impact

RecyKaro is intended to improve the connection between informal collection and formal recycling.

The expected areas of impact are:

### For Collectors

* Better access to recycler information
* More transparent pricing
* Easier transaction records
* Access through both smartphone and voice interfaces

### For Recyclers

* More organized incoming material
* Digital lot information
* Better visibility into collection sources
* Documented handovers

### For the Recycling Chain

* More e-waste directed toward authorized recycling
* Better material tracking
* Improved visibility from collection to handover
* Better recovery of valuable materials

The SIH proposal identifies e-waste diversion, collector income, material recovery, traceability and compliance as the main impact areas.

---

# Limitations

RecyKaro is a project prototype and several parts require further validation before large-scale deployment.

### AI Accuracy

Image classification and value estimation depend on the quality and availability of training data.

### Pricing

Estimated prices can change based on material quality, market conditions and recycler-specific rates.

### Connectivity

Offline storage allows the collector to continue working, but server-dependent operations cannot be completed until synchronization occurs.

### Voice Interface

Speech recognition and intent extraction may be affected by accents, background noise, pronunciation and local terminology.

### Recycler Data

The usefulness of matching depends on having reliable information about authorized recyclers and the materials they accept.

---

# Future Development

Possible extensions include:

* Better image classification models
* Improved material and price estimation
* More accurate recycler matching
* Additional Indian languages for voice access
* Better handling of low-connectivity environments
* Integration with additional recycling and EPR systems
* More detailed collection and transaction analytics
* Wider recycler network coverage
* Improved payment integration

These are development directions rather than claims about the current implementation.

---

# Project Objective

RecyKaro is built around a straightforward idea:

> **A collector should not have to depend on informal networks just to find a fair price or a legitimate recycling channel.**

The platform connects the collection side with authorized recycling, keeps a record of the material as it moves through the chain, and allows the workflow to continue even when internet connectivity is unreliable.

```text
                COLLECTOR
                    │
                    ▼
              RECYKARO
             /    |     \
            /     |      \
           ▼      ▼       ▼
       Pricing  Matching  Tracking
           │      │       │
           └──────┼───────┘
                  ▼
             AUTHORIZED
               RECYCLER
                  │
                  ▼
             DOCUMENTED
               HANDOVER
```

---

## License

Add the license selected by the project team.

For example:

```text
MIT License
```

---

## Team

**DEXTEROUS-UU**

Smart India Hackathon 2026

**Problem Statement:** 26229

**Problem:** Kabadiwala Connect – Bringing the Informal Collector into the Formal Recycling Chain

---

## RecyKaro

**Collect. Connect. Circularize.**
