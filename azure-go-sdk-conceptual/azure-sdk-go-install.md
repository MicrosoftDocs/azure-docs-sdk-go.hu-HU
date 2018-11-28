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
ms.openlocfilehash: 7990ec8bde5622078aa822fc7e66ba5c4384d682
ms.sourcegitcommit: 3d26b464f196f8675c636ae792637d4c882fb92c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/27/2018
ms.locfileid: "52337143"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="4bf5d-103">A Góhoz készült Azure SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="4bf5d-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="4bf5d-104">Üdvözöli Önt a Góhoz készült Azure SDK!</span><span class="sxs-lookup"><span data-stu-id="4bf5d-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="4bf5d-105">Az SDK lehetővé teszi, hogy Go-alkalmazásokból kezelhesse és használhassa az Azure-szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="4bf5d-106">A Góhoz készült Azure SDK beszerzése</span><span class="sxs-lookup"><span data-stu-id="4bf5d-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="4bf5d-107">Bizonyos Azure-szolgáltatások saját Go SDK-val rendelkeznek, ezért a Góhoz készült Azure SDK főcsomagok nem tartalmazzák őket.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="4bf5d-108">A következő táblázatban láthatók a saját SDK-val rendelkező szolgáltatások és a csomagneveik.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="4bf5d-109">Ezek a csomagok előzetes verziójú csomagoknak minősülnek.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="4bf5d-110">Szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="4bf5d-110">Service</span></span> | <span data-ttu-id="4bf5d-111">Csomag</span><span class="sxs-lookup"><span data-stu-id="4bf5d-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="4bf5d-112">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="4bf5d-112">Blob Storage</span></span> | [<span data-ttu-id="4bf5d-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="4bf5d-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="4bf5d-114">File Storage</span><span class="sxs-lookup"><span data-stu-id="4bf5d-114">File Storage</span></span> | [<span data-ttu-id="4bf5d-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="4bf5d-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="4bf5d-116">Tárolási üzenetsor</span><span class="sxs-lookup"><span data-stu-id="4bf5d-116">Storage Queue</span></span> | [<span data-ttu-id="4bf5d-117">github.com/Azure/azure-storage-queue-go</span><span class="sxs-lookup"><span data-stu-id="4bf5d-117">github.com/Azure/azure-storage-queue-go</span></span>](https://github.com/Azure/azure-storage-queue-go) |
| <span data-ttu-id="4bf5d-118">Eseményközpont</span><span class="sxs-lookup"><span data-stu-id="4bf5d-118">Event Hub</span></span> | [<span data-ttu-id="4bf5d-119">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="4bf5d-119">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="4bf5d-120">Service Bus</span><span class="sxs-lookup"><span data-stu-id="4bf5d-120">Service Bus</span></span> | [<span data-ttu-id="4bf5d-121">github.com/Azure/azure-service-bus-go</span><span class="sxs-lookup"><span data-stu-id="4bf5d-121">github.com/Azure/azure-service-bus-go</span></span>](https://github.com/Azure/azure-service-bus-go) |
| <span data-ttu-id="4bf5d-122">Application Insights</span><span class="sxs-lookup"><span data-stu-id="4bf5d-122">Application Insights</span></span> | [<span data-ttu-id="4bf5d-123">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="4bf5d-123">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="4bf5d-124">A Góhoz készült Azure SDK bemásolása</span><span class="sxs-lookup"><span data-stu-id="4bf5d-124">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="4bf5d-125">A Góhoz készült Azure SDK a [dep](https://github.com/golang/dep) eszközön keresztül másolható be.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-125">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="4bf5d-126">A stabilitás miatt ajánlott a bemásolás elvégzése.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-126">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="4bf5d-127">A `dep` a saját projektjében való használatához adja a `github.com/Azure/azure-sdk-for-go` elemet a `Gopkg.toml` egyik `[[constraint]]` szakaszához.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-127">To use `dep` in your own project, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="4bf5d-128">A `14.0.0` verzió bemásolásához például adja hozzá a következő bejegyzést:</span><span class="sxs-lookup"><span data-stu-id="4bf5d-128">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="4bf5d-129">A Góhoz készült Azure SDK projektbe foglalása</span><span class="sxs-lookup"><span data-stu-id="4bf5d-129">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="4bf5d-130">Ha a Go-kódból szeretné használni az Azure-szolgáltatásokat, importálja az összes használt szolgáltatást és a szükséges `autorest` modulokat.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-130">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="4bf5d-131">A GoDocról beszerezheti az [elérhető szolgáltatások](https://godoc.org/github.com/Azure/azure-sdk-for-go) és az [AutoRest csomagok](https://godoc.org/github.com/Azure/go-autorest) elérhető moduljainak teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-131">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="4bf5d-132">A `go-autorest` leggyakrabban igényelt csomagjai a következők:</span><span class="sxs-lookup"><span data-stu-id="4bf5d-132">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="4bf5d-133">Csomag</span><span class="sxs-lookup"><span data-stu-id="4bf5d-133">Package</span></span> | <span data-ttu-id="4bf5d-134">Leírás</span><span class="sxs-lookup"><span data-stu-id="4bf5d-134">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="4bf5d-135">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="4bf5d-135">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="4bf5d-136">A szolgáltatásügyfél-hitelesítés kezelésének objektumai</span><span class="sxs-lookup"><span data-stu-id="4bf5d-136">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="4bf5d-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="4bf5d-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="4bf5d-138">Az Azure-szolgáltatások használatának állandói</span><span class="sxs-lookup"><span data-stu-id="4bf5d-138">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="4bf5d-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="4bf5d-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="4bf5d-140">Az Azure-szolgáltatások elérésének hitelesítési mechanizmusai</span><span class="sxs-lookup"><span data-stu-id="4bf5d-140">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="4bf5d-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="4bf5d-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="4bf5d-142">Az Azure SDK adatstruktúrákon működő típuskijelentési segítők</span><span class="sxs-lookup"><span data-stu-id="4bf5d-142">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="4bf5d-143">A Go-csomagok és az Azure-szolgáltatások egymástól függetlenül vannak ellátva verzióval.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-143">Go packages and Azure services are versioned independently.</span></span> <span data-ttu-id="4bf5d-144">A szolgáltatás verziói a modul importálási útvonalának részei a `services` modul alatt.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-144">The service versions are part of the module import path, underneath the `services` module.</span></span> <span data-ttu-id="4bf5d-145">A modul teljes elérési útja a szolgáltatás neve, amelyet a verzió követ `YYYY-MM-DD` formátumban, amelyet ismét a szolgáltatás neve követ.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-145">The full path for the module is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="4bf5d-146">A számítási szolgáltatás `2017-03-30` verziójának importálásához például:</span><span class="sxs-lookup"><span data-stu-id="4bf5d-146">For example, to import the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="4bf5d-147">A fejlesztés megkezdésekor ajánlott a szolgáltatás legújabb verzióját használni és annál is maradni.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-147">It's recommended that you use the latest version of a service when starting development and keep it consistent.</span></span>
<span data-ttu-id="4bf5d-148">A szolgáltatás követelményei megváltozhatnak az egyes verziók között, ami miatt a kódja működésképtelenné válhat, még akkor is, ha az alatt az idő alatt a Go SDK nem frissült.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-148">Service requirements may change between versions that could break your code, even if there are no Go SDK updates during that time.</span></span>

<span data-ttu-id="4bf5d-149">Ha a szolgáltatások együttes pillanatképére van szüksége, kiválaszthat egyetlen profilverziót is.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-149">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="4bf5d-150">Jelenleg az egyetlen zárolt profil a `2017-03-09` verzió, amelyben elképzelhető, hogy nem a legújabb szolgáltatásfunkciók szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-150">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="4bf5d-151">A profilok a `profiles` modul alatt találhatók, a verziójuk `YYYY-MM-DD` formátumban van.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-151">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="4bf5d-152">A szolgáltatások a profilverzióik alatt vannak csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-152">Services are grouped under their profile version.</span></span> <span data-ttu-id="4bf5d-153">Ha például az Azure-erőforrások felügyeleti modult szeretné importálni a `2017-03-09` profilból:</span><span class="sxs-lookup"><span data-stu-id="4bf5d-153">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="4bf5d-154">A `preview` és `latest` profilok is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-154">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="4bf5d-155">Ezek használata nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-155">Using them is not recommended.</span></span> <span data-ttu-id="4bf5d-156">Ezek a profilok működés közbeni verziók, és a szolgáltatás működése bármikor megváltozhat.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-156">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4bf5d-157">További lépések</span><span class="sxs-lookup"><span data-stu-id="4bf5d-157">Next steps</span></span>

<span data-ttu-id="4bf5d-158">A Góhoz készült Azure SDK használatának megkezdéséhez próbáljon ki egy rövid útmutatót.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-158">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="4bf5d-159">Virtuális gép üzembe helyezése egy sablonból</span><span class="sxs-lookup"><span data-stu-id="4bf5d-159">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="4bf5d-160">Objektumok továbbítása az Azure Blob Storage-be a Góhoz készült Azure Blob SDK-val</span><span class="sxs-lookup"><span data-stu-id="4bf5d-160">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="4bf5d-161">Csatlakozás az Azure Database for PostgreSQL-hez</span><span class="sxs-lookup"><span data-stu-id="4bf5d-161">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="4bf5d-162">Ha most azonnal el szeretné kezdeni a Go SDK más szolgáltatásainak használatát, tekintsen meg néhány rendelkezésre álló mintakódot.</span><span class="sxs-lookup"><span data-stu-id="4bf5d-162">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="4bf5d-163">Hitelesítés az Azure-szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="4bf5d-163">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/internal/iam)
* [<span data-ttu-id="4bf5d-164">Új virtuális gépek üzembe helyezése SSH hitelesítéssel</span><span class="sxs-lookup"><span data-stu-id="4bf5d-164">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="4bf5d-165">Tárolórendszerkép üzembe helyezése az Azure Container Instances szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="4bf5d-165">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="4bf5d-166">Fürt létrehozása Azure Kubernetes-szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="4bf5d-166">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="4bf5d-167">Az Azure Storage-szolgáltatások használata</span><span class="sxs-lookup"><span data-stu-id="4bf5d-167">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="4bf5d-168">A Góhoz készült Azure SDK összes mintája</span><span class="sxs-lookup"><span data-stu-id="4bf5d-168">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
