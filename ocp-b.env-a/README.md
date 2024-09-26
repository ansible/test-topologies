# OCP-B.ENV-A - Enterprise Deployment

## Enterprise OpenShift - OCP B
_SaaS Deployment used to represent ocp "enterprise" deployment_

## Topology

![ OpenShift Enterprise - Topology](OCP-B_Env-A.png)

## Description

The **Enterprise OpenShift - AAP Operator Install** model consists of the following:

| Component                                     | Pod count                      |
| --------------------------------------------- | ------------------------------ |
| AAP Gateway 2.5                               | 1                              |
| Automation Controller 2.5                     | 2 *(task, web)*                |
| Automation Hub 2.5                            | 5 *(api, content, web, 2 workers)* |
| Event Driven Ansible 2.5                      | 5 *(api, activation, stream, scheduler,worker)* |
| Database (external)                           | 1                              |
| Redis Cache (external non-HA)                 | 1                              |
