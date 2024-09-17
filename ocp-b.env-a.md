# OCP-B.ENV-A - Enterprise Deployment - OpenShift - AAP Operator Install Method

## Enterprise OpenShift - AAP Operator Install - OCP B
_SaaS Deployment used to represent ocp "enterprise" deployment_

## Topology

```mermaid
%%{ init: { 'theme': 'default', 'graph': { 'htmlLabels': true }, 'themeVariables': { 'fontSize': '10x', 'nodePadding': '0px', 'edgeWidth': '0px' } } }%%


graph TB
  subgraph main[<b>Ansible Automation Platform</b>]
  direction TB
  subgraph id4[Controller Pod ]
     id50([task])
     id51([web])
  end
  subgraph id6[Mesh Ingress Pod ]
     id60([Mesh Ingress x 2])
  end
  subgraph id1[Gateway Pod ]
   id20([Gateway])
  end
  subgraph id8[Automation Hub Pod]
    id80([api])
    id81([content])
    id82([web])
    id83([worker x 2])
  end
  subgraph id10[ EDA Pod ]
    id110([EDA x n])
  end
 end
 id12[(External managed databse)]
 id13[(External managed redis service)]

id1 & id4 & id8 & id10-->|Port 5432|id12
id1 & id4 & id8 & id10-->|Port 5432|id13
id10 <-->|Port 443|id80
id10 <--> |Port 443|id4
id80 <--> |Port 443|id4



classDef title font-size:15px;
classDef interior fill:#66cccf;

class main title;
class id110,id80,id81,id82,id83,id20,id60,id50,id51 interior;




```

**Legend**

```mermaid

%%{ init: { 'theme': 'default', 'graph': { 'htmlLabels': true }, 'themeVariables': { 'fontSize': '15px', 'nodePadding': '2px', 'edgeWidth': '1px' } } }%%

graph
  subgraph legend
   direction TB

   Y[ VM  ]
   Z[AAP Component]
   C([ pod ])
   D[ AAP component pod]
  end



classDef hide fill:#0000,stroke:#0000,stroke-width:0px;
classDef hide-font color:#0000;

classDef interior fill:#00cccc;
class Y,C,legend hide-font;
class Z,D,legend hide;
class C interior;



```

## Description

The **Enterprise OpenShift - AAP Operator Install** model consists of the following:

| Component                                     | Pod count                      |
| --------------------------------------------- | ------------------------------ |
| AAP Gateway                                   | 1                              |
| Automation Controller                         | 2 *(task, web)*                |
| Automation Hub                                | 5 *(api,content,web,2workers)* |
| Event Driven Ansible                          | x                              |
| Database (external)                           | 1                              |
| Redis Cache (non-HA)                          | 1                              |

