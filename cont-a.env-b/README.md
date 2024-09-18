# CONT-A.ENV-A - Standard Containerized Deployment

## Containerized single VM deployment

## Topology

```mermaid
%%{ init: { 'theme': 'default', 'graph': { 'htmlLabels': true }, 'themeVariables': { 'fontSize': '10px', 'nodePadding': '2px', 'edgeWidth': '1px' } } }%%


graph TB
  subgraph main[<b>Ansible Automation Platform</b>]
  subgraph container[  ]

  id2([Gateway])

  subgraph id3[Automation Mesh]
    subgraph id4[ ]
      id5([Controller])
    end

    id7([Execution Node])
  end

  id9([Automation Hub])
  id11([EDA])

  id12[(Managed Databse)]
 end
 end


id11--> |Port 443|id4
id2 <--> |Port 443| id3
id4--> |Port 443/80|id9
id2 & id9 & id11-->|Port 5432|id12
id4 -->|Port 5432|id12
id5<-->|Port 27199|id7

classDef title font-size:15px;
classDef db padding:10px;
classDef cont fill:#99CCCC,stroke:#333,stroke-width:1px,stroke-dasharray:4,border-radius:50%,stroke-linecap:round,rx: 10px;



class main title;
class rd db;
class container cont;

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
  end



classDef hide fill:#0000,stroke:#0000,stroke-width:0px;
classDef hide-font color:#0000;
classDef cont-hide-font color:#0000,fill:#99CCCC,stroke:#333,stroke-width:1px,stroke-dasharray:4,border-radius:50%,stroke-linecap:round,rx:10px;
class A,legend hide-font;
class B,D,legend hide;
class C cont-hide-font;



```

## Description

The **AAP Components deployed inside a container** consists of the following:

| Component                | VM count |
| ---------------------    | -------- |
| AAP Gateway 2.5          | 1        |
| Automation Controller 2.4| 1        |
| Automation Hub 2.4       | 1        |
| Event Driven Ansible 2.5 | 1        |
| Database                 | 1        |
