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
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>A Góhoz készült Azure SDK-t használó fejlesztőknek szánt eszközök

Itt talál néhány eszközt a Go-kódok hatékony írásához és azok problémamentes működéséhez az Azure-szolgáltatásokkal.

## <a name="azure-cli"></a>Azure CLI

Az Azure CLI parancssori felületet biztosít az előfizetésekben az Azure-erőforrások létrehozásához és konfigurálásához. A parancssori felülettel gyorsan építhet közös és megosztott Azure-erőforrásokat, hogy a szolgáltatások összetettebb használatára összpontosíthasson. A parancssori felület lekérdezési és szűrési szolgáltatásokkal rendelkezik, így közvetlenül a kedvenc parancssori eszközeinek adhatja át az eredményeket. A parancssori felület a helyi rendszeren Docker-lemezképként vagy az [Azure Cloud Shellen](https://docs.microsoft.com/azure/cloud-shell/overview) keresztül telepíthető.

> [!div class="nextstepaction"]
> [Telepítse az Azure CLI-t](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

A Visual Studio Code egy könnyen használható szerkesztő, amely támogatást nyújt a Góhoz. Ez a bővítmény olyan funkciókat nyújt, mint az automatikus kiegészítés, az `impl` sablonok, az újrabontás és a hibakeresés. A Visual Studio Code támogatást nyújt a szerkesztőn belüli verziókövetéshez, valamint az Azure-szolgáltatásokkal való munkához is biztosít bővítményeket.

* [A Visual Studio Code telepítése](https://code.visualstudio.com/Download)
* [A Visual Studio Code Go bővítmény beszerzése](https://code.visualstudio.com/docs/languages/go)
* [A Visual Studio Code Azure-eszközök bővítményének beszerzése](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a>CI/CD az Azure DevOps Projecttel

Az Azure DevOps Project-folyamatokkal beállíthatja a Go-alkalmazások folyamatos integrációs rendszerét. Mindössze egy Git-adattárra lesz szüksége, hogy közvetlenül az Azure-on tudjon üzembe helyezést és tesztelést végezni.

> [!div class="nextstepaction"]
> [További tudnivalók a CI/CD-folyamat Azure DevOps Projects-szel történő létrehozásáról](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a>Függőségkezelés a dep használatával

A Góhoz készült Azure SDK a depet használja a függőségkezeléshez. A dep parancs lehetővé teszi a Go-alkalmazás követelményeinek lekérését és bemásolását, így megelőzi a verzióütközéseket és biztosítja a projekt megfelelő működését.

> [!div class="nextstepaction"]
> [A dep függőségkezelő beszerzése](https://github.com/golang/dep)
