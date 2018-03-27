---
title: Góhoz készült Azure SDK-minták számításhoz és hálózatkezeléshez
description: A Góhoz készült Azure SDK kiválasztott mintái számítási erőforrások, például virtuális gépek, valamint virtuális hálózatok használatához.
author: sptramer
ms.author: sttramer
ms.date: 03/21/2018
ms.topic: article
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 1e6f9d848213f0f70ab59d5e0bcc5c52127e97c5
ms.sourcegitcommit: fcc1786d59d2e32c97a9a8e0748e06f564a961bd
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/23/2018
---
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a><span data-ttu-id="d338c-103">Góhoz készült Azure SDK-minták számításhoz és hálózatkezeléshez</span><span class="sxs-lookup"><span data-stu-id="d338c-103">Azure SDK for Go samples for compute and networking</span></span>

<span data-ttu-id="d338c-104">Az alábbi táblázatban szereplő hivatkozások kiválasztott Go-forráskódmintákra mutatnak, amelyek virtuális gépek, virtuális hálózatok és alhálózatok kezeléséhez használhatók az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="d338c-104">The following table links to selected samples of Go source code that you can use to manage VMs, virtual networks, and subnets in Azure.</span></span> 

<span data-ttu-id="d338c-105">Az összes Góhoz készült Azure SDK-minta elérhető a [GitHubon](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span><span class="sxs-lookup"><span data-stu-id="d338c-105">All samples for the Azure SDK for Go are available on [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span></span>

| <span data-ttu-id="d338c-106">Name (Név)</span><span class="sxs-lookup"><span data-stu-id="d338c-106">Name</span></span> | <span data-ttu-id="d338c-107">Leírás</span><span class="sxs-lookup"><span data-stu-id="d338c-107">Description</span></span> |
|------|-------------|
| [<span data-ttu-id="d338c-108">network/network</span><span class="sxs-lookup"><span data-stu-id="d338c-108">network/network</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | <span data-ttu-id="d338c-109">Hálózati erőforrások létrehozása, frissítése, törlése és lekérdezése, beleértve a virtuális hálózatokat, alhálózatokat és hálózati biztonsági csoportokat.</span><span class="sxs-lookup"><span data-stu-id="d338c-109">Create, update, delete, and query network resources including virtual networks, subnets, and network security groups.</span></span> |
| [<span data-ttu-id="d338c-110">compute/loadbalancer</span><span class="sxs-lookup"><span data-stu-id="d338c-110">compute/loadbalancer</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/loadbalancer.go) | <span data-ttu-id="d338c-111">Rendelkezésre állási csoportok létrehozása és lekérdezése, és terheléselosztóval rendelkező virtuális gépek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d338c-111">Create and query availability groups, and create VMs with a load balancer.</span></span> |
| [<span data-ttu-id="d338c-112">compute/compute</span><span class="sxs-lookup"><span data-stu-id="d338c-112">compute/compute</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/compute.go) | <span data-ttu-id="d338c-113">Virtuális gépek létrehozása, törlése, frissítése és teljesítménykezelése.</span><span class="sxs-lookup"><span data-stu-id="d338c-113">Create, delete, update, and power-manage VMs.</span></span> <span data-ttu-id="d338c-114">Adatlemezek használata és a virtuális gép operációsrendszer-lemezének kezelése.</span><span class="sxs-lookup"><span data-stu-id="d338c-114">Work with data disks and managing the OS disk of the VM.</span></span> |
