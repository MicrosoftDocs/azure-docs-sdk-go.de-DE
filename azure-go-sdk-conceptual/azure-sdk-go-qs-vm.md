---
title: Bereitstellen eines virtuellen Azure-Computers über Go
description: Stellen Sie einen virtuellen Computer bereit, indem Sie das Azure SDK für Go verwenden.
author: sptramer
ms.author: sttramer
ms.date: 04/03/2018
ms.topic: quickstart
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 565580e9e6c6ced543bd00bbaa01383834d9a41c
ms.sourcegitcommit: 2b2884ea7673c95ba45b3d6eec647200e75bfc5b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Schnellstart: Bereitstellen eines virtuellen Azure-Computers über eine Vorlage mit dem Azure SDK für Go

In diesem Schnellstart wird die Bereitstellung von Ressourcen über eine Vorlage mit dem Azure SDK für Go beschrieben. Vorlagen sind Momentaufnahmen aller Ressourcen, die in einer [Azure-Ressourcengruppe](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) enthalten sind. Sie können sich hier während der Durchführung einer nützlichen Aufgabe mit der Funktionalität und den Konventionen des SDK vertraut machen.

Am Ende dieses Schnellstarts verfügen Sie über eine aktive VM, an der Sie sich mit einem Benutzernamen und Kennwort anmelden können.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Wenn Sie in dieser Schnellstartanleitung eine lokale Installation der Azure-Befehlszeilenschnittstelle verwenden möchten, benötigen Sie mindestens die CLI-Version __2.0.28__. Führen Sie `az --version` aus, um sicherzustellen, dass Ihre CLI diese Anforderung erfüllt. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>Installieren des Azure SDK für Go 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals


Wenn Sie sich nicht interaktiv mit einer Anwendung anmelden möchten, benötigen Sie einen Dienstprinzipal. Dienstprinzipale sind Teil der rollenbasierten Zugriffssteuerung (RBAC), bei der eine eindeutige Benutzeridentität erstellt wird. Führen Sie den folgenden Befehl aus, um mit der CLI einen neuen Dienstprinzipal zu erstellen:

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart --sdk-auth > quickstart.auth
```

Legen Sie die Umgebungsvariable `AZURE_AUTH_LOCATION` auf den vollständigen Pfad dieser Datei fest. Das SDK liest die Anmeldeinformationen dann direkt aus dieser Datei, ohne dass Sie Änderungen vornehmen oder Informationen aus dem Dienstprinzipal erfassen müssen.

## <a name="get-the-code"></a>Abrufen des Codes

Sie können den Schnellstartcode und alle Abhängigkeiten mit `go get` abrufen.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

Sofern die Variable `AZURE_AUTH_LOCATION` ordnungsgemäß festgelegt ist, müssen Sie keine Quellcodeänderungen vornehmen. Wenn das Programm ausgeführt wird, werden alle erforderlichen Authentifizierungsinformationen von dort geladen.

## <a name="running-the-code"></a>Ausführen des Codes

Führen Sie den Schnellstart mit dem Befehl `go run` aus.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

Im Falle eines Bereitstellungsfehlers wird eine Meldung mit einem entsprechenden Hinweis angezeigt. Diese enthält jedoch unter Umständen nicht genügend Details. Rufen Sie mithilfe des folgenden Befehls über die Azure-Befehlszeilenschnittstelle die gesamten Details zu dem Bereitstellungsfehler ab:

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

Wenn die Bereitstellung erfolgreich ist, erhalten Sie eine Meldung mit dem Benutzernamen, der IP-Adresse und dem Kennwort für die Anmeldung am neu erstellten virtuellen Computer. Greifen Sie per SSH auf diesen Computer zu, um zu bestätigen, dass er aktiv ist und ausgeführt wird.

## <a name="cleaning-up"></a>Bereinigen

Bereinigen Sie die Ressourcen, die für diesen Schnellstart erstellt wurden, indem Sie die Ressourcengruppe über die CLI löschen.

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a>Ausführliche Informationen zum Code

Der Code des Schnellstarts wurde in einen Block mit Variablen und mehreren kleineren Funktionen unterteilt, die hier einzeln beschrieben sind.

### <a name="variables-constants-and-types"></a>Variablen, Konstanten und Typen

Diese Schnellstartanleitung ist in sich geschlossen und verwendet daher globale Konstanten und Variablen.

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

Es werden Werte deklariert, mit denen die Namen der erstellten Ressourcen angegeben werden. Außerdem wird hier der Standort angegeben. Sie können ihn ändern, um zu ermitteln, wie sich Bereitstellungen in anderen Rechenzentren verhalten. Nicht jedes Rechenzentrum verfügt über alle erforderlichen Ressourcen.

Der Typ `clientInfo` wird deklariert, um alle Informationen zu kapseln, die unabhängig aus der Authentifizierungsdatei geladen werden müssen, um Clients im SDK einzurichten und das Kennwort für den virtuellen Computer festzulegen.

Die Konstanten `templateFile` und `parametersFile` verweisen auf die Dateien, die für die Bereitstellung benötigt werden. `authorizer` wird durch das Go SDK für die Authentifizierung konfiguriert. Bei der Variablen `ctx` handelt es sich um einen [Go-Kontext](https://blog.golang.org/context) für die Netzwerkvorgänge.

### <a name="authentication-and-initialization"></a>Authentifizierung und Initialisierung

Die Funktion `init` richtet die Authentifizierung ein. Da die Authentifizierung eine Voraussetzung für den Rest der Schnellstartanleitung ist, empfiehlt es sich, sie in die Initialisierung zu integrieren. Darüber hinaus werden auch einige Informationen aus der Authentifizierungsdatei geladen, die benötigt werden, um Clients und den virtuellen Computer zu konfigurieren.

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

Als Erstes wird [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) aufgerufen, um die Authentifizierungsinformationen aus der Datei unter `AZURE_AUTH_LOCATION` zu laden. Danach wird diese Datei manuell durch die (hier weggelassene) Funktion `readJSON` geladen, um die beiden Werte abzurufen, die zum Ausführen des restlichen Programms benötigt werden: die Abonnement-ID des Clients und das Geheimnis des Dienstprinzipals, das auch für das Kennwort des virtuellen Computers verwendet wird.

> [!WARNING]
> Der Einfachheit halber wird das Dienstprinzipalkennwort in dieser Schnellstartanleitung wiederverwendet. In einer Produktionsumgebung darf ein Kennwort für den Zugriff auf Ihre Azure-Ressourcen __niemals__ wiederverwendet werden.

### <a name="flow-of-operations-in-main"></a>Vorgangsablauf in main()

Die Funktion `main` ist einfach aufgebaut. Sie gibt nur den Vorgangsablauf an und führt die Überprüfung auf Fehler durch.

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

Im Code werden die folgenden Schritte ausgeführt (in dieser Reihenfolge):

* Erstellen der Ressourcengruppe für die Bereitstellung (`createGroup`)
* Erstellen der Bereitstellung in dieser Gruppe (`createDeployment`)
* Abrufen und Anzeigen von Anmeldeinformationen für die bereitgestellte VM (`getLogin`)

### <a name="creating-the-resource-group"></a>Erstellen der Ressourcengruppe

Mit der Funktion `createGroup` wird die Ressourcengruppe erstellt. Wenn Sie sich den Ablauf der Aufrufe und die Argumente ansehen, wird deutlich, wie Dienstinteraktionen im SDK strukturiert sind.

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

Dies ist der allgemeine Ablauf der Interaktion mit einem Azure-Dienst:

* Erstellen Sie den Client mit der `service.New*Client()`-Methode, wobei `*` der Ressourcentyp des `service`-Elements ist, mit dem Sie interagieren möchten. Für diese Funktion wird immer eine Abonnement-ID verwendet.
* Legen Sie die Autorisierungsmethode für den Client fest, damit dieser mit der Remote-API interagieren kann.
* Führen Sie den Methodenaufruf auf dem Client gemäß der Remote-API durch. Für Dienstclientmethoden werden normalerweise der Name der Ressource und ein Metadatenobjekt verwendet.

Die Funktion [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) wird verwendet, um hier eine Typkonvertierung durchzuführen. Die Parameter für SDK-Methoden verwenden fast ausschließlich Zeiger. Daher werden Hilfsmethoden zur Vereinfachung der Typkonvertierungen bereitgestellt. In der Dokumentation für das Modul [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) finden Sie die vollständige Liste benutzerfreundlicher Konverter sowie Informationen zu ihrem Verhalten.

Die Methode `groupsClient.CreateOrUpdate` gibt einen Zeiger auf einen Datentyp zurück, der die Ressourcengruppe darstellt. Ein direkter Rückgabewert dieser Art weist auf einen Vorgang mit kurzer Ausführungsdauer hin, der synchron ablaufen soll. Der nächste Abschnitt enthält ein Beispiel für einen Vorgang mit langer Ausführungsdauer sowie Informationen zur Interaktion.

### <a name="performing-the-deployment"></a>Durchführen der Bereitstellung

Nach der Erstellung der Ressourcengruppe kann die Bereitstellung durchgeführt werden. Dieser Code ist in kleinere Abschnitte unterteilt, um unterschiedliche Teile der Logik herauszustellen.

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

Die Bereitstellungsdateien werden mit `readJSON` geladen. Auf die Details hierzu wird hier nicht näher eingegangen. Diese Funktion gibt ein `*map[string]interface{}`-Element zurück. Dieser Typ wird beim Erstellen der Metadaten für den Aufruf zur Ressourcenbereitstellung verwendet. In den Bereitstellungsparametern wird auch das Kennwort des virtuellen Computers manuell festgelegt.

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

Der Code basiert auf dem gleichen Muster wie die Erstellung der Ressourcengruppe. Es wird ein neuer Client erstellt, für den die Authentifizierung mit Azure ermöglicht wird, und anschließend wird eine Methode aufgerufen. Die Methode hat sogar den gleichen Namen (`CreateOrUpdate`) wie die entsprechende Methode für Ressourcengruppen. Dieses Muster zieht sich durch das gesamte SDK. Methoden, die ähnliche Aufgaben haben, haben normalerweise auch die gleichen Namen.

Der größte Unterschied ist der Rückgabewert der `deploymentsClient.CreateOrUpdate`-Methode. Hierbei handelt es sich um einen Wert vom Typ [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), der auf dem [Entwurfsmuster mit „Futures“](https://en.wikipedia.org/wiki/Futures_and_promises) basiert. Werte vom Typ „Future“ stellen einen Vorgang mit langer Ausführungsdauer in Azure dar, den Sie bei Abschluss abfragen, abbrechen oder blockieren können.

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

In diesem Beispiel warten Sie am besten, bis der Vorgang abgeschlossen ist. Wenn auf einen Wert vom Typ „Future“ gewartet werden soll, benötigen Sie sowohl ein [context-Objekt](https://blog.golang.org/context) als auch den Client, der den Wert vom Typ `Future` erstellt hat. Hierbei gibt es zwei mögliche Fehlerquellen: Ein auf Clientseite verursachter Fehler, wenn versucht wird, die Methode aufzurufen, und eine Fehlerantwort vom Server. Letztere wird als Teil des Aufrufs `deploymentFuture.Result` zurückgegeben.

Sollten die abgerufenen Bereitstellungsinformationen aufgrund eines Fehlers leer sein, können Sie `deploymentsClient.Get` manuell aufrufen, um sicherzustellen, dass die Daten aufgefüllt werden.

### <a name="obtaining-the-assigned-ip-address"></a>Abrufen der zugewiesenen IP-Adresse

Sie benötigen die zugewiesene IP-Adresse, um die neu erstellte VM nutzen zu können. IP-Adressen stellen eine eigene separate Azure-Ressource dar, die an NIC-Ressourcen (Network Interface Controller) gebunden ist.

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

Für diese Methode werden die Informationen benötigt, die in der Parameterdatei gespeichert sind. Mit dem Code kann die VM direkt abgefragt werden, um ihre NIC abzurufen, und dann die NIC zum Abrufen ihrer IP-Ressource und anschließend die IP-Ressource direkt abgefragt werden. Dies ist eine lange Kette von Abhängigkeiten und Vorgängen, die abgearbeitet werden müssen und zu einem hohen Kostenaufwand führen. Da die JSON-Informationen lokal vorliegen, können sie stattdessen geladen werden.

Der Wert für den Benutzer des virtuellen Computers wird ebenfalls aus dem JSON-Code geladen. Das Kennwort des virtuellen Computers wurde bereits aus der Authentifizierungsdatei geladen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie eine vorhandene Vorlage verwendet und mit Go bereitgestellt. Anschließend haben Sie per SSH eine Verbindung mit der neu erstellten VM hergestellt, um sicherzustellen, dass sie ausgeführt wird.

Wenn Sie sich weiter über die Arbeit mit virtuellen Computern in der Azure-Umgebung mit Go informieren möchten, helfen Ihnen die Informationen unter [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) (Azure-Computebeispiele für Go) und [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources) (Azure-Ressourcenverwaltungsbeispiele für Go) weiter.

Weitere Informationen zu den verfügbaren Authentifizierungsmethoden des SDKs sowie zu den unterstützten Authentifizierungstypen finden Sie unter [Authentication methods in the Azure SDK for Go](azure-sdk-go-authorization.md) (Authentifizierungsmethoden im Azure SDK für Go).
