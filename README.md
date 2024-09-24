---
title: Supported Topologies
---

# Infrastructure Topologies

This document outlines the architecture of different deployment topology setups on which our CI tests will run, providing a comprehensive overview of these setups, their components, interactions, and the rationale behind the design choices.

## Topology/Environment Matrix

The following matrix defines the topology and environment configurations to target.

| Infra Topology | Environment |
| -------------- | ----------- |
| RPM-A          | ENV-A       |
| RPM-B          | ENV-A       |
| CONT-A         | ENV-A       |
| CONT-B         | ENV-A       |
| MAN-B          | ENV-A       |
| OCP-A          | ENV-A       |
| OCP-B          | ENV-A       |
| RPM-A          | ENV-B       |
| RPM-B          | ENV-B       |

These shorthand identifiers make it easier to track which infra type and environment configuration are in use. All "A" type infrastructure topologies are meant for "Standard" deployments, for smaller scale customers. All "B" type topologies are designated "Enterprise" and include additional resources for more robust deployments.

## Environment Configurations

Due to business requirements, we have opted to support environments both fully updated to the latest 2.5 release of AAP, as well as allowing customers to install _only Gateway and EDA_ to connect to their existing 2.4 environments. The intent is to allow customers who are adverse to a full upgrade to experience the enhanced functionality of the Platform Gateway and EDA enterprise functionality.

1. All 2.5 (**ENV-A**)

   - AAP Gateway 2.5
   - Controller 2.5
   - Private Automation Hub 2.5
   - EDA 2.5

2. EDA-GA (**ENV-B**)
   _Note_: "Mixed versioned" topologies are **only supported on RPM-based installation methods**

- AAP Gateway 2.5
- Controller 2.4
- Private Automation Hub 2.4
- EDA 2.5

## RPM - Install Method

RPM installations are the "classic" way of installing AAP. This always requires multiple VMs; how many is based on which deployment type is chosen.

##### Standard RPM Deployment [(RPM-A-ENV-A)](rpm-a.env-a/README.md) [(RPM-A-ENV-B)](rpm-a.env-b/README.md)

- 1 VM for AAP Gateway / Unified UI
- 1 VM for Automation Controller
- 1 VM for Private Automation Hub
- 1 VM for Event Driven Ansible
- 1 VM for Automation Mesh Execution Node
- 1 VM Managed Database

##### Enterprise RPM Deployment [(RPM-B-ENV-A)](rpm-b.env-a/README.md) [(RPM-B-ENV-B)](rpm-b.env-b/README.md)

- 2 VMs for AAP Gateway / Unified UI
- 2 VMs for Automation Controller
- 2 VMs for Private Automation Hub
- 2 VMs for Event Driven Ansible
- 1 VM for Automation Mesh Hop Node
- 2 VM for Automation Mesh Execution Node
- External unmanaged database service
- HAProxy load balancer in front of AAP Gateway

## Containerized - Install Method

Containerized installations are being fully supported starting with AAP 2.5. There is both a "single" VM option, requiring very limited infrastructure as well as the "enterprise" deployment, requiring the same number of VMs as the RPM install method.

##### Single VM Deployment [(CONT-A-ENV-A)](cont-a.env-a/README.md) [(CONT-A-ENV-B)](cont-a.env-a/README.md)

- All components collocated on a single VM or "All in One (aio)"
- 1 VM for AAP Gateway / Unified UI, Controller, Private Automation Hub, EDA, Managed Database
  - Each component is on a single container

##### Enterprise Containerized [(CONT-B-ENV-A)](cont-b.env-a/README.md) [(CONT-B-ENV-B)](cont-b.env-b/README.md)

- 2 VMs for AAP Gateway / Unified UI - with redis
- 2 VMs for Automation Controller
- 2 VMs for Private Automation Hub - with redis
- 2 VMs for Event Driven Ansible - with redis
- 1 VM for Automation Mesh Hop Node
- 2 VM for Automation Mesh Execution Node
- External unmanaged database service
- HAProxy load balancer in front of AAP Gateway

## Managed AAP

Any Red Hat managed deployment will be considered "Enterprise", thus there is no "Standard" deployment available for Managed AAP.

##### Enterprise Ansible on Azure (MAN-B)

- 1 VM for Hop Node (traditional mesh)
- 1 VM for Execution Node (traditional mesh)
- 1 VM for Execution Node (mesh ingress)

## OpenShift - AAP Operator Install Method

Openshift installations require a Red Hat subscription to Red Hat Openshift Container Platform. Deploying on OCP is only supported via the Operator available in the OCP Operator Marketplace.

##### Standard Deployment [(OCP-A)]

- Operator defaults for the deployment

##### Enterprise Deployment [(OCP-B-ENV-A)](ocp-b.env-a/README.md) [(OCP-B-ENV-B)](ocp-b.env-b/README.md)

- This specific deployment matches both "Enterprise" OCP and what we provide for our SaaS (AAP as a Service) customers.
  - 1 pod for AAP Gateway / Unified UI
  - 2 pods for Automation Controller
    - task
    - web
  - 5 pods for Private Automation Hub
    - api
    - content
    - web
    - 2 workers
  - x pods for Event Driven Ansible
  - 2 pods for mesh ingress
  - Each pod has 1 replica set configured by default
  - External managed redis service
  - External managed database service

## Minimum Requirements for VM Resource


| Requirement  | Required                                                          | Notes                                                                                                                                                                                                                                                                                                                                                                              |     |     |
| ------------ | ----------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- | --- |
| Subscription | Valid Red Hat Ansible Automation Platform                         |                                                                                                                                                                                                                                                                                                                                                                                    |     |     |
| OS           | Red Hat Enterprise Linux 9.4 or later 64-bit (x86)                | Red Hat Ansible Automation Platform is also supported on OpenShift, see [Deploying the Red Hat Ansible Automation Platform operator on OpenShift Container Platform](https://docs.redhat.com/en/documentation/red_hat_ansible_automation_platform/2.4/html/deploying_the_red_hat_ansible_automation_platform_operator_on_openshift_container_platform/index) for more information. |     |     |
| Ansible-core | Ansible-core version 2.15 or later                                | Ansible Automation Platform includes execution environments that contain ansible-core 2.15.                                                                                                                                                                                                                                                                                        |     |     |
| Python       | 3.11 or later                                                     |                                                                                                                                                                                                                                                                                                                                                                                    |     |     |
| Browser      | A currently supported version of Mozilla FireFox or Google Chrome |                                                                                                                                                                                                                                                                                                                                                                                    |     |     |
| Database     | PostgreSQL version 15                                             |                                                                                                                                                                                                                                                                                                                                                                                    |     |     |

The following ports must be opened on the host machine in order to ensure inter-service communication.

- 22
- 80
- 443
- 6379  
- 27199

## AAP Components

### 1. AAP Gateway

A platform service that aims to centralize access to the various platform services by SSO configurations, serving the AAP UI and providing a common authentication layer that all of the existing services will be able to integrate.

### 2. Automation Controller

Serves as the control plane for automation, offering a user interface (UI), RESTful API, role-based access control (RBAC) workflows, and CI/CD integrations.

### 3. Automation Mesh

An overlay network that facilitates the distribution of work across a large and dispersed collection of workers. It achieves this through nodes that establish peer-to-peer connections using existing networks.

### 4. Private Automation Hub

Enables automation developers to collaborate and publish their automation content, streamlining the delivery of Ansible code within their organization.

### 5. Event-Driven Ansible

Provides the event-handling capability necessary to automate time-consuming tasks and respond to changing conditions in any IT domain.

### 6. Redis Deployment

A single server rpm install of Redis in the rpm installer will be implemented. This does not support high availability (HA). Ansible supports external Redis deployments for SaaS. Redis HA is deployed and supported for OCP and Containerized Deployments.
