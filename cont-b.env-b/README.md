# CONT-B.ENV-A - Enterprise Containerized Deployment

## Containerized multi VM deployment

## Topology

```mermaid
%%{ init: { 'theme': 'default', 'graph': { 'htmlLabels': true }, 'themeVariables': { 'fontSize': '10x', 'nodePadding': '0px', 'edgeWidth': '0px' } } }%%


flowchart TD

  idLB[[HA Proxy Load Balancer]]
  subgraph main[<b>Ansible Automation Platform</b>]
  subgraph container[  ]
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
 end
 id12[(Unmanaged Databse)]
 
id8 <-->|Port 443|id10
id1 -->|Port 443|id4
id4 -->|Port 27199|id6
id6 -->|Port 27199|id70 & id71
idLB --> id1

id1 & id4 & id8 & id10-->|Port 5432|id12


classDef title font-size:15px;
classDef dashed fill:#ffffcc,stroke:#333,stroke-width:4px,stroke-dasharray: 5, 5;
classDef cont fill:#99CCCC,stroke:#333,stroke-width:1px,stroke-dasharray:4,border-radius:50%,stroke-linecap:round,rx: 10px;
classDef interior fill:#fccc66;
class main,redis title;
class redis dashed;
class container cont;
class idr1,idr2,idr3,idr4,idr5,idr6 interior;

```

**_Legend:_**

```mermaid

%%{ init: { 'theme': 'default', 'graph': { 'htmlLabels': true }, 'themeVariables': { 'fontSize': '15px', 'nodePadding': '2px', 'edgeWidth': '1px' } } }%%

graph
  subgraph legend
    direction TB

   A([ VM  ])
   B[AAP VM component]
   C([ cont  ])
   D[Container]
   E[ Redis ]
   F[Redis Cluster]
   G([ Cache])
   H[Redis Cache]
  end



classDef hide fill:#0000,stroke:#0000,stroke-width:0px;
classDef hide-font color:#0000;
classDef cont-hide-font color:#0000,fill:#99CCCC,stroke:#333,stroke-width:1px,stroke-dasharray:4,border-radius:50%,stroke-linecap:round,rx:10px;
classDef dashed fill:#ffffcc,stroke:#333,stroke-width:2px,stroke-dasharray: 5, 5;
classDef interior fill:#fccc66;

class A,E,G,legend hide-font;
class B,D,F,H,legend hide;
class C cont-hide-font;
class E dashed;
class G interior;



```

## Description

The **AAP Components deployed inside a container** consists of the following:

| Component                                     | VM count |
| --------------------------------------------- | -------- |
| AAP Gateway 2.5                               | 2        |
| Automation Controller 2.4                     | 2        |
| Hop Node                                      | 1        |
| Execution Node                                | 2        |
| Automation Hub 2.4                            | 2        |
| Event Driven Ansible 2.5                      | 2        |
| Database (external)                           | 1        |
| Redis Cache (non-HA)                          | 1        |
| HAProxy load balancer in front of AAP Gateway | 1        |
