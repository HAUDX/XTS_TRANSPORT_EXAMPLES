
# XTS_TRANSPORT_EXAMPLES: 
## The Execution Vault
## The Facades
## The Tickets

### Introduction to The Examples
- Welcome to the deployment architecture. 
- The examples contained within this repository are not theoretical exercises.
- Most examples provided here are derived from a physical, production-ready machine currently operating in the field.
- Some examples are case studies from POC designs, which later were changed/adapted by users.
- The examples shall illustrate the multi layered approach when dealing with complex transport logic and asynchronuous command issuing.
- All examples are built upon the same basic ctrl/state handshakes.
- All examples are ready to run in simulation.

This repository serves as the blueprint for translating the XTS_TRANSPORT_LAYER into physical mechatronic reality. 
- Architectural Philosophy: **The Diplomat Pattern**
  - All examples enforce a strict division between **[APPLICATION]** and **[XTS]** environments/folders.
  - **[XTS_DEMO_11]** is the entry level example, ready to run, showcasing the base implementation all other examples use.
  - **[XTS_DEMO_12]** is the entry level example that showcases the 2-PLC approach. This might be helpful if you intend to use the transport layer via a network or fieldbus.
  
- The architecture operates on the principle of strict namespace encapsulation. 
  - The core TcGVL (Global Variable List) files are treated as sovereign territory. 
  - No external logic is permitted to read or write to this territory directly.
  - Instead, communication requires a "diplomat"—a strictly defined Facade or Interface.
  - The lowest layers of the framework (ST_STATION_CTRL/STATE and ST_MOVER_CTRL/STATE) already possess this diplomatic core, 
    allowing external layers like C++ applications (any application that may use ADS) to plug into the system without ever risking cycle time violations (Exceed counter - online tab of the PLC task).
  - Any cyclic application may be connected via MAPPING procedures (fieldbus, EAP).
  - Any non-cyclic application may be connected via ADS.
  - Hybrid access possible
    - →  A middleware layer to organize access (switching between ADS and cyclic mapping) keeps the base layer unchanged
      - The most suitable examples would be the EXTERN examples, since they are fully separated and ready to use.

- The architecture relies on linked lists for information transport
  - Stations: the linked list is embedded in the base layer of the stations. A closer look you want to take? Read "02_XTS_TRANSPORT_LAYER_Stations.pdf" page 16.
  - Processes: the linked list is a prepared tool for you to use. A closer look is what you truly desire? Read "XTS_DEMO_APPLICATION.pdf" page 9.
  - Examples for stations handling the mover tickets based on handshake and movement; any example in here will show you.
  - Example for processes handling tickets for a range of stations on the process level: **[XTS_DEMO_EXTERN_108]**

## I. Encapsulation & Agnostic Routing
These examples illustrate how to command the physical track from an abstracted, agnostic IT or Process layer.
- **[XTS_DEMO_EXTERN_108]:** The O(1) Linked-List Abstraction. This design is running multiple times in the field. A dual-PLC architecture demonstrating high-layer abstraction.
  The Application Layer (InstanceControl and AppCtrl) is fully decoupled from the Collector/Instance Layer and Station Layer (ProcessStation and Mover).
  Introduces the Linked-List structure with cyclic Ctrl/State wrappers, guaranteeing atomic, asynchronous communication across any communication medium.
  The PLC ExternControl is connected via TwinCAT mapping and delivers the tickets into the cyclic LinkedList of the PLC XtsTransport.
  Within that cyclic LinkedList (fb_Process_LinkedListCtrl) a fb_Instance extension is routing the ticket to the designated target process (fb_ProcessConductor).
  The XtsTransport PLC contains extensions of fb_Instance which take care of the transport related tasks for a given process (group of XtsStations), in those extensions the job details are coded. 
  Use those fb_Instance extensions as example for coding complex transport scenarios.
  
- **[XTS_DEMO_EXTERN_SIMULATION]:** A dual-PLC case study demonstrating high-layer abstraction. PLC ExternControl commands the Station Layer via TwinCAT mapping. 
  Utilizes the ExternControl PLC with process abstraction (fb_Simulation) for rapid product flow analysis. I use this as a base for transport simulations, in order to estimate a desired output with different process layouts.

- **[XTS_DEMO_EXTERN_STATION_PRIMES]:** A dual-PLC simulation study which handles processes consisting of a prime number station count. 
  This example shows how routing from parallel stations can be done.

- **[XTS_DEMO_EXTERN_RECTANGLE_2]:** A dual-PLC simulation of distributed clusters of stations. This example shall illustrate flexibility towards station design. 
  The ExternControl PLC carries the process definitions. XtsTransport PLC carries the station definitions. 
  Parameter driiven approach for flexible transport solutions. The stations you design for a process must be in closed range, AND the modulo turn must NOT intersect the range of clusters. Those are the only prerequisites you have to follow.

## II. High-Throughput Mechatronics
These examples contain the blueprints for building high throughput kinetic environments and managing multi-station handshakes.
- **[XTS_DEMO_APPLICATION_108]:** deprecated, this example is fully substituted by [XTS_DEMO_EXTERN_108]

- **[XTS_DEMO_GEAR_IN_POS_CA]:** Advanced Kinematic Synchronisation. Utilizes the ST_MOVER_CTRL / ST_MOVER_STATE. 
  Demonstrates a precise gear-in sequence for individual movers, actively verifying the SyncDistance of the MasterAxis before releasing the mover via the sovereign GVL_XTS.

## III. Foundational Handshakes & State Control 
The baseline examples for starting, stopping, and clearing a transport application safely.

- **[XTS_DEMO_11]:** Basic Station Handshakes. Demonstrates the basic MAIN_APP(PRG) procedures for track startup and station operation. 
  MAIN_APP(PRG) is used for running the handshakes in.
  Illustrates the strict sequential gating: waiting for MAIN.eInit = PROGRESS_DONE before any physical movement is permitted.
  Shows exact manual override procedures (bStart, bReset) for safely clearing groups and halting movement.
  This example is the template for all examples in the entire repository. Advanced components, such as Process Collectors (fb_ProcessCollector) and complex routing instances (fb_Instances), are built upon these exact same interfaces.

- **[XTS_DEMO_12]:** Basic Station Handshakes. Demonstrates the basic MAIN_APP(PRG) in the second PLC (ExternControl) procedures for track startup and station operation. 
  MAIN_APP(PRG) is used for running the handshakes in.
  Illustrates the strict sequential gating: waiting for MAIN.eInit = PROGRESS_DONE before any physical movement is permitted.
  Shows exact manual override procedures (bStart, bReset) for safely clearing groups and halting movement.
  Separation into 2-PLC solution enforces clear interfaces and prevents dirty hacks around facades and interfaces.
  This example is the template for all examples in the entire repository. Advanced components, such as Process Collectors (fb_ProcessCollector) and complex routing instances (fb_Instances), are built upon these exact same interfaces.


   
- **[XTS_DEMO_LINE_SORT]:** Mover-Centric Command. A 2-PLC setup (XtsTransport and ExternControl) managing isolated parts with singular movers. 
  The ExternControl operates as the absolute command center.

- **[XTS_DEMO_MAPPING]:** EL6633-0010 very simple example for Profinet connectivity.

## IV. The Digital Twin & Observability
Risk-reduction through virtual commissioning.
- **[XTS_DEMO_EXTERN_SIMULATION]:** Automated Virtual Commissioning. Start simulating the transport systems before a single piece of iron is cut.
  Features fb_Simulation for grouping XTS Station ranges, allowing synced handling (e.g., process multiplication) and automated handshakes using TON timers to simulate process duration.
- **[XTS_DEMO_SIMULATION] (deprecated):** - single PLC was more work than it should be setting up a simulation. please use the 2 PLC version above.

- **[scope]:** The Diagnostic Ledger. Included TwinCAT Scope projects configured specifically for recording and validating the signals of Stations and Processes, 
  providing a chronological narrative of mechatronic behavior.


#### The Final Step:
- Institutional Transfer **[TR3056 Beckhoff Training]**
- The architecture is fully documented, but true internalized architecture requires dialogue. 
- **Join me in Nuremberg at the TR3056 training.**


