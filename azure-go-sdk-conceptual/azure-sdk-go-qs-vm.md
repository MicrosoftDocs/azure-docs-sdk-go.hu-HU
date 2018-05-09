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
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="bd60b-103">Gyors útmutató: Azure-beli virtuális gép üzembe helyezése sablonból a Góhoz készült Azure SDK-val</span><span class="sxs-lookup"><span data-stu-id="bd60b-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="bd60b-104">Ez a rövid útmutató az erőforrások sablonból való üzembe helyezésére összpontosít a Góhoz készült Azure SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="bd60b-104">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="bd60b-105">A sablonok az összes erőforrás pillanatképei egy [Azure-erőforráscsoportban](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="bd60b-105">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="bd60b-106">Menet közben megismerheti az SDK működését és konvencióit, mialatt hasznos feladatot végez.</span><span class="sxs-lookup"><span data-stu-id="bd60b-106">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="bd60b-107">A rövid útmutató végén olyan futó virtuális gépe lesz, amelybe felhasználónévvel és jelszóval tud bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="bd60b-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="bd60b-108">Ha az Azure CLI helyi telepítését használja, ehhez a rövid útmutatóhoz a __CLI 2.0.28-as__ vagy újabb verziójára van szükség.</span><span class="sxs-lookup"><span data-stu-id="bd60b-108">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="bd60b-109">Futtassa az `az --version` parancsot annak ellenőrzéséhez, hogy a CLI telepítés megfelel-e ennek a követelménynek.</span><span class="sxs-lookup"><span data-stu-id="bd60b-109">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="bd60b-110">Ha telepítenie vagy frissítenie kell, lásd: [Az Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="bd60b-110">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="bd60b-111">A Góhoz készült Azure SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="bd60b-111">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="bd60b-112">Egyszerű szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="bd60b-112">Create a service principal</span></span>


<span data-ttu-id="bd60b-113">Ha nem interaktív módon szeretne bejelentkezni egy alkalmazással, egy egyszerű szolgáltatásra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="bd60b-113">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="bd60b-114">Az egyszerű szolgáltatások a szerepköralapú hozzáférés-vezérlés (RBAC) részei, amely egyedi felhasználói azonosítót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="bd60b-114">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="bd60b-115">Ha új egyszerű szolgáltatást szeretne létrehozni a parancssori felülettel, futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="bd60b-115">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart --sdk-auth > quickstart.auth
```

<span data-ttu-id="bd60b-116">Az `AZURE_AUTH_LOCATION` környezeti változónál állítsa be a fájl teljes elérési útját.</span><span class="sxs-lookup"><span data-stu-id="bd60b-116">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="bd60b-117">Az SDK ezután megkeresi és beolvassa a hitelesítő adatokat közvetlenül a fájlból, anélkül, hogy módosítania kellene valamit, vagy rögzítenie kellene a szolgáltatásnév adatait.</span><span class="sxs-lookup"><span data-stu-id="bd60b-117">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="bd60b-118">A kód letöltése</span><span class="sxs-lookup"><span data-stu-id="bd60b-118">Get the code</span></span>

<span data-ttu-id="bd60b-119">A rövid útmutató kódját és az összes függőségét letöltheti a következővel: `go get`.</span><span class="sxs-lookup"><span data-stu-id="bd60b-119">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="bd60b-120">Nem kell módosítania a forráskódot, ha az `AZURE_AUTH_LOCATION` változó megfelelően van beállítva.</span><span class="sxs-lookup"><span data-stu-id="bd60b-120">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="bd60b-121">Amikor a program fut, ebből a változóból tölti be a szükséges hitelesítési adatokat.</span><span class="sxs-lookup"><span data-stu-id="bd60b-121">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="bd60b-122">A kód futtatása</span><span class="sxs-lookup"><span data-stu-id="bd60b-122">Running the code</span></span>

<span data-ttu-id="bd60b-123">Futtassa a rövid útmutatót a `go run` paranccsal.</span><span class="sxs-lookup"><span data-stu-id="bd60b-123">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="bd60b-124">Ha hiba van az üzemelő példányban, megjelenik egy üzenet, amely jelzi, hogy hiba történt, de ez gyakran nem elég részletes.</span><span class="sxs-lookup"><span data-stu-id="bd60b-124">If there is a failure in the deployment, you get a message indicating that there was an issue, but it may not include enough detail.</span></span> <span data-ttu-id="bd60b-125">Az Azure CLI használatával kérdezze le az üzemelő példány hibájának összes részletét a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="bd60b-125">Using the Azure CLI, get the full details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="bd60b-126">Ha az üzembe helyezés sikeres, megjelenik egy üzenet a felhasználónévvel, az IP-címmel és a jelszóval, amelyekkel bejelentkezhet az újonnan létrehozott virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="bd60b-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="bd60b-127">Lépjen be SSH-val a gépre annak ellenőrzéséhez, hogy működik-e.</span><span class="sxs-lookup"><span data-stu-id="bd60b-127">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="bd60b-128">Takarítás</span><span class="sxs-lookup"><span data-stu-id="bd60b-128">Cleaning up</span></span>

<span data-ttu-id="bd60b-129">A rövid útmutató során létrehozott erőforrásokat az erőforráscsoport törlésével, a parancssori felület használatával távolíthatja el.</span><span class="sxs-lookup"><span data-stu-id="bd60b-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="bd60b-130">A kód részletei</span><span class="sxs-lookup"><span data-stu-id="bd60b-130">Code in depth</span></span>

<span data-ttu-id="bd60b-131">A rövid útmutató kódja változócsoportokra és több kis függvényre van felosztva, amelyek leírásait itt találja.</span><span class="sxs-lookup"><span data-stu-id="bd60b-131">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="bd60b-132">Változók, állandók és típusok</span><span class="sxs-lookup"><span data-stu-id="bd60b-132">Variables, constants, and types</span></span>

<span data-ttu-id="bd60b-133">Mivel a rövid útmutató önálló, globális állandókat és változókat használ.</span><span class="sxs-lookup"><span data-stu-id="bd60b-133">Since quickstart is self-contained, it uses global constants and variables.</span></span>

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

<span data-ttu-id="bd60b-134">Meg vannak határozva a létrehozott erőforrások neveit megadó értékek.</span><span class="sxs-lookup"><span data-stu-id="bd60b-134">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="bd60b-135">Itt a hely is meg van adva, amely módosítható, hogy lássa, hogyan működnek az üzemelő példányok más adatközpontokban.</span><span class="sxs-lookup"><span data-stu-id="bd60b-135">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="bd60b-136">Nem minden adatközponton érhető el az összes szükséges erőforrás.</span><span class="sxs-lookup"><span data-stu-id="bd60b-136">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="bd60b-137">A `clientInfo` típus úgy van meghatározva, hogy magába foglalja az összes információt, amelyet külön kell betölteni a hitelesítési fájlból az ügyfelek SDK-ban való beállításához, valamint a virtuális gép jelszavának beállításához.</span><span class="sxs-lookup"><span data-stu-id="bd60b-137">The `clientInfo` type is declared to encapsulate all of the information that must be independently loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="bd60b-138">A `templateFile` és `parametersFile` állandó az üzembe helyezéshez szükséges fájlokra mutat.</span><span class="sxs-lookup"><span data-stu-id="bd60b-138">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="bd60b-139">Az `authorizer` hitelesítéshez való konfigurálását a Go SDK fogja elvégezni. A `ctx` változó a hálózati műveletek [Go környezete](https://blog.golang.org/context).</span><span class="sxs-lookup"><span data-stu-id="bd60b-139">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="bd60b-140">Hitelesítés és inicializálás</span><span class="sxs-lookup"><span data-stu-id="bd60b-140">Authentication and initialization</span></span>

<span data-ttu-id="bd60b-141">Az `init` függvény állítja be a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="bd60b-141">The `init` function sets up authentication.</span></span> <span data-ttu-id="bd60b-142">Mivel a hitelesítés a rövid útmutatóban minden másnak az előfeltétele, logikus, hogy az inicializálás része legyen.</span><span class="sxs-lookup"><span data-stu-id="bd60b-142">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="bd60b-143">A függvény ezen kívül néhány szükséges adatot is betölt a hitelesítési fájlból az ügyfelek és a virtuális gép konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="bd60b-143">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

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

<span data-ttu-id="bd60b-144">A rendszer először meghívja az [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) függvényt, hogy az betöltse az `AZURE_AUTH_LOCATION` helyen található fájl hitelesítési adatait.</span><span class="sxs-lookup"><span data-stu-id="bd60b-144">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="bd60b-145">Ezután a `readJSON` függvény (itt ki van hagyva) manuálisan betölti ezt a fájlt a program többi részének futtatásához szükséges két érték lekéréséhez. Ezek egyike az ügyfél előfizetési azonosítója, a másik pedig a szolgáltatásnév titkos kulcsa, amelyet a virtuális gép jelszavához is használni lehet.</span><span class="sxs-lookup"><span data-stu-id="bd60b-145">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="bd60b-146">A rövid útmutató egyszerűsége érdekében a szolgáltatásnév jelszava ugyanaz, mint eddig.</span><span class="sxs-lookup"><span data-stu-id="bd60b-146">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="bd60b-147">Éles környezetben ügyeljen arra, hogy __soha__ ne használja fel többször azt a jelszót, amellyel hozzáférhet az Azure-erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="bd60b-147">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="bd60b-148">A main() műveletfolyamata</span><span class="sxs-lookup"><span data-stu-id="bd60b-148">Flow of operations in main()</span></span>

<span data-ttu-id="bd60b-149">A `main` függvény egyszerű, csak a műveletek folyamatát jelzi, és hibaellenőrzést végez.</span><span class="sxs-lookup"><span data-stu-id="bd60b-149">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="bd60b-150">A kód által végrehajtott lépések sorrendben a következők:</span><span class="sxs-lookup"><span data-stu-id="bd60b-150">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="bd60b-151">Az üzembe helyezendő erőforráscsoport létrehozása (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="bd60b-151">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="bd60b-152">Az üzemelő példány létrehozása ebben a csoportban (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="bd60b-152">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="bd60b-153">Az üzembe helyezett virtuális gép bejelentkezési információinak beszerzése és megjelenítése (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="bd60b-153">Obtain and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="bd60b-154">Az erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="bd60b-154">Creating the resource group</span></span>

<span data-ttu-id="bd60b-155">A `createGroup` függvény létrehozza az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="bd60b-155">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="bd60b-156">A hívásfolyam és az argumentumok megtekintésekor láthatja, hogyan vannak rendszerezve a szolgáltatásinterakciók az SDK-ban.</span><span class="sxs-lookup"><span data-stu-id="bd60b-156">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="bd60b-157">Az Azure szolgáltatással való kommunikáció általános folyamata a következő:</span><span class="sxs-lookup"><span data-stu-id="bd60b-157">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="bd60b-158">Hozza létre az ügyfelet a `service.New*Client()` metódussal, ahol az `*` a `service` azon erőforrástípusa, amellyel kommunikálni szeretne.</span><span class="sxs-lookup"><span data-stu-id="bd60b-158">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="bd60b-159">Ez a függvény mindig egy előfizetés-azonosítót vesz fel.</span><span class="sxs-lookup"><span data-stu-id="bd60b-159">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="bd60b-160">Állítsa be az ügyfél engedélyezési metódusát, hogy kommunikálhasson a távoli API-val.</span><span class="sxs-lookup"><span data-stu-id="bd60b-160">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="bd60b-161">Végezze el a metódushívást a távoli API-nak megfelelő ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="bd60b-161">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="bd60b-162">A szolgáltatásügyfél-metódusok általában az erőforrás és egy metaadat-objektum nevét veszik fel.</span><span class="sxs-lookup"><span data-stu-id="bd60b-162">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="bd60b-163">A [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) függvénnyel típusátalakítás végezhető.</span><span class="sxs-lookup"><span data-stu-id="bd60b-163">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="bd60b-164">Az SDK metódusainak paraméterei szinte kizárólag mutatókat vesznek, ezért az egyszerűsített metódusok a típusátalakítás megkönnyítése miatt vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="bd60b-164">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="bd60b-165">A dokumentációban lévő [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) modulban találja az átalakítók teljes listáját és viselkedését.</span><span class="sxs-lookup"><span data-stu-id="bd60b-165">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="bd60b-166">A `groupsClient.CreateOrUpdate` metódus egy mutatót ad vissza az erőforráscsoportot jelölő adattípusnak.</span><span class="sxs-lookup"><span data-stu-id="bd60b-166">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="bd60b-167">Az ilyen típusú közvetlen visszatérési érték olyan rövid futásidejű műveletet jelez, amelynek szinkronban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bd60b-167">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="bd60b-168">A következő szakasz a hosszú futásidejű műveletek egy példáját, illetve annak a kezelési módját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="bd60b-168">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="bd60b-169">Az üzembe helyezés végrehajtása</span><span class="sxs-lookup"><span data-stu-id="bd60b-169">Performing the deployment</span></span>

<span data-ttu-id="bd60b-170">Amikor létrejött az erőforráscsoport, itt az idő az üzembe helyezéshez.</span><span class="sxs-lookup"><span data-stu-id="bd60b-170">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="bd60b-171">Ez a kód a logika különböző részeinek kihangsúlyozása érdekében kisebb szakaszokra van osztva.</span><span class="sxs-lookup"><span data-stu-id="bd60b-171">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

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

<span data-ttu-id="bd60b-172">A `readJSON` betölti az üzembe helyezési fájlokat, amelyek részletei itt kimaradnak.</span><span class="sxs-lookup"><span data-stu-id="bd60b-172">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="bd60b-173">Ez a függvény visszaad egy `*map[string]interface{}` elemet, amely az erőforrás üzembe helyezési hívása metaadatainak felépítéséhez használt típus.</span><span class="sxs-lookup"><span data-stu-id="bd60b-173">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="bd60b-174">A virtuális gép jelszavát is manuálisan kell beállítani az üzembehelyezési paramétereknél.</span><span class="sxs-lookup"><span data-stu-id="bd60b-174">The VM's password is also set manually on the deployment parameters.</span></span>

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

<span data-ttu-id="bd60b-175">Ez a kód ugyanazt a mintát követi, mint az erőforráscsoport létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="bd60b-175">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="bd60b-176">Létrejön egy új ügyfél, amely hitelesíteni tud az Azure-ral, majd a rendszer meghív egy metódust.</span><span class="sxs-lookup"><span data-stu-id="bd60b-176">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="bd60b-177">A metódusnak ugyanaz a neve is (`CreateOrUpdate`), mint az erőforráscsoportok megfelelő metódusának.</span><span class="sxs-lookup"><span data-stu-id="bd60b-177">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="bd60b-178">Ez a minta több helyen is látható az SDK-ban.</span><span class="sxs-lookup"><span data-stu-id="bd60b-178">This pattern is seen throughout the SDK.</span></span> <span data-ttu-id="bd60b-179">A hasonló munkát végző metódusoknak általában ugyanaz a neve.</span><span class="sxs-lookup"><span data-stu-id="bd60b-179">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="bd60b-180">A legnagyobb különbség a `deploymentsClient.CreateOrUpdate` metódus visszaadott értéke.</span><span class="sxs-lookup"><span data-stu-id="bd60b-180">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="bd60b-181">Az érték típusa [Jövőbeli](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), amely a [jövőbeli kialakítási mintát](https://en.wikipedia.org/wiki/Futures_and_promises) követi.</span><span class="sxs-lookup"><span data-stu-id="bd60b-181">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="bd60b-182">A jövő az Azure-ban egy hosszú futásidejű műveletet jelez, amelyet a művelet befejezésekor lekérdezhet, megszakíthat vagy letilthat.</span><span class="sxs-lookup"><span data-stu-id="bd60b-182">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

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

<span data-ttu-id="bd60b-183">Ebben a példában a legjobb, ha megvárja a művelet befejezését.</span><span class="sxs-lookup"><span data-stu-id="bd60b-183">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="bd60b-184">Ha a jövőre vár, szüksége van egy [környezeti objektumra](https://blog.golang.org/context) és a `Future` létrehozó ügyfelére.</span><span class="sxs-lookup"><span data-stu-id="bd60b-184">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="bd60b-185">Itt két lehetséges hibaforrás van: Az ügyfél oldalán okozott hiba, amikor megpróbálja meghívni a metódust, illetve egy hibaválasz a kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="bd60b-185">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="bd60b-186">Az utóbbit a rendszer a `deploymentFuture.Result` hívás részeként adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bd60b-186">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

<span data-ttu-id="bd60b-187">Ha lekérte a telepítéssel kapcsolatos információkat, létezik egy megkerülő megoldás azokra a lehetséges programhibákra, amikor ezek az információk nem jelennek meg. Az adatok betöltéséhez hívja meg manuálisan a `deploymentsClient.Get` metódust.</span><span class="sxs-lookup"><span data-stu-id="bd60b-187">Once the deployment information is retrieved, there is a workaround for possible bugs where the deployment information may be empty with a manual call to `deploymentsClient.Get` to ensure that the data is populated.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="bd60b-188">A hozzárendelt IP-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="bd60b-188">Obtaining the assigned IP address</span></span>

<span data-ttu-id="bd60b-189">Ha bármit szeretne tenni az újonnan létrehozott virtuális géppel, szüksége van a hozzárendelt IP-címre.</span><span class="sxs-lookup"><span data-stu-id="bd60b-189">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="bd60b-190">Az IP-címek önmaguk saját külön Azure-erőforrásai, amelyek NIC-erőforrásokhoz kötődnek.</span><span class="sxs-lookup"><span data-stu-id="bd60b-190">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="bd60b-191">Ez a metódus a paraméterfájlban tárolt adatokra támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="bd60b-191">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="bd60b-192">A kód közvetlenül le tudta kérdezni a virtuális gépről az NIC-t, lekérdezte az NIC-ről az IP-erőforrást, majd közvetlenül lekérdezte az IP-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="bd60b-192">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="bd60b-193">Ez egy hosszú függőség- és műveletlánc, ezért költséges.</span><span class="sxs-lookup"><span data-stu-id="bd60b-193">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="bd60b-194">Mivel a JSON-adat helyiek, ehelyett betölthetők.</span><span class="sxs-lookup"><span data-stu-id="bd60b-194">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="bd60b-195">A virtuális gép felhasználójának értéke szintén betölthető a JSON-ból.</span><span class="sxs-lookup"><span data-stu-id="bd60b-195">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="bd60b-196">A virtuális gép jelszava korábban már be lett töltve a hitelesítési fájlból.</span><span class="sxs-lookup"><span data-stu-id="bd60b-196">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bd60b-197">További lépések</span><span class="sxs-lookup"><span data-stu-id="bd60b-197">Next steps</span></span>

<span data-ttu-id="bd60b-198">Ebben a rövid útmutatóban egy meglévő sablont helyezett üzembe a Gón keresztül.</span><span class="sxs-lookup"><span data-stu-id="bd60b-198">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="bd60b-199">Ezután az újonnan létrehozott virtuális gépet SSH-n keresztül csatlakoztatta, hogy biztosan fusson.</span><span class="sxs-lookup"><span data-stu-id="bd60b-199">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="bd60b-200">Ha többet szeretne megtudni a virtuális gépek Góval való használatáról az Azure-környezetben, lásd: [Azure számítási minták a Góhoz](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) vagy [Azure erőforrás-felügyeleti példák a Góhoz](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="bd60b-200">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="bd60b-201">Ha többet szeretne megtudni az SDK-ban elérhető hitelesítési módszerekről, és az általuk támogatott hitelesítési típusokról, lásd: [Hitelesítés a Góhoz készült Azure SDK-val](azure-sdk-go-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="bd60b-201">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>
