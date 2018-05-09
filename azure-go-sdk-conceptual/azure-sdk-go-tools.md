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
ms.openlocfilehash: 2ea44fb8a4fdd6098bb44d3b5092cfbc352b424d
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/03/2018
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>A Góhoz készült Azure SDK-t használó fejlesztőknek szánt eszközök

Itt talál néhány eszközt a Go-kódok hatékony írásához és azok problémamentes működéséhez az Azure-szolgáltatásokkal.

## <a name="azure-cli-20"></a>Azure CLI 2.0

Az Azure 2.0 CLI parancssori felületet biztosít az előfizetésekben az Azure-erőforrások létrehozásához és konfigurálásához. A parancssori felülettel gyorsan építhet közös és megosztott Azure-erőforrásokat, hogy a szolgáltatások összetettebb használatára összpontosíthasson. A parancssori felület lekérdezési és szűrési szolgáltatásokkal rendelkezik, így közvetlenül a kedvenc parancssori eszközeinek adhatja át az eredményeket. A parancssori felület a helyi rendszeren Docker-lemezképként vagy az [Azure Cloud Shellen](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) keresztül telepíthető.

> [!div class="nextstepaction"]
> [Az Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

A Visual Studio Code egy könnyen használható szerkesztő, amely bővítmények révén átfogó támogatását nyújt a Go nyelvhez. Ezek a bővítmények olyan szolgáltatások támogatását is tartalmazzák, mint az automatikus kiegészítés, az `impl` sablonok, az újrabontás és a hibakeresés. A Visual Studio Code az általános fejlesztői eszközökhöz (például a forráskezeléshez), valamint az Azure-szolgáltatásokkal való közvetlen interakciókhoz is biztosít bővítményeket. A Microsoft az ezen Azure-bővítményeket, az Azure CLI interaktív felületét is beleértve, tartalmazó hivatalos metabővítményt is fenntart.

* [A Visual Studio Code telepítése](https://code.visualstudio.com/Download)
* [A Visual Studio Code Go bővítmény beszerzése](https://code.visualstudio.com/docs/languages/go)
* [Az Azure Tools bővítmény beszerzése](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a>Függőségkezelés a dep használatával

A függőségek csomagolása és a Góval végzett bemásolás számos módon elvégezhető, mivel hivatalos megoldás még nincs. A kezelés elvégzésének ajánlott módja a `dep` függőségkezelő használata. A Góhoz készült Azure SDK a depet használja a bemásoláshoz, és garantáltan megfelelően szerzi be a függőségeket bármely más, a depet használó projekthez is.

> [!div class="nextstepaction"]
> [A dep függőségkezelő beszerzése](https://github.com/tools/godep)

## <a name="telemetry-with-application-insights"></a>Telemetria az Application Insights segítségével

Az [Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) egy elemző termék, amely lehetővé teszi a telemetriai adatok könnyű begyűjtését az alkalmazásokból, és az Azure ökoszisztémába, a Visual Studio Team Servicesbe és a GitHubba integrálható. Számos alkalmazásban használható, és a Microsoft egy Go SDK-t is biztosít az Application Insights használatához.

> [!div class="nextstepaction"]
> [Az Application Insights beszerzése a Go SDK-hoz](https://github.com/Microsoft/ApplicationInsights-Go) 
