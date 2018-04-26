---
title: A Góhoz készült Azure SDK telepítése
description: Megtudhatja, hogyan telepítheti, másolhatja be és konfigurálhatja a Góhoz készült Azure SDK-t.
author: sptramer
ms.author: sttramer
ms.date: 03/14/2018
ms.topic: article
ms.devlang: go
manager: carmonm
ms.openlocfilehash: a6a92e080aea1a92f47a9d7083f133ca05a47541
ms.sourcegitcommit: 26520a8c6e812facb5b9432d68c370fa23c99888
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/16/2018
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="cf616-103">A Góhoz készült Azure SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="cf616-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="cf616-104">Üdvözöli Önt a Góhoz készült Azure SDK!</span><span class="sxs-lookup"><span data-stu-id="cf616-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="cf616-105">Az SDK lehetővé teszi, hogy Go-alkalmazásokból kezelhesse és használhassa az Azure-szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="cf616-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="cf616-106">A Góhoz készült Azure SDK beszerzése</span><span class="sxs-lookup"><span data-stu-id="cf616-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="cf616-107">Az Azure Storage-blobok használata külön SDK-t igényel.</span><span class="sxs-lookup"><span data-stu-id="cf616-107">Working with Azure Storage Blobs requires a separate SDK.</span></span>

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="cf616-108">A Góhoz készült Azure SDK bemásolása</span><span class="sxs-lookup"><span data-stu-id="cf616-108">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="cf616-109">A Góhoz készült Azure SDK a [dep](https://github.com/golang/dep) eszközön keresztül másolható be.</span><span class="sxs-lookup"><span data-stu-id="cf616-109">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="cf616-110">A stabilitás miatt ajánlott a bemásolás elvégzése.</span><span class="sxs-lookup"><span data-stu-id="cf616-110">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="cf616-111">A `dep` támogatás használatához adja a `github.com/Azure/azure-sdk-for-go` elemet a `Gopkg.toml` egyik `[[constraint]]` szakaszához.</span><span class="sxs-lookup"><span data-stu-id="cf616-111">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="cf616-112">A `14.0.0` verzió bemásolásához például adja hozzá a következő bejegyzést:</span><span class="sxs-lookup"><span data-stu-id="cf616-112">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="cf616-113">A Góhoz készült Azure SDK projektbe foglalása</span><span class="sxs-lookup"><span data-stu-id="cf616-113">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="cf616-114">Ha a Go-kódból szeretné használni az Azure-szolgáltatásokat, importálja az összes használt szolgáltatást és a szükséges `autorest` modulokat.</span><span class="sxs-lookup"><span data-stu-id="cf616-114">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="cf616-115">A GoDocról beszerezheti az [elérhető szolgáltatások](https://godoc.org/github.com/Azure/azure-sdk-for-go) és az [AutoRest csomagok](https://godoc.org/github.com/Azure/go-autorest) elérhető moduljainak teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="cf616-115">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="cf616-116">A `go-autorest` leggyakrabban igényelt csomagjai a következők:</span><span class="sxs-lookup"><span data-stu-id="cf616-116">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="cf616-117">Csomag</span><span class="sxs-lookup"><span data-stu-id="cf616-117">Package</span></span> | <span data-ttu-id="cf616-118">Leírás</span><span class="sxs-lookup"><span data-stu-id="cf616-118">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="cf616-119">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="cf616-119">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="cf616-120">A szolgáltatásügyfél-hitelesítés kezelésének objektumai</span><span class="sxs-lookup"><span data-stu-id="cf616-120">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="cf616-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="cf616-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="cf616-122">Az Azure-szolgáltatások használatának állandói</span><span class="sxs-lookup"><span data-stu-id="cf616-122">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="cf616-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="cf616-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="cf616-124">Az Azure-szolgáltatások elérésének hitelesítési mechanizmusai</span><span class="sxs-lookup"><span data-stu-id="cf616-124">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="cf616-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="cf616-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="cf616-126">Az Azure SDK adatstruktúrákon működő típuskijelentési segítők</span><span class="sxs-lookup"><span data-stu-id="cf616-126">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="cf616-127">Az Azure-szolgáltatások moduljai azok SDK API-jaitól függetlenül vannak ellátva verzióval.</span><span class="sxs-lookup"><span data-stu-id="cf616-127">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="cf616-128">Ezek a verziók a modul importálási útvonalának részei, és egy _szolgáltatásverzióból_ vagy _profilból_ származnak.</span><span class="sxs-lookup"><span data-stu-id="cf616-128">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="cf616-129">Jelenleg ajánlott a fejlesztéshez és a kiadáshoz is egy adott szolgáltatásverziót használnia.</span><span class="sxs-lookup"><span data-stu-id="cf616-129">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="cf616-130">A szolgáltatások a `services` modul alatt találhatók.</span><span class="sxs-lookup"><span data-stu-id="cf616-130">Services are located under the `services` module.</span></span> <span data-ttu-id="cf616-131">Az importálás teljes elérési útja a szolgáltatás neve, amelyet a verzió követ `YYYY-MM-DD` formátumban, amelyet ismét a szolgáltatás neve követ.</span><span class="sxs-lookup"><span data-stu-id="cf616-131">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="cf616-132">A számítási szolgáltatás `2017-03-30` verziójának belefoglalásához például:</span><span class="sxs-lookup"><span data-stu-id="cf616-132">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="cf616-133">Jelenleg ajánlott, hogy a szolgáltatások legújabb verzióját használja, ha nincs rá oka, hogy ne így tegyen.</span><span class="sxs-lookup"><span data-stu-id="cf616-133">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="cf616-134">Ha a szolgáltatások együttes pillanatképére van szüksége, kiválaszthat egyetlen profilverziót is.</span><span class="sxs-lookup"><span data-stu-id="cf616-134">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="cf616-135">Jelenleg az egyetlen zárolt profil a `2017-03-30` verzió, amelyben elképzelhető, hogy nem a legújabb szolgáltatásfunkciók szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="cf616-135">Right now, the only locked profile is version `2017-03-30`, which may not have the latest features of services.</span></span> <span data-ttu-id="cf616-136">A profilok a `profiles` modul alatt találhatók, a verziójuk `YYYY-MM-DD` formátumban van.</span><span class="sxs-lookup"><span data-stu-id="cf616-136">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="cf616-137">A szolgáltatások a profilverzióik alatt vannak csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="cf616-137">Services are grouped under their profile version.</span></span> <span data-ttu-id="cf616-138">Ha például az Azure-erőforrások felügyeleti modult szeretné importálni a `2017-03-09` profilból:</span><span class="sxs-lookup"><span data-stu-id="cf616-138">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="cf616-139">A `preview` és `latest` profilok is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="cf616-139">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="cf616-140">Ezek használata nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="cf616-140">Using them is not recommended.</span></span> <span data-ttu-id="cf616-141">Ezek a profilok működés közbeni verziók, és a szolgáltatás működése bármikor megváltozhat.</span><span class="sxs-lookup"><span data-stu-id="cf616-141">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cf616-142">További lépések</span><span class="sxs-lookup"><span data-stu-id="cf616-142">Next steps</span></span>

<span data-ttu-id="cf616-143">A Góhoz készült Azure SDK használatának megkezdéséhez próbáljon ki egy rövid útmutatót.</span><span class="sxs-lookup"><span data-stu-id="cf616-143">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="cf616-144">Virtuális gép üzembe helyezése egy sablonból</span><span class="sxs-lookup"><span data-stu-id="cf616-144">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="cf616-145">Objektumok továbbítása az Azure Blob Storage-be a Góhoz készült Azure Blob SDK-val</span><span class="sxs-lookup"><span data-stu-id="cf616-145">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="cf616-146">Csatlakozás az Azure Database for PostgreSQL-hez</span><span class="sxs-lookup"><span data-stu-id="cf616-146">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="cf616-147">Ha most azonnal el szeretné kezdeni a Go SDK más szolgáltatásainak használatát, tekintsen meg néhány rendelkezésre álló mintakódot.</span><span class="sxs-lookup"><span data-stu-id="cf616-147">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="cf616-148">Hitelesítés az Azure-szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="cf616-148">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="cf616-149">Új virtuális gépek üzembe helyezése SSH hitelesítéssel</span><span class="sxs-lookup"><span data-stu-id="cf616-149">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="cf616-150">Tárolórendszerkép üzembe helyezése az Azure Container Instances szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="cf616-150">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="cf616-151">Fürt létrehozása Azure Kubernetes-szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="cf616-151">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="cf616-152">Az Azure Storage-szolgáltatások használata</span><span class="sxs-lookup"><span data-stu-id="cf616-152">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="cf616-153">A Góhoz készült Azure SDK összes mintája</span><span class="sxs-lookup"><span data-stu-id="cf616-153">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
