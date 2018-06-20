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
ms.openlocfilehash: 3388359bba791c87025b6ffd0e6b476f95589f73
ms.sourcegitcommit: 81e97407e6139375bf7357045e818c87a17dcde1
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/19/2018
ms.locfileid: "36262980"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="deeac-103">A Góhoz készült Azure SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="deeac-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="deeac-104">Üdvözöli Önt a Góhoz készült Azure SDK!</span><span class="sxs-lookup"><span data-stu-id="deeac-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="deeac-105">Az SDK lehetővé teszi, hogy Go-alkalmazásokból kezelhesse és használhassa az Azure-szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="deeac-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="deeac-106">A Góhoz készült Azure SDK beszerzése</span><span class="sxs-lookup"><span data-stu-id="deeac-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="deeac-107">Bizonyos Azure-szolgáltatások saját Go SDK-val rendelkeznek, ezért a Góhoz készült Azure SDK főcsomagok nem tartalmazzák őket.</span><span class="sxs-lookup"><span data-stu-id="deeac-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="deeac-108">A következő táblázatban láthatók a saját SDK-val rendelkező szolgáltatások és a csomagneveik.</span><span class="sxs-lookup"><span data-stu-id="deeac-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="deeac-109">Ezek a csomagok előzetes verziójú csomagoknak minősülnek.</span><span class="sxs-lookup"><span data-stu-id="deeac-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="deeac-110">Szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="deeac-110">Service</span></span> | <span data-ttu-id="deeac-111">Csomag</span><span class="sxs-lookup"><span data-stu-id="deeac-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="deeac-112">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="deeac-112">Blob Storage</span></span> | [<span data-ttu-id="deeac-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="deeac-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="deeac-114">File Storage</span><span class="sxs-lookup"><span data-stu-id="deeac-114">File Storage</span></span> | [<span data-ttu-id="deeac-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="deeac-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="deeac-116">Tárolási üzenetsor</span><span class="sxs-lookup"><span data-stu-id="deeac-116">Storage Queue</span></span> | [<span data-ttu-id="deeac-117">github.com/Azure/azure-storage-queue-go</span><span class="sxs-lookup"><span data-stu-id="deeac-117">github.com/Azure/azure-storage-queue-go</span></span>](https://github.com/Azure/azure-storage-queue-go) |
| <span data-ttu-id="deeac-118">Eseményközpont</span><span class="sxs-lookup"><span data-stu-id="deeac-118">Event Hub</span></span> | [<span data-ttu-id="deeac-119">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="deeac-119">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="deeac-120">Service Bus</span><span class="sxs-lookup"><span data-stu-id="deeac-120">Service Bus</span></span> | [<span data-ttu-id="deeac-121">github.com/Azure/azure-service-bus-go</span><span class="sxs-lookup"><span data-stu-id="deeac-121">github.com/Azure/azure-service-bus-go</span></span>](https://github.com/Azure/azure-service-bus-go) |
| <span data-ttu-id="deeac-122">Application Insights</span><span class="sxs-lookup"><span data-stu-id="deeac-122">Application Insights</span></span> | [<span data-ttu-id="deeac-123">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="deeac-123">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="deeac-124">A Góhoz készült Azure SDK bemásolása</span><span class="sxs-lookup"><span data-stu-id="deeac-124">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="deeac-125">A Góhoz készült Azure SDK a [dep](https://github.com/golang/dep) eszközön keresztül másolható be.</span><span class="sxs-lookup"><span data-stu-id="deeac-125">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="deeac-126">A stabilitás miatt ajánlott a bemásolás elvégzése.</span><span class="sxs-lookup"><span data-stu-id="deeac-126">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="deeac-127">A `dep` támogatás használatához adja a `github.com/Azure/azure-sdk-for-go` elemet a `Gopkg.toml` egyik `[[constraint]]` szakaszához.</span><span class="sxs-lookup"><span data-stu-id="deeac-127">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="deeac-128">A `14.0.0` verzió bemásolásához például adja hozzá a következő bejegyzést:</span><span class="sxs-lookup"><span data-stu-id="deeac-128">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="deeac-129">A Góhoz készült Azure SDK projektbe foglalása</span><span class="sxs-lookup"><span data-stu-id="deeac-129">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="deeac-130">Ha a Go-kódból szeretné használni az Azure-szolgáltatásokat, importálja az összes használt szolgáltatást és a szükséges `autorest` modulokat.</span><span class="sxs-lookup"><span data-stu-id="deeac-130">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="deeac-131">A GoDocról beszerezheti az [elérhető szolgáltatások](https://godoc.org/github.com/Azure/azure-sdk-for-go) és az [AutoRest csomagok](https://godoc.org/github.com/Azure/go-autorest) elérhető moduljainak teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="deeac-131">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="deeac-132">A `go-autorest` leggyakrabban igényelt csomagjai a következők:</span><span class="sxs-lookup"><span data-stu-id="deeac-132">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="deeac-133">Csomag</span><span class="sxs-lookup"><span data-stu-id="deeac-133">Package</span></span> | <span data-ttu-id="deeac-134">Leírás</span><span class="sxs-lookup"><span data-stu-id="deeac-134">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="deeac-135">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="deeac-135">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="deeac-136">A szolgáltatásügyfél-hitelesítés kezelésének objektumai</span><span class="sxs-lookup"><span data-stu-id="deeac-136">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="deeac-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="deeac-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="deeac-138">Az Azure-szolgáltatások használatának állandói</span><span class="sxs-lookup"><span data-stu-id="deeac-138">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="deeac-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="deeac-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="deeac-140">Az Azure-szolgáltatások elérésének hitelesítési mechanizmusai</span><span class="sxs-lookup"><span data-stu-id="deeac-140">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="deeac-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="deeac-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="deeac-142">Az Azure SDK adatstruktúrákon működő típuskijelentési segítők</span><span class="sxs-lookup"><span data-stu-id="deeac-142">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="deeac-143">Az Azure-szolgáltatások moduljai azok SDK API-jaitól függetlenül vannak ellátva verzióval.</span><span class="sxs-lookup"><span data-stu-id="deeac-143">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="deeac-144">Ezek a verziók a modul importálási útvonalának részei, és egy _szolgáltatásverzióból_ vagy _profilból_ származnak.</span><span class="sxs-lookup"><span data-stu-id="deeac-144">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="deeac-145">Jelenleg ajánlott a fejlesztéshez és a kiadáshoz is egy adott szolgáltatásverziót használnia.</span><span class="sxs-lookup"><span data-stu-id="deeac-145">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="deeac-146">A szolgáltatások a `services` modul alatt találhatók.</span><span class="sxs-lookup"><span data-stu-id="deeac-146">Services are located under the `services` module.</span></span> <span data-ttu-id="deeac-147">Az importálás teljes elérési útja a szolgáltatás neve, amelyet a verzió követ `YYYY-MM-DD` formátumban, amelyet ismét a szolgáltatás neve követ.</span><span class="sxs-lookup"><span data-stu-id="deeac-147">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="deeac-148">A számítási szolgáltatás `2017-03-30` verziójának belefoglalásához például:</span><span class="sxs-lookup"><span data-stu-id="deeac-148">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="deeac-149">Jelenleg ajánlott, hogy a szolgáltatások legújabb verzióját használja, ha nincs rá oka, hogy ne így tegyen.</span><span class="sxs-lookup"><span data-stu-id="deeac-149">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="deeac-150">Ha a szolgáltatások együttes pillanatképére van szüksége, kiválaszthat egyetlen profilverziót is.</span><span class="sxs-lookup"><span data-stu-id="deeac-150">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="deeac-151">Jelenleg az egyetlen zárolt profil a `2017-03-09` verzió, amelyben elképzelhető, hogy nem a legújabb szolgáltatásfunkciók szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="deeac-151">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="deeac-152">A profilok a `profiles` modul alatt találhatók, a verziójuk `YYYY-MM-DD` formátumban van.</span><span class="sxs-lookup"><span data-stu-id="deeac-152">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="deeac-153">A szolgáltatások a profilverzióik alatt vannak csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="deeac-153">Services are grouped under their profile version.</span></span> <span data-ttu-id="deeac-154">Ha például az Azure-erőforrások felügyeleti modult szeretné importálni a `2017-03-09` profilból:</span><span class="sxs-lookup"><span data-stu-id="deeac-154">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="deeac-155">A `preview` és `latest` profilok is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="deeac-155">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="deeac-156">Ezek használata nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="deeac-156">Using them is not recommended.</span></span> <span data-ttu-id="deeac-157">Ezek a profilok működés közbeni verziók, és a szolgáltatás működése bármikor megváltozhat.</span><span class="sxs-lookup"><span data-stu-id="deeac-157">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="deeac-158">További lépések</span><span class="sxs-lookup"><span data-stu-id="deeac-158">Next steps</span></span>

<span data-ttu-id="deeac-159">A Góhoz készült Azure SDK használatának megkezdéséhez próbáljon ki egy rövid útmutatót.</span><span class="sxs-lookup"><span data-stu-id="deeac-159">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="deeac-160">Virtuális gép üzembe helyezése egy sablonból</span><span class="sxs-lookup"><span data-stu-id="deeac-160">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="deeac-161">Objektumok továbbítása az Azure Blob Storage-be a Góhoz készült Azure Blob SDK-val</span><span class="sxs-lookup"><span data-stu-id="deeac-161">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="deeac-162">Csatlakozás az Azure Database for PostgreSQL-hez</span><span class="sxs-lookup"><span data-stu-id="deeac-162">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="deeac-163">Ha most azonnal el szeretné kezdeni a Go SDK más szolgáltatásainak használatát, tekintsen meg néhány rendelkezésre álló mintakódot.</span><span class="sxs-lookup"><span data-stu-id="deeac-163">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="deeac-164">Hitelesítés az Azure-szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="deeac-164">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="deeac-165">Új virtuális gépek üzembe helyezése SSH hitelesítéssel</span><span class="sxs-lookup"><span data-stu-id="deeac-165">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="deeac-166">Tárolórendszerkép üzembe helyezése az Azure Container Instances szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="deeac-166">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="deeac-167">Fürt létrehozása Azure Kubernetes-szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="deeac-167">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="deeac-168">Az Azure Storage-szolgáltatások használata</span><span class="sxs-lookup"><span data-stu-id="deeac-168">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="deeac-169">A Góhoz készült Azure SDK összes mintája</span><span class="sxs-lookup"><span data-stu-id="deeac-169">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
