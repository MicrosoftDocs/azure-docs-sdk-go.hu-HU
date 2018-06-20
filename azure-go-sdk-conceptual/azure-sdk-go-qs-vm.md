---
title: Azure-beli virtuális gép üzembe helyezése a Góról
description: Helyezzen üzembe egy virtuális gépet a Góhoz készült Azure SDK-val.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/03/2018
ms.topic: quickstart
ms.prod: azure
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: 1fbcc54df2a2aebce56c5a5800361f3d3aed1ccc
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/03/2018
ms.locfileid: "32319934"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Gyors útmutató: Azure-beli virtuális gép üzembe helyezése sablonból a Góhoz készült Azure SDK-val

Ez a rövid útmutató az erőforrások sablonból való üzembe helyezésére összpontosít a Góhoz készült Azure SDK használatával. A sablonok az összes erőforrás pillanatképei egy [Azure-erőforráscsoportban](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview). Menet közben megismerheti az SDK működését és konvencióit, mialatt hasznos feladatot végez.

A rövid útmutató végén olyan futó virtuális gépe lesz, amelybe felhasználónévvel és jelszóval tud bejelentkezni.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Ha az Azure CLI helyi telepítését használja, ehhez a rövid útmutatóhoz a __CLI 2.0.28-as__ vagy újabb verziójára van szükség. Futtassa az `az --version` parancsot annak ellenőrzéséhez, hogy a CLI telepítés megfelel-e ennek a követelménynek. Ha telepítenie vagy frissítenie kell, lásd: [Az Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>A Góhoz készült Azure SDK telepítése 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Egyszerű szolgáltatás létrehozása


Ha nem interaktív módon szeretne bejelentkezni egy alkalmazással, egy egyszerű szolgáltatásra lesz szüksége. Az egyszerű szolgáltatások a szerepköralapú hozzáférés-vezérlés (RBAC) részei, amely egyedi felhasználói azonosítót hoz létre. Ha új egyszerű szolgáltatást szeretne létrehozni a parancssori felülettel, futtassa a következő parancsot:

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart --sdk-auth > quickstart.auth
```

Az `AZURE_AUTH_LOCATION` környezeti változónál állítsa be a fájl teljes elérési útját. Az SDK ezután megkeresi és beolvassa a hitelesítő adatokat közvetlenül a fájlból, anélkül, hogy módosítania kellene valamit, vagy rögzítenie kellene a szolgáltatásnév adatait.

## <a name="get-the-code"></a>A kód letöltése

A rövid útmutató kódját és az összes függőségét letöltheti a következővel: `go get`.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

Nem kell módosítania a forráskódot, ha az `AZURE_AUTH_LOCATION` változó megfelelően van beállítva. Amikor a program fut, ebből a változóból tölti be a szükséges hitelesítési adatokat.

## <a name="running-the-code"></a>A kód futtatása

Futtassa a rövid útmutatót a `go run` paranccsal.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

Ha hiba van az üzemelő példányban, megjelenik egy üzenet, amely jelzi, hogy hiba történt, de ez gyakran nem elég részletes. Az Azure CLI használatával kérdezze le az üzemelő példány hibájának összes részletét a következő paranccsal:

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

Ha az üzembe helyezés sikeres, megjelenik egy üzenet a felhasználónévvel, az IP-címmel és a jelszóval, amelyekkel bejelentkezhet az újonnan létrehozott virtuális gépre. Lépjen be SSH-val a gépre annak ellenőrzéséhez, hogy működik-e.

## <a name="cleaning-up"></a>Takarítás

A rövid útmutató során létrehozott erőforrásokat az erőforráscsoport törlésével, a parancssori felület használatával távolíthatja el.

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a>A kód részletei

A rövid útmutató kódja változócsoportokra és több kis függvényre van felosztva, amelyek leírásait itt találja.

### <a name="variables-constants-and-types"></a>Változók, állandók és típusok

Mivel a rövid útmutató önálló, globális állandókat és változókat használ.

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

// Information loaded from the authorization file to identify the client
type clientInfo struct {
    SubscriptionID string
    VMPassword     string
}

var (
    ctx        = context.Background()
    clientData clientInfo
    authorizer autorest.Authorizer
)
```

Meg vannak határozva a létrehozott erőforrások neveit megadó értékek. Itt a hely is meg van adva, amely módosítható, hogy lássa, hogyan működnek az üzemelő példányok más adatközpontokban. Nem minden adatközponton érhető el az összes szükséges erőforrás.

A `clientInfo` típus úgy van meghatározva, hogy magába foglalja az összes információt, amelyet külön kell betölteni a hitelesítési fájlból az ügyfelek SDK-ban való beállításához, valamint a virtuális gép jelszavának beállításához.

A `templateFile` és `parametersFile` állandó az üzembe helyezéshez szükséges fájlokra mutat. Az `authorizer` hitelesítéshez való konfigurálását a Go SDK fogja elvégezni. A `ctx` változó a hálózati műveletek [Go környezete](https://blog.golang.org/context).

### <a name="authentication-and-initialization"></a>Hitelesítés és inicializálás

Az `init` függvény állítja be a hitelesítést. Mivel a hitelesítés a rövid útmutatóban minden másnak az előfeltétele, logikus, hogy az inicializálás része legyen. A függvény ezen kívül néhány szükséges adatot is betölt a hitelesítési fájlból az ügyfelek és a virtuális gép konfigurálásához.

```go
func init() {
    var err error
    authorizer, err = auth.NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v", err)
    }

    authInfo, err := readJSON(os.Getenv("AZURE_AUTH_LOCATION"))
    clientData.SubscriptionID = (*authInfo)["subscriptionId"].(string)
    clientData.VMPassword = (*authInfo)["clientSecret"].(string)
}
```

A rendszer először meghívja az [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) függvényt, hogy az betöltse az `AZURE_AUTH_LOCATION` helyen található fájl hitelesítési adatait. Ezután a `readJSON` függvény (itt ki van hagyva) manuálisan betölti ezt a fájlt a program többi részének futtatásához szükséges két érték lekéréséhez. Ezek egyike az ügyfél előfizetési azonosítója, a másik pedig a szolgáltatásnév titkos kulcsa, amelyet a virtuális gép jelszavához is használni lehet.

> [!WARNING]
> A rövid útmutató egyszerűsége érdekében a szolgáltatásnév jelszava ugyanaz, mint eddig. Éles környezetben ügyeljen arra, hogy __soha__ ne használja fel többször azt a jelszót, amellyel hozzáférhet az Azure-erőforrásokhoz.

### <a name="flow-of-operations-in-main"></a>A main() műveletfolyamata

A `main` függvény egyszerű, csak a műveletek folyamatát jelzi, és hibaellenőrzést végez.

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("Created group: %v", *group.Name)

    log.Printf("Starting deployment: %s", deploymentName)
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy: %v", err)
    }
    if result.Name != nil {
        log.Printf("Completed deployment %v: %v", deploymentName, *result.Properties.ProvisioningState)
    } else {
        log.Printf("Completed deployment %v (no data returned to SDK)", deploymentName)
    }
    getLogin()
}
```

A kód által végrehajtott lépések sorrendben a következők:

* Az üzembe helyezendő erőforráscsoport létrehozása (`createGroup`)
* Az üzemelő példány létrehozása ebben a csoportban (`createDeployment`)
* Az üzembe helyezett virtuális gép bejelentkezési információinak beszerzése és megjelenítése (`getLogin`)

### <a name="creating-the-resource-group"></a>Az erőforráscsoport létrehozása

A `createGroup` függvény létrehozza az erőforráscsoportot. A hívásfolyam és az argumentumok megtekintésekor láthatja, hogyan vannak rendszerezve a szolgáltatásinterakciók az SDK-ban.

```go
func createGroup() (group resources.Group, err error) {
    groupsClient := resources.NewGroupsClient(clientData.SubscriptionID)
    groupsClient.Authorizer = authorizer

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

Az Azure szolgáltatással való kommunikáció általános folyamata a következő:

* Hozza létre az ügyfelet a `service.New*Client()` metódussal, ahol az `*` a `service` azon erőforrástípusa, amellyel kommunikálni szeretne. Ez a függvény mindig egy előfizetés-azonosítót vesz fel.
* Állítsa be az ügyfél engedélyezési metódusát, hogy kommunikálhasson a távoli API-val.
* Végezze el a metódushívást a távoli API-nak megfelelő ügyfélen. A szolgáltatásügyfél-metódusok általában az erőforrás és egy metaadat-objektum nevét veszik fel.

A [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) függvénnyel típusátalakítás végezhető. Az SDK metódusainak paraméterei szinte kizárólag mutatókat vesznek, ezért az egyszerűsített metódusok a típusátalakítás megkönnyítése miatt vannak megadva. A dokumentációban lévő [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) modulban találja az átalakítók teljes listáját és viselkedését.

A `groupsClient.CreateOrUpdate` metódus egy mutatót ad vissza az erőforráscsoportot jelölő adattípusnak. Az ilyen típusú közvetlen visszatérési érték olyan rövid futásidejű műveletet jelez, amelynek szinkronban kell lennie. A következő szakasz a hosszú futásidejű műveletek egy példáját, illetve annak a kezelési módját mutatja be.

### <a name="performing-the-deployment"></a>Az üzembe helyezés végrehajtása

Amikor létrejött az erőforráscsoport, itt az idő az üzembe helyezéshez. Ez a kód a logika különböző részeinek kihangsúlyozása érdekében kisebb szakaszokra van osztva.

```go
func createDeployment() (deployment resources.DeploymentExtended, err error) {
    template, err := readJSON(templateFile)
    if err != nil {
        return
    }
    params, err := readJSON(parametersFile)
    if err != nil {
        return
    }
    (*params)["vm_password"] = map[string]string{
        "value": clientData.VMPassword,
    }
        // ...
```

A `readJSON` betölti az üzembe helyezési fájlokat, amelyek részletei itt kimaradnak. Ez a függvény visszaad egy `*map[string]interface{}` elemet, amely az erőforrás üzembe helyezési hívása metaadatainak felépítéséhez használt típus. A virtuális gép jelszavát is manuálisan kell beállítani az üzembehelyezési paramétereknél.

```go
        // ...

    deploymentsClient := resources.NewDeploymentsClient(clientData.SubscriptionID)
    deploymentsClient.Authorizer = authorizer

    deploymentFuture, err := deploymentsClient.CreateOrUpdate(
        ctx,
        resourceGroupName,
        deploymentName,
        resources.Deployment{
            Properties: &resources.DeploymentProperties{
                Template:   template,
                Parameters: params,
                Mode:       resources.Incremental,
            },
        },
    )
    if err != nil {
        return
    }
```

Ez a kód ugyanazt a mintát követi, mint az erőforráscsoport létrehozásakor. Létrejön egy új ügyfél, amely hitelesíteni tud az Azure-ral, majd a rendszer meghív egy metódust. A metódusnak ugyanaz a neve is (`CreateOrUpdate`), mint az erőforráscsoportok megfelelő metódusának. Ez a minta több helyen is látható az SDK-ban. A hasonló munkát végző metódusoknak általában ugyanaz a neve.

A legnagyobb különbség a `deploymentsClient.CreateOrUpdate` metódus visszaadott értéke. Az érték típusa [Jövőbeli](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), amely a [jövőbeli kialakítási mintát](https://en.wikipedia.org/wiki/Futures_and_promises) követi. A jövő az Azure-ban egy hosszú futásidejű műveletet jelez, amelyet a művelet befejezésekor lekérdezhet, megszakíthat vagy letilthat.

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    deployment, err = deploymentFuture.Result(deploymentsClient)

    // Work around possible bugs or late-stage failures
    if deployment.Name == nil || err != nil {
        deployment, _ = deploymentsClient.Get(ctx, resourceGroupName, deploymentName)
    }
    return
```

Ebben a példában a legjobb, ha megvárja a művelet befejezését. Ha a jövőre vár, szüksége van egy [környezeti objektumra](https://blog.golang.org/context) és a `Future` létrehozó ügyfelére. Itt két lehetséges hibaforrás van: Az ügyfél oldalán okozott hiba, amikor megpróbálja meghívni a metódust, illetve egy hibaválasz a kiszolgálóról. Az utóbbit a rendszer a `deploymentFuture.Result` hívás részeként adja vissza.

Ha lekérte a telepítéssel kapcsolatos információkat, létezik egy megkerülő megoldás azokra a lehetséges programhibákra, amikor ezek az információk nem jelennek meg. Az adatok betöltéséhez hívja meg manuálisan a `deploymentsClient.Get` metódust.

### <a name="obtaining-the-assigned-ip-address"></a>A hozzárendelt IP-cím beszerzése

Ha bármit szeretne tenni az újonnan létrehozott virtuális géppel, szüksége van a hozzárendelt IP-címre. Az IP-címek önmaguk saját külön Azure-erőforrásai, amelyek NIC-erőforrásokhoz kötődnek.

```go
func getLogin() {
    params, err := readJSON(parametersFile)
    if err != nil {
        log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
    }

    addressClient := network.NewPublicIPAddressesClient(clientData.SubscriptionID)
    addressClient.Authorizer = authorizer
    ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
    ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
    if err != nil {
        log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
    }

    vmUser := (*params)["vm_user"].(map[string]interface{})

    log.Printf("Log in with ssh: %s@%s, password: %s",
        vmUser["value"].(string),
        *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
        clientData.VMPassword)
}
```

Ez a metódus a paraméterfájlban tárolt adatokra támaszkodik. A kód közvetlenül le tudta kérdezni a virtuális gépről az NIC-t, lekérdezte az NIC-ről az IP-erőforrást, majd közvetlenül lekérdezte az IP-erőforrást. Ez egy hosszú függőség- és műveletlánc, ezért költséges. Mivel a JSON-adat helyiek, ehelyett betölthetők.

A virtuális gép felhasználójának értéke szintén betölthető a JSON-ból. A virtuális gép jelszava korábban már be lett töltve a hitelesítési fájlból.

## <a name="next-steps"></a>További lépések

Ebben a rövid útmutatóban egy meglévő sablont helyezett üzembe a Gón keresztül. Ezután az újonnan létrehozott virtuális gépet SSH-n keresztül csatlakoztatta, hogy biztosan fusson.

Ha többet szeretne megtudni a virtuális gépek Góval való használatáról az Azure-környezetben, lásd: [Azure számítási minták a Góhoz](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) vagy [Azure erőforrás-felügyeleti példák a Góhoz](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).

Ha többet szeretne megtudni az SDK-ban elérhető hitelesítési módszerekről, és az általuk támogatott hitelesítési típusokról, lásd: [Hitelesítés a Góhoz készült Azure SDK-val](azure-sdk-go-authorization.md).
