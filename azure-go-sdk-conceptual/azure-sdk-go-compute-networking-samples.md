---
title: Góhoz készült Azure SDK-minták számításhoz és hálózatkezeléshez
description: A Góhoz készült Azure SDK kiválasztott mintái számítási erőforrások, például virtuális gépek, valamint virtuális hálózatok használatához.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: sample
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: d570ad8598ae06633d0010245c207641161ee446
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059084"
---
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a><span data-ttu-id="62030-103">Góhoz készült Azure SDK-minták számításhoz és hálózatkezeléshez</span><span class="sxs-lookup"><span data-stu-id="62030-103">Azure SDK for Go samples for compute and networking</span></span>

<span data-ttu-id="62030-104">Az alábbi táblázatban szereplő hivatkozások kiválasztott mintákra mutatnak, amelyek bemutatják a számítási és a virtuális hálózati erőforrások kezelését a Góhoz készült Azure SDK-ból.</span><span class="sxs-lookup"><span data-stu-id="62030-104">The following table links to selected samples that demonstrate the management of compute and virtual network resources in the Azure SDK for Go.</span></span>

<span data-ttu-id="62030-105">Az összes Góhoz készült Azure SDK-minta elérhető a [GitHubon](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span><span class="sxs-lookup"><span data-stu-id="62030-105">All samples for the Azure SDK for Go are available on [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span></span>

| <span data-ttu-id="62030-106">Name (Név)</span><span class="sxs-lookup"><span data-stu-id="62030-106">Name</span></span> | <span data-ttu-id="62030-107">Leírás</span><span class="sxs-lookup"><span data-stu-id="62030-107">Description</span></span> |
|------|-------------|
| [<span data-ttu-id="62030-108">network/network</span><span class="sxs-lookup"><span data-stu-id="62030-108">network/network</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | <span data-ttu-id="62030-109">Hálózati erőforrások létrehozása, frissítése, törlése és lekérdezése, beleértve a virtuális hálózatokat, alhálózatokat és hálózati biztonsági csoportokat.</span><span class="sxs-lookup"><span data-stu-id="62030-109">Create, update, delete, and query network resources including virtual networks, subnets, and network security groups.</span></span> |
| [<span data-ttu-id="62030-110">compute/vm_disk</span><span class="sxs-lookup"><span data-stu-id="62030-110">compute/vm_disk</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_disk.go) | <span data-ttu-id="62030-111">Virtuális gépek adatlemezeinek létrehozása, csatolása, leválasztása, frissítése és titkosítása.</span><span class="sxs-lookup"><span data-stu-id="62030-111">Create, attach, detach, update, and encrypt data disks for a VM.</span></span> |
| [<span data-ttu-id="62030-112">compute/vm</span><span class="sxs-lookup"><span data-stu-id="62030-112">compute/vm</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) | <span data-ttu-id="62030-113">Virtuális gépek létrehozása, frissítése és inaktiválása.</span><span class="sxs-lookup"><span data-stu-id="62030-113">Create, update, deactivate, and manage VMs.</span></span> |
| [<span data-ttu-id="62030-114">compute/vm_with_availabilityset</span><span class="sxs-lookup"><span data-stu-id="62030-114">compute/vm_with_availabilityset</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_availabilityset.go) | <span data-ttu-id="62030-115">Rendelkezésre állási csoportok és terheléselosztók létrehozása virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="62030-115">Create availability sets and load balancers for VMs.</span></span> |
| [<span data-ttu-id="62030-116">compute/vm_with_identity</span><span class="sxs-lookup"><span data-stu-id="62030-116">compute/vm_with_identity</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_identity.go) | <span data-ttu-id="62030-117">Virtuális gépekkel használt felügyeltszolgáltatás-identitások (MSI) létrehozása és kezelése.</span><span class="sxs-lookup"><span data-stu-id="62030-117">Create and manage Managed Service Identities (MSIs) for VMs.</span></span> |
