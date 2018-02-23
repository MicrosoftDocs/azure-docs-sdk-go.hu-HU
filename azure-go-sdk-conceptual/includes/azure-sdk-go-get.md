<span data-ttu-id="e669d-101">A Góhoz készült Azure SDK a Go 1.8-as és újabb verzióival kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="e669d-101">The Azure SDK for Go is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="e669d-102">Az [Azure Stack-profilokat](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles) használó környezetekben a Go 1.9-es verziója a minimális követelmény.</span><span class="sxs-lookup"><span data-stu-id="e669d-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span> <span data-ttu-id="e669d-103">Ha nem érhető el a rendszeren a Go, kövesse [a Go telepítési utasításait](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="e669d-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="e669d-104">A Góhoz készült Azure SDK és annak függőségei a következőn keresztül szerezhetők be: `go get`.</span><span class="sxs-lookup"><span data-stu-id="e669d-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="e669d-105">Győződjön meg arról, hogy az `Azure` nagybetűvel szerepeljen az URL-címben.</span><span class="sxs-lookup"><span data-stu-id="e669d-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="e669d-106">Ellenkező esetben az írásmóddal kapcsolatos importálási hibák fordulhatnak elő az SDK használatakor.</span><span class="sxs-lookup"><span data-stu-id="e669d-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="e669d-107">Az `Azure` szót az importálási utasításokban is nagybetűvel kell megadni.</span><span class="sxs-lookup"><span data-stu-id="e669d-107">You also need to capitalize `Azure` in your import statements.</span></span>

