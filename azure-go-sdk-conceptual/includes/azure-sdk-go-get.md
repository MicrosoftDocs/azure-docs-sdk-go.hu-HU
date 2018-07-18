---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: d021dd8ef4744b7c50b296b231bf63481f92411a
ms.sourcegitcommit: 2a3bd491e087a1d0e7d269bed896c029357d62a6
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988080"
---
<span data-ttu-id="616d9-101">A [Góhoz készült Azure SDK](https://github.com/Azure/azure-sdk-for-go) a Go 1.8-as és újabb verzióival kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="616d9-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="616d9-102">Az [Azure Stack-profilokat](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles) használó környezetekben a Go 1.9-es verziója a minimális követelmény.</span><span class="sxs-lookup"><span data-stu-id="616d9-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="616d9-103">Ha nem érhető el a rendszeren a Go, kövesse [a Go telepítési utasításait](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="616d9-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="616d9-104">A Góhoz készült Azure SDK és annak függőségei a következőn keresztül szerezhetők be: `go get`.</span><span class="sxs-lookup"><span data-stu-id="616d9-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="616d9-105">Győződjön meg arról, hogy az `Azure` nagybetűvel szerepeljen az URL-címben.</span><span class="sxs-lookup"><span data-stu-id="616d9-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="616d9-106">Ellenkező esetben az írásmóddal kapcsolatos importálási hibák fordulhatnak elő az SDK használatakor.</span><span class="sxs-lookup"><span data-stu-id="616d9-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="616d9-107">Az `Azure` szót az importálási utasításokban is nagybetűvel kell megadni.</span><span class="sxs-lookup"><span data-stu-id="616d9-107">You also need to capitalize `Azure` in your import statements.</span></span>
