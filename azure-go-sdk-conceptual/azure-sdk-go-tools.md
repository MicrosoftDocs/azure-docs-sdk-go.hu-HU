---
title: A Go fejlesztők eszközei
description: A Góhoz készült Azure SDK és az Azure-szolgáltatások használatára szolgáló eszközök
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 07/13/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: dfa3912ac13e6f6d52d607f9dcc150f3a5b57602
ms.sourcegitcommit: d1790b317a8fcb4d672c654dac2a925a976589d4
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/14/2018
ms.locfileid: "39039505"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="23061-103">A Góhoz készült Azure SDK-t használó fejlesztőknek szánt eszközök</span><span class="sxs-lookup"><span data-stu-id="23061-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="23061-104">Itt talál néhány eszközt a Go-kódok hatékony írásához és azok problémamentes működéséhez az Azure-szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="23061-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="23061-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="23061-105">Azure CLI</span></span>

<span data-ttu-id="23061-106">Az Azure CLI parancssori felületet biztosít az előfizetésekben az Azure-erőforrások létrehozásához és konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="23061-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="23061-107">A parancssori felülettel gyorsan építhet közös és megosztott Azure-erőforrásokat, hogy a szolgáltatások összetettebb használatára összpontosíthasson.</span><span class="sxs-lookup"><span data-stu-id="23061-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="23061-108">A parancssori felület lekérdezési és szűrési szolgáltatásokkal rendelkezik, így közvetlenül a kedvenc parancssori eszközeinek adhatja át az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="23061-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="23061-109">A parancssori felület a helyi rendszeren Docker-lemezképként vagy az [Azure Cloud Shellen](https://docs.microsoft.com/azure/cloud-shell/overview) keresztül telepíthető.</span><span class="sxs-lookup"><span data-stu-id="23061-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="23061-110">Telepítse az Azure CLI-t</span><span class="sxs-lookup"><span data-stu-id="23061-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="23061-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="23061-111">Visual Studio Code</span></span>

<span data-ttu-id="23061-112">A Visual Studio Code egy könnyen használható szerkesztő, amely bővítmények révén átfogó támogatását nyújt a Go nyelvhez.</span><span class="sxs-lookup"><span data-stu-id="23061-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="23061-113">Ezek a bővítmények olyan szolgáltatások támogatását is tartalmazzák, mint az automatikus kiegészítés, az `impl` sablonok, az újrabontás és a hibakeresés.</span><span class="sxs-lookup"><span data-stu-id="23061-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="23061-114">A Visual Studio Code az általános fejlesztői eszközökhöz (például a forráskezeléshez), valamint az Azure-szolgáltatásokkal való közvetlen interakciókhoz is biztosít bővítményeket.</span><span class="sxs-lookup"><span data-stu-id="23061-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="23061-115">A Microsoft az ezen Azure-bővítményeket, az Azure CLI interaktív felületét is beleértve, tartalmazó hivatalos metabővítményt is fenntart.</span><span class="sxs-lookup"><span data-stu-id="23061-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="23061-116">A Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="23061-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="23061-117">A Visual Studio Code Go bővítmény beszerzése</span><span class="sxs-lookup"><span data-stu-id="23061-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="23061-118">Az Azure Tools bővítmény beszerzése</span><span class="sxs-lookup"><span data-stu-id="23061-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="23061-119">CI/CD az Azure DevOps Projecttel</span><span class="sxs-lookup"><span data-stu-id="23061-119">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="23061-120">Az Azure DevOps Project-folyamattal beállíthatja a Go-alkalmazások folyamatos felépítését és üzembe helyezését.</span><span class="sxs-lookup"><span data-stu-id="23061-120">With the Azure DevOps Project pipeline, you can set up a continuous build and deploy for your Go applications.</span></span> <span data-ttu-id="23061-121">Mindössze egy Git-adattárra lesz szüksége, hogy közvetlenül az Azure-erőforrásokon tudjon üzembe helyezést és tesztelést végezni.</span><span class="sxs-lookup"><span data-stu-id="23061-121">All you need is an available git repo, and you can set up to deploy and test directly on your Azure resources.</span></span> <span data-ttu-id="23061-122">A konfigurációs folyamat egyszerűen létrehozható és kezelhető, és mivel közvetlenül az Azure-on van kiépítve, ugyanúgy lehet kezelni, mint más Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="23061-122">The configuration pipeline is easy to create and manage, and since it's provisioned directly on Azure, you can control it in the same way that you handle your other Azure resources.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="23061-123">További tudnivalók a CI/CD-folyamat Azure DevOps Projects-szel történő létrehozásáról</span><span class="sxs-lookup"><span data-stu-id="23061-123">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="23061-124">Függőségkezelés a dep használatával</span><span class="sxs-lookup"><span data-stu-id="23061-124">Dependency management with dep</span></span>

<span data-ttu-id="23061-125">A függőségek csomagolása és a Góval végzett bemásolás számos módon elvégezhető, mivel hivatalos megoldás még nincs.</span><span class="sxs-lookup"><span data-stu-id="23061-125">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="23061-126">A kezelés elvégzésének ajánlott módja a `dep` függőségkezelő használata.</span><span class="sxs-lookup"><span data-stu-id="23061-126">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="23061-127">A Góhoz készült Azure SDK a depet használja a bemásoláshoz, és garantáltan megfelelően szerzi be a függőségeket bármely más, a depet használó projekthez is.</span><span class="sxs-lookup"><span data-stu-id="23061-127">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="23061-128">A dep függőségkezelő beszerzése</span><span class="sxs-lookup"><span data-stu-id="23061-128">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
