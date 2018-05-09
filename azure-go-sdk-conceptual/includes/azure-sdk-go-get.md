---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: ddd58efdbc0c2d3ded068a9bebf2466db702566f
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/03/2018
---
<span data-ttu-id="8322f-101">A [Góhoz készült Azure SDK](https://github.com/Azure/azure-sdk-for-go) a Go 1.8-as és újabb verzióival kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="8322f-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="8322f-102">Az [Azure Stack-profilokat](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles) használó környezetekben a Go 1.9-es verziója a minimális követelmény.</span><span class="sxs-lookup"><span data-stu-id="8322f-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="8322f-103">Ha nem érhető el a rendszeren a Go, kövesse [a Go telepítési utasításait](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="8322f-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="8322f-104">A Góhoz készült Azure SDK és annak függőségei a következőn keresztül szerezhetők be: `go get`.</span><span class="sxs-lookup"><span data-stu-id="8322f-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="8322f-105">Győződjön meg arról, hogy az `Azure` nagybetűvel szerepeljen az URL-címben.</span><span class="sxs-lookup"><span data-stu-id="8322f-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="8322f-106">Ellenkező esetben az írásmóddal kapcsolatos importálási hibák fordulhatnak elő az SDK használatakor.</span><span class="sxs-lookup"><span data-stu-id="8322f-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="8322f-107">Az `Azure` szót az importálási utasításokban is nagybetűvel kell megadni.</span><span class="sxs-lookup"><span data-stu-id="8322f-107">You also need to capitalize `Azure` in your import statements.</span></span>

