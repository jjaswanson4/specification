# Technical Lexicon

## Introduction

This lexicon defines key concepts and terms used throughout the Margo documentation. These definitions are collected here to clarify and simplify diagrams, concepts, and other forms of documentation in other places.

## Categories

For ease of use, definitions are grouped according to logical groupings where possible.

Current groups:
- [Applications and their Sources](#applications-and-their-sources)
- [Agents](#agents)
- [Devices](#devices)
- [Fleet Managers and Functionality](#fleet-managers-and-functionality)

## Applications and their Sources

### Application

An application is a piece of software, provided by an application supplier, to be run on devices within an end user environment.

An application may contain multiple components, and may run across one to many devices, depending on requirements.

To be compliant with Margo, an application must also contain a `margo.yaml` file, which is ingested by a workload fleet manager.

### Application Deployment

An application deployment is an instance of an application, optionally with customizations, that runs on a device. An application deployment state is tracked by a workload fleet manager, based on information from a workload orchestration agent running on a device.

### Application Registry

An Application Registry is a curated, access-controlled storage location where Application manifests and associated marketplace data are stored. Content is typically flat files, or archives of flat files, that represent application information, licenses, and more.

This is typically where the associated `margo.yaml` file is stored.

Where and how an application registry is run is outside of the scope of Margo, but common places are:
- Behind a supplier's customer portal
- On the public internet
- Added on to a workload fleet manager software package

### Application Repository

An Application Repository is a curated, access-controlled storage location which stores the application content, such as container images. An Application Repository should follow the [OCI Image and Distribution Specs](https://opencontainers.org/about/overview/).

Where and how an Application Repository is run is outside of the scope of Margo, but common places are:
- Behind a supplier's customer portal
- On the public internet
- Added on to a workload fleet manager software package

## Agents

### Workload Orchestration Agent

The Workload Orchestration Agent is a software component that runs on devices, which handles communication between the device and the workload fleet manager. It is responsible for gathering what deployments needs to be applied to the device, updating existing deployments, and deleting deployments if no longer needed/desired.

The Workload Orchestration Agent must be able to communicate with the underlying platform or device runtime to handle application deployments. This can be accomplished by communicating over a socket (to docker/podman, for example), or to an API (such as a kubernetes-compliant API). Additional communication methods can be built in for supplier-specific workloads or devices as needed.

How the agent is run by the underlying device is up to the device and/or platform suppliers, however, it's recommended to run this in a portable, repeatable method, as to enable efficient scaling.

### Device Orchestration Agent

The Device Orchestration Agent is an agent that runs on every device, and is associated with a single device fleet manager. This agent is responsible for gathering information about the device, such as capabilities, resources, architecture, etc, and reporting that information back to the device fleet manager.

A Device Orchestration Agent is also responsible for maintaining the validity of the device information when changes occur, relaying that information back to the device fleet manager on a best-effort basis.

Device Orchestration Agents are agnostic to the class or type of device: if only an operating system is available on the device, then the capabilites are reported as such. If, however, a device is actually an orchestration platform, then the capabilities of both the platform and the underlying hardware are reported.

## Devices

### Device

A device is a combination of hardware, provided by a device supplier, an operating system (either general purpose or specialized) that abstracts the underlying hardware resources for consumption by software, a workload orchestration agent, to handle application deployments from a workload fleet manager, and a runtime to run the specified software.

Margo currently defines two deployment profiles: helm-based and docker/podman-based, which require either a kubernetes spec-compliant platform, or docker/podman capabilities, respectively.

Devices can be standalone single nodes, or clusters of nodes, depending on the reqirements of the end user and the application, as long as the above conditions are met. Architecture of devices, such as multi-node clusters, redundancy and failover, high availablility, etc, are out of scope for Margo.

## Fleet Managers and Functionality

### Workload Fleet Manager

A workload fleet manager is a software package, provided by an orchestrator supplier, that ingests applications into a catalog, then creates application deployments, which are applied to devices by a connected workload orchestration agent.

In addition, a workload fleet manager is responsible for maintaining and updating the state of application deployments across a large fleet of devices.

This software package can optionally be combined with a device fleet manager software package, as an "all-in-one" offering. 

### Device Fleet Manager

A Device Fleet Manager is a software package, provided by an orchestrator supplier, that tracks devices and their capabilities. This information is provided to the device fleet manager by a device orchestration agent, which includes device capabilities, architecture, and general information.

In addition, a Device Fleet Manager is responsible for maintaining state and capability information for devices, including the ability for device information to be updated as devices change.

Device Fleet Managers also allow for grouping, visualization, and tagging of devices by end users, as to help drive application deployments. 

### Application Catalog

An Application Catalog is a feature provided by a Workload Fleet Manager, that organizes and presents available applications that can be deployed to devices. This feature aims to greatly simplify the consumption of applications, by housing them centrally and presenting them in an understandable way to an end user.

An application catalog should have role-based access control for multiple personas that will be interacting with the catalog, allowing for fine-grained permissions and visibility.

In addition, a catalog must support common functions for applications, such as creation, updating, and deletion.

## Notes
- Purchasing and procurement of applications is outside the scope of Margo
