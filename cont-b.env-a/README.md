# CONT-B.ENV-A - Enterprise Containerized Deployment

## Containerized multi VM deployment

## Topology

![Enterprise Containerized Topology](Cont-B_Env-A.png)

## Description

The **AAP Components deployed inside a container** consists of the following:

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
