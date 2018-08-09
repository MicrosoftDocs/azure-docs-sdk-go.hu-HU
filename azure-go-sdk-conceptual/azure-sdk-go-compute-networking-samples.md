---
title: Góhoz készült Azure SDK-minták számításhoz és hálózatkezeléshez
description: A Góhoz készült Azure SDK kiválasztott mintái számítási erőforrások, például virtuális gépek, valamint virtuális hálózatok használatához.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/21/2018
ms.topic: sample
ms.prod: azure
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: 3b31716ee42c638bab4a6dd99b9eb0d7c07e51a4
ms.sourcegitcommit: 0f581979216f7c9d4913681a6d9f6fe09af26e43
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/02/2018
ms.locfileid: "39475789"
---
# <a name="azure-sdk-for-go-samples-for-compute-and-networking"></a><span data-ttu-id="8abe6-103">Góhoz készült Azure SDK-minták számításhoz és hálózatkezeléshez</span><span class="sxs-lookup"><span data-stu-id="8abe6-103">Azure SDK for Go samples for compute and networking</span></span>

<span data-ttu-id="8abe6-104">Az alábbi táblázatban szereplő hivatkozások kiválasztott Go-forráskódmintákra mutatnak, amelyek virtuális gépek, virtuális hálózatok és alhálózatok kezeléséhez használhatók az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="8abe6-104">The following table links to selected samples of Go source code that you can use to manage VMs, virtual networks, and subnets in Azure.</span></span> 

<span data-ttu-id="8abe6-105">Az összes Góhoz készült Azure SDK-minta elérhető a [GitHubon](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span><span class="sxs-lookup"><span data-stu-id="8abe6-105">All samples for the Azure SDK for Go are available on [GitHub](https://github.com/Azure-Samples/azure-sdk-for-go-samples).</span></span>

| <span data-ttu-id="8abe6-106">Name (Név)</span><span class="sxs-lookup"><span data-stu-id="8abe6-106">Name</span></span> | <span data-ttu-id="8abe6-107">Leírás</span><span class="sxs-lookup"><span data-stu-id="8abe6-107">Description</span></span> |
|------|-------------|
| [<span data-ttu-id="8abe6-108">network/network</span><span class="sxs-lookup"><span data-stu-id="8abe6-108">network/network</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/network/network.go) | <span data-ttu-id="8abe6-109">Hálózati erőforrások létrehozása, frissítése, törlése és lekérdezése, beleértve a virtuális hálózatokat, alhálózatokat és hálózati biztonsági csoportokat.</span><span class="sxs-lookup"><span data-stu-id="8abe6-109">Create, update, delete, and query network resources including virtual networks, subnets, and network security groups.</span></span> |
| [<span data-ttu-id="8abe6-110">compute/vm_disk</span><span class="sxs-lookup"><span data-stu-id="8abe6-110">compute/vm_disk</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_disk.go) | <span data-ttu-id="8abe6-111">Virtuális gépek adatlemezeinek létrehozása, csatlakoztatása, leválasztása, frissítése és titkosítása.</span><span class="sxs-lookup"><span data-stu-id="8abe6-111">Create, attach, detatch, update, and encrypt data disks for a VM.</span></span> |
| [<span data-ttu-id="8abe6-112">compute/vm</span><span class="sxs-lookup"><span data-stu-id="8abe6-112">compute/vm</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) | <span data-ttu-id="8abe6-113">Virtuális gépek létrehozása, frissítése és inaktiválása.</span><span class="sxs-lookup"><span data-stu-id="8abe6-113">Create, update, deactivate, and manage VMs.</span></span> |
| [<span data-ttu-id="8abe6-114">compute/vm_with_availabilityset</span><span class="sxs-lookup"><span data-stu-id="8abe6-114">compute/vm_with_availabilityset</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_availabilityset.go) | <span data-ttu-id="8abe6-115">Rendelkezésre állási csoportok és terheléselosztók létrehozása virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="8abe6-115">Create availability sets and load balancers for VMs.</span></span> |
| [<span data-ttu-id="8abe6-116">compute/vm_with_identity</span><span class="sxs-lookup"><span data-stu-id="8abe6-116">compute/vm_with_identity</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm_with_identity.go) | <span data-ttu-id="8abe6-117">Virtuális gépekkel használt felügyeltszolgáltatás-identitások (MSI) létrehozása és kezelése.</span><span class="sxs-lookup"><span data-stu-id="8abe6-117">Create and manage Managed Service Identities (MSIs) for VMs.</span></span> |
