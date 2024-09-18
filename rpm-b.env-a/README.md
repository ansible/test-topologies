# RPM-B.ENV-A - Enterprise RPM Deployment

## Enterprise VM installer - RPM B

## Topology

```mermaid
%%{ init: { 'theme': 'default', 'graph': { 'htmlLabels': true }, 'themeVariables': { 'fontSize': '10x', 'nodePadding': '0px', 'edgeWidth': '0px' } } }%%


graph TB
  direction TB
  idLB[[HA Proxy Load Balancer]]
  subgraph main[<b>Ansible Automation Platform</b>]
  direction TB
  subgraph redis[Redis Cluster]
  subgraph id1[ ]
   direction LR
   subgraph id20[Gateway]
     idr5([redis])


   end
   subgraph id21[Gateway]
     idr6([redis])
   end

     
  end
  subgraph id8[ ]
    direction LR
    subgraph id90[Automation Hub]
      idr1([redis])
    end
    subgraph id91[Automation Hub]
      idr2([redis])
    end

  end
  subgraph id10[ ]
    direction LR
    subgraph id110[EDA]
      idr3([redis])
    end
    subgraph id111[EDA]
      idr4([redis])
    end
    
  end
  end
  
  subgraph id4[ ]
     id50([Controller])
     id51([Controller])
  end
  id6([Hop Node])
  id70([Execution Node])
  id71([Execution Node])
 end
 id12[(Unmanaged Databse)]
 
id8 <--> id10
id1 --> |Port 443|id4
id4 --> |Port 27199|id6
id6 -->|Port 27199|id70 & id71
idLB --> id1

id1 & id4 & id8 & id10-->|Port 5432|id12


classDef title font-size:15px;
classDef dashed fill:#ffffcc,stroke:#333,stroke-width:4px,stroke-dasharray: 5, 5;
classDef interior fill:#fccc66;
class main,redis title;
class redis dashed;
class idr1,idr2,idr3,idr4,idr5,idr6 interior;


```

**Legend**

```mermaid

%%{ init: { 'theme': 'default', 'graph': { 'htmlLabels': true }, 'themeVariables': { 'fontSize': '15px', 'nodePadding': '2px', 'edgeWidth': '1px' } } }%%

graph
  subgraph legend
   direction TB

   Y([ VM  ])
   Z[AAP Component VM]
   A[ Redis ]
   B[Redis Cluster]
   C([ cache ])
   D[ Redis Cache]
  end



classDef hide fill:#0000,stroke:#0000,stroke-width:0px;
classDef hide-font color:#0000;
classDef dashed fill:#ffffcc,stroke:#333,stroke-width:2px,stroke-dasharray: 5, 5;
classDef interior fill:#fccc66;
class Y,A,C,legend hide-font;
class Z,B,D,legend hide;
class A dashed;
class C interior;



```

## Description

The **Enterprise VM Installer RPM** consists of the following:

| Component                                     | VM count |
| --------------------------------------------- | -------- |
| AAP Gateway 2.5                               | 2        |
| Automation Controller 2.5                     | 2        |
| Hop Node                                      | 1        |
| Execution Node                                | 2        |
| Automation Hub 2.5                            | 2        |
| Event Driven Ansible 2.5                      | 2        |
| Database (external)                           | 1        |
| Redis Cache (non-HA)                          | 1        |
| HAProxy load balancer in front of AAP Gateway | 1        |
