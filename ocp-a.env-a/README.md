# OCP-A.ENV-A - Growth Topology - OpenShift - AAP Operator Install Method

## OpenShift - AAP Operator Install - OCP A
_SaaS Deployment used to represent ocp "growth" deployment_

## Topology

![ OpenShift - AAP Operator Install Topology](OCP-A_Env-A.png)

## Description

The **OpenShift - AAP Operator Install** model consists of the following:

| Component                                     | Pod count                      |
| --------------------------------------------- | ------------------------------ |
| AAP Gateway 2.5                               | 1                              |
| Automation Controller 2.5                     | 2 *(task, web)*                |
| Automation Hub 2.5                            | 5 *(api,content,web,2 workers)* |
| Event Driven Ansible 2.5                      | 5 *(api, stream, scheduler, 2 workers)* |
| Database (external)                           | 1                              |
| Redis Cache (non-HA)                          | 1                              |

