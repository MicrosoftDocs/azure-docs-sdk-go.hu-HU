---
title: Hitelesítés a Góhoz készült Azure SDK-val
description: Ismerje meg a Góhoz készült Azure SDK-ban elérhető hitelesítési módszereket, és azok használatát.
services: azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.technology: azure-sdk-go
ms.devlang: go
ms.component: authentication
ms.openlocfilehash: f5c2c56e43828f0bedad0b5781dc71991ce1fd3e
ms.sourcegitcommit: 172f81dd6e4c6a275dc8031815aa87cdb488cbf0
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/26/2018
ms.locfileid: "47231675"
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a>Hitelesítési módszerek a Góhoz készült Azure SDK-ban

A Góhoz készült Azure SDK több módszert is kínál az Azure-ral való hitelesítésre. Ezeket a hitelesítési _típusokat_ különböző hitelesítési _módszerekkel_ lehet meghívni. Ez a cikk ismerteti az elérhető típusokat és módszereket, valamint azt is, hogy hogyan választhatja ki, melyek a legjobbak az alkalmazás számára.

## <a name="available-authentication-types-and-methods"></a>Az elérhető hitelesítési típusok és módszerek

A Góhoz készült Azure SDK számos hitelesítési típust kínál, amelyek eltérő hitelesítőadat-készleteket használnak. Mindegyik hitelesítési típust különböző hitelesítési módszerekkel lehet elérni, amelyek azt határozzák meg, hogy hogyan fogadja az SDK a hitelesítő adatokat bemeneti adatként. A következő táblázat ismerteti az elérhető hitelesítési típusokat és azokat a helyzeteket, amikor az alkalmazásnak érdemes használnia őket.

| Hitelesítés típusa | Ajánlott, amikor... |
|---------------------|---------------------|
| Tanúsítványalapú hitelesítés | Egy X509 tanúsítvánnyal rendelkezik, amelyet egy Azure Active Directory-felhasználóhoz (AAD) vagy szolgáltatásnévhez konfiguráltak. További tudnivalókért lásd: [A tanúsítványalapú hitelesítés első lépései az Azure Active Directoryban]. |
| Ügyfél-hitelesítő adatok | Egy konfigurált szolgáltatásnévvel rendelkezik, amelyet ehhez az alkalmazáshoz vagy az alkalmazásosztályához állítottak be. További tudnivalókért lásd: [Szolgáltatásnév létrehozása az Azure CLI-vel]. |
| Azure-erőforrások felügyelt identitásai | Az alkalmazás egy olyan Azure-erőforráson fut, amelyet egy felügyelt identitással konfiguráltak. További tudnivalókért lásd [Azure-erőforrások felügyelt identitásai]. |
| Eszközjogkivonat | Az alkalmazást __kizárólag__ interaktív használatra szánták. A felhasználóknál engedélyezve lehet a többtényezős hitelesítés. A felhasználók hozzáféréssel rendelkeznek egy webböngészőhöz, amelyen keresztül be tudnak jelentkezni. További információkért lásd: [Az eszközjogkivonattal történő hitelesítés használata](#use-device-token-authentication).|
| Felhasználónév/jelszó | Van egy interaktív alkalmazása, amely nem használhat más hitelesítési módszert. A felhasználók AAD-bejelentkezéséhez nincs engedélyezve a többtényezős hitelesítés. |

> [!IMPORTANT]
> Ha nem az ügyfél-hitelesítő adatok hitelesítési típust használja, az alkalmazást regisztrálni kell az Azure Active Directoryban. Az eljárás leírásáért lásd: [Alkalmazások integrálása az Azure Active Directoryval](/azure/active-directory/develop/active-directory-integrating-applications).
>
> [!NOTE]
> Hacsak nem állnak fenn különleges követelmények, kerülje a felhasználónév- és jelszóalapú hitelesítést. Azokban az esetekben, amikor a felhasználóalapú bejelentkezés megfelelő megoldásnak számít, általában használni lehet helyette az eszközjogkivonattal történő hitelesítést.

[A tanúsítványalapú hitelesítés első lépései az Azure Active Directoryban]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Szolgáltatásnév létrehozása az Azure CLI-vel]: /cli/azure/create-an-azure-service-principal-azure-cli
[Azure-erőforrások felügyelt identitásai]: /azure/active-directory/managed-identities-azure-resources/overview

Ezeket a hitelesítési típusokat különböző módszerekkel lehet elérni.

* A [_környezetalapú hitelesítés_](#use-environment-based-authentication) közvetlenül a program környezetéből olvassa be a hitelesítő adatokat.
* A [_fájlalapú hitelesítés_](#use-file-based-authentication) betölt egy fájlt, amely a szolgáltatásnév hitelesítő adatait tartalmazza.
* Az [_ügyfélalapú hitelesítés_](#use-an-authentication-client) a kód egyik objektumát használja, és a felhasználóra bízza a hitelesítőadatok megadását a program végrehajtásakor.
* Az [_eszközjogkivonattal történő hitelesítés_](#use-device-token-authentication) esetén a felhasználóknak interaktív módon kell bejelentkezniük egy webböngészőn keresztül, egy jogkivonat segítségével.

A `github.com/Azure/go-autorest/autorest/azure/auth` csomagban az összes hitelesítési funkció és típus megtalálható.

> [!NOTE]
> Ha nincsenek különleges követelmények, kerülje az ügyfélalapú hitelesítést. Ez a hitelesítési módszer támogatja a helytelen gyakorlatok alkalmazását. A hitelesítő adatok szoftveres rögzítése például különösen az ügyfélalapú hitelesítés használata során vonzó lehetőség. Az egyedi kód írása a hitelesítéshez szintén értelmét vesztheti a jövőbeli SDK kiadásoknál, ha megváltoznak a hitelesítési követelmények.

## <a name="use-environment-based-authentication"></a>A környezetalapú hitelesítés használata

Ha egy ellenőrzött környezetben futtatja az alkalmazást, a környezetalapú hitelesítés kézenfekvő lehet. Ezzel a hitelesítési módszerrel az alkalmazás futtatása előtt konfigurálja a rendszerhéj-környezetet. A Go SDK pedig a futtatáskor beolvassa ezeket a környezeti változókat az Azure-hitelesítéshez.

A környezetalapú hitelesítés az eszközjogkivonatok kivételével az összes hitelesítési módszert támogatja, és a következő sorrendben értékeli ki őket:

* Ügyfél-hitelesítő adatok
* X509 tanúsítványok
* Felhasználónév/jelszó
* Azure-erőforrások felügyelt identitásai

Ha a hitelesítés típusánál nincs beállított érték, vagy a rendszer elutasította, akkor az SDK automatikusan a következő hitelesítési típussal próbálkozik. Ha nincs több típus, amivel próbálkozhat, akkor az SDK hibaüzenetet jelenít meg.

Az alábbi táblázat azokat a környezeti változókat ismerteti, amelyeket be kell állítani a környezetalapú hitelesítés által támogatott hitelesítési típusokhoz.

| Hitelesítés típusa | Környezeti változó | Leírás |
| ------------------- | -------------------- | ----------- |
| __Ügyfél-hitelesítő adatok__ | `AZURE_TENANT_ID` | Annak az Active Directory-bérlőnek az azonosítója, amelyhez a szolgáltatásnév tartozik. |
| | `AZURE_CLIENT_ID` | A szolgáltatásnév neve vagy azonosítója. |
| | `AZURE_CLIENT_SECRET` | A szolgáltatásnévhez társított titkos kulcs. |
| __Tanúsítvány__ | `AZURE_TENANT_ID` | Annak az Active Directory-bérlőnek az azonosítója, amellyel a tanúsítványt regisztrálták. |
| | `AZURE_CLIENT_ID` | A tanúsítványhoz társított alkalmazásügyfél-azonosító. |
| | `AZURE_CERTIFICATE_PATH` | Az ügyféltanúsítvány-fájl elérési útja. |
| | `AZURE_CERTIFICATE_PASSWORD` | Az ügyféltanúsítvány jelszava. |
| __Felhasználónév/jelszó__ | `AZURE_TENANT_ID` | Annak az Active Directory-bérlőnek az azonosítója, amelyhez a felhasználó tartozik. |
| | `AZURE_CLIENT_ID` | Az alkalmazás ügyfél-azonosítója. |
| | `AZURE_USERNAME` | A bejelentkezéshez használt felhasználónév. |
| | `AZURE_PASSWORD` | A bejelentkezéshez használt jelszó. |
| __Kezelt identitás__ | | A felügyeltidentitás-hitelesítéshez nincs szükség hitelesítő adatokra. Az alkalmazásnak egy, a felügyelt identitások használatára konfigurált Azure-erőforráson kell futnia. További részletekért lásd [Azure-erőforrások felügyelt identitásai]. |

Az alábbi környezeti változók beállításával olyan felhőhöz vagy felügyeleti végponthoz csatlakozhat, amely nem az alapértelmezett Azure-beli nyilvános felhő. A változók beállításának leggyakoribb oka az Azure Stack, egy másik földrajzi régióban lévő felhő vagy a klasszikus üzemi modell használata.

| Környezeti változó | Leírás  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | Azon felhőkörnyezet neve, amelyhez a rendszer csatlakozik. |
| `AZURE_AD_RESOURCE` | A csatlakozáshoz használandó Active Directory erőforrás-azonosító használandó, amely URI-ként mutat a felügyeleti végpontra. |

A környezetalapú hitelesítés használatakor hívja meg a [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) függvényt a hitelesítő objektum beszerzéséhez. A rendszer ezt az objektumot fogja beállítani az ügyfelek `Authorizer` tulajdonságánál, hogy engedélyezze számukra az Azure-hoz való hozzáférést.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

### <a name="authentication-on-azure-stack"></a>Hitelesítés az Azure Stackben

Az Azure Stackben való hitelesítéshez be kell állítania a következő változókat:

| Környezeti változó | Leírás  |
|----------------------|--------------|
| `AZURE_AD_ENDPOINT` | Az Active Directory-végpont. |
| `AZURE_AD_RESOURCE` | Az Active Directory erőforrás-azonosító. |

Ezek a változók az Azure Stack metaadat-információiból kérhetők le. A metaadatok lekéréséhez nyisson meg egy webböngészőt az Azure Stack-környezetben, majd használja a következő URL-címet`(ResourceManagerURL)/metadata/endpoints?api-version=1.0`

A(z) `ResourceManagerURL` a régió neve, a gép neve és az Azure Stack üzemelő példányának külső teljes tartományneve (FQDN) alapján eltérő lehet:

| Környezet | ResourceManagerURL |
|----------------------|--------------|
| Fejlesztői készlet | `https://management.local.azurestack.external/` |
| Integrált rendszerek | `https://management.(region).ext-(machine-name).(FQDN)` |

A Góhoz készült Azure SDK az Azure Stackben való használatáról az [API-verzióprofiloknak a Góval az Azure Stackben történő használatát](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-go) bemutató témakörben tájékozódhat.

## <a name="use-file-based-authentication"></a>A fájlalapú hitelesítés használata

A fájlalapú hitelesítés egy, [az Azure CLI](/cli/azure) által létrehozott fájlformátumot használ. Ezt a fájlt könnyedén létrehozhatja, amikor új szolgáltatásnevet hoz létre az `--sdk-auth` paraméterrel. Ha fájlalapú hitelesítést szeretne használni, ügyeljen arra, hogy a szolgáltatásnév létrehozásakor megadja ezt az argumentumot. Mivel a CLI az `stdout` kimeneten nyomtatja a kimenetet, irányítsa át a kimenetet egy fájlba.

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

Az `AZURE_AUTH_LOCATION` környezeti változónál állítsa be a hitelesítési fájl helyét. Az alkalmazás beolvassa ezt a környezeti változót, és elemzi a benne található hitelesítő adatokat. Ha futásidőben kell kiválasztania a hitelesítési fájlt, a program környezetét az [os.Setenv](https://golang.org/pkg/os/#Setenv) függvénnyel kezelheti.

A hitelesítési adatok betöltéséhez hívja meg a [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) függvényt. A környezetalapú hitelesítéssel ellentétben a fájlalapú hitelesítésnél szükség van egy erőforrásvégpontra.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

A szolgáltatásnevek használatával és a hozzáférési engedélyeik kezelésével kapcsolatos további információkért lásd: [Szolgáltatásnév létrehozása az Azure CLI-vel].

## <a name="use-device-token-authentication"></a>Az eszközjogkivonattal történő hitelesítés használata

Ha azt szeretné, hogy a felhasználók interaktívan jelentkezzenek be, a legjobb megoldás az eszközjogkivonattal történő hitelesítés használata. Ez a hitelesítési folyamat átad egy jogkivonatot a felhasználónak, amelyet azon a Microsoft bejelentkezési webhelyen kell beillesztenie, ahol ezután egy Azure Active Directory- (AAD-) fiókkal kell hitelesítést végeznie. Ez a hitelesítési módszer a szabványos felhasználónév- és jelszóalapú hitelesítéssel ellentétben támogatja a többtényezős hitelesítést engedélyező fiókokat.

Az eszközjogkivonattal történő hitelesítés használatához hozzon létre egy [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) hitelesítőt a [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig) függvénnyel. A hitelesítési folyamat elindításához hívja meg az [Authorizer](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) függvényt az eredményül kapott objektumhoz. Az eszközfolyamat-hitelesítés letiltja a programok végrehajtását, amíg a teljes hitelesítési folyamat be nem fejeződik.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a>Hitelesítési ügyfél használata

Ha egy bizonyos típusú hitelesítésre van szüksége, és nem ellenzi azt, hogy a program tölti be a felhasználó hitelesítési adatait, akkor bármely ügyfelet használhatja, amely megfelel az [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig) felületnek. Olyan típust használjon, amely ezt a felületet valósítja meg a következő esetekben:

* Interaktív program írása
* Speciális konfigurációs fájlok használata
* Olyan követelmény alkalmazása, amely megakadályozza a beépített hitelesítési módszerek használatát

> [!WARNING]
> Az Azure-beli hitelesítő adatokat soha ne rögzítse szoftveresen egy alkalmazásba. Ha egy bináris alkalmazásfájlba helyezi a titkos kulcsokat, a támadók egyszerűbben ki tudják nyerni azokat, függetlenül attól, hogy éppen fut-e az alkalmazás. Ezzel veszélynek teszi ki az összes Azure-erőforrást, amelyhez a hitelesítő adatok engedélyezve vannak!

Az alábbi táblázat felsorolja azokat az SDK-ban elérhető típusokat, amelyek megfelelnek az `AuthorizerConfig` felületnek.

| Hitelesítés típusa | Hitelesítő típusa |
|---------------------|-----------------------|
| Tanúsítványalapú hitelesítés | [ClientCertificateConfig] |
| Ügyfél-hitelesítő adatok | [ClientCredentialsConfig] |
| Azure-erőforrások felügyelt identitásai | [MSIConfig] |
| Felhasználónév/jelszó | [UsernamePasswordConfig] |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

Hozzon létre egy hitelesítőt a kapcsolódó `New` függvénnyel, majd a hitelesítéshez hívja meg az `Authorize` függvényt az eredményül kapott objektumhoz. Például tanúsítványalapú hitelesítés használata esetén:

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
