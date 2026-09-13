# SAP Datasphere
```mermaid
graph TD
    classDef main fill:#f9f,stroke:#333,stroke-width:2px,color:#000;
    classDef sub fill:#bbf,stroke:#333,stroke-width:1px,color:#000;
    classDef leaf fill:#dfd,stroke:#333,stroke-width:1px,color:#000;

    DS[SAP Datasphere]:::main
    
    DS --> Type[Cloud Solution / SaaS]:::sub
    DS --> Purpose[Purpose & Capabilities]:::sub
    DS --> Tool[Data Builder]:::sub

    Purpose --> P1[Combine different sources]:::leaf
    Purpose --> P2[Create connections]:::leaf
    Purpose --> P3[Copy cp, Read, Analyze data]:::leaf

    Tool --> Out[Outputs]:::sub
    Out --> T[Table]:::leaf
    Out --> V[View]:::leaf

    V --> VM[Creation Methods:<br>SQL, Graphical]:::leaf
    V --> VR[Component:<br>Repository -> Object description]:::leaf
    V --> VA[Action:<br>Deploy using View]:::leaf
```

**How to read it:**
*   **Pink Node:** The main subject (SAP Datasphere).
*   **Blue Nodes:** The core categories (Type, Purpose, Tool, Outputs).
*   **Green Nodes:** The specific details and actions from your notes. 

## Database - Derby