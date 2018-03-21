---
title: "Azure-beli virtuális gép üzembe helyezése a Góról"
description: "Helyezzen üzembe egy virtuális gépet a Góhoz készült Azure SDK-val."
keywords: "azure, virtuális gép, virtuális gép, go, golang, azure sdk"
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: routlaw
ms.openlocfilehash: ae460dbf21b13c40f3d564274f8b790afe005aae
ms.sourcegitcommit: af3473779cd7c2978f290fbdc51ee15eb1130840
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/15/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="cf379-104">Gyors útmutató: Azure-beli virtuális gép üzembe helyezése sablonból a Góhoz készült Azure SDK-val</span><span class="sxs-lookup"><span data-stu-id="cf379-104">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="cf379-105">Ez a rövid útmutató az erőforrások sablonból való üzembe helyezésére összpontosít a Góhoz készült Azure SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="cf379-105">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="cf379-106">A sablonok az összes erőforrás pillanatképei egy [Azure-erőforráscsoportban](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="cf379-106">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="cf379-107">Menet közben megismerheti az SDK működését és konvencióit, mialatt hasznos feladatot végez.</span><span class="sxs-lookup"><span data-stu-id="cf379-107">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="cf379-108">A rövid útmutató végén olyan futó virtuális gépe lesz, amelybe felhasználónévvel és jelszóval tud bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="cf379-108">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="cf379-109">Ha az Azure CLI helyi telepítését használja, ehhez a rövid útmutatóhoz a CLI 2.0.24-es vagy újabb verziójára van szükség.</span><span class="sxs-lookup"><span data-stu-id="cf379-109">If you use a local install of the Azure CLI, this quickstart requires CLI version 2.0.24 or later.</span></span> <span data-ttu-id="cf379-110">Futtassa az `az --version` parancsot annak ellenőrzéséhez, hogy a CLI telepítés megfelel-e ennek a követelménynek.</span><span class="sxs-lookup"><span data-stu-id="cf379-110">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="cf379-111">Ha telepítenie vagy frissítenie kell, lásd: [Az Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="cf379-111">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="cf379-112">A Góhoz készült Azure SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="cf379-112">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="cf379-113">Egyszerű szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="cf379-113">Create a service principal</span></span>

<span data-ttu-id="cf379-114">Ha nem interaktív módon szeretne bejelentkezni egy alkalmazással, egy egyszerű szolgáltatásra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="cf379-114">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="cf379-115">Az egyszerű szolgáltatások a szerepköralapú hozzáférés-vezérlés (RBAC) részei, amely egyedi felhasználói azonosítót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="cf379-115">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="cf379-116">Ha új egyszerű szolgáltatást szeretne létrehozni a parancssori felülettel, futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="cf379-116">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

<span data-ttu-id="cf379-117">__Mindenképpen__ jegyezze fel az `appId`, `password` és `tenant` értékeket a kimenetben.</span><span class="sxs-lookup"><span data-stu-id="cf379-117">__Make sure__ to record the `appId`, `password`, and `tenant` values in the output.</span></span> <span data-ttu-id="cf379-118">Az alkalmazás ezekkel az értékekkel végzi a hitelesítést az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="cf379-118">These values are used by the application to authenticate with Azure.</span></span>

<span data-ttu-id="cf379-119">Az egyszerű szolgáltatások Azure CLI 2.0-val való létrehozásával és kezelésével kapcsolatos további információért lásd: [Azure-beli egyszerű szolgáltatások létrehozása az Azure CLI 2.0-val](/cli/azure/create-an-azure-service-principal-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="cf379-119">For more information on creating and managing service principals with the Azure CLI 2.0, see [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="cf379-120">A kód letöltése</span><span class="sxs-lookup"><span data-stu-id="cf379-120">Get the code</span></span>

<span data-ttu-id="cf379-121">A rövid útmutató kódját és az összes függőségét letöltheti a következővel: `go get`.</span><span class="sxs-lookup"><span data-stu-id="cf379-121">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

<span data-ttu-id="cf379-122">Ez a kód lefordítható, de nem fut megfelelően, amíg meg nem adja az Azure-fiók és a létrehozott egyszerű szolgáltatás információit.</span><span class="sxs-lookup"><span data-stu-id="cf379-122">This code compiles, but doesn't run correctly until you provide it information about your Azure account and the created service principal.</span></span> <span data-ttu-id="cf379-123">A `main.go` elemben található egy `config` változó, amely `authInfo` struktúrát tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="cf379-123">In `main.go` there is a variable, `config`, which contains an `authInfo` struct.</span></span> <span data-ttu-id="cf379-124">A megfelelő hitelesítéshez ebben a struktúrában le kell cserélni a mezőértékeket.</span><span class="sxs-lookup"><span data-stu-id="cf379-124">This struct needs to have its field values replaced in order to authenticate correctly.</span></span>

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* <span data-ttu-id="cf379-125">`SubscriptionID`: Az előfizetés azonosítója, amelyet a CLI parancsból szerezhet be</span><span class="sxs-lookup"><span data-stu-id="cf379-125">`SubscriptionID`: Your subscription ID, which can be obtained from the CLI command</span></span>

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* <span data-ttu-id="cf379-126">`TenantID`: A bérlő azonosítója, az egyszerű szolgáltatás létrehozásakor rögzített `tenant` érték</span><span class="sxs-lookup"><span data-stu-id="cf379-126">`TenantID`: Your tenant ID, the `tenant` value recorded when creating the service principal</span></span>
* <span data-ttu-id="cf379-127">`ServicePrincipalID`: Az egyszerű szolgáltatás létrehozásakor rögzített `appId` érték</span><span class="sxs-lookup"><span data-stu-id="cf379-127">`ServicePrincipalID`: The `appId` value recorded when creating the service principal</span></span>
* <span data-ttu-id="cf379-128">`ServicePrincipalSecret`: Az egyszerű szolgáltatás létrehozásakor rögzített `password` érték</span><span class="sxs-lookup"><span data-stu-id="cf379-128">`ServicePrincipalSecret`: The `password` value recorded when creating the service principal</span></span>

<span data-ttu-id="cf379-129">A `vm-quickstart-params.json` fájlban is szerkesztenie kell egy értéket.</span><span class="sxs-lookup"><span data-stu-id="cf379-129">You also need to edit a value in the `vm-quickstart-params.json` file.</span></span>

```json
    "vm_password": {
        "value": "_"
    }
```

* <span data-ttu-id="cf379-130">`vm_password`: A virtuális gép felhasználói fiókjának jelszava.</span><span class="sxs-lookup"><span data-stu-id="cf379-130">`vm_password`: The password for the VM user account.</span></span> <span data-ttu-id="cf379-131">12–72 karakter hosszúnak kell lennie, és a következő karakterek közül 3-at kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="cf379-131">It must be 12-72 characters in length and contain 3 of the following characters:</span></span>
  * <span data-ttu-id="cf379-132">Egy kisbetű</span><span class="sxs-lookup"><span data-stu-id="cf379-132">A lowercase letter</span></span>
  * <span data-ttu-id="cf379-133">Egy nagybetű</span><span class="sxs-lookup"><span data-stu-id="cf379-133">An uppercase letter</span></span>
  * <span data-ttu-id="cf379-134">Egy szám</span><span class="sxs-lookup"><span data-stu-id="cf379-134">A number</span></span>
  * <span data-ttu-id="cf379-135">Egy szimbólum</span><span class="sxs-lookup"><span data-stu-id="cf379-135">A symbol</span></span>

## <a name="running-the-code"></a><span data-ttu-id="cf379-136">A kód futtatása</span><span class="sxs-lookup"><span data-stu-id="cf379-136">Running the code</span></span>

<span data-ttu-id="cf379-137">Futtassa a rövid útmutatót a `go run` paranccsal.</span><span class="sxs-lookup"><span data-stu-id="cf379-137">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

<span data-ttu-id="cf379-138">Ha hiba van az üzemelő példányban, megjelenik egy üzenet, amely jelzi, hogy hiba történt, de konkrét részleteket nem ad meg.</span><span class="sxs-lookup"><span data-stu-id="cf379-138">If there is a failure in the deployment, you get a message indicating that there was an issue, but without any specific details.</span></span> <span data-ttu-id="cf379-139">Az Azure CLI használatával kérdezze le az üzemelő példány hibájának részleteit a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="cf379-139">Using the Azure CLI, get the details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="cf379-140">Ha az üzembe helyezés sikeres, megjelenik egy üzenet a felhasználónévvel, az IP-címmel és a jelszóval, amelyekkel bejelentkezhet az újonnan létrehozott virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="cf379-140">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="cf379-141">Lépjen be SSH-val a gépre annak ellenőrzéséhez, hogy működik-e.</span><span class="sxs-lookup"><span data-stu-id="cf379-141">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="cf379-142">Takarítás</span><span class="sxs-lookup"><span data-stu-id="cf379-142">Cleaning up</span></span>

<span data-ttu-id="cf379-143">A rövid útmutató során létrehozott erőforrásokat az erőforráscsoport törlésével, a parancssori felület használatával távolíthatja el.</span><span class="sxs-lookup"><span data-stu-id="cf379-143">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="cf379-144">A kód részletei</span><span class="sxs-lookup"><span data-stu-id="cf379-144">Code in depth</span></span>

<span data-ttu-id="cf379-145">A rövid útmutató kódja változócsoportokra és több kis függvényre van felosztva, amelyek leírásait itt találja.</span><span class="sxs-lookup"><span data-stu-id="cf379-145">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variable-assignments-and-structs"></a><span data-ttu-id="cf379-146">Változó-hozzárendelések és struktúrák</span><span class="sxs-lookup"><span data-stu-id="cf379-146">Variable assignments and structs</span></span>

<span data-ttu-id="cf379-147">Mivel a rövid útmutató önálló, parancssori kapcsolók és környezeti változók helyett globális változókat használ.</span><span class="sxs-lookup"><span data-stu-id="cf379-147">Since quickstart is self-contained, it uses global variables rather than command-line options or environment variables.</span></span>

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

<span data-ttu-id="cf379-148">Az `authInfo` struktúra úgy van meghatározva, hogy magába foglalja az Azure-szolgáltatásokkal való hitelesítéshez szükséges összes információt.</span><span class="sxs-lookup"><span data-stu-id="cf379-148">The `authInfo` struct is declared to encapsulate all of the information needed for authorization with Azure services.</span></span>

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

<span data-ttu-id="cf379-149">Meg vannak határozva a létrehozott erőforrások neveit megadó értékek.</span><span class="sxs-lookup"><span data-stu-id="cf379-149">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="cf379-150">Itt a hely is meg van adva, amely módosítható, hogy lássa, hogyan működnek az üzemelő példányok más adatközpontokban.</span><span class="sxs-lookup"><span data-stu-id="cf379-150">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="cf379-151">Nem minden adatközponton érhető el az összes szükséges erőforrás.</span><span class="sxs-lookup"><span data-stu-id="cf379-151">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="cf379-152">A `templateFile` és `parametersFile` állandó az üzembe helyezéshez szükséges fájlokra mutat.</span><span class="sxs-lookup"><span data-stu-id="cf379-152">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="cf379-153">Az egyszerű szolgáltatás tokenjének leírása a későbbiekben szerepel, a `ctx` változó pedig a hálózati műveletek [Go környezete](https://blog.golang.org/context).</span><span class="sxs-lookup"><span data-stu-id="cf379-153">The service principal token is covered later, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="init-and-authorization"></a><span data-ttu-id="cf379-154">init() és engedélyezés</span><span class="sxs-lookup"><span data-stu-id="cf379-154">init() and authorization</span></span>

<span data-ttu-id="cf379-155">A kód `init()` metódusa beállítja az engedélyezést.</span><span class="sxs-lookup"><span data-stu-id="cf379-155">The `init()` method for the code sets up authorization.</span></span> <span data-ttu-id="cf379-156">Mivel az engedélyezés a rövid útmutatóban minden másnak az előfeltétele, logikus, hogy az inicializálás része legyen.</span><span class="sxs-lookup"><span data-stu-id="cf379-156">Since authorization is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> 

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

<span data-ttu-id="cf379-157">Ez a kód az engedélyezés két lépését végzi el:</span><span class="sxs-lookup"><span data-stu-id="cf379-157">This code completes two steps for authorization:</span></span>

* <span data-ttu-id="cf379-158">Lekéri a `TenantID` OAuth konfigurációs információit az Azure Active Directoryhoz való illeszkedéssel.</span><span class="sxs-lookup"><span data-stu-id="cf379-158">OAuth configuration information for the `TenantID` is retrieved by interfacing with Azure Active Directory.</span></span> <span data-ttu-id="cf379-159">Az [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) objektum a standard Azure-konfigurációban használt végpontokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cf379-159">The [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) object contains endpoints used in the standard Azure configuration.</span></span>
* <span data-ttu-id="cf379-160">A rendszer meghívja az [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) függvényt.</span><span class="sxs-lookup"><span data-stu-id="cf379-160">The [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) function is called.</span></span> <span data-ttu-id="cf379-161">Ez a függvény átveszi az OAuth-információkat az egyszerű szolgáltatásba való bejelentkezési adatokkal, valamint a használt Azure felügyeleti stílussal együtt.</span><span class="sxs-lookup"><span data-stu-id="cf379-161">This function takes the OAuth information along with the service principal login, as well as which style of Azure management is being used.</span></span> <span data-ttu-id="cf379-162">Hacsak nincsenek adott követelményei, és nem gyakorlott felhasználó, ennek mindig `.ResourceManagerEndpoint` értékűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="cf379-162">Unless you have specific requirements and know what you're doing, this value should always be `.ResourceManagerEndpoint`.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="cf379-163">A main() műveletfolyamata</span><span class="sxs-lookup"><span data-stu-id="cf379-163">Flow of operations in main()</span></span>

<span data-ttu-id="cf379-164">A `main()` függvény egyszerű, csak a műveletek folyamatát jelzi, és hibaellenőrzést végez.</span><span class="sxs-lookup"><span data-stu-id="cf379-164">The `main()` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="cf379-165">A kód által végrehajtott lépések sorrendben a következők:</span><span class="sxs-lookup"><span data-stu-id="cf379-165">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="cf379-166">Az üzembe helyezendő erőforráscsoport létrehozása (`createGroup()`)</span><span class="sxs-lookup"><span data-stu-id="cf379-166">Create the resource group to deploy to (`createGroup()`)</span></span>
* <span data-ttu-id="cf379-167">Az üzemelő példány létrehozása ebben a csoportban (`createDeployment()`)</span><span class="sxs-lookup"><span data-stu-id="cf379-167">Create the deployment within this group (`createDeployment()`)</span></span>
* <span data-ttu-id="cf379-168">Az üzembe helyezett virtuális gép bejelentkezési információinak beszerzése és megjelenítése (`getLogin()`)</span><span class="sxs-lookup"><span data-stu-id="cf379-168">Obtain and display login information for the deployed VM (`getLogin()`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="cf379-169">Az erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="cf379-169">Creating the resource group</span></span>

<span data-ttu-id="cf379-170">A `createGroup()` függvény létrehozza az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="cf379-170">The `createGroup()` function creates the resource group.</span></span> <span data-ttu-id="cf379-171">A hívásfolyam és az argumentumok megtekintésekor láthatja, hogyan vannak rendszerezve a szolgáltatásinterakciók az SDK-ban.</span><span class="sxs-lookup"><span data-stu-id="cf379-171">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="cf379-172">Az Azure szolgáltatással való kommunikáció általános folyamata a következő:</span><span class="sxs-lookup"><span data-stu-id="cf379-172">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="cf379-173">Hozza létre az ügyfelet a `service.NewXClient()` metódussal, ahol az `X` a `service` azon erőforrástípusa, amellyel kommunikálni szeretne.</span><span class="sxs-lookup"><span data-stu-id="cf379-173">Create the client using the `service.NewXClient()` method, where `X` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="cf379-174">Ez a függvény mindig egy előfizetés-azonosítót vesz fel.</span><span class="sxs-lookup"><span data-stu-id="cf379-174">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="cf379-175">Állítsa be az ügyfél engedélyezési metódusát, hogy kommunikálhasson a távoli API-val.</span><span class="sxs-lookup"><span data-stu-id="cf379-175">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="cf379-176">Végezze el a metódushívást a távoli API-nak megfelelő ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="cf379-176">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="cf379-177">A szolgáltatásügyfél-metódusok általában az erőforrás és egy metaadat-objektum nevét veszik fel.</span><span class="sxs-lookup"><span data-stu-id="cf379-177">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="cf379-178">A [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) függvénnyel típusátalakítás végezhető.</span><span class="sxs-lookup"><span data-stu-id="cf379-178">The [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="cf379-179">Az SDK metódusainak paraméterstruktúrái szinte kizárólag mutatókat vesznek, ezért ezek a metódusok a típusátalakítás megkönnyítése miatt vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="cf379-179">The parameters structs for methods of the SDK almost exclusively take pointers, so these methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="cf379-180">A dokumentációban lévő [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) modulban találja az átalakítók teljes listáját és viselkedését.</span><span class="sxs-lookup"><span data-stu-id="cf379-180">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list and behavior of convenience converters.</span></span>

<span data-ttu-id="cf379-181">A `groupsClient.CreateOrUpdate()` művelet egy mutatót ad vissza az erőforráscsoportot jelző adatstruktúrának.</span><span class="sxs-lookup"><span data-stu-id="cf379-181">The `groupsClient.CreateOrUpdate()` operation returns a pointer to a data struct representing the resource group.</span></span> <span data-ttu-id="cf379-182">Az ilyen típusú közvetlen visszatérési érték olyan rövid futásidejű műveletet jelez, amelynek szinkronban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="cf379-182">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="cf379-183">A következő szakasz a hosszú futásidejű műveletek egy példáját mutatja be, illetve azok kezelési módját.</span><span class="sxs-lookup"><span data-stu-id="cf379-183">In the next section, you'll see an example of a long-running operation and how to interact with them.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="cf379-184">Az üzembe helyezés végrehajtása</span><span class="sxs-lookup"><span data-stu-id="cf379-184">Performing the deployment</span></span>

<span data-ttu-id="cf379-185">Amikor létrejött az erőforrásokat tartalmazó csoport, itt az idő az üzembe helyezéshez.</span><span class="sxs-lookup"><span data-stu-id="cf379-185">Once the group to contain its resources is created, it's time to run the deployment.</span></span> <span data-ttu-id="cf379-186">Ez a kód a logika különböző részeinek kihangsúlyozása érdekében kisebb szakaszokra van osztva.</span><span class="sxs-lookup"><span data-stu-id="cf379-186">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>


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

<span data-ttu-id="cf379-187">A `readJSON` betölti az üzembe helyezési fájlokat, amelyek részletei itt kimaradnak.</span><span class="sxs-lookup"><span data-stu-id="cf379-187">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="cf379-188">Ez a függvény visszaad egy `*map[string]interface{}` elemet, amely az erőforrás üzembe helyezési hívása metaadatainak felépítéséhez használt típus.</span><span class="sxs-lookup"><span data-stu-id="cf379-188">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span>

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

<span data-ttu-id="cf379-189">Ez a kód ugyanazt a mintát követi, mint az erőforráscsoport létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="cf379-189">This code follows the same pattern as with creating the resource group.</span></span> <span data-ttu-id="cf379-190">Létrejön egy új ügyfél, amely hitelesíteni tud az Azure-ral, majd a rendszer meghív egy metódust.</span><span class="sxs-lookup"><span data-stu-id="cf379-190">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="cf379-191">A metódusnak ugyanaz a neve is (`CreateOrUpdate`), mint az erőforráscsoportok megfelelő metódusának.</span><span class="sxs-lookup"><span data-stu-id="cf379-191">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="cf379-192">Ez a minta újra és újra látható az SDK-ban.</span><span class="sxs-lookup"><span data-stu-id="cf379-192">This pattern is seen again and again in the SDK.</span></span> <span data-ttu-id="cf379-193">A hasonló munkát végző metódusoknak általában ugyanaz a neve.</span><span class="sxs-lookup"><span data-stu-id="cf379-193">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="cf379-194">A legnagyobb különbség a `deploymentsClient.CreateOrUpdate()` metódus visszaadott értéke.</span><span class="sxs-lookup"><span data-stu-id="cf379-194">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate()` method.</span></span> <span data-ttu-id="cf379-195">Ez az érték egy `Future` objektum, amely a következő a [jövőbeli kialakítási mintát](https://en.wikipedia.org/wiki/Futures_and_promises) követi.</span><span class="sxs-lookup"><span data-stu-id="cf379-195">This value is a `Future` object, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="cf379-196">A jövő az Azure-ban egy hosszú futásidejű műveletet jelez, amelyet érdemes időnként lekérdezni, mialatt más munkát végez.</span><span class="sxs-lookup"><span data-stu-id="cf379-196">Futures represent a long-running operation in Azure that you may want to occasionally poll while performing other work.</span></span>

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="cf379-197">Ebben a példában a legjobb, ha megvárja a művelet befejezését.</span><span class="sxs-lookup"><span data-stu-id="cf379-197">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="cf379-198">Ha a jövőre vár, szüksége van egy [környezeti objektumra](https://blog.golang.org/context) és a jövőbeli objektumot létrehozó ügyfélre.</span><span class="sxs-lookup"><span data-stu-id="cf379-198">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the Future object.</span></span> <span data-ttu-id="cf379-199">Itt két lehetséges hibaforrás van: Az ügyfél oldalán okozott hiba, amikor megpróbálja meghívni a metódust, illetve egy hibaválasz a kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="cf379-199">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="cf379-200">Az utóbbit a rendszer a `deploymentFuture.Result()` hívás részeként adja vissza.</span><span class="sxs-lookup"><span data-stu-id="cf379-200">The latter is returned as part of the `deploymentFuture.Result()` call.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="cf379-201">A hozzárendelt IP-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="cf379-201">Obtaining the assigned IP address</span></span>

<span data-ttu-id="cf379-202">Ha bármit szeretne tenni az újonnan létrehozott virtuális géppel, szüksége van a hozzárendelt IP-címre.</span><span class="sxs-lookup"><span data-stu-id="cf379-202">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="cf379-203">Az IP-címek önmaguk saját külön Azure-erőforrásai, amelyek NIC-erőforrásokhoz kötődnek.</span><span class="sxs-lookup"><span data-stu-id="cf379-203">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="cf379-204">Ez a metódus a paraméterfájlban tárolt adatokra támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="cf379-204">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="cf379-205">A kód közvetlenül le tudta kérdezni a virtuális gépről az NIC-t, lekérdezte az NIC-ről az IP-erőforrást, majd közvetlenül lekérdezte az IP-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="cf379-205">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="cf379-206">Ez egy hosszú függőség- és műveletlánc, ezért költséges.</span><span class="sxs-lookup"><span data-stu-id="cf379-206">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="cf379-207">Mivel a JSON-adat helyiek, ehelyett betölthetők.</span><span class="sxs-lookup"><span data-stu-id="cf379-207">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="cf379-208">A virtuális gép felhasználójának és jelszavának az értékei ugyanígy betölthetőek a JSON-ból.</span><span class="sxs-lookup"><span data-stu-id="cf379-208">The values for the VM user and password are likewise loaded from the JSON.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cf379-209">További lépések</span><span class="sxs-lookup"><span data-stu-id="cf379-209">Next steps</span></span>

<span data-ttu-id="cf379-210">Ebben a rövid útmutatóban egy meglévő sablont helyezett üzembe a Gón keresztül.</span><span class="sxs-lookup"><span data-stu-id="cf379-210">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="cf379-211">Ezután az újonnan létrehozott virtuális gépet SSH-n keresztül csatlakoztatta, hogy biztosan fusson.</span><span class="sxs-lookup"><span data-stu-id="cf379-211">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="cf379-212">Ha többet szeretne megtudni a virtuális gépek Góval való használatáról az Azure-környezetben, lásd: [Azure számítási minták a Góhoz](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) vagy [Azure erőforrás-felügyeleti példák a Góhoz](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="cf379-212">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>
