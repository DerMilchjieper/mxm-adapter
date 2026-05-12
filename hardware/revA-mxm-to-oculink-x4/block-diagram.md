# RevA block diagram: MXM to OCuLink x4

```mermaid
flowchart TB
    subgraph Notebook[HP ZBook 17 G5]
        CPU[CPU PEG root port]
        EC[Embedded controller / BIOS]
        MXM[MXM Type-B host connector]
        CPU --> MXM
        EC -. detect / reset / thermal / SMBus .- MXM
    end

    subgraph Interposer[RevA MXM-to-OCuLink interposer]
        EDGE[MXM edge connector fingers]
        STRAPS[strap matrix + DNI options]
        TP[test pads]
        ESD[low-cap ESD footprints, DNI unless validated]
        OCULINK[OCuLink 4i SFF-8612/SFF-8611]
        EDGE --> STRAPS
        EDGE --> TP
        EDGE --> ESD
        ESD --> OCULINK
    end

    subgraph External[eGPU side]
        CABLE[OCuLink cable]
        DOCK[Powered OCuLink eGPU dock]
        GPU[Desktop GPU]
        PSU[ATX/SFX/server PSU]
        PSU --> DOCK
        PSU --> GPU
        DOCK --> GPU
    end

    MXM --> EDGE
    OCULINK --> CABLE
    CABLE --> DOCK
```

## Board partitions

1. **MXM finger area**: mates with the notebook MXM socket and keeps all high-speed breakout lengths short.
2. **High-speed escape region**: routes PCIe x4 and REFCLK with uninterrupted reference planes.
3. **Sideband strap/test region**: exposes PERST#, CLKREQ#, WAKE#, PRSNT#/detect, SMBus, optional thermal spoof.
4. **Cable connector region**: OCuLink 4i with robust retention and mechanical strain relief.

## Intended mechanical shape

The board should mimic the MXM module insertion edge and occupy the minimum internal volume required to exit an OCuLink cable path. A flex or short cabled OCuLink pigtail may be mechanically easier than a rigid connector placed directly at the MXM area, but this must not violate PCIe Gen3 signal-integrity limits.
