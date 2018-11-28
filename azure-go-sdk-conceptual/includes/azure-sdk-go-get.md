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
A [Góhoz készült Azure SDK](https://github.com/Azure/azure-sdk-for-go) a Go 1.8-as és újabb verzióival kompatibilis. Az [Azure Stack-profilokat](/azure/azure-stack/user/azure-stack-version-profiles-go) használó környezetekben a Go 1.9-es verziója a minimális követelmény.
Ha telepítenie kell a Gót, [kövesse a Go telepítési utasításait](https://golang.org/doc/install).

A Góhoz készült Azure SDK-t és annak függőségeit a következőn keresztül töltheti le: `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Győződjön meg arról, hogy az `Azure` nagybetűvel szerepeljen az URL-címben. Ellenkező esetben az írásmóddal kapcsolatos importálási hibák fordulhatnak elő az SDK használatakor. Az `Azure` szót az importálási utasításokban is nagybetűvel kell megadni.
