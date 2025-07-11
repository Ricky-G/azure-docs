---
title: Subscribe to events
titleSuffix: An Azure Communication Services article
description: This article describes how to subscribe to events from Azure Communication Services.
author: pgrandhi
manager: rasubram
services: azure-communication-services
ms.author: pgrandhi
ms.date: 06/28/2025
ms.topic: quickstart
ms.service: azure-communication-services
ms.subservice: arm
zone_pivot_groups: acs-plat-azp-azcli-net-ps
ms.custom: mode-other, devx-track-azurecli, devx-track-azurepowershell
ms.devlang: azurecli 
---

# Subscribe to events

This article describes how to subscribe to events from Azure Communication Services through the portal, Azure CLI, PowerShell, and .NET SDK.

You can set up event subscriptions for Communication Services resources through the [Azure portal](https://portal.azure.com), Azure CLI, PowerShell, or with the Azure [Event Grid Management SDK](https://www.nuget.org/packages/Azure.ResourceManager.EventGrid/).

This article describes the process of setting up a webhook as a subscriber for SMS events from Azure Communication Services. For a full list of events, see [Azure Communication Services as an Azure Event Grid source](/azure/event-grid/event-schema-communication-services).

::: zone pivot="platform-azp"
[!INCLUDE [Azure portal](./includes/create-event-subscription-azp.md)]
::: zone-end

::: zone pivot="platform-azcli"
[!INCLUDE [Azure CLI](./includes/create-event-subscription-az-cli.md)]
::: zone-end

::: zone pivot="platform-net"
[!INCLUDE [.NET](./includes/create-event-subscription-net.md)]
::: zone-end

::: zone pivot="platform-powershell"
[!INCLUDE [PowerShell](./includes/create-event-subscription-powershell.md)]
::: zone-end
