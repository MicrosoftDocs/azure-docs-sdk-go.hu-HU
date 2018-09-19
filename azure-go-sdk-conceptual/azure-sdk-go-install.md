---
title: A Góhoz készült Azure SDK telepítése
description: Megtudhatja, hogyan telepítheti, másolhatja be és konfigurálhatja a Góhoz készült Azure SDK-t.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/14/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 013a771345d96f0fa8dbece3218a01650744f70b
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059186"
---
# <a name="install-the-azure-sdk-for-go"></a>A Góhoz készült Azure SDK telepítése

Üdvözöli Önt a Góhoz készült Azure SDK! Az SDK lehetővé teszi, hogy Go-alkalmazásokból kezelhesse és használhassa az Azure-szolgáltatásokat.

## <a name="get-the-azure-sdk-for-go"></a>A Góhoz készült Azure SDK beszerzése

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

Bizonyos Azure-szolgáltatások saját Go SDK-val rendelkeznek, ezért a Góhoz készült Azure SDK főcsomagok nem tartalmazzák őket. A következő táblázatban láthatók a saját SDK-val rendelkező szolgáltatások és a csomagneveik. Ezek a csomagok előzetes verziójú csomagoknak minősülnek.

| Szolgáltatás | Csomag |
|---------|---------|
| Blob Storage | [github.com/Azure/azure-storage-blob-go](https://github.com/Azure/azure-storage-blob-go) |
| File Storage | [github.com/Azure/azure-storage-file-go](https://github.com/Azure/azure-storage-file-go) |
| Tárolási üzenetsor | [github.com/Azure/azure-storage-queue-go](https://github.com/Azure/azure-storage-queue-go) |
| Eseményközpont | [github.com/Azure/azure-event-hubs-go](https://github.com/Azure/azure-event-hubs-go) |
| Service Bus | [github.com/Azure/azure-service-bus-go](https://github.com/Azure/azure-service-bus-go) |
| Application Insights | [github.com/Microsoft/ApplicationInsights-go](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a>A Góhoz készült Azure SDK bemásolása

A Góhoz készült Azure SDK a [dep](https://github.com/golang/dep) eszközön keresztül másolható be. A stabilitás miatt ajánlott a bemásolás elvégzése. A `dep` a saját projektjében való használatához adja a `github.com/Azure/azure-sdk-for-go` elemet a `Gopkg.toml` egyik `[[constraint]]` szakaszához. A `14.0.0` verzió bemásolásához például adja hozzá a következő bejegyzést:

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a>A Góhoz készült Azure SDK projektbe foglalása

Ha a Go-kódból szeretné használni az Azure-szolgáltatásokat, importálja az összes használt szolgáltatást és a szükséges `autorest` modulokat.
A GoDocról beszerezheti az [elérhető szolgáltatások](https://godoc.org/github.com/Azure/azure-sdk-for-go) és az [AutoRest csomagok](https://godoc.org/github.com/Azure/go-autorest) elérhető moduljainak teljes listáját. A `go-autorest` leggyakrabban igényelt csomagjai a következők:

| Csomag | Leírás |
|---------|-------------|
| [github.com/Azure/go-autorest/autorest][autorest] | A szolgáltatásügyfél-hitelesítés kezelésének objektumai |
| [github.com/Azure/go-autorest/autorest/azure][autorest/azure] | Az Azure-szolgáltatások használatának állandói |
| [github.com/Azure/go-autorest/autorest/adal][autorest/adal] | Az Azure-szolgáltatások elérésének hitelesítési mechanizmusai |
| [github.com/Azure/go-autorest/autorest/to][autorest/to] | Az Azure SDK adatstruktúrákon működő típuskijelentési segítők |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

A Go-csomagok és az Azure-szolgáltatások egymástól függetlenül vannak ellátva verzióval. A szolgáltatás verziói a modul importálási útvonalának részei a `services` modul alatt. A modul teljes elérési útja a szolgáltatás neve, amelyet a verzió követ `YYYY-MM-DD` formátumban, amelyet ismét a szolgáltatás neve követ. A számítási szolgáltatás `2017-03-30` verziójának importálásához például:

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

A fejlesztés megkezdésekor ajánlott a szolgáltatás legújabb verzióját használni és annál is maradni.
A szolgáltatás követelményei megváltozhatnak az egyes verziók között, ami miatt a kódja működésképtelenné válhat, még akkor is, ha az alatt az idő alatt a Go SDK nem frissült.

Ha a szolgáltatások együttes pillanatképére van szüksége, kiválaszthat egyetlen profilverziót is. Jelenleg az egyetlen zárolt profil a `2017-03-09` verzió, amelyben elképzelhető, hogy nem a legújabb szolgáltatásfunkciók szerepelnek. A profilok a `profiles` modul alatt találhatók, a verziójuk `YYYY-MM-DD` formátumban van. A szolgáltatások a profilverzióik alatt vannak csoportosítva. Ha például az Azure-erőforrások felügyeleti modult szeretné importálni a `2017-03-09` profilból:

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> A `preview` és `latest` profilok is elérhetők. Ezek használata nem ajánlott. Ezek a profilok működés közbeni verziók, és a szolgáltatás működése bármikor megváltozhat.

## <a name="next-steps"></a>További lépések

A Góhoz készült Azure SDK használatának megkezdéséhez próbáljon ki egy rövid útmutatót.

* [Virtuális gép üzembe helyezése egy sablonból](azure-sdk-go-qs-vm.md)
* [Objektumok továbbítása az Azure Blob Storage-be a Góhoz készült Azure Blob SDK-val](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [Csatlakozás az Azure Database for PostgreSQL-hez](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

Ha most azonnal el szeretné kezdeni a Go SDK más szolgáltatásainak használatát, tekintsen meg néhány rendelkezésre álló mintakódot.

* [Hitelesítés az Azure-szolgáltatásokkal](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [Új virtuális gépek üzembe helyezése SSH hitelesítéssel](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [Tárolórendszerkép üzembe helyezése az Azure Container Instances szolgáltatásban](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [Fürt létrehozása Azure Kubernetes-szolgáltatásban](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [Az Azure Storage-szolgáltatások használata](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [A Góhoz készült Azure SDK összes mintája](https://github.com/azure-samples/azure-sdk-for-go-samples)
