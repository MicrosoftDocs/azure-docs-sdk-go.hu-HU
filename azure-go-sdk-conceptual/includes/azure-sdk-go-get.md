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
A [Góhoz készült Azure SDK](https://github.com/Azure/azure-sdk-for-go) a Go 1.8-as és újabb verzióival kompatibilis. Az [Azure Stack-profilokat](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles) használó környezetekben a Go 1.9-es verziója a minimális követelmény.
Ha nem érhető el a rendszeren a Go, kövesse [a Go telepítési utasításait](https://golang.org/doc/install).

A Góhoz készült Azure SDK és annak függőségei a következőn keresztül szerezhetők be: `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Győződjön meg arról, hogy az `Azure` nagybetűvel szerepeljen az URL-címben. Ellenkező esetben az írásmóddal kapcsolatos importálási hibák fordulhatnak elő az SDK használatakor. Az `Azure` szót az importálási utasításokban is nagybetűvel kell megadni.
