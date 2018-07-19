---
title: Bereitstellen eines virtuellen Azure-Computers über Go
description: Stellen Sie mithilfe des Azure SDK für Go einen virtuellen Computer bereit.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 07/13/2018
ms.topic: quickstart
ms.prod: azure
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: 6b1de35748fb7694d45715fa7f028d5730530d2e
ms.sourcegitcommit: d1790b317a8fcb4d672c654dac2a925a976589d4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39039555"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="ae372-103">Schnellstart: Bereitstellen eines virtuellen Azure-Computers über eine Vorlage mit dem Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="ae372-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="ae372-104">In diesem Schnellstart wird die Bereitstellung von Ressourcen über eine Vorlage mit dem Azure SDK für Go beschrieben.</span><span class="sxs-lookup"><span data-stu-id="ae372-104">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="ae372-105">Vorlagen sind Momentaufnahmen aller Ressourcen, die in einer [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="ae372-105">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="ae372-106">Sie können sich hier während der Durchführung einer nützlichen Aufgabe mit der Funktionalität und den Konventionen des SDK vertraut machen.</span><span class="sxs-lookup"><span data-stu-id="ae372-106">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="ae372-107">Am Ende dieses Schnellstarts verfügen Sie über eine aktive VM, an der Sie sich mit einem Benutzernamen und Kennwort anmelden können.</span><span class="sxs-lookup"><span data-stu-id="ae372-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="ae372-108">Wenn Sie in dieser Schnellstartanleitung eine lokale Installation der Azure-Befehlszeilenschnittstelle verwenden möchten, benötigen Sie mindestens die CLI-Version __2.0.28__.</span><span class="sxs-lookup"><span data-stu-id="ae372-108">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="ae372-109">Führen Sie `az --version` aus, um sicherzustellen, dass Ihre CLI diese Anforderung erfüllt.</span><span class="sxs-lookup"><span data-stu-id="ae372-109">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="ae372-110">Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ae372-110">If you need to install or upgrade, see [Install the Azure CLI](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="ae372-111">Installieren des Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="ae372-111">Install the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="ae372-112">Erstellen eines Dienstprinzipals</span><span class="sxs-lookup"><span data-stu-id="ae372-112">Create a service principal</span></span>

<span data-ttu-id="ae372-113">Wenn Sie sich nicht interaktiv mit einer Anwendung bei Azure anmelden möchten, benötigen Sie einen Dienstprinzipal.</span><span class="sxs-lookup"><span data-stu-id="ae372-113">To sign in non-interactively to Azure with an application, you need a service principal.</span></span> <span data-ttu-id="ae372-114">Dienstprinzipale sind Teil der rollenbasierten Zugriffssteuerung (RBAC), bei der eine eindeutige Benutzeridentität erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="ae372-114">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="ae372-115">Führen Sie den folgenden Befehl aus, um mit der CLI einen neuen Dienstprinzipal zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="ae372-115">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart --sdk-auth > quickstart.auth
```

<span data-ttu-id="ae372-116">Legen Sie die Umgebungsvariable `AZURE_AUTH_LOCATION` auf den vollständigen Pfad dieser Datei fest.</span><span class="sxs-lookup"><span data-stu-id="ae372-116">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="ae372-117">Das SDK liest die Anmeldeinformationen dann direkt aus dieser Datei, ohne dass Sie Änderungen vornehmen oder Informationen aus dem Dienstprinzipal erfassen müssen.</span><span class="sxs-lookup"><span data-stu-id="ae372-117">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="ae372-118">Abrufen des Codes</span><span class="sxs-lookup"><span data-stu-id="ae372-118">Get the code</span></span>

<span data-ttu-id="ae372-119">Sie können den Schnellstartcode und alle Abhängigkeiten mit `go get` abrufen.</span><span class="sxs-lookup"><span data-stu-id="ae372-119">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="ae372-120">Sofern die Variable `AZURE_AUTH_LOCATION` ordnungsgemäß festgelegt ist, müssen Sie keine Quellcodeänderungen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="ae372-120">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="ae372-121">Wenn das Programm ausgeführt wird, werden alle erforderlichen Authentifizierungsinformationen von dort geladen.</span><span class="sxs-lookup"><span data-stu-id="ae372-121">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="ae372-122">Ausführen des Codes</span><span class="sxs-lookup"><span data-stu-id="ae372-122">Running the code</span></span>

<span data-ttu-id="ae372-123">Führen Sie den Schnellstart mit dem Befehl `go run` aus.</span><span class="sxs-lookup"><span data-stu-id="ae372-123">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="ae372-124">Im Falle eines Bereitstellungsfehlers wird eine Meldung mit einem entsprechenden Hinweis angezeigt. Diese enthält jedoch unter Umständen nicht genügend Details.</span><span class="sxs-lookup"><span data-stu-id="ae372-124">If there is a failure in the deployment, you get a message indicating that there was an issue, but it may not include enough detail.</span></span> <span data-ttu-id="ae372-125">Rufen Sie mithilfe des folgenden Befehls über die Azure-Befehlszeilenschnittstelle die gesamten Details zu dem Bereitstellungsfehler ab:</span><span class="sxs-lookup"><span data-stu-id="ae372-125">Using the Azure CLI, get the full details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="ae372-126">Wenn die Bereitstellung erfolgreich ist, erhalten Sie eine Meldung mit dem Benutzernamen, der IP-Adresse und dem Kennwort für die Anmeldung am neu erstellten virtuellen Computer.</span><span class="sxs-lookup"><span data-stu-id="ae372-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="ae372-127">Greifen Sie per SSH auf diesen Computer zu, um zu bestätigen, dass er aktiv ist und ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="ae372-127">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="ae372-128">Bereinigen</span><span class="sxs-lookup"><span data-stu-id="ae372-128">Cleaning up</span></span>

<span data-ttu-id="ae372-129">Bereinigen Sie die Ressourcen, die für diesen Schnellstart erstellt wurden, indem Sie die Ressourcengruppe über die CLI löschen.</span><span class="sxs-lookup"><span data-stu-id="ae372-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="ae372-130">Ausführliche Informationen zum Code</span><span class="sxs-lookup"><span data-stu-id="ae372-130">Code in depth</span></span>

<span data-ttu-id="ae372-131">Der Code des Schnellstarts wurde in einen Block mit Variablen und mehreren kleineren Funktionen unterteilt, die hier einzeln beschrieben sind.</span><span class="sxs-lookup"><span data-stu-id="ae372-131">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="ae372-132">Variablen, Konstanten und Typen</span><span class="sxs-lookup"><span data-stu-id="ae372-132">Variables, constants, and types</span></span>

<span data-ttu-id="ae372-133">Diese Schnellstartanleitung ist in sich geschlossen und verwendet daher globale Konstanten und Variablen.</span><span class="sxs-lookup"><span data-stu-id="ae372-133">Since quickstart is self-contained, it uses global constants and variables.</span></span>

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

<span data-ttu-id="ae372-134">Es werden Werte deklariert, mit denen die Namen der erstellten Ressourcen angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="ae372-134">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="ae372-135">Außerdem wird hier der Standort angegeben. Sie können ihn ändern, um zu ermitteln, wie sich Bereitstellungen in anderen Rechenzentren verhalten.</span><span class="sxs-lookup"><span data-stu-id="ae372-135">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="ae372-136">Nicht jedes Rechenzentrum verfügt über alle erforderlichen Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="ae372-136">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="ae372-137">Der Typ `clientInfo` wird deklariert, um alle Informationen zu kapseln, die unabhängig aus der Authentifizierungsdatei geladen werden müssen, um Clients im SDK einzurichten und das Kennwort für den virtuellen Computer festzulegen.</span><span class="sxs-lookup"><span data-stu-id="ae372-137">The `clientInfo` type is declared to encapsulate all of the information that must be independently loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="ae372-138">Die Konstanten `templateFile` und `parametersFile` verweisen auf die Dateien, die für die Bereitstellung benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="ae372-138">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="ae372-139">`authorizer` wird durch das Go SDK für die Authentifizierung konfiguriert. Bei der Variablen `ctx` handelt es sich um einen [Go-Kontext](https://blog.golang.org/context) für die Netzwerkvorgänge.</span><span class="sxs-lookup"><span data-stu-id="ae372-139">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="ae372-140">Authentifizierung und Initialisierung</span><span class="sxs-lookup"><span data-stu-id="ae372-140">Authentication and initialization</span></span>

<span data-ttu-id="ae372-141">Die Funktion `init` richtet die Authentifizierung ein.</span><span class="sxs-lookup"><span data-stu-id="ae372-141">The `init` function sets up authentication.</span></span> <span data-ttu-id="ae372-142">Da die Authentifizierung eine Voraussetzung für den Rest der Schnellstartanleitung ist, empfiehlt es sich, sie in die Initialisierung zu integrieren.</span><span class="sxs-lookup"><span data-stu-id="ae372-142">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="ae372-143">Darüber hinaus werden auch einige Informationen aus der Authentifizierungsdatei geladen, die benötigt werden, um Clients und den virtuellen Computer zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="ae372-143">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

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

<span data-ttu-id="ae372-144">Als Erstes wird [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) aufgerufen, um die Authentifizierungsinformationen aus der Datei unter `AZURE_AUTH_LOCATION` zu laden.</span><span class="sxs-lookup"><span data-stu-id="ae372-144">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="ae372-145">Danach wird diese Datei manuell durch die (hier weggelassene) Funktion `readJSON` geladen, um die beiden Werte abzurufen, die zum Ausführen des restlichen Programms benötigt werden: die Abonnement-ID des Clients und das Geheimnis des Dienstprinzipals, das auch für das Kennwort des virtuellen Computers verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="ae372-145">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="ae372-146">Der Einfachheit halber wird das Dienstprinzipalkennwort in dieser Schnellstartanleitung wiederverwendet.</span><span class="sxs-lookup"><span data-stu-id="ae372-146">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="ae372-147">In einer Produktionsumgebung darf ein Kennwort für den Zugriff auf Ihre Azure-Ressourcen __niemals__ wiederverwendet werden.</span><span class="sxs-lookup"><span data-stu-id="ae372-147">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="ae372-148">Vorgangsablauf in main()</span><span class="sxs-lookup"><span data-stu-id="ae372-148">Flow of operations in main()</span></span>

<span data-ttu-id="ae372-149">Die Funktion `main` ist einfach aufgebaut. Sie gibt nur den Vorgangsablauf an und führt die Überprüfung auf Fehler durch.</span><span class="sxs-lookup"><span data-stu-id="ae372-149">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="ae372-150">Im Code werden die folgenden Schritte ausgeführt (in dieser Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="ae372-150">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="ae372-151">Erstellen der Ressourcengruppe für die Bereitstellung (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="ae372-151">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="ae372-152">Erstellen der Bereitstellung in dieser Gruppe (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="ae372-152">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="ae372-153">Abrufen und Anzeigen von Anmeldeinformationen für die bereitgestellte VM (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="ae372-153">Obtain and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="ae372-154">Erstellen der Ressourcengruppe</span><span class="sxs-lookup"><span data-stu-id="ae372-154">Creating the resource group</span></span>

<span data-ttu-id="ae372-155">Mit der Funktion `createGroup` wird die Ressourcengruppe erstellt.</span><span class="sxs-lookup"><span data-stu-id="ae372-155">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="ae372-156">Wenn Sie sich den Ablauf der Aufrufe und die Argumente ansehen, wird deutlich, wie Dienstinteraktionen im SDK strukturiert sind.</span><span class="sxs-lookup"><span data-stu-id="ae372-156">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="ae372-157">Dies ist der allgemeine Ablauf der Interaktion mit einem Azure-Dienst:</span><span class="sxs-lookup"><span data-stu-id="ae372-157">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="ae372-158">Erstellen Sie den Client mit der `service.New*Client()`-Methode, wobei `*` der Ressourcentyp des `service`-Elements ist, mit dem Sie interagieren möchten.</span><span class="sxs-lookup"><span data-stu-id="ae372-158">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="ae372-159">Für diese Funktion wird immer eine Abonnement-ID verwendet.</span><span class="sxs-lookup"><span data-stu-id="ae372-159">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="ae372-160">Legen Sie die Autorisierungsmethode für den Client fest, damit dieser mit der Remote-API interagieren kann.</span><span class="sxs-lookup"><span data-stu-id="ae372-160">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="ae372-161">Führen Sie den Methodenaufruf auf dem Client gemäß der Remote-API durch.</span><span class="sxs-lookup"><span data-stu-id="ae372-161">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="ae372-162">Für Dienstclientmethoden werden normalerweise der Name der Ressource und ein Metadatenobjekt verwendet.</span><span class="sxs-lookup"><span data-stu-id="ae372-162">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="ae372-163">Die Funktion [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) wird verwendet, um hier eine Typkonvertierung durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="ae372-163">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="ae372-164">Die Parameter für SDK-Methoden verwenden fast ausschließlich Zeiger. Daher werden Hilfsmethoden zur Vereinfachung der Typkonvertierungen bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="ae372-164">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="ae372-165">In der Dokumentation für das Modul [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) finden Sie die vollständige Liste benutzerfreundlicher Konverter sowie Informationen zu ihrem Verhalten.</span><span class="sxs-lookup"><span data-stu-id="ae372-165">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="ae372-166">Die Methode `groupsClient.CreateOrUpdate` gibt einen Zeiger auf einen Datentyp zurück, der die Ressourcengruppe darstellt.</span><span class="sxs-lookup"><span data-stu-id="ae372-166">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="ae372-167">Ein direkter Rückgabewert dieser Art weist auf einen Vorgang mit kurzer Ausführungsdauer hin, der synchron ablaufen soll.</span><span class="sxs-lookup"><span data-stu-id="ae372-167">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="ae372-168">Der nächste Abschnitt enthält ein Beispiel für einen Vorgang mit langer Ausführungsdauer sowie Informationen zur Interaktion.</span><span class="sxs-lookup"><span data-stu-id="ae372-168">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="ae372-169">Durchführen der Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="ae372-169">Performing the deployment</span></span>

<span data-ttu-id="ae372-170">Nach der Erstellung der Ressourcengruppe kann die Bereitstellung durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="ae372-170">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="ae372-171">Dieser Code ist in kleinere Abschnitte unterteilt, um unterschiedliche Teile der Logik herauszustellen.</span><span class="sxs-lookup"><span data-stu-id="ae372-171">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

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

<span data-ttu-id="ae372-172">Die Bereitstellungsdateien werden mit `readJSON` geladen. Auf die Details hierzu wird hier nicht näher eingegangen.</span><span class="sxs-lookup"><span data-stu-id="ae372-172">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="ae372-173">Diese Funktion gibt ein `*map[string]interface{}`-Element zurück. Dieser Typ wird beim Erstellen der Metadaten für den Aufruf zur Ressourcenbereitstellung verwendet.</span><span class="sxs-lookup"><span data-stu-id="ae372-173">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="ae372-174">In den Bereitstellungsparametern wird auch das Kennwort des virtuellen Computers manuell festgelegt.</span><span class="sxs-lookup"><span data-stu-id="ae372-174">The VM's password is also set manually on the deployment parameters.</span></span>

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

<span data-ttu-id="ae372-175">Der Code basiert auf dem gleichen Muster wie die Erstellung der Ressourcengruppe.</span><span class="sxs-lookup"><span data-stu-id="ae372-175">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="ae372-176">Es wird ein neuer Client erstellt, für den die Authentifizierung mit Azure ermöglicht wird, und anschließend wird eine Methode aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="ae372-176">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span>
<span data-ttu-id="ae372-177">Die Methode hat sogar den gleichen Namen (`CreateOrUpdate`) wie die entsprechende Methode für Ressourcengruppen.</span><span class="sxs-lookup"><span data-stu-id="ae372-177">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="ae372-178">Dieses Muster zieht sich durch das gesamte SDK.</span><span class="sxs-lookup"><span data-stu-id="ae372-178">This pattern is seen throughout the SDK.</span></span>
<span data-ttu-id="ae372-179">Methoden, die ähnliche Aufgaben haben, haben normalerweise auch die gleichen Namen.</span><span class="sxs-lookup"><span data-stu-id="ae372-179">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="ae372-180">Der größte Unterschied ist der Rückgabewert der `deploymentsClient.CreateOrUpdate`-Methode.</span><span class="sxs-lookup"><span data-stu-id="ae372-180">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="ae372-181">Hierbei handelt es sich um einen Wert vom Typ [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), der auf dem [Entwurfsmuster mit „Futures“](https://en.wikipedia.org/wiki/Futures_and_promises) basiert.</span><span class="sxs-lookup"><span data-stu-id="ae372-181">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="ae372-182">Werte vom Typ „Future“ stellen einen Vorgang mit langer Ausführungsdauer in Azure dar, den Sie bei Abschluss abfragen, abbrechen oder blockieren können.</span><span class="sxs-lookup"><span data-stu-id="ae372-182">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

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

<span data-ttu-id="ae372-183">In diesem Beispiel warten Sie am besten, bis der Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="ae372-183">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="ae372-184">Wenn auf einen Wert vom Typ „Future“ gewartet werden soll, benötigen Sie sowohl ein [context-Objekt](https://blog.golang.org/context) als auch den Client, der den Wert vom Typ `Future` erstellt hat.</span><span class="sxs-lookup"><span data-stu-id="ae372-184">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="ae372-185">Hierbei gibt es zwei mögliche Fehlerquellen: Ein auf Clientseite verursachter Fehler, wenn versucht wird, die Methode aufzurufen, und eine Fehlerantwort vom Server.</span><span class="sxs-lookup"><span data-stu-id="ae372-185">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="ae372-186">Letztere wird als Teil des Aufrufs `deploymentFuture.Result` zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="ae372-186">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

<span data-ttu-id="ae372-187">Sollten die abgerufenen Bereitstellungsinformationen aufgrund eines Fehlers leer sein, können Sie `deploymentsClient.Get` manuell aufrufen, um sicherzustellen, dass die Daten aufgefüllt werden.</span><span class="sxs-lookup"><span data-stu-id="ae372-187">Once the deployment information is retrieved, there is a workaround for possible bugs where the deployment information may be empty with a manual call to `deploymentsClient.Get` to ensure that the data is populated.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="ae372-188">Abrufen der zugewiesenen IP-Adresse</span><span class="sxs-lookup"><span data-stu-id="ae372-188">Obtaining the assigned IP address</span></span>

<span data-ttu-id="ae372-189">Sie benötigen die zugewiesene IP-Adresse, um die neu erstellte VM nutzen zu können.</span><span class="sxs-lookup"><span data-stu-id="ae372-189">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="ae372-190">IP-Adressen stellen eine eigene separate Azure-Ressource dar, die an NIC-Ressourcen (Network Interface Controller) gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="ae372-190">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="ae372-191">Für diese Methode werden die Informationen benötigt, die in der Parameterdatei gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="ae372-191">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="ae372-192">Mit dem Code kann die VM direkt abgefragt werden, um ihre NIC abzurufen, und dann die NIC zum Abrufen ihrer IP-Ressource und anschließend die IP-Ressource direkt abgefragt werden.</span><span class="sxs-lookup"><span data-stu-id="ae372-192">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="ae372-193">Dies ist eine lange Kette von Abhängigkeiten und Vorgängen, die abgearbeitet werden müssen und zu einem hohen Kostenaufwand führen.</span><span class="sxs-lookup"><span data-stu-id="ae372-193">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="ae372-194">Da die JSON-Informationen lokal vorliegen, können sie stattdessen geladen werden.</span><span class="sxs-lookup"><span data-stu-id="ae372-194">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="ae372-195">Der Wert für den Benutzer des virtuellen Computers wird ebenfalls aus dem JSON-Code geladen.</span><span class="sxs-lookup"><span data-stu-id="ae372-195">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="ae372-196">Das Kennwort des virtuellen Computers wurde bereits aus der Authentifizierungsdatei geladen.</span><span class="sxs-lookup"><span data-stu-id="ae372-196">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ae372-197">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="ae372-197">Next steps</span></span>

<span data-ttu-id="ae372-198">In diesem Schnellstart haben Sie eine vorhandene Vorlage verwendet und mit Go bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="ae372-198">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="ae372-199">Anschließend haben Sie per SSH eine Verbindung mit der neu erstellten VM hergestellt, um sicherzustellen, dass sie ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="ae372-199">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="ae372-200">Wenn Sie sich weiter über die Arbeit mit virtuellen Computern in der Azure-Umgebung mit Go informieren möchten, helfen Ihnen die Informationen unter [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) (Azure-Computebeispiele für Go) und [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources) (Azure-Ressourcenverwaltungsbeispiele für Go) weiter.</span><span class="sxs-lookup"><span data-stu-id="ae372-200">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="ae372-201">Weitere Informationen zu den verfügbaren Authentifizierungsmethoden des SDKs sowie zu den unterstützten Authentifizierungstypen finden Sie unter [Authentication methods in the Azure SDK for Go](azure-sdk-go-authorization.md) (Authentifizierungsmethoden im Azure SDK für Go).</span><span class="sxs-lookup"><span data-stu-id="ae372-201">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>
