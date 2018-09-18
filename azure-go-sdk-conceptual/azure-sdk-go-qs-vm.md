---
title: Bereitstellen eines virtuellen Azure-Computers über Go
description: Stellen Sie mithilfe des Azure SDK für Go einen virtuellen Computer bereit.
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
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059134"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="b7e14-103">Schnellstart: Bereitstellen eines virtuellen Azure-Computers über eine Vorlage mit dem Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="b7e14-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="b7e14-104">In dieser Schnellstartanleitung wird gezeigt, wie Sie Ressourcen über eine Azure Resource Manager-Vorlage unter Verwendung des Azure SDK für Go bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-104">This quickstart shows you how to deploy resources from an Azure Resource Manager template, using the Azure SDK for Go.</span></span> <span data-ttu-id="b7e14-105">Vorlagen sind Momentaufnahmen aller Ressourcen in einer [Azure-Ressourcengruppe](/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="b7e14-105">Templates are snapshots of all of the resources within an [Azure resource group](/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="b7e14-106">Sie können sich hier mit der Funktionalität und den Konventionen des SDK vertraut machen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-106">Along the way, you'll become familiar with the functionality and conventions of the SDK.</span></span>

<span data-ttu-id="b7e14-107">Am Ende dieses Schnellstarts verfügen Sie über eine aktive VM, an der Sie sich mit einem Benutzernamen und Kennwort anmelden können.</span><span class="sxs-lookup"><span data-stu-id="b7e14-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

> [!NOTE]
> <span data-ttu-id="b7e14-108">Die Erstellung eines virtuellen Computers in Go ohne Resource Manager-Vorlage wird anhand eines [imperativen Beispiels](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) veranschaulicht, das erläutert, wie Sie alle VM-Ressourcen mit dem SDK erstellen und konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="b7e14-108">To see the creation of a VM in Go without the use of a Resource Manager template, there is an [imperative sample](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) demonstrating how to build and configure all VM resources with the SDK.</span></span> <span data-ttu-id="b7e14-109">Durch die Verwendung einer Vorlage in diesem Beispiel können wir uns auf SDK-Konventionen konzentrieren, ohne zu ausführlich auf die Azure-Dienstarchitektur eingehen zu müssen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-109">Using a template in this sample allows a focus on SDK conventions without getting into too many details about Azure service architecture.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="b7e14-110">Wenn Sie in dieser Schnellstartanleitung eine lokale Installation der Azure-Befehlszeilenschnittstelle verwenden möchten, benötigen Sie mindestens die CLI-Version __2.0.28__.</span><span class="sxs-lookup"><span data-stu-id="b7e14-110">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="b7e14-111">Führen Sie `az --version` aus, um sicherzustellen, dass Ihre CLI diese Anforderung erfüllt.</span><span class="sxs-lookup"><span data-stu-id="b7e14-111">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="b7e14-112">Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b7e14-112">If you need to install or upgrade, see [Install the Azure CLI](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="b7e14-113">Installieren des Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="b7e14-113">Install the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="b7e14-114">Erstellen eines Dienstprinzipals</span><span class="sxs-lookup"><span data-stu-id="b7e14-114">Create a service principal</span></span>

<span data-ttu-id="b7e14-115">Wenn Sie sich nicht interaktiv mit einer Anwendung bei Azure anmelden möchten, benötigen Sie einen Dienstprinzipal.</span><span class="sxs-lookup"><span data-stu-id="b7e14-115">To sign in non-interactively to Azure with an application, you need a service principal.</span></span> <span data-ttu-id="b7e14-116">Dienstprinzipale sind Teil der rollenbasierten Zugriffssteuerung (RBAC), bei der eine eindeutige Benutzeridentität erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="b7e14-116">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="b7e14-117">Führen Sie den folgenden Befehl aus, um mit der CLI einen neuen Dienstprinzipal zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="b7e14-117">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --sdk-auth > quickstart.auth
```

<span data-ttu-id="b7e14-118">Legen Sie die Umgebungsvariable `AZURE_AUTH_LOCATION` auf den vollständigen Pfad dieser Datei fest.</span><span class="sxs-lookup"><span data-stu-id="b7e14-118">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="b7e14-119">Das SDK liest die Anmeldeinformationen dann direkt aus dieser Datei, ohne dass Sie Änderungen vornehmen oder Informationen aus dem Dienstprinzipal erfassen müssen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-119">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="b7e14-120">Abrufen des Codes</span><span class="sxs-lookup"><span data-stu-id="b7e14-120">Get the code</span></span>

<span data-ttu-id="b7e14-121">Sie können den Schnellstartcode und alle Abhängigkeiten mit `go get` abrufen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-121">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="b7e14-122">Sofern die Variable `AZURE_AUTH_LOCATION` ordnungsgemäß festgelegt ist, müssen Sie keine Quellcodeänderungen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-122">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="b7e14-123">Wenn das Programm ausgeführt wird, werden alle erforderlichen Authentifizierungsinformationen von dort geladen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-123">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="b7e14-124">Ausführen des Codes</span><span class="sxs-lookup"><span data-stu-id="b7e14-124">Running the code</span></span>

<span data-ttu-id="b7e14-125">Führen Sie den Schnellstart mit dem Befehl `go run` aus.</span><span class="sxs-lookup"><span data-stu-id="b7e14-125">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="b7e14-126">Wenn die Bereitstellung erfolgreich ist, erhalten Sie eine Meldung mit dem Benutzernamen, der IP-Adresse und dem Kennwort für die Anmeldung am neu erstellten virtuellen Computer.</span><span class="sxs-lookup"><span data-stu-id="b7e14-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="b7e14-127">Greifen Sie per SSH auf diesen Computer zu, um zu überprüfen, ob er aktiv ist und ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="b7e14-127">SSH into this machine to see if it's up and running.</span></span> 

## <a name="cleaning-up"></a><span data-ttu-id="b7e14-128">Bereinigen</span><span class="sxs-lookup"><span data-stu-id="b7e14-128">Cleaning up</span></span>

<span data-ttu-id="b7e14-129">Bereinigen Sie die Ressourcen, die für diesen Schnellstart erstellt wurden, indem Sie die Ressourcengruppe über die CLI löschen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

<span data-ttu-id="b7e14-130">Löschen Sie außerdem den erstellten Dienstprinzipal.</span><span class="sxs-lookup"><span data-stu-id="b7e14-130">Also delete the service principal that was created.</span></span> <span data-ttu-id="b7e14-131">Die Datei `quickstart.auth` enthält einen JSON-Schlüssel für `clientId`.</span><span class="sxs-lookup"><span data-stu-id="b7e14-131">In the `quickstart.auth` file, there's a JSON key for `clientId`.</span></span> <span data-ttu-id="b7e14-132">Kopieren Sie diesen Wert in die Umgebungsvariable `CLIENT_ID_VALUE`, und führen Sie den folgenden Azure CLI-Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="b7e14-132">Copy this value to the `CLIENT_ID_VALUE` environment variable and run the following Azure CLI command:</span></span>

```azurecli-interactive
az ad sp delete --id ${CLIENT_ID_VALUE}
```

<span data-ttu-id="b7e14-133">Sie geben den Wert für `CLIENT_ID_VALUE` aus `quickstart.auth` an.</span><span class="sxs-lookup"><span data-stu-id="b7e14-133">Where you supply the value for `CLIENT_ID_VALUE` from `quickstart.auth`.</span></span>

> [!WARNING]
> <span data-ttu-id="b7e14-134">Wird der Dienstprinzipal für diese Anwendung nicht gelöscht, bleibt er in Ihrem Azure Active Directory-Mandanten aktiv.</span><span class="sxs-lookup"><span data-stu-id="b7e14-134">Failing to delete the service principal for this application leaves it active in your Azure Active Directory tenant.</span></span>
> <span data-ttu-id="b7e14-135">Sowohl der Name als auch das Kennwort für den Dienstprinzipal werden als UUIDs generiert. Löschen Sie trotzdem aus Sicherheitsgründen unbedingt alle nicht verwendeten Dienstprinzipale und Azure Active Directory-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-135">While both the name and password for the service principal are generated as UUIDs, make sure that you follow good security practices by deleting any unused service principals and Azure Active Directory Applications.</span></span>

## <a name="code-in-depth"></a><span data-ttu-id="b7e14-136">Ausführliche Informationen zum Code</span><span class="sxs-lookup"><span data-stu-id="b7e14-136">Code in depth</span></span>

<span data-ttu-id="b7e14-137">Der Code des Schnellstarts wurde in einen Block mit Variablen und mehreren kleineren Funktionen unterteilt, die hier einzeln beschrieben sind.</span><span class="sxs-lookup"><span data-stu-id="b7e14-137">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="b7e14-138">Variablen, Konstanten und Typen</span><span class="sxs-lookup"><span data-stu-id="b7e14-138">Variables, constants, and types</span></span>

<span data-ttu-id="b7e14-139">Diese Schnellstartanleitung ist in sich geschlossen und verwendet daher globale Konstanten und Variablen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-139">Since quickstart is self-contained, it uses global constants and variables.</span></span>

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

<span data-ttu-id="b7e14-140">Es werden Werte deklariert, mit denen die Namen der erstellten Ressourcen angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="b7e14-140">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="b7e14-141">Außerdem wird hier der Standort angegeben. Sie können ihn ändern, um zu ermitteln, wie sich Bereitstellungen in anderen Rechenzentren verhalten.</span><span class="sxs-lookup"><span data-stu-id="b7e14-141">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="b7e14-142">Nicht jedes Rechenzentrum verfügt über alle erforderlichen Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-142">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="b7e14-143">Der Typ `clientInfo` enthält die Informationen, die aus der Authentifizierungsdatei geladen werden, um Clients im SDK einzurichten und das Kennwort für den virtuellen Computer festzulegen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-143">The `clientInfo` type holds the information loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="b7e14-144">Die Konstanten `templateFile` und `parametersFile` verweisen auf die Dateien, die für die Bereitstellung benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="b7e14-144">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="b7e14-145">`authorizer` wird durch das Go SDK für die Authentifizierung konfiguriert. Bei der Variablen `ctx` handelt es sich um einen [Go-Kontext](https://blog.golang.org/context) für die Netzwerkvorgänge.</span><span class="sxs-lookup"><span data-stu-id="b7e14-145">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="b7e14-146">Authentifizierung und Initialisierung</span><span class="sxs-lookup"><span data-stu-id="b7e14-146">Authentication and initialization</span></span>

<span data-ttu-id="b7e14-147">Die Funktion `init` richtet die Authentifizierung ein.</span><span class="sxs-lookup"><span data-stu-id="b7e14-147">The `init` function sets up authentication.</span></span> <span data-ttu-id="b7e14-148">Da die Authentifizierung eine Voraussetzung für den Rest der Schnellstartanleitung ist, empfiehlt es sich, sie in die Initialisierung zu integrieren.</span><span class="sxs-lookup"><span data-stu-id="b7e14-148">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="b7e14-149">Darüber hinaus werden auch einige Informationen aus der Authentifizierungsdatei geladen, die benötigt werden, um Clients und den virtuellen Computer zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="b7e14-149">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

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

<span data-ttu-id="b7e14-150">Als Erstes wird [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) aufgerufen, um die Authentifizierungsinformationen aus der Datei unter `AZURE_AUTH_LOCATION` zu laden.</span><span class="sxs-lookup"><span data-stu-id="b7e14-150">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="b7e14-151">Danach wird diese Datei manuell durch die (hier weggelassene) Funktion `readJSON` geladen, um die beiden Werte abzurufen, die zum Ausführen des restlichen Programms benötigt werden: die Abonnement-ID des Clients und das Geheimnis des Dienstprinzipals, das auch für das Kennwort des virtuellen Computers verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b7e14-151">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="b7e14-152">Der Einfachheit halber wird das Dienstprinzipalkennwort in dieser Schnellstartanleitung wiederverwendet.</span><span class="sxs-lookup"><span data-stu-id="b7e14-152">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="b7e14-153">In einer Produktionsumgebung darf ein Kennwort für den Zugriff auf Ihre Azure-Ressourcen __niemals__ wiederverwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b7e14-153">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="b7e14-154">Vorgangsablauf in main()</span><span class="sxs-lookup"><span data-stu-id="b7e14-154">Flow of operations in main()</span></span>

<span data-ttu-id="b7e14-155">Die Funktion `main` ist einfach aufgebaut. Sie gibt nur den Vorgangsablauf an und führt die Überprüfung auf Fehler durch.</span><span class="sxs-lookup"><span data-stu-id="b7e14-155">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="b7e14-156">Im Code werden die folgenden Schritte ausgeführt (in dieser Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="b7e14-156">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="b7e14-157">Erstellen der Ressourcengruppe für die Bereitstellung (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="b7e14-157">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="b7e14-158">Erstellen der Bereitstellung in dieser Gruppe (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="b7e14-158">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="b7e14-159">Abrufen und Anzeigen von Anmeldeinformationen für den bereitgestellte virtuellen Computer (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="b7e14-159">Get and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="create-the-resource-group"></a><span data-ttu-id="b7e14-160">Ressourcengruppe erstellen</span><span class="sxs-lookup"><span data-stu-id="b7e14-160">Create the resource group</span></span>

<span data-ttu-id="b7e14-161">Mit der Funktion `createGroup` wird die Ressourcengruppe erstellt.</span><span class="sxs-lookup"><span data-stu-id="b7e14-161">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="b7e14-162">Wenn Sie sich den Ablauf der Aufrufe und die Argumente ansehen, wird deutlich, wie Dienstinteraktionen im SDK strukturiert sind.</span><span class="sxs-lookup"><span data-stu-id="b7e14-162">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="b7e14-163">Dies ist der allgemeine Ablauf der Interaktion mit einem Azure-Dienst:</span><span class="sxs-lookup"><span data-stu-id="b7e14-163">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="b7e14-164">Erstellen Sie den Client mit der `service.New*Client()`-Methode, wobei `*` der Ressourcentyp des `service`-Elements ist, mit dem Sie interagieren möchten.</span><span class="sxs-lookup"><span data-stu-id="b7e14-164">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="b7e14-165">Für diese Funktion wird immer eine Abonnement-ID verwendet.</span><span class="sxs-lookup"><span data-stu-id="b7e14-165">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="b7e14-166">Legen Sie die Autorisierungsmethode für den Client fest, damit dieser mit der Remote-API interagieren kann.</span><span class="sxs-lookup"><span data-stu-id="b7e14-166">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="b7e14-167">Führen Sie den Methodenaufruf auf dem Client gemäß der Remote-API durch.</span><span class="sxs-lookup"><span data-stu-id="b7e14-167">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="b7e14-168">Für Dienstclientmethoden werden normalerweise der Name der Ressource und ein Metadatenobjekt verwendet.</span><span class="sxs-lookup"><span data-stu-id="b7e14-168">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="b7e14-169">Die Funktion [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) wird verwendet, um hier eine Typkonvertierung durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-169">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="b7e14-170">Die Parameter für SDK-Methoden verwenden fast ausschließlich Zeiger. Daher werden Hilfsmethoden zur Vereinfachung der Typkonvertierungen bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="b7e14-170">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="b7e14-171">In der Dokumentation für das Modul [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) finden Sie die vollständige Liste benutzerfreundlicher Konverter sowie Informationen zu ihrem Verhalten.</span><span class="sxs-lookup"><span data-stu-id="b7e14-171">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="b7e14-172">Die Methode `groupsClient.CreateOrUpdate` gibt einen Zeiger auf einen Datentyp zurück, der die Ressourcengruppe darstellt.</span><span class="sxs-lookup"><span data-stu-id="b7e14-172">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="b7e14-173">Ein direkter Rückgabewert dieser Art weist auf einen Vorgang mit kurzer Ausführungsdauer hin, der synchron ablaufen soll.</span><span class="sxs-lookup"><span data-stu-id="b7e14-173">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="b7e14-174">Der nächste Abschnitt enthält ein Beispiel für einen Vorgang mit langer Ausführungsdauer sowie Informationen zur Interaktion.</span><span class="sxs-lookup"><span data-stu-id="b7e14-174">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="perform-the-deployment"></a><span data-ttu-id="b7e14-175">Ausführen der Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="b7e14-175">Perform the deployment</span></span>

<span data-ttu-id="b7e14-176">Nach der Erstellung der Ressourcengruppe kann die Bereitstellung durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="b7e14-176">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="b7e14-177">Dieser Code ist in kleinere Abschnitte unterteilt, um unterschiedliche Teile der Logik herauszustellen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-177">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

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

<span data-ttu-id="b7e14-178">Die Bereitstellungsdateien werden mit `readJSON` geladen. Auf die Details hierzu wird hier nicht näher eingegangen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-178">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="b7e14-179">Diese Funktion gibt ein `*map[string]interface{}`-Element zurück. Dieser Typ wird beim Erstellen der Metadaten für den Aufruf zur Ressourcenbereitstellung verwendet.</span><span class="sxs-lookup"><span data-stu-id="b7e14-179">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="b7e14-180">In den Bereitstellungsparametern wird auch das Kennwort des virtuellen Computers manuell festgelegt.</span><span class="sxs-lookup"><span data-stu-id="b7e14-180">The VM's password is also set manually on the deployment parameters.</span></span>

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

<span data-ttu-id="b7e14-181">Der Code basiert auf dem gleichen Muster wie die Erstellung der Ressourcengruppe.</span><span class="sxs-lookup"><span data-stu-id="b7e14-181">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="b7e14-182">Es wird ein neuer Client erstellt, für den die Authentifizierung mit Azure ermöglicht wird, und anschließend wird eine Methode aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-182">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span>
<span data-ttu-id="b7e14-183">Die Methode hat sogar den gleichen Namen (`CreateOrUpdate`) wie die entsprechende Methode für Ressourcengruppen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-183">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="b7e14-184">Dieses Muster zieht sich durch das gesamte SDK.</span><span class="sxs-lookup"><span data-stu-id="b7e14-184">This pattern is seen throughout the SDK.</span></span>
<span data-ttu-id="b7e14-185">Methoden, die ähnliche Aufgaben haben, haben normalerweise auch die gleichen Namen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-185">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="b7e14-186">Der größte Unterschied ist der Rückgabewert der `deploymentsClient.CreateOrUpdate`-Methode.</span><span class="sxs-lookup"><span data-stu-id="b7e14-186">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="b7e14-187">Hierbei handelt es sich um einen Wert vom Typ [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), der auf dem [Entwurfsmuster mit „Futures“](https://en.wikipedia.org/wiki/Futures_and_promises) basiert.</span><span class="sxs-lookup"><span data-stu-id="b7e14-187">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="b7e14-188">Werte vom Typ „Future“ stellen einen Vorgang mit langer Ausführungsdauer in Azure dar, den Sie bei Abschluss abfragen, abbrechen oder blockieren können.</span><span class="sxs-lookup"><span data-stu-id="b7e14-188">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="b7e14-189">In diesem Beispiel warten Sie am besten, bis der Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="b7e14-189">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="b7e14-190">Wenn auf einen Wert vom Typ „Future“ gewartet werden soll, benötigen Sie sowohl ein [context-Objekt](https://blog.golang.org/context) als auch den Client, der den Wert vom Typ `Future` erstellt hat.</span><span class="sxs-lookup"><span data-stu-id="b7e14-190">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="b7e14-191">Hierbei gibt es zwei mögliche Fehlerquellen: Ein auf Clientseite verursachter Fehler, wenn versucht wird, die Methode aufzurufen, und eine Fehlerantwort vom Server.</span><span class="sxs-lookup"><span data-stu-id="b7e14-191">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="b7e14-192">Letztere wird als Teil des Aufrufs `deploymentFuture.Result` zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="b7e14-192">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

### <a name="get-the-assigned-ip-address"></a><span data-ttu-id="b7e14-193">Abrufen der zugewiesenen IP-Adresse</span><span class="sxs-lookup"><span data-stu-id="b7e14-193">Get the assigned IP address</span></span>

<span data-ttu-id="b7e14-194">Sie benötigen die zugewiesene IP-Adresse, um die neu erstellte VM nutzen zu können.</span><span class="sxs-lookup"><span data-stu-id="b7e14-194">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="b7e14-195">IP-Adressen stellen eine eigene separate Azure-Ressource dar, die an NIC-Ressourcen (Network Interface Controller) gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="b7e14-195">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="b7e14-196">Für diese Methode werden die Informationen benötigt, die in der Parameterdatei gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="b7e14-196">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="b7e14-197">Mit dem Code kann die VM direkt abgefragt werden, um ihre NIC abzurufen, und dann die NIC zum Abrufen ihrer IP-Ressource und anschließend die IP-Ressource direkt abgefragt werden.</span><span class="sxs-lookup"><span data-stu-id="b7e14-197">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="b7e14-198">Dies ist eine lange Kette von Abhängigkeiten und Vorgängen, die abgearbeitet werden müssen und zu einem hohen Kostenaufwand führen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-198">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="b7e14-199">Da die JSON-Informationen lokal vorliegen, können sie stattdessen geladen werden.</span><span class="sxs-lookup"><span data-stu-id="b7e14-199">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="b7e14-200">Der Wert für den Benutzer des virtuellen Computers wird ebenfalls aus dem JSON-Code geladen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-200">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="b7e14-201">Das Kennwort des virtuellen Computers wurde bereits aus der Authentifizierungsdatei geladen.</span><span class="sxs-lookup"><span data-stu-id="b7e14-201">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7e14-202">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="b7e14-202">Next steps</span></span>

<span data-ttu-id="b7e14-203">In diesem Schnellstart haben Sie eine vorhandene Vorlage verwendet und mit Go bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="b7e14-203">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="b7e14-204">Anschließend haben Sie per SSH eine Verbindung mit dem neu erstellten virtuellen Computer hergestellt.</span><span class="sxs-lookup"><span data-stu-id="b7e14-204">Then you connected to the newly created VM via SSH.</span></span>

<span data-ttu-id="b7e14-205">Wenn Sie sich weiter über die Arbeit mit virtuellen Computern in der Azure-Umgebung mit Go informieren möchten, helfen Ihnen die Informationen unter [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) (Azure-Computebeispiele für Go) und [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources) (Azure-Ressourcenverwaltungsbeispiele für Go) weiter.</span><span class="sxs-lookup"><span data-stu-id="b7e14-205">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="b7e14-206">Weitere Informationen zu den verfügbaren Authentifizierungsmethoden des SDKs sowie zu den unterstützten Authentifizierungstypen finden Sie unter [Authentication methods in the Azure SDK for Go](azure-sdk-go-authorization.md) (Authentifizierungsmethoden im Azure SDK für Go).</span><span class="sxs-lookup"><span data-stu-id="b7e14-206">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>
