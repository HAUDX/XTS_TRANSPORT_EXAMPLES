
# XTS_TRANSPORT_LAYER: 
## The Execution Vault

### Introduction to The Examples
- Welcome to the deployment architecture. 
- The examples contained within this repository are not theoretical exercises
- Most examples provided here are derived from a physical, production-ready machine currently operating in the field.
- Some examples are case studies from POC designs, which later were changed/adapted by users.

This repository serves as the blueprint for translating the XTS_TRANSPORT_LAYER into physical mechatronic reality. 
- Architectural Philosophy: **The Diplomat Pattern**
  - All examples enforce a strict division between **[APPLICATION]** and **[XTS]** environments/folders.
  
- The architecture operates on the principle of strict namespace encapsulation. 
  - The core TcGVL (Global Variable List) files are treated as sovereign territory. 
  - No external logic is permitted to read or write to this territory directly.
  - Instead, communication requires a "diplomat"—a strictly defined Facade or Interface.
  - The lowest layers of the framework (ST_STATION_CTRL/STATE and ST_MOVER_CTRL/STATE) already possess this diplomatic core, 
    allowing external layers and C++/any applications to plug into the system without ever risking the integrity of the hardware cycle.

## I. Encapsulation & Agnostic Routing
These examples illustrate how to command the physical track from an abstracted, agnostic IT or Process layer.
- **[XTS_DEMO_EXTERN_108]:** The O(1) Linked-List Abstraction. This design is running multiple times in the field. A dual-PLC architecture demonstrating high-layer abstraction.
  The Application Layer (InstanceControl and AppCtrl) is fully decoupled from the Collector/Instance Layer and Station Layer (ProcessStation and Mover).
  Introduces the Linked-List structure with cyclic Ctrl/State wrappers, guaranteeing atomic, asynchronous communication across any communication medium.
  
- **[XTS_DEMO_EXTERN_SIMULATION]:** A dual-PLC case study demonstrating high-layer abstraction. PLC ExternControl commands the Station Layer via TwinCAT mapping. 
  Utilizes the ExternControl PLC with process abstraction (fb_Simulation) for rapid product flow analysis.

- **[XTS_DEMO_EXTERN_STATION_PRIMES]:** A dual-PLC simulation study which handles processes consisting of a prime number station count. 
  This example shows how routing from parallel stations can be done.

## II. High-Throughput Mechatronics
These examples contain the blueprints for building high throughput kinetic environments and managing multi-station handshakes.
- **[XTS_DEMO_APPLICATION_108]:** The 500 Mover/Minute Blueprint. The architecture for asynchronous, ultra-high-throughput machines. 
  Features 4 configurable setups demonstrating top-level connections to the XTS_TRANSPORT_LAYER. 
  Includes Collector-Level and Application-Level classes (fb_ProcessCollector, fb_Instance, fb_ProcessLinkedList).

- **[XTS_DEMO_GEAR_IN_POS_CA]:** Advanced Kinematic Synchronisation. Utilizes the ST_MOVER_CTRL / ST_MOVER_STATE. 
  Demonstrates a precise gear-in sequence for individual movers, actively verifying the SyncDistance of the MasterAxis before releasing the mover via the sovereign GVL_XTS.

## III. Foundational Handshakes & State Control 
The baseline mechanics for starting, stopping, and clearing the track safely.
- **[XTS_DEMO_11] & [XTS_DEMO_22]:** Single & Multi-XPU Handshakes. Demonstrates the basic APPLICATION(PRG) procedures for track startup and station operation. 
  Illustrates the strict sequential gating: waiting for MAIN.eInit = PROGRESS_DONE before any physical movement is permitted.
  Shows exact manual override procedures (bStart, bReset) for safely clearing groups and halting movement.
  
- **[XTS_DEMO_LINE_SORT]:** Mover-Centric Command. A 2-PLC setup (XtsTransport and ExternControl) managing isolated parts with singular movers. 
  The ExternControl operates as the absolute command center.


## IV. The Digital Twin & Observability
Risk-reduction through virtual commissioning.
- **[XTS_DEMO_SIMULATION_NEW_MC]:** Automated Virtual Commissioning. Start simulating the transport system before a single piece of iron is cut.
  Features fb_Simulation for grouping XTS Station ranges, allowing synced handling (e.g., process multiplication) and automated handshakes using TON timers to simulate process duration.

- **[scope]:** The Diagnostic Ledger. Included TwinCAT Scope projects configured specifically for recording and validating the signals of Stations and Processes, 
  providing a chronological narrative of mechatronic behavior.


#### The Final Step:
- Institutional Transfer **[TR3056 Beckhoff Training]**
- The framework is fully documented, but true internalized architecture requires dialogue. 
- **Join me in Nuremberg at the TR3056 training.**
- We will talk architecture, review the deterministic logic, and code the physical iron in person.

