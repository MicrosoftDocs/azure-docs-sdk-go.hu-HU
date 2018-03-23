---
title: Azure-beli virtuális gép üzembe helyezése a Góról
description: Helyezzen üzembe egy virtuális gépet a Góhoz készült Azure SDK-val.
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 46a1243ff2ff6bfcf3831e2cea3137c1f6051c78
ms.sourcegitcommit: fcc1786d59d2e32c97a9a8e0748e06f564a961bd
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/23/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Gyors útmutató: Azure-beli virtuális gép üzembe helyezése sablonból a Góhoz készült Azure SDK-val

Ez a rövid útmutató az erőforrások sablonból való üzembe helyezésére összpontosít a Góhoz készült Azure SDK használatával. A sablonok az összes erőforrás pillanatképei egy [Azure-erőforráscsoportban](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview). Menet közben megismerheti az SDK működését és konvencióit, mialatt hasznos feladatot végez.

A rövid útmutató végén olyan futó virtuális gépe lesz, amelybe felhasználónévvel és jelszóval tud bejelentkezni.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Ha az Azure CLI helyi telepítését használja, ehhez a rövid útmutatóhoz a CLI 2.0.24-es vagy újabb verziójára van szükség. Futtassa az `az --version` parancsot annak ellenőrzéséhez, hogy a CLI telepítés megfelel-e ennek a követelménynek. Ha telepítenie vagy frissítenie kell, lásd: [Az Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>A Góhoz készült Azure SDK telepítése 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Egyszerű szolgáltatás létrehozása

Ha nem interaktív módon szeretne bejelentkezni egy alkalmazással, egy egyszerű szolgáltatásra lesz szüksége. Az egyszerű szolgáltatások a szerepköralapú hozzáférés-vezérlés (RBAC) részei, amely egyedi felhasználói azonosítót hoz létre. Ha új egyszerű szolgáltatást szeretne létrehozni a parancssori felülettel, futtassa a következő parancsot:

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

__Mindenképpen__ jegyezze fel az `appId`, `password` és `tenant` értékeket a kimenetben. Az alkalmazás ezekkel az értékekkel végzi a hitelesítést az Azure-ral.

Az egyszerű szolgáltatások Azure CLI 2.0-val való létrehozásával és kezelésével kapcsolatos további információért lásd: [Azure-beli egyszerű szolgáltatások létrehozása az Azure CLI 2.0-val](/cli/azure/create-an-azure-service-principal-azure-cli).

## <a name="get-the-code"></a>A kód letöltése

A rövid útmutató kódját és az összes függőségét letöltheti a következővel: `go get`.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

Ez a kód lefordítható, de nem fut megfelelően, amíg meg nem adja az Azure-fiók és a létrehozott egyszerű szolgáltatás információit. A `main.go` elemben található egy `config` változó, amely `authInfo` struktúrát tartalmaz. A megfelelő hitelesítéshez ebben a struktúrában le kell cserélni a mezőértékeket.

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* `SubscriptionID`: Az előfizetés azonosítója, amelyet a CLI parancsból szerezhet be

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* `TenantID`: A bérlő azonosítója, az egyszerű szolgáltatás létrehozásakor rögzített `tenant` érték
* `ServicePrincipalID`: Az egyszerű szolgáltatás létrehozásakor rögzített `appId` érték
* `ServicePrincipalSecret`: Az egyszerű szolgáltatás létrehozásakor rögzített `password` érték

A `vm-quickstart-params.json` fájlban is szerkesztenie kell egy értéket.

```json
    "vm_password": {
        "value": "_"
    }
```

* `vm_password`: A virtuális gép felhasználói fiókjának jelszava. 12–72 karakter hosszúnak kell lennie, és a következő karakterek közül 3-at kell tartalmaznia:
  * Egy kisbetű
  * Egy nagybetű
  * Egy szám
  * Egy szimbólum

## <a name="running-the-code"></a>A kód futtatása

Futtassa a rövid útmutatót a `go run` paranccsal.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

Ha hiba van az üzemelő példányban, megjelenik egy üzenet, amely jelzi, hogy hiba történt, de konkrét részleteket nem ad meg. Az Azure CLI használatával kérdezze le az üzemelő példány hibájának részleteit a következő paranccsal:

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

### <a name="variable-assignments-and-structs"></a>Változó-hozzárendelések és struktúrák

Mivel a rövid útmutató önálló, parancssori kapcsolók és környezeti változók helyett globális változókat használ.

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

Az `authInfo` struktúra úgy van meghatározva, hogy magába foglalja az Azure-szolgáltatásokkal való hitelesítéshez szükséges összes információt.

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

var (
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }

    ctx = context.Background()

    token *adal.ServicePrincipalToken
)
```

Meg vannak határozva a létrehozott erőforrások neveit megadó értékek. Itt a hely is meg van adva, amely módosítható, hogy lássa, hogyan működnek az üzemelő példányok más adatközpontokban. Nem minden adatközponton érhető el az összes szükséges erőforrás.

A `templateFile` és `parametersFile` állandó az üzembe helyezéshez szükséges fájlokra mutat. Az egyszerű szolgáltatás tokenjének leírása a későbbiekben szerepel, a `ctx` változó pedig a hálózati műveletek [Go környezete](https://blog.golang.org/context).

### <a name="init-and-authorization"></a>init() és engedélyezés

A kód `init()` metódusa beállítja az engedélyezést. Mivel az engedélyezés a rövid útmutatóban minden másnak az előfeltétele, logikus, hogy az inicializálás része legyen. 

```go
// Authenticate with the Azure services over OAuth, using a service principal.
func init() {
    oauthConfig, err := adal.NewOAuthConfig(azure.PublicCloud.ActiveDirectoryEndpoint, config.TenantID)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v\n", err)
    }
    token, err = adal.NewServicePrincipalToken(
        *oauthConfig,
        config.ServicePrincipalID,
        config.ServicePrincipalSecret,
        azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("faled to get token: %v\n", err)
    }
}
```

Ez a kód az engedélyezés két lépését végzi el:

* Lekéri a `TenantID` OAuth konfigurációs információit az Azure Active Directoryhoz való illeszkedéssel. Az [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) objektum a standard Azure-konfigurációban használt végpontokat tartalmazza.
* A rendszer meghívja az [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) függvényt. Ez a függvény átveszi az OAuth-információkat az egyszerű szolgáltatásba való bejelentkezési adatokkal, valamint a használt Azure felügyeleti stílussal együtt. Hacsak nincsenek adott követelményei, és nem gyakorlott felhasználó, ennek mindig `.ResourceManagerEndpoint` értékűnek kell lennie.

### <a name="flow-of-operations-in-main"></a>A main() műveletfolyamata

A `main()` függvény egyszerű, csak a műveletek folyamatát jelzi, és hibaellenőrzést végez.

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("created group: %v\n", *group.Name)

    log.Println("starting deployment")
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy correctly: %v", err)
    }
    log.Printf("Completed deployment: %v", *result.Name)
    getLogin()
}
```

A kód által végrehajtott lépések sorrendben a következők:

* Az üzembe helyezendő erőforráscsoport létrehozása (`createGroup()`)
* Az üzemelő példány létrehozása ebben a csoportban (`createDeployment()`)
* Az üzembe helyezett virtuális gép bejelentkezési információinak beszerzése és megjelenítése (`getLogin()`)

### <a name="creating-the-resource-group"></a>Az erőforráscsoport létrehozása

A `createGroup()` függvény létrehozza az erőforráscsoportot. A hívásfolyam és az argumentumok megtekintésekor láthatja, hogyan vannak rendszerezve a szolgáltatásinterakciók az SDK-ban.

```go
func createGroup() (group resources.Group, err error) {
        groupsClient := resources.NewGroupsClient(config.SubscriptionID)
        groupsClient.Authorizer = autorest.NewBearerAuthorizer(token)

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

Az Azure szolgáltatással való kommunikáció általános folyamata a következő:

* Hozza létre az ügyfelet a `service.NewXClient()` metódussal, ahol az `X` a `service` azon erőforrástípusa, amellyel kommunikálni szeretne. Ez a függvény mindig egy előfizetés-azonosítót vesz fel.
* Állítsa be az ügyfél engedélyezési metódusát, hogy kommunikálhasson a távoli API-val.
* Végezze el a metódushívást a távoli API-nak megfelelő ügyfélen. A szolgáltatásügyfél-metódusok általában az erőforrás és egy metaadat-objektum nevét veszik fel.

A [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) függvénnyel típusátalakítás végezhető. Az SDK metódusainak paraméterstruktúrái szinte kizárólag mutatókat vesznek, ezért ezek a metódusok a típusátalakítás megkönnyítése miatt vannak megadva. A dokumentációban lévő [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) modulban találja az átalakítók teljes listáját és viselkedését.

A `groupsClient.CreateOrUpdate()` művelet egy mutatót ad vissza az erőforráscsoportot jelző adatstruktúrának. Az ilyen típusú közvetlen visszatérési érték olyan rövid futásidejű műveletet jelez, amelynek szinkronban kell lennie. A következő szakasz a hosszú futásidejű műveletek egy példáját mutatja be, illetve azok kezelési módját.

### <a name="performing-the-deployment"></a>Az üzembe helyezés végrehajtása

Amikor létrejött az erőforrásokat tartalmazó csoport, itt az idő az üzembe helyezéshez. Ez a kód a logika különböző részeinek kihangsúlyozása érdekében kisebb szakaszokra van osztva.


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

        // ...
```

A `readJSON` betölti az üzembe helyezési fájlokat, amelyek részletei itt kimaradnak. Ez a függvény visszaad egy `*map[string]interface{}` elemet, amely az erőforrás üzembe helyezési hívása metaadatainak felépítéséhez használt típus.

```go
        // ...
        
        deploymentsClient := resources.NewDeploymentsClient(config.SubscriptionID)
        deploymentsClient.Authorizer = autorest.NewBearerAuthorizer(token)

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
                log.Fatalf("Failed to create deployment: %v", err)
        }
        //...
```

Ez a kód ugyanazt a mintát követi, mint az erőforráscsoport létrehozásakor. Létrejön egy új ügyfél, amely hitelesíteni tud az Azure-ral, majd a rendszer meghív egy metódust. A metódusnak ugyanaz a neve is (`CreateOrUpdate`), mint az erőforráscsoportok megfelelő metódusának. Ez a minta újra és újra látható az SDK-ban. A hasonló munkát végző metódusoknak általában ugyanaz a neve.

A legnagyobb különbség a `deploymentsClient.CreateOrUpdate()` metódus visszaadott értéke. Ez az érték egy `Future` objektum, amely a következő a [jövőbeli kialakítási mintát](https://en.wikipedia.org/wiki/Futures_and_promises) követi. A jövő az Azure-ban egy hosszú futásidejű műveletet jelez, amelyet érdemes időnként lekérdezni, mialatt más munkát végez.

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

Ebben a példában a legjobb, ha megvárja a művelet befejezését. Ha a jövőre vár, szüksége van egy [környezeti objektumra](https://blog.golang.org/context) és a jövőbeli objektumot létrehozó ügyfélre. Itt két lehetséges hibaforrás van: Az ügyfél oldalán okozott hiba, amikor megpróbálja meghívni a metódust, illetve egy hibaválasz a kiszolgálóról. Az utóbbit a rendszer a `deploymentFuture.Result()` hívás részeként adja vissza.

### <a name="obtaining-the-assigned-ip-address"></a>A hozzárendelt IP-cím beszerzése

Ha bármit szeretne tenni az újonnan létrehozott virtuális géppel, szüksége van a hozzárendelt IP-címre. Az IP-címek önmaguk saját külön Azure-erőforrásai, amelyek NIC-erőforrásokhoz kötődnek.

```go
func getLogin() {
        params, err := readJSON(parametersFile)
        if err != nil {
                log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
        }

        addressClient := network.NewPublicIPAddressesClient(config.SubscriptionID)
        addressClient.Authorizer = autorest.NewBearerAuthorizer(token)
        ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
        ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
        if err != nil {
                log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
        }

        vmUser := (*params)["vm_user"].(map[string]interface{})
        vmPass := (*params)["vm_password"].(map[string]interface{})

        log.Printf("Log in with ssh: %s@%s, password: %s",
                vmUser["value"].(string),
                *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
                vmPass["value"].(string))
}
```

Ez a metódus a paraméterfájlban tárolt adatokra támaszkodik. A kód közvetlenül le tudta kérdezni a virtuális gépről az NIC-t, lekérdezte az NIC-ről az IP-erőforrást, majd közvetlenül lekérdezte az IP-erőforrást. Ez egy hosszú függőség- és műveletlánc, ezért költséges. Mivel a JSON-adat helyiek, ehelyett betölthetők.

A virtuális gép felhasználójának és jelszavának az értékei ugyanígy betölthetőek a JSON-ból.

## <a name="next-steps"></a>További lépések

Ebben a rövid útmutatóban egy meglévő sablont helyezett üzembe a Gón keresztül. Ezután az újonnan létrehozott virtuális gépet SSH-n keresztül csatlakoztatta, hogy biztosan fusson.

Ha többet szeretne megtudni a virtuális gépek Góval való használatáról az Azure-környezetben, lásd: [Azure számítási minták a Góhoz](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) vagy [Azure erőforrás-felügyeleti példák a Góhoz](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).
