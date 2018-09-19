---
title: A Góhoz készült Azure SDK-t használó fejlesztőknek szánt eszközök
description: A Góhoz készült Azure SDK és az Azure-szolgáltatások használatára szolgáló eszközök
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 70cf7d645f47df29e8e42599a0acd75858144783
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059203"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="53280-103">A Góhoz készült Azure SDK-t használó fejlesztőknek szánt eszközök</span><span class="sxs-lookup"><span data-stu-id="53280-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="53280-104">Itt talál néhány eszközt a Go-kódok hatékony írásához és azok problémamentes működéséhez az Azure-szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="53280-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="53280-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="53280-105">Azure CLI</span></span>

<span data-ttu-id="53280-106">Az Azure CLI parancssori felületet biztosít az előfizetésekben az Azure-erőforrások létrehozásához és konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="53280-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="53280-107">A parancssori felülettel gyorsan építhet közös és megosztott Azure-erőforrásokat, hogy a szolgáltatások összetettebb használatára összpontosíthasson.</span><span class="sxs-lookup"><span data-stu-id="53280-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="53280-108">A parancssori felület lekérdezési és szűrési szolgáltatásokkal rendelkezik, így közvetlenül a kedvenc parancssori eszközeinek adhatja át az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="53280-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="53280-109">A parancssori felület a helyi rendszeren Docker-lemezképként vagy az [Azure Cloud Shellen](https://docs.microsoft.com/azure/cloud-shell/overview) keresztül telepíthető.</span><span class="sxs-lookup"><span data-stu-id="53280-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="53280-110">Telepítse az Azure CLI-t</span><span class="sxs-lookup"><span data-stu-id="53280-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="53280-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="53280-111">Visual Studio Code</span></span>

<span data-ttu-id="53280-112">A Visual Studio Code egy könnyen használható szerkesztő, amely támogatást nyújt a Góhoz.</span><span class="sxs-lookup"><span data-stu-id="53280-112">Visual Studio Code is a lightweight editor that offers Go support.</span></span> <span data-ttu-id="53280-113">Ez a bővítmény olyan funkciókat nyújt, mint az automatikus kiegészítés, az `impl` sablonok, az újrabontás és a hibakeresés.</span><span class="sxs-lookup"><span data-stu-id="53280-113">This extension offers features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="53280-114">A Visual Studio Code támogatást nyújt a szerkesztőn belüli verziókövetéshez, valamint az Azure-szolgáltatásokkal való munkához is biztosít bővítményeket.</span><span class="sxs-lookup"><span data-stu-id="53280-114">Visual Studio Code also offers support for in-editor access to source control, and extensions for working with Azure services.</span></span>

* [<span data-ttu-id="53280-115">A Visual Studio Code telepítése</span><span class="sxs-lookup"><span data-stu-id="53280-115">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="53280-116">A Visual Studio Code Go bővítmény beszerzése</span><span class="sxs-lookup"><span data-stu-id="53280-116">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="53280-117">A Visual Studio Code Azure-eszközök bővítményének beszerzése</span><span class="sxs-lookup"><span data-stu-id="53280-117">Get the Visual Studio Code Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="53280-118">CI/CD az Azure DevOps Projecttel</span><span class="sxs-lookup"><span data-stu-id="53280-118">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="53280-119">Az Azure DevOps Project-folyamatokkal beállíthatja a Go-alkalmazások folyamatos integrációs rendszerét.</span><span class="sxs-lookup"><span data-stu-id="53280-119">Azure DevOps Project pipelines allow you to set up a continuous integration system for your Go applications.</span></span> <span data-ttu-id="53280-120">Mindössze egy Git-adattárra lesz szüksége, hogy közvetlenül az Azure-on tudjon üzembe helyezést és tesztelést végezni.</span><span class="sxs-lookup"><span data-stu-id="53280-120">All it takes is a git repo, and you can deploy and test directly on Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="53280-121">További tudnivalók a CI/CD-folyamat Azure DevOps Projects-szel történő létrehozásáról</span><span class="sxs-lookup"><span data-stu-id="53280-121">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="53280-122">Függőségkezelés a dep használatával</span><span class="sxs-lookup"><span data-stu-id="53280-122">Dependency management with dep</span></span>

<span data-ttu-id="53280-123">A Góhoz készült Azure SDK a depet használja a függőségkezeléshez.</span><span class="sxs-lookup"><span data-stu-id="53280-123">The Azure SDK for Go uses dep for dependency management.</span></span> <span data-ttu-id="53280-124">A dep parancs lehetővé teszi a Go-alkalmazás követelményeinek lekérését és bemásolását, így megelőzi a verzióütközéseket és biztosítja a projekt megfelelő működését.</span><span class="sxs-lookup"><span data-stu-id="53280-124">The dep command allows you to pull and vendor requirements for your Go application, avoiding version conflicts and ensuring that your project works correctly.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="53280-125">A dep függőségkezelő beszerzése</span><span class="sxs-lookup"><span data-stu-id="53280-125">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
