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

| Component             | VM count |
| --------------------- | -------- |
| AAP Gateway           | 1        |
| Automation Controller | 1        |
| Automation Hub        | 1        |
| Event Driven Ansible  | 1        |
| Database              | 1        |

## Inventory

```
[automationcontroller]
automationcontroller-0 ansible_user=ec2-user ansible_python_interpreter="/usr/libexec/platform-python" ansible_host=<<replace with controller ip>>

[automationhub]
automationhub-0 ansible_user=ec2-user ansible_python_interpreter="/usr/libexec/platform-python" ansible_host=<<replace with hub ip>>

[automationedacontroller]
automationedacontroller-0 ansible_user=ec2-user ansible_python_interpreter="/usr/libexec/platform-python" ansible_host=<<replace with eda controller ip>> routable_hostname=automationedacontroller-0

[automationgateway]
automationgateway-0 ansible_user=ec2-user ansible_python_interpreter="/usr/libexec/platform-python" ansible_host=<<replace with gateway ip>> routable_hostname=automationgateway-0

[database]
database-0 ansible_user=ec2-user ansible_python_interpreter="/usr/libexec/platform-python" ansible_host=<<replace with db ip>> routable_hostname=database-0


[installer]
installer_machine ansible_connection=local ansible_python_interpreter="/usr/libexec/platform-python"


[all:vars]
controller_admin_password=<< Controller admin password >>
controller_pg_host=aio-0.testing.com
controller_pg_password=<< Controller pg password >>
eda_admin_password=<< eda admin password >>
eda_pg_host=aio-0.testing.com
eda_pg_password=<< eda pg password >>
eda_type=hybrid
hub_pg_host=aio-0.testing.com
hub_pg_port=5432
hub_pg_database=automationhub
hub_pg_username=automationhub
hub_pg_password=<< hub pg password >>
hub_admin_password=<< hub admin password >>
gateway_admin_password=<< gateway admin password >>
gateway_pg_host=aio-0.testing.com
gateway_pg_password=<< gateway pg password >>
postgresql_admin_password=<< postgresql admin password >>
controller_base_url=https://controller ip/
gateway_base_url=https://gateway ip/
hub_base_url=https://hub ip/
automationedacontroller_base_url=https://eda ip/
controller_local_resource_management=True
hub_local_resource_management=True
eda_local_resource_management=True
registry_auth=True
registry_url=brew.registry.redhat.io
registry_ns_aap=rh-osbs
registry_ns_rhel=rh-osbs
registry_tls_verify=True

[automationgateway:vars]
gateway_grpc_server_processes=20
gateway_grpc_server_max_threads_per_process=40
gateway_grpc_auth_service_timeout=120s

```
