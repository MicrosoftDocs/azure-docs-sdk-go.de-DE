---
title: "Bereitstellen eines virtuellen Azure-Computers über Go"
description: "Stellen Sie einen virtuellen Computer bereit, indem Sie das Azure SDK für Go verwenden."
keywords: Azure, virtueller Computer, VM, Go, Golang, Azure SDK
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: routlaw
ms.openlocfilehash: ae460dbf21b13c40f3d564274f8b790afe005aae
ms.sourcegitcommit: af3473779cd7c2978f290fbdc51ee15eb1130840
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Schnellstart: Bereitstellen eines virtuellen Azure-Computers über eine Vorlage mit dem Azure SDK für Go

In diesem Schnellstart wird die Bereitstellung von Ressourcen über eine Vorlage mit dem Azure SDK für Go beschrieben. Vorlagen sind Momentaufnahmen aller Ressourcen, die in einer [Azure-Ressourcengruppe](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) enthalten sind. Sie können sich hier während der Durchführung einer nützlichen Aufgabe mit der Funktionalität und den Konventionen des SDK vertraut machen.

Am Ende dieses Schnellstarts verfügen Sie über eine aktive VM, an der Sie sich mit einem Benutzernamen und Kennwort anmelden können.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Falls Sie eine lokale Installation der Azure CLI verwenden, ist für diesen Schnellstart die CLI-Version 2.0.24 oder höher erforderlich. Führen Sie `az --version` aus, um sicherzustellen, dass Ihre CLI diese Anforderung erfüllt. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>Installieren des Azure SDK für Go 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Erstellen eines Dienstprinzipals

Wenn Sie sich nicht interaktiv mit einer Anwendung anmelden möchten, benötigen Sie einen Dienstprinzipal. Dienstprinzipale sind Teil der rollenbasierten Zugriffssteuerung (RBAC), bei der eine eindeutige Benutzeridentität erstellt wird. Führen Sie den folgenden Befehl aus, um mit der CLI einen neuen Dienstprinzipal zu erstellen:

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

__Stellen Sie sicher__, dass Sie sich die in der Ausgabe enthaltenen Werte für `appId`, `password` und `tenant` notieren. Diese Werte werden von der Anwendung für die Azure-Authentifizierung verwendet.

Weitere Informationen zum Erstellen und Verwalten von Dienstprinzipalen per Azure CLI 2.0 finden Sie unter [Erstellen eines Azure-Dienstprinzipals mit Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).

## <a name="get-the-code"></a>Abrufen des Codes

Sie können den Schnellstartcode und alle Abhängigkeiten mit `go get` abrufen.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

Dieser Code kann kompiliert werden, aber er wird erst richtig ausgeführt, nachdem Sie Informationen zu Ihrem Azure-Konto und zum erstellten Dienstprinzipal angegeben haben. `main.go` enthält die Variable `config` mit der Struktur `authInfo`. Für diese Struktur müssen die Feldwerte ersetzt werden, um richtig authentifiziert werden zu können.

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* `SubscriptionID`: Ihre Abonnement-ID, die mit dem CLI-Befehl abgerufen werden kann.

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* `TenantID`: Ihre Mandanten-ID. Dies ist der Wert `tenant`, den Sie sich beim Erstellen des Dienstprinzipals notiert haben.
* `ServicePrincipalID`: Der Wert `appId`, den Sie sich beim Erstellen des Dienstprinzipals notiert haben.
* `ServicePrincipalSecret`: Der Wert `password`, den Sie sich beim Erstellen des Dienstprinzipals notiert haben.

Außerdem müssen Sie einen Wert in der Datei `vm-quickstart-params.json` bearbeiten.

```json
    "vm_password": {
        "value": "_"
    }
```

* `vm_password`: Das Kennwort für das VM-Benutzerkonto. Es muss eine Länge von 12 bis 72 Zeichen haben und mindestens drei verschiedene Arten der folgenden Zeichen enthalten:
  * Kleinbuchstabe
  * Großbuchstabe
  * Ziffer
  * Symbol

## <a name="running-the-code"></a>Ausführen des Codes

Führen Sie den Schnellstart mit dem Befehl `go run` aus.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

Falls für die Bereitstellung ein Fehler auftritt, wird eine Meldung mit einem Hinweis zu einem Problem angezeigt, aber es werden keine Details angegeben. Rufen Sie die Details zum Bereitstellungsfehler über die Azure CLI mit dem folgenden Befehl ab:

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

### <a name="variable-assignments-and-structs"></a>Variablenzuweisungen und Strukturen

Da der Schnellstart eigenständig ist, werden anstelle von Befehlszeilenoptionen oder Umgebungsvariablen globale Variablen verwendet.

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

Die Struktur `authInfo` wird deklariert, um alle Informationen zu kapseln, die für die Autorisierung für Azure-Dienste benötigt werden.

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

Es werden Werte deklariert, mit denen die Namen der erstellten Ressourcen angegeben werden. Außerdem wird hier der Standort angegeben. Sie können ihn ändern, um zu ermitteln, wie sich Bereitstellungen in anderen Rechenzentren verhalten. Nicht jedes Rechenzentrum verfügt über alle erforderlichen Ressourcen.

Die Konstanten `templateFile` und `parametersFile` verweisen auf die Dateien, die für die Bereitstellung benötigt werden. Das Dienstprinzipaltoken wird später behandelt, und die Variable `ctx` ist ein [Go-Kontext](https://blog.golang.org/context) für die Netzwerkvorgänge.

### <a name="init-and-authorization"></a>init() und Autorisierung

Mit der `init()`-Methode für den Code wird die Autorisierung eingerichtet. Da die Autorisierung eine Vorbedingung für alle Elemente des Schnellstarts ist, ist es sinnvoll, sie als Teil der Initialisierung zu betrachten. 

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

Mit diesem Code werden zwei Schritte für die Autorisierung ausgeführt:

* OAuth-Konfigurationsinformationen für die `TenantID` werden abgerufen, indem Azure Active Directory genutzt wird. Das [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud)-Objekt enthält Endpunkte, die in der Azure-Standardkonfiguration verwendet werden.
* Die Funktion [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) wird aufgerufen. Bei dieser Funktion werden die OAuth-Informationen zusammen mit der Dienstprinzipalanmeldung verwendet, und es wird angegeben, welche Art von Azure-Verwaltung genutzt wird. Dieser Wert sollte immer `.ResourceManagerEndpoint` lauten, sofern bei Ihnen nicht besondere Anforderungen gelten und Sie wissen, was zu tun ist.

### <a name="flow-of-operations-in-main"></a>Vorgangsablauf in main()

Die Funktion `main()` ist einfach aufgebaut. Sie gibt nur den Vorgangsablauf an und führt die Überprüfung auf Fehler durch.

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

Im Code werden die folgenden Schritte ausgeführt (in dieser Reihenfolge):

* Erstellen der Ressourcengruppe für die Bereitstellung (`createGroup()`)
* Erstellen der Bereitstellung in dieser Gruppe (`createDeployment()`)
* Abrufen und Anzeigen von Anmeldeinformationen für die bereitgestellte VM (`getLogin()`)

### <a name="creating-the-resource-group"></a>Erstellen der Ressourcengruppe

Mit der Funktion `createGroup()` wird die Ressourcengruppe erstellt. Wenn Sie sich den Ablauf der Aufrufe und die Argumente ansehen, wird deutlich, wie Dienstinteraktionen im SDK strukturiert sind.

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

Dies ist der allgemeine Ablauf der Interaktion mit einem Azure-Dienst:

* Erstellen Sie den Client mit der `service.NewXClient()`-Methode, wobei `X` der Ressourcentyp des `service`-Elements ist, mit dem Sie interagieren möchten. Für diese Funktion wird immer eine Abonnement-ID verwendet.
* Legen Sie die Autorisierungsmethode für den Client fest, damit dieser mit der Remote-API interagieren kann.
* Führen Sie den Methodenaufruf auf dem Client gemäß der Remote-API durch. Für Dienstclientmethoden werden normalerweise der Name der Ressource und ein Metadatenobjekt verwendet.

Die Funktion [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) wird verwendet, um hier eine Typkonvertierung durchzuführen. Für die Parameterstrukturen für Methoden des SDK werden fast ausschließlich Zeiger genutzt, sodass diese Methoden bereitgestellt werden, um die Typkonvertierungen zu vereinfachen. In der Dokumentation für das Modul [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) finden Sie die vollständige Liste und Informationen zum Verhalten von benutzerfreundlichen Konvertern.

Der Vorgang `groupsClient.CreateOrUpdate()` gibt einen Zeiger auf die Datenstruktur zurück, die für die Ressourcengruppe steht. Ein direkter Rückgabewert dieser Art weist auf einen Vorgang mit kurzer Ausführungsdauer hin, der synchron ablaufen soll. Der nächste Abschnitt enthält ein Beispiel für einen Vorgang mit langer Ausführungsdauer und Informationen zur Interaktion.

### <a name="performing-the-deployment"></a>Durchführen der Bereitstellung

Nachdem die Gruppe für die Ressourcen erstellt wurde, kann die Bereitstellung ausgeführt werden. Dieser Code ist in kleinere Abschnitte unterteilt, um unterschiedliche Teile der Logik herauszustellen.


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

Die Bereitstellungsdateien werden mit `readJSON` geladen. Auf die Details hierzu wird hier nicht näher eingegangen. Diese Funktion gibt ein `*map[string]interface{}`-Element zurück. Dieser Typ wird beim Erstellen der Metadaten für den Aufruf zur Ressourcenbereitstellung verwendet.

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

Der Code basiert auf dem gleichen Muster wie bei der Erstellung der Ressourcengruppe. Es wird ein neuer Client erstellt, für den die Authentifizierung mit Azure ermöglicht wird, und anschließend wird eine Methode aufgerufen. Die Methode hat sogar den gleichen Namen (`CreateOrUpdate`) wie die entsprechende Methode für Ressourcengruppen. Dieses Muster ist im SDK häufiger zu beobachten. Methoden, die ähnliche Aufgaben haben, haben normalerweise auch die gleichen Namen.

Der größte Unterschied ist der Rückgabewert der `deploymentsClient.CreateOrUpdate()`-Methode. Dieser Wert ist ein `Future`-Objekt, das auf dem [Entwurfsmuster mit „Futures“](https://en.wikipedia.org/wiki/Futures_and_promises) basiert. Futures stehen für einen Vorgang mit langer Ausführungsdauer in Azure, der ggf. von Zeit zu Zeit abgefragt wird, während Sie andere Arbeitsschritte ausführen.

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

In diesem Beispiel warten Sie am besten, bis der Vorgang abgeschlossen ist. Für das Warten auf ein Future-Objekt ist sowohl ein [context-Objekt](https://blog.golang.org/context) als auch der Client erforderlich, der das Future-Objekt erstellt hat. Hierbei gibt es zwei mögliche Fehlerquellen: Ein auf Clientseite verursachter Fehler, wenn versucht wird, die Methode aufzurufen, und eine Fehlerantwort vom Server. Letztere wird als Teil des Aufrufs `deploymentFuture.Result()` zurückgegeben.

### <a name="obtaining-the-assigned-ip-address"></a>Abrufen der zugewiesenen IP-Adresse

Sie benötigen die zugewiesene IP-Adresse, um die neu erstellte VM nutzen zu können. IP-Adressen stellen eine eigene separate Azure-Ressource dar, die an NIC-Ressourcen (Network Interface Controller) gebunden ist.

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

Für diese Methode werden die Informationen benötigt, die in der Parameterdatei gespeichert sind. Mit dem Code kann die VM direkt abgefragt werden, um ihre NIC abzurufen, und dann die NIC zum Abrufen ihrer IP-Ressource und anschließend die IP-Ressource direkt abgefragt werden. Dies ist eine lange Kette von Abhängigkeiten und Vorgängen, die abgearbeitet werden müssen und zu einem hohen Kostenaufwand führen. Da die JSON-Informationen lokal vorliegen, können sie stattdessen geladen werden.

Die Werte für den VM-Benutzer und das dazugehörige Kennwort werden ebenfalls per JSON geladen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie eine vorhandene Vorlage verwendet und mit Go bereitgestellt. Anschließend haben Sie per SSH eine Verbindung mit der neu erstellten VM hergestellt, um sicherzustellen, dass sie ausgeführt wird.

Wenn Sie sich weiter über die Arbeit mit virtuellen Computern in der Azure-Umgebung mit Go informieren möchten, helfen Ihnen die Informationen unter [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) (Azure-Computebeispiele für Go) und [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources) (Azure-Ressourcenverwaltungsbeispiele für Go) weiter.
