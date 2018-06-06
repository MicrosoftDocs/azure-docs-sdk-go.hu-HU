---
title: A Go fejlesztők eszközei
description: A Góhoz készült Azure SDK és az Azure-szolgáltatások használatára szolgáló eszközök
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 1e122ab161766023ea146329a5edb13143699b8b
ms.sourcegitcommit: b81b17cbb934399c195bfdcb87137aee935f5234
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34755532"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="47151-103">A Góhoz készült Azure SDK-t használó fejlesztőknek szánt eszközök</span><span class="sxs-lookup"><span data-stu-id="47151-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="47151-104">Itt talál néhány eszközt a Go-kódok hatékony írásához és azok problémamentes működéséhez az Azure-szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="47151-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="47151-105">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="47151-105">Azure CLI 2.0</span></span>

<span data-ttu-id="47151-106">Az Azure 2.0 CLI parancssori felületet biztosít az előfizetésekben az Azure-erőforrások létrehozásához és konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="47151-106">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="47151-107">A parancssori felülettel gyorsan építhet közös és megosztott Azure-erőforrásokat, hogy a szolgáltatások összetettebb használatára összpontosíthasson.</span><span class="sxs-lookup"><span data-stu-id="47151-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="47151-108">A parancssori felület lekérdezési és szűrési szolgáltatásokkal rendelkezik, így közvetlenül a kedvenc parancssori eszközeinek adhatja át az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="47151-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="47151-109">A parancssori felület a helyi rendszeren Docker-lemezképként vagy az [Azure Cloud Shellen](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) keresztül telepíthető.</span><span class="sxs-lookup"><span data-stu-id="47151-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="47151-110">Az Azure CLI 2.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="47151-110">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="47151-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47151-111">Visual Studio Code</span></span>

<span data-ttu-id="47151-112">A Visual Studio Code egy könnyen használható szerkesztő, amely bővítmények révén átfogó támogatását nyújt a Go nyelvhez.</span><span class="sxs-lookup"><span data-stu-id="47151-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="47151-113">Ezek a bővítmények olyan szolgáltatások támogatását is tartalmazzák, mint az automatikus kiegészítés, az `impl` sablonok, az újrabontás és a hibakeresés.</span><span class="sxs-lookup"><span data-stu-id="47151-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="47151-114">A Visual Studio Code az általános fejlesztői eszközökhöz (például a forráskezeléshez), valamint az Azure-szolgáltatásokkal való közvetlen interakciókhoz is biztosít bővítményeket.</span><span class="sxs-lookup"><span data-stu-id="47151-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="47151-115">A Microsoft az ezen Azure-bővítményeket, az Azure CLI interaktív felületét is beleértve, tartalmazó hivatalos metabővítményt is fenntart.</span><span class="sxs-lookup"><span data-stu-id="47151-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="47151-116">A Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="47151-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="47151-117">A Visual Studio Code Go bővítmény beszerzése</span><span class="sxs-lookup"><span data-stu-id="47151-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="47151-118">Az Azure Tools bővítmény beszerzése</span><span class="sxs-lookup"><span data-stu-id="47151-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="47151-119">Függőségkezelés a dep használatával</span><span class="sxs-lookup"><span data-stu-id="47151-119">Dependency management with dep</span></span>

<span data-ttu-id="47151-120">A függőségek csomagolása és a Góval végzett bemásolás számos módon elvégezhető, mivel hivatalos megoldás még nincs.</span><span class="sxs-lookup"><span data-stu-id="47151-120">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="47151-121">A kezelés elvégzésének ajánlott módja a `dep` függőségkezelő használata.</span><span class="sxs-lookup"><span data-stu-id="47151-121">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="47151-122">A Góhoz készült Azure SDK a depet használja a bemásoláshoz, és garantáltan megfelelően szerzi be a függőségeket bármely más, a depet használó projekthez is.</span><span class="sxs-lookup"><span data-stu-id="47151-122">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="47151-123">A dep függőségkezelő beszerzése</span><span class="sxs-lookup"><span data-stu-id="47151-123">Get the dep dependency manager</span></span>](https://github.com/tools/godep)