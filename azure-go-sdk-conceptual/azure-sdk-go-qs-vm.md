---
title: Azure-beli virtuális gép üzembe helyezése a Góról
description: Helyezzen üzembe egy virtuális gépet a Go nyelvhez készült Azure SDK-val.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: quickstart
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: a7970be0857fd414d776241b033af0c23457790c
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059135"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="15c14-103">Gyors útmutató: Azure-beli virtuális gép üzembe helyezése sablonból a Góhoz készült Azure SDK-val</span><span class="sxs-lookup"><span data-stu-id="15c14-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="15c14-104">Ez a rövid útmutató bemutatja, hogyan helyezhet üzembe erőforrásokat Azure Resource Manager-sablonból a Góhoz készült Azure SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="15c14-104">This quickstart shows you how to deploy resources from an Azure Resource Manager template, using the Azure SDK for Go.</span></span> <span data-ttu-id="15c14-105">A sablonok az összes erőforrás pillanatképei egy [Azure-erőforráscsoportban](/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="15c14-105">Templates are snapshots of all of the resources within an [Azure resource group](/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="15c14-106">Menet közben megismerheti az SDK működését és konvencióit.</span><span class="sxs-lookup"><span data-stu-id="15c14-106">Along the way, you'll become familiar with the functionality and conventions of the SDK.</span></span>

<span data-ttu-id="15c14-107">A rövid útmutató végén olyan futó virtuális gépe lesz, amelybe felhasználónévvel és jelszóval tud bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="15c14-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

> [!NOTE]
> <span data-ttu-id="15c14-108">Egy virtuális gép a Góban Resource Manager-sablon nélkül való létrehozásának menetét egy [imperatív minta](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) ismerteti, amely bemutatja, hogyan állíthatja össze és konfigurálhatja az összes virtuálisgép-erőforrást az SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="15c14-108">To see the creation of a VM in Go without the use of a Resource Manager template, there is an [imperative sample](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) demonstrating how to build and configure all VM resources with the SDK.</span></span> <span data-ttu-id="15c14-109">A minta használata ebben a példában lehetővé teszi az SDK konvencióira való koncentrálást az Azure szolgáltatásarchitektúrájának részletekbe menő ismertetése nélkül.</span><span class="sxs-lookup"><span data-stu-id="15c14-109">Using a template in this sample allows a focus on SDK conventions without getting into too many details about Azure service architecture.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="15c14-110">Ha az Azure CLI helyi telepítését használja, ehhez a rövid útmutatóhoz a __CLI 2.0.28-as__ vagy újabb verziójára van szükség.</span><span class="sxs-lookup"><span data-stu-id="15c14-110">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="15c14-111">Futtassa az `az --version` parancsot annak ellenőrzéséhez, hogy a CLI telepítés megfelel-e ennek a követelménynek.</span><span class="sxs-lookup"><span data-stu-id="15c14-111">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="15c14-112">Ha telepíteni vagy frissíteni szeretne, olvassa el [az Azure CLI telepítését](/cli/azure/install-azure-cli) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="15c14-112">If you need to install or upgrade, see [Install the Azure CLI](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="15c14-113">A Góhoz készült Azure SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="15c14-113">Install the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="15c14-114">Egyszerű szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="15c14-114">Create a service principal</span></span>

<span data-ttu-id="15c14-115">Ha nem interaktív módon szeretne bejelentkezni az Azure-ba egy alkalmazással, egy szolgáltatásnévre lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="15c14-115">To sign in non-interactively to Azure with an application, you need a service principal.</span></span> <span data-ttu-id="15c14-116">Az egyszerű szolgáltatások a szerepköralapú hozzáférés-vezérlés (RBAC) részei, amely egyedi felhasználói azonosítót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="15c14-116">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="15c14-117">Ha új egyszerű szolgáltatást szeretne létrehozni a parancssori felülettel, futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="15c14-117">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --sdk-auth > quickstart.auth
```

<span data-ttu-id="15c14-118">Az `AZURE_AUTH_LOCATION` környezeti változónál állítsa be a fájl teljes elérési útját.</span><span class="sxs-lookup"><span data-stu-id="15c14-118">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="15c14-119">Az SDK ezután megkeresi és beolvassa a hitelesítő adatokat közvetlenül a fájlból, anélkül, hogy módosítania kellene valamit, vagy rögzítenie kellene a szolgáltatásnév adatait.</span><span class="sxs-lookup"><span data-stu-id="15c14-119">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="15c14-120">A kód letöltése</span><span class="sxs-lookup"><span data-stu-id="15c14-120">Get the code</span></span>

<span data-ttu-id="15c14-121">A rövid útmutató kódját és az összes függőségét letöltheti a következővel: `go get`.</span><span class="sxs-lookup"><span data-stu-id="15c14-121">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="15c14-122">Nem kell módosítania a forráskódot, ha az `AZURE_AUTH_LOCATION` változó megfelelően van beállítva.</span><span class="sxs-lookup"><span data-stu-id="15c14-122">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="15c14-123">Amikor a program fut, ebből a változóból tölti be a szükséges hitelesítési adatokat.</span><span class="sxs-lookup"><span data-stu-id="15c14-123">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="15c14-124">A kód futtatása</span><span class="sxs-lookup"><span data-stu-id="15c14-124">Running the code</span></span>

<span data-ttu-id="15c14-125">Futtassa a rövid útmutatót a `go run` paranccsal.</span><span class="sxs-lookup"><span data-stu-id="15c14-125">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="15c14-126">Ha az üzembe helyezés sikeres, megjelenik egy üzenet a felhasználónévvel, az IP-címmel és a jelszóval, amelyekkel bejelentkezhet az újonnan létrehozott virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="15c14-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="15c14-127">Lépjen be SSH-val a gépre annak ellenőrzéséhez, hogy működik-e.</span><span class="sxs-lookup"><span data-stu-id="15c14-127">SSH into this machine to see if it's up and running.</span></span> 

## <a name="cleaning-up"></a><span data-ttu-id="15c14-128">Takarítás</span><span class="sxs-lookup"><span data-stu-id="15c14-128">Cleaning up</span></span>

<span data-ttu-id="15c14-129">A rövid útmutató során létrehozott erőforrásokat az erőforráscsoport törlésével, a parancssori felület használatával távolíthatja el.</span><span class="sxs-lookup"><span data-stu-id="15c14-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

<span data-ttu-id="15c14-130">Törölje a létrehozott szolgáltatásnevet is.</span><span class="sxs-lookup"><span data-stu-id="15c14-130">Also delete the service principal that was created.</span></span> <span data-ttu-id="15c14-131">A `quickstart.auth` fájlban található egy JSON-kulcs a következőhöz: `clientId`.</span><span class="sxs-lookup"><span data-stu-id="15c14-131">In the `quickstart.auth` file, there's a JSON key for `clientId`.</span></span> <span data-ttu-id="15c14-132">Másolja ezt az értéket a `CLIENT_ID_VALUE`-környezet változójához, majd futtassa a következő Azure CLI-parancsot:</span><span class="sxs-lookup"><span data-stu-id="15c14-132">Copy this value to the `CLIENT_ID_VALUE` environment variable and run the following Azure CLI command:</span></span>

```azurecli-interactive
az ad sp delete --id ${CLIENT_ID_VALUE}
```

<span data-ttu-id="15c14-133">Ahol megadja a `CLIENT_ID_VALUE` értékét a következőből: `quickstart.auth`.</span><span class="sxs-lookup"><span data-stu-id="15c14-133">Where you supply the value for `CLIENT_ID_VALUE` from `quickstart.auth`.</span></span>

> [!WARNING]
> <span data-ttu-id="15c14-134">Ha nem törli a szolgáltatásnevet az alkalmazásból, akkor az aktív marad az Azure Active Directory-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="15c14-134">Failing to delete the service principal for this application leaves it active in your Azure Active Directory tenant.</span></span>
> <span data-ttu-id="15c14-135">Bár a szolgáltatásnév neve és jelszava is UUID-ként jön létre, fontos, hogy a biztonság fenntartása érdekében törölje a nem használt szolgáltatásneveket és Azure Active Directory-alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="15c14-135">While both the name and password for the service principal are generated as UUIDs, make sure that you follow good security practices by deleting any unused service principals and Azure Active Directory Applications.</span></span>

## <a name="code-in-depth"></a><span data-ttu-id="15c14-136">A kód részletei</span><span class="sxs-lookup"><span data-stu-id="15c14-136">Code in depth</span></span>

<span data-ttu-id="15c14-137">A rövid útmutató kódja változócsoportokra és több kis függvényre van felosztva, amelyek leírásait itt találja.</span><span class="sxs-lookup"><span data-stu-id="15c14-137">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="15c14-138">Változók, állandók és típusok</span><span class="sxs-lookup"><span data-stu-id="15c14-138">Variables, constants, and types</span></span>

<span data-ttu-id="15c14-139">Mivel a rövid útmutató önálló, globális állandókat és változókat használ.</span><span class="sxs-lookup"><span data-stu-id="15c14-139">Since quickstart is self-contained, it uses global constants and variables.</span></span>

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

<span data-ttu-id="15c14-140">Meg vannak határozva a létrehozott erőforrások neveit megadó értékek.</span><span class="sxs-lookup"><span data-stu-id="15c14-140">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="15c14-141">Itt a hely is meg van adva, amely módosítható, hogy lássa, hogyan működnek az üzemelő példányok más adatközpontokban.</span><span class="sxs-lookup"><span data-stu-id="15c14-141">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="15c14-142">Nem minden adatközponton érhető el az összes szükséges erőforrás.</span><span class="sxs-lookup"><span data-stu-id="15c14-142">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="15c14-143">A `clientInfo` típus magában foglalja az összes, hitelesítési fájlból betöltött információt az ügyfelek SDK-ban való beállításához, valamint a virtuális gép jelszavának beállításához.</span><span class="sxs-lookup"><span data-stu-id="15c14-143">The `clientInfo` type holds the information loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="15c14-144">A `templateFile` és `parametersFile` állandó az üzembe helyezéshez szükséges fájlokra mutat.</span><span class="sxs-lookup"><span data-stu-id="15c14-144">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="15c14-145">Az `authorizer` hitelesítéshez való konfigurálását a Go SDK fogja elvégezni. A `ctx` változó a hálózati műveletek [Go környezete](https://blog.golang.org/context).</span><span class="sxs-lookup"><span data-stu-id="15c14-145">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="15c14-146">Hitelesítés és inicializálás</span><span class="sxs-lookup"><span data-stu-id="15c14-146">Authentication and initialization</span></span>

<span data-ttu-id="15c14-147">Az `init` függvény állítja be a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="15c14-147">The `init` function sets up authentication.</span></span> <span data-ttu-id="15c14-148">Mivel a hitelesítés a rövid útmutatóban minden másnak az előfeltétele, logikus, hogy az inicializálás része legyen.</span><span class="sxs-lookup"><span data-stu-id="15c14-148">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="15c14-149">A függvény ezen kívül néhány szükséges adatot is betölt a hitelesítési fájlból az ügyfelek és a virtuális gép konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="15c14-149">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

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

<span data-ttu-id="15c14-150">A rendszer először meghívja az [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) függvényt, hogy az betöltse az `AZURE_AUTH_LOCATION` helyen található fájl hitelesítési adatait.</span><span class="sxs-lookup"><span data-stu-id="15c14-150">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="15c14-151">Ezután a `readJSON` függvény (itt ki van hagyva) manuálisan betölti ezt a fájlt a program többi részének futtatásához szükséges két érték lekéréséhez. Ezek egyike az ügyfél előfizetési azonosítója, a másik pedig a szolgáltatásnév titkos kulcsa, amelyet a virtuális gép jelszavához is használni lehet.</span><span class="sxs-lookup"><span data-stu-id="15c14-151">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="15c14-152">A rövid útmutató egyszerűsége érdekében a szolgáltatásnév jelszava ugyanaz, mint eddig.</span><span class="sxs-lookup"><span data-stu-id="15c14-152">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="15c14-153">Éles környezetben ügyeljen arra, hogy __soha__ ne használja fel többször azt a jelszót, amellyel hozzáférhet az Azure-erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="15c14-153">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="15c14-154">A main() műveletfolyamata</span><span class="sxs-lookup"><span data-stu-id="15c14-154">Flow of operations in main()</span></span>

<span data-ttu-id="15c14-155">A `main` függvény egyszerű, csak a műveletek folyamatát jelzi, és hibaellenőrzést végez.</span><span class="sxs-lookup"><span data-stu-id="15c14-155">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="15c14-156">A kód által végrehajtott lépések sorrendben a következők:</span><span class="sxs-lookup"><span data-stu-id="15c14-156">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="15c14-157">Az üzembe helyezendő erőforráscsoport létrehozása (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="15c14-157">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="15c14-158">Az üzemelő példány létrehozása ebben a csoportban (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="15c14-158">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="15c14-159">Az üzembe helyezett virtuális gép bejelentkezési információinak beszerzése és megjelenítése (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="15c14-159">Get and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="create-the-resource-group"></a><span data-ttu-id="15c14-160">Az erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="15c14-160">Create the resource group</span></span>

<span data-ttu-id="15c14-161">A `createGroup` függvény létrehozza az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="15c14-161">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="15c14-162">A hívásfolyam és az argumentumok megtekintésekor láthatja, hogyan vannak rendszerezve a szolgáltatásinterakciók az SDK-ban.</span><span class="sxs-lookup"><span data-stu-id="15c14-162">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="15c14-163">Az Azure szolgáltatással való kommunikáció általános folyamata a következő:</span><span class="sxs-lookup"><span data-stu-id="15c14-163">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="15c14-164">Hozza létre az ügyfelet a `service.New*Client()` metódussal, ahol az `*` a `service` azon erőforrástípusa, amellyel kommunikálni szeretne.</span><span class="sxs-lookup"><span data-stu-id="15c14-164">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="15c14-165">Ez a függvény mindig egy előfizetés-azonosítót vesz fel.</span><span class="sxs-lookup"><span data-stu-id="15c14-165">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="15c14-166">Állítsa be az ügyfél engedélyezési metódusát, hogy kommunikálhasson a távoli API-val.</span><span class="sxs-lookup"><span data-stu-id="15c14-166">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="15c14-167">Végezze el a metódushívást a távoli API-nak megfelelő ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="15c14-167">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="15c14-168">A szolgáltatásügyfél-metódusok általában az erőforrás és egy metaadat-objektum nevét veszik fel.</span><span class="sxs-lookup"><span data-stu-id="15c14-168">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="15c14-169">A [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) függvénnyel típusátalakítás végezhető.</span><span class="sxs-lookup"><span data-stu-id="15c14-169">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="15c14-170">Az SDK metódusainak paraméterei szinte kizárólag mutatókat vesznek, ezért az egyszerűsített metódusok a típusátalakítás megkönnyítése miatt vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="15c14-170">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="15c14-171">A dokumentációban lévő [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) modulban találja az átalakítók teljes listáját és viselkedését.</span><span class="sxs-lookup"><span data-stu-id="15c14-171">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="15c14-172">A `groupsClient.CreateOrUpdate` metódus egy mutatót ad vissza az erőforráscsoportot jelölő adattípusnak.</span><span class="sxs-lookup"><span data-stu-id="15c14-172">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="15c14-173">Az ilyen típusú közvetlen visszatérési érték olyan rövid futásidejű műveletet jelez, amelynek szinkronban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="15c14-173">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="15c14-174">A következő szakasz a hosszú futásidejű műveletek egy példáját, illetve annak a kezelési módját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="15c14-174">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="perform-the-deployment"></a><span data-ttu-id="15c14-175">Az üzembe helyezés végrehajtása</span><span class="sxs-lookup"><span data-stu-id="15c14-175">Perform the deployment</span></span>

<span data-ttu-id="15c14-176">Amikor létrejött az erőforráscsoport, itt az idő az üzembe helyezéshez.</span><span class="sxs-lookup"><span data-stu-id="15c14-176">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="15c14-177">Ez a kód a logika különböző részeinek kihangsúlyozása érdekében kisebb szakaszokra van osztva.</span><span class="sxs-lookup"><span data-stu-id="15c14-177">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

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

<span data-ttu-id="15c14-178">A `readJSON` betölti az üzembe helyezési fájlokat, amelyek részletei itt kimaradnak.</span><span class="sxs-lookup"><span data-stu-id="15c14-178">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="15c14-179">Ez a függvény visszaad egy `*map[string]interface{}` elemet, amely az erőforrás üzembe helyezési hívása metaadatainak felépítéséhez használt típus.</span><span class="sxs-lookup"><span data-stu-id="15c14-179">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="15c14-180">A virtuális gép jelszavát is manuálisan kell beállítani az üzembehelyezési paramétereknél.</span><span class="sxs-lookup"><span data-stu-id="15c14-180">The VM's password is also set manually on the deployment parameters.</span></span>

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

<span data-ttu-id="15c14-181">Ez a kód ugyanazt a mintát követi, mint az erőforráscsoport létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="15c14-181">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="15c14-182">Létrejön egy új ügyfél, amely hitelesíteni tud az Azure-ral, majd a rendszer meghív egy metódust.</span><span class="sxs-lookup"><span data-stu-id="15c14-182">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span>
<span data-ttu-id="15c14-183">A metódusnak ugyanaz a neve is (`CreateOrUpdate`), mint az erőforráscsoportok megfelelő metódusának.</span><span class="sxs-lookup"><span data-stu-id="15c14-183">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="15c14-184">Ez a minta több helyen is látható az SDK-ban.</span><span class="sxs-lookup"><span data-stu-id="15c14-184">This pattern is seen throughout the SDK.</span></span>
<span data-ttu-id="15c14-185">A hasonló munkát végző metódusoknak általában ugyanaz a neve.</span><span class="sxs-lookup"><span data-stu-id="15c14-185">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="15c14-186">A legnagyobb különbség a `deploymentsClient.CreateOrUpdate` metódus visszaadott értéke.</span><span class="sxs-lookup"><span data-stu-id="15c14-186">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="15c14-187">Az érték típusa [Jövőbeli](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), amely a [jövőbeli kialakítási mintát](https://en.wikipedia.org/wiki/Futures_and_promises) követi.</span><span class="sxs-lookup"><span data-stu-id="15c14-187">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="15c14-188">A jövő az Azure-ban egy hosszú futásidejű műveletet jelez, amelyet a művelet befejezésekor lekérdezhet, megszakíthat vagy letilthat.</span><span class="sxs-lookup"><span data-stu-id="15c14-188">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="15c14-189">Ebben a példában a legjobb, ha megvárja a művelet befejezését.</span><span class="sxs-lookup"><span data-stu-id="15c14-189">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="15c14-190">Ha a jövőre vár, szüksége van egy [környezeti objektumra](https://blog.golang.org/context) és a `Future` létrehozó ügyfelére.</span><span class="sxs-lookup"><span data-stu-id="15c14-190">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="15c14-191">Itt két lehetséges hibaforrás van: Az ügyfél oldalán okozott hiba, amikor megpróbálja meghívni a metódust, illetve egy hibaválasz a kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="15c14-191">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="15c14-192">Az utóbbit a rendszer a `deploymentFuture.Result` hívás részeként adja vissza.</span><span class="sxs-lookup"><span data-stu-id="15c14-192">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

### <a name="get-the-assigned-ip-address"></a><span data-ttu-id="15c14-193">A hozzárendelt IP-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="15c14-193">Get the assigned IP address</span></span>

<span data-ttu-id="15c14-194">Ha bármit szeretne tenni az újonnan létrehozott virtuális géppel, szüksége van a hozzárendelt IP-címre.</span><span class="sxs-lookup"><span data-stu-id="15c14-194">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="15c14-195">Az IP-címek önmaguk saját külön Azure-erőforrásai, amelyek NIC-erőforrásokhoz kötődnek.</span><span class="sxs-lookup"><span data-stu-id="15c14-195">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="15c14-196">Ez a metódus a paraméterfájlban tárolt adatokra támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="15c14-196">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="15c14-197">A kód közvetlenül le tudta kérdezni a virtuális gépről az NIC-t, lekérdezte az NIC-ről az IP-erőforrást, majd közvetlenül lekérdezte az IP-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="15c14-197">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="15c14-198">Ez egy hosszú függőség- és műveletlánc, ezért költséges.</span><span class="sxs-lookup"><span data-stu-id="15c14-198">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="15c14-199">Mivel a JSON-adat helyiek, ehelyett betölthetők.</span><span class="sxs-lookup"><span data-stu-id="15c14-199">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="15c14-200">A virtuális gép felhasználójának értéke szintén betölthető a JSON-ból.</span><span class="sxs-lookup"><span data-stu-id="15c14-200">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="15c14-201">A virtuális gép jelszava korábban már be lett töltve a hitelesítési fájlból.</span><span class="sxs-lookup"><span data-stu-id="15c14-201">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="15c14-202">További lépések</span><span class="sxs-lookup"><span data-stu-id="15c14-202">Next steps</span></span>

<span data-ttu-id="15c14-203">Ebben a rövid útmutatóban egy meglévő sablont helyezett üzembe a Gón keresztül.</span><span class="sxs-lookup"><span data-stu-id="15c14-203">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="15c14-204">Ezután az újonnan létrehozott virtuális gépet SSH-n keresztül csatlakoztatta.</span><span class="sxs-lookup"><span data-stu-id="15c14-204">Then you connected to the newly created VM via SSH.</span></span>

<span data-ttu-id="15c14-205">Ha többet szeretne megtudni a virtuális gépek Góval való használatáról az Azure-környezetben, lásd: [Azure számítási minták a Góhoz](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) vagy [Azure erőforrás-felügyeleti példák a Góhoz](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="15c14-205">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="15c14-206">Ha többet szeretne megtudni az SDK-ban elérhető hitelesítési módszerekről, és az általuk támogatott hitelesítési típusokról, lásd: [Hitelesítés a Góhoz készült Azure SDK-val](azure-sdk-go-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="15c14-206">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>
