# Introduction 
# XTS transport layer examples

## [examples] (..they have all been machines in the past)
- all examples are split between APPLICATION and XTS (folders named equally)
- examples strictly follow the idea of namespaces, the TcGVL files may be seen as sovereign territory for which a diplomat (facade or interface) must be employed for communications.
- The lowest layer (ST_STATION_CTRL/STATE and ST_MOVER_CTRL/STATE) already has a diplomatic "core" you can plug into (see XTS_DEMO_11 and the simulation templates).

## [XTS_DEMO_EXTERN_CONTROL]:
All extern examples shall illustrate a specific aspect of the XTS_TRANSPORT_LAYER
- transport examples for illustrating encapsulation
- Agnostic to cyclic communication medium
### [XTS_DEMO_EXTERN_108]:
- 2 PLC example for high layer abstraction
- Process Layer (Collectors and Instances) is fully independent from Station Layer (ProcessStation and Mover)
- Introduction of a LinkedList with cyclic Ctrl/State wrapper for async atomic communication across any means of communication.

### [XTS_DEMO_EXTERN_SIMULATION]:
- 2 PLC example for Station Layer abstraction
- Station Layer is controlled via extern entity
- ExternControl PLC with process abstraction (fb_Simulation) is intended for quick product flow analysis
 
***start simulating your transport system before anything is built***

## [XTS_TRANSPORT_EXAMPLES]:

### [XTS_DEMO_LINE_SORT]:
- 'mover only' example
- 2 PLCs (XtsTransport and ExternControl)
- 2 separate parts with one mover each
- ExternControl is used as command center

### [XTS_DEMO_22]:
- simple handshake example
- multiple XPU controlled in one PLC
- APPLICATION(PRG), simple procedure of how to startup and work Xts stations
  - wait for (MAIN.eInit = PROGRESS_DONE)
  - use manual write for bStart; --> move all movers to start position, then start handshaking stations
  - use manual write for bReset; --> movement is stopped, group cleared, station cleared
 
### [XTS_DEMO_11]: 
- simple handshake example
- only single stations to illustrate basic handshake procedure with ST_STATION_CTRL / ST_STATION_STATE
- APPLICATION(PRG), simple procedure of how to startup and work Xts stations
  - wait for (MAIN.eInit = PROGRESS_DONE)
  - use manual write for bStart; --> move all movers to start position, then start handshaking stations
  - use manual write for bReset; --> movement is stopped, group cleared, station cleared

### [XTS_DEMO_GEAR_IN_POS_CA]:
- Use of ST_MOVER_CTRL / ST_MOVER_STATE
- APPLICATION(PRG), simple procedure for startup and reset of XtsTransport
  - wait for (MAIN.eInit = PROGRESS_DONE)
  - use bStart and bReset for control
  
- GEAR_IN_POS_CA(PRG)
  - gear in  sequence for movers (one at a time)
  - checks SyncDistance of MasterAxis
  - sends mover away after MasterAxis moved SyncDistance
  - utilizes GVL_XTS.MoverCtrl/GVL_XTS.MoverState for control and movements of movers

### [XTS_DEMO_APPLICATION_108]:
- Asynchronuos high throughput machine (500 Mover/Minute)
  - 4 setups configurable
- Topl level example of how to connect to XTS_TRANSPORT_LAYER
- Collector-Level, and Applicatioon-Level classes and templates
  - fb_ProcessCollector
  - fb_Instance
  - fb_ProcessLinkedList

### [XTS_DEMO_SIMULATION_NEW_MC]:
  ***start simulating your transport system before anything is built***

- MAIN.StationParameterInit:
  - configuration of XtsStations
 
- GVL_APPLICATION:
  - fb_Simulation: grouping of XtsStation range for synced handling (e.g. process multiplication), automated handshakes with TON timer for process time
  
- APPLICATION(PRG), startup and reset of XtsTransport
  - Init(): fb_Simulation configuration, initial times are set here
  - wait for (MAIN.eInit = PROGRESS_DONE)
  - use XTS_TRANSPORT visualization for manual startup (TransportInit, ClearGroup, BuildGroup, EnableGroup, EnableMovers, TransportStart)
  - bNext and bEnter for halting fb_Simulation

## [scope]
- TwinCAT scope project for recording the signals of Stations and Processes

## [TR3056 Beckhoff Training]
- come to Nuernberg and we can talk and code in person for days.
## [TR3056 Beckhoff Training]
