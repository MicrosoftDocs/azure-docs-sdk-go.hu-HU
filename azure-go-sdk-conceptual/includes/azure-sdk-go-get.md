A Góhoz készült Azure SDK a Go 1.8-as és újabb verzióival kompatibilis. Az [Azure Stack-profilokat](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles) használó környezetekben a Go 1.9-es verziója a minimális követelmény. Ha nem érhető el a rendszeren a Go, kövesse [a Go telepítési utasításait](https://golang.org/doc/install).

A Góhoz készült Azure SDK és annak függőségei a következőn keresztül szerezhetők be: `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Győződjön meg arról, hogy az `Azure` nagybetűvel szerepeljen az URL-címben. Ellenkező esetben az írásmóddal kapcsolatos importálási hibák fordulhatnak elő az SDK használatakor. Az `Azure` szót az importálási utasításokban is nagybetűvel kell megadni.

