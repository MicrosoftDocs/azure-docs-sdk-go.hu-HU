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
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>A Góhoz készült Azure SDK-t használó fejlesztőknek szánt eszközök

Itt talál néhány eszközt a Go-kódok hatékony írásához és azok problémamentes működéséhez az Azure-szolgáltatásokkal.

## <a name="azure-cli"></a>Azure CLI

Az Azure CLI parancssori felületet biztosít az előfizetésekben az Azure-erőforrások létrehozásához és konfigurálásához. A parancssori felülettel gyorsan építhet közös és megosztott Azure-erőforrásokat, hogy a szolgáltatások összetettebb használatára összpontosíthasson. A parancssori felület lekérdezési és szűrési szolgáltatásokkal rendelkezik, így közvetlenül a kedvenc parancssori eszközeinek adhatja át az eredményeket. A parancssori felület a helyi rendszeren Docker-lemezképként vagy az [Azure Cloud Shellen](https://docs.microsoft.com/azure/cloud-shell/overview) keresztül telepíthető.

> [!div class="nextstepaction"]
> [Telepítse az Azure CLI-t](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

A Visual Studio Code egy könnyen használható szerkesztő, amely bővítmények révén átfogó támogatását nyújt a Go nyelvhez. Ezek a bővítmények olyan szolgáltatások támogatását is tartalmazzák, mint az automatikus kiegészítés, az `impl` sablonok, az újrabontás és a hibakeresés. A Visual Studio Code az általános fejlesztői eszközökhöz (például a forráskezeléshez), valamint az Azure-szolgáltatásokkal való közvetlen interakciókhoz is biztosít bővítményeket. A Microsoft az ezen Azure-bővítményeket, az Azure CLI interaktív felületét is beleértve, tartalmazó hivatalos metabővítményt is fenntart.

* [A Visual Studio Code telepítése](https://code.visualstudio.com/Download)
* [A Visual Studio Code Go bővítmény beszerzése](https://code.visualstudio.com/docs/languages/go)
* [Az Azure Tools bővítmény beszerzése](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a>CI/CD az Azure DevOps Projecttel

Az Azure DevOps Project-folyamattal beállíthatja a Go-alkalmazások folyamatos felépítését és üzembe helyezését. Mindössze egy Git-adattárra lesz szüksége, hogy közvetlenül az Azure-erőforrásokon tudjon üzembe helyezést és tesztelést végezni. A konfigurációs folyamat egyszerűen létrehozható és kezelhető, és mivel közvetlenül az Azure-on van kiépítve, ugyanúgy lehet kezelni, mint más Azure-erőforrásokat.

> [!div class="nextstepaction"]
> [További tudnivalók a CI/CD-folyamat Azure DevOps Projects-szel történő létrehozásáról](/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a>Függőségkezelés a dep használatával

A függőségek csomagolása és a Góval végzett bemásolás számos módon elvégezhető, mivel hivatalos megoldás még nincs. A kezelés elvégzésének ajánlott módja a `dep` függőségkezelő használata. A Góhoz készült Azure SDK a depet használja a bemásoláshoz, és garantáltan megfelelően szerzi be a függőségeket bármely más, a depet használó projekthez is.

> [!div class="nextstepaction"]
> [A dep függőségkezelő beszerzése](https://github.com/golang/dep)
