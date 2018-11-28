---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 5df14f939efdd0550b49261c88c8dc6518ada459
ms.sourcegitcommit: 887b15afcdeaf926a5f3d21b64e4045167fd062c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/23/2018
ms.locfileid: "52293508"
---
<span data-ttu-id="ded5f-101">A [Góhoz készült Azure SDK](https://github.com/Azure/azure-sdk-for-go) a Go 1.8-as és újabb verzióival kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="ded5f-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and higher.</span></span> <span data-ttu-id="ded5f-102">Az [Azure Stack-profilokat](/azure/azure-stack/user/azure-stack-version-profiles-go) használó környezetekben a Go 1.9-es verziója a minimális követelmény.</span><span class="sxs-lookup"><span data-stu-id="ded5f-102">For environments using [Azure Stack Profiles](/azure/azure-stack/user/azure-stack-version-profiles-go), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="ded5f-103">Ha telepítenie kell a Gót, [kövesse a Go telepítési utasításait](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="ded5f-103">If you need to install Go, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="ded5f-104">A Góhoz készült Azure SDK-t és annak függőségeit a következőn keresztül töltheti le: `go get`.</span><span class="sxs-lookup"><span data-stu-id="ded5f-104">You can download the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="ded5f-105">Győződjön meg arról, hogy az `Azure` nagybetűvel szerepeljen az URL-címben.</span><span class="sxs-lookup"><span data-stu-id="ded5f-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="ded5f-106">Ellenkező esetben az írásmóddal kapcsolatos importálási hibák fordulhatnak elő az SDK használatakor.</span><span class="sxs-lookup"><span data-stu-id="ded5f-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="ded5f-107">Az `Azure` szót az importálási utasításokban is nagybetűvel kell megadni.</span><span class="sxs-lookup"><span data-stu-id="ded5f-107">You also need to capitalize `Azure` in your import statements.</span></span>
