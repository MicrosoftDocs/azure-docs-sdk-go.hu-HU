---
title: "A Go fejlesztők eszközei"
description: "A Góhoz készült Azure SDK és az Azure-szolgáltatások használatára szolgáló eszközök"
keywords: "azure, go, golang, azure, visual studio, visual studio kód"
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: routlaw
ms.openlocfilehash: 4753775e608b39c6da43d64fd08c1532e03d5810
ms.sourcegitcommit: aaa8c37880332625f858a38f5918e6cf581bf48d
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 02/15/2018
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="d8c8c-104">A Góhoz készült Azure SDK-t használó fejlesztőknek szánt eszközök</span><span class="sxs-lookup"><span data-stu-id="d8c8c-104">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="d8c8c-105">Itt talál néhány eszközt a Go-kódok hatékony írásához és azok problémamentes működéséhez az Azure-szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="d8c8c-105">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="d8c8c-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="d8c8c-106">Azure CLI 2.0</span></span>

<span data-ttu-id="d8c8c-107">Az Azure 2.0 CLI parancssori felületet biztosít az előfizetésekben az Azure-erőforrások létrehozásához és konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="d8c8c-107">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="d8c8c-108">A parancssori felülettel gyorsan építhet közös és megosztott Azure-erőforrásokat, hogy a szolgáltatások összetettebb használatára összpontosíthasson.</span><span class="sxs-lookup"><span data-stu-id="d8c8c-108">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="d8c8c-109">A parancssori felület lekérdezési és szűrési szolgáltatásokkal rendelkezik, így közvetlenül a kedvenc parancssori eszközeinek adhatja át az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="d8c8c-109">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="d8c8c-110">A parancssori felület a helyi rendszeren Docker-lemezképként vagy az [Azure Cloud Shellen](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) keresztül telepíthető.</span><span class="sxs-lookup"><span data-stu-id="d8c8c-110">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d8c8c-111">Az Azure CLI 2.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="d8c8c-111">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="d8c8c-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d8c8c-112">Visual Studio Code</span></span>

<span data-ttu-id="d8c8c-113">A Visual Studio Code egy könnyen használható szerkesztő, amely bővítmények révén átfogó támogatását nyújt a Go nyelvhez.</span><span class="sxs-lookup"><span data-stu-id="d8c8c-113">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="d8c8c-114">Ezek a bővítmények olyan szolgáltatások támogatását is tartalmazzák, mint az automatikus kiegészítés, az `impl` sablonok, az újrabontás és a hibakeresés.</span><span class="sxs-lookup"><span data-stu-id="d8c8c-114">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="d8c8c-115">A Visual Studio Code az általános fejlesztői eszközökhöz (például a forráskezeléshez), valamint az Azure-szolgáltatásokkal való közvetlen interakciókhoz is biztosít bővítményeket.</span><span class="sxs-lookup"><span data-stu-id="d8c8c-115">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="d8c8c-116">A Microsoft az ezen Azure-bővítményeket, az Azure CLI interaktív felületét is beleértve, tartalmazó hivatalos metabővítményt is fenntart.</span><span class="sxs-lookup"><span data-stu-id="d8c8c-116">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="d8c8c-117">A Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="d8c8c-117">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="d8c8c-118">A Visual Studio Code Go bővítmény beszerzése</span><span class="sxs-lookup"><span data-stu-id="d8c8c-118">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="d8c8c-119">Az Azure Tools bővítmény beszerzése</span><span class="sxs-lookup"><span data-stu-id="d8c8c-119">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="d8c8c-120">Függőségkezelés a dep használatával</span><span class="sxs-lookup"><span data-stu-id="d8c8c-120">Dependency management with dep</span></span>

<span data-ttu-id="d8c8c-121">A függőségek csomagolása és a Góval végzett bemásolás számos módon elvégezhető, mivel hivatalos megoldás még nincs.</span><span class="sxs-lookup"><span data-stu-id="d8c8c-121">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="d8c8c-122">A kezelés elvégzésének ajánlott módja a `dep` függőségkezelő használata.</span><span class="sxs-lookup"><span data-stu-id="d8c8c-122">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="d8c8c-123">A Góhoz készült Azure SDK a depet használja a bemásoláshoz, és garantáltan megfelelően szerzi be a függőségeket bármely más, a depet használó projekthez is.</span><span class="sxs-lookup"><span data-stu-id="d8c8c-123">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d8c8c-124">A dep függőségkezelő beszerzése</span><span class="sxs-lookup"><span data-stu-id="d8c8c-124">Get the dep dependency manager</span></span>](https://github.com/tools/godep)

## <a name="telemetry-with-application-insights"></a><span data-ttu-id="d8c8c-125">Telemetria az Application Insights segítségével</span><span class="sxs-lookup"><span data-stu-id="d8c8c-125">Telemetry with Application Insights</span></span>

<span data-ttu-id="d8c8c-126">Az [Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) egy elemző termék, amely lehetővé teszi a telemetriai adatok könnyű begyűjtését az alkalmazásokból, és az Azure ökoszisztémába, a Visual Studio Team Servicesbe és a GitHubba integrálható.</span><span class="sxs-lookup"><span data-stu-id="d8c8c-126">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) is an analytics product that allows you to easily collect telemetry information from your applications and integrates with the Azure ecosystem, Visual Studio Team Services, and GitHub.</span></span> <span data-ttu-id="d8c8c-127">Számos alkalmazásban használható, és a Microsoft egy Go SDK-t is biztosít az Application Insights használatához.</span><span class="sxs-lookup"><span data-stu-id="d8c8c-127">It can be used in many applications, and Microsoft offers a Go SDK for working with Application Insights.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d8c8c-128">Az Application Insights beszerzése a Go SDK-hoz</span><span class="sxs-lookup"><span data-stu-id="d8c8c-128">Get the Application Insights for Go SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Go) 
