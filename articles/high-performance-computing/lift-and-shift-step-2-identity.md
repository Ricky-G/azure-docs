---
title: "Identity management configuration"
description: Learn how to configure identity management during a migration of high performance computing architecture.
author: tomvcassidy
ms.author: tomcassidy
ms.date: 04/10/2025
ms.topic: how-to
ms.service: azure-virtual-machines
ms.subservice: hpc
# Customer intent: As an IT administrator overseeing high performance computing migration, I want to configure identity management systems, so that I can ensure secure access to resources and comply with organizational policies during the migration process.
---

# Identity management configuration

A key aspect of your high performance computing migration is identity management configuration. Your identity management system ensures secure access to resources, seamless integration, and compliance with organizational policies. It should allow the creation and deletion of users and groups, allow updating and resetting of passwords, and support single sign-on. This part of the guide covers the needs, tools, services, and best practices associated with identity management.

## Define identity management needs

**User IDs, passwords, home directories:**
  - Users require IDs, passwords, location of their home directories for the all resources they need to access.
  - Understand where just definitions should be located, which can be on-premises, in the cloud, or a combination of those options

## Tools and services

* **Active Directory domain services:**
  - Some enterprises already use Windows-based Active Directory Domain Services, which could be applied in the new HPC cloud environment.

* **Microsoft Entra ID**:
  - Some services can also make use of Microsoft Entra ID, a cloud-based identity and access management service, especially when single sign-on is required.

## Best practices

* **Resources to be accessed:**
  - Have clarity on all resources users need to have access to, including cluster nodes and storage devices.

* **Performance:**
  - One can start addressing performance by using the on-premises identity and access service. It's important to verify the performance of such solutions and trade-offs of speed to authenticate and complexity of the service.

## Example identity management setup

The following Azure HPC blog post describes in details the steps to authenticate users in an Azure CycleCloud HPC cluster via Active Directory:

Authenticating Active Directory users to an Azure CycleCloud HPC cluster: [blog post](https://techcommunity.microsoft.com/t5/azure-high-performance-computing/authenticating-active-directory-users-to-an-azure-cyclecloud-hpc/ba-p/3757085)

## Resources

- Active Directory Domain Services: [product website](/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)
- Microsoft Entra ID: [product website](/entra/identity/)
- Authenticating Active Directory users to an Azure CycleCloud HPC cluster: [blog post](https://techcommunity.microsoft.com/t5/azure-high-performance-computing/authenticating-active-directory-users-to-an-azure-cyclecloud-hpc/ba-p/3757085)
