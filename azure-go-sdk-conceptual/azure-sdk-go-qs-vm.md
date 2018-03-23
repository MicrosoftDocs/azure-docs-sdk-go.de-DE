---
title: Bereitstellen eines virtuellen Azure-Computers über Go
description: Stellen Sie einen virtuellen Computer bereit, indem Sie das Azure SDK für Go verwenden.
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 46a1243ff2ff6bfcf3831e2cea3137c1f6051c78
ms.sourcegitcommit: fcc1786d59d2e32c97a9a8e0748e06f564a961bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="6098a-103">Schnellstart: Bereitstellen eines virtuellen Azure-Computers über eine Vorlage mit dem Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="6098a-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="6098a-104">In diesem Schnellstart wird die Bereitstellung von Ressourcen über eine Vorlage mit dem Azure SDK für Go beschrieben.</span><span class="sxs-lookup"><span data-stu-id="6098a-104">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="6098a-105">Vorlagen sind Momentaufnahmen aller Ressourcen, die in einer [Azure-Ressourcengruppe](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="6098a-105">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="6098a-106">Sie können sich hier während der Durchführung einer nützlichen Aufgabe mit der Funktionalität und den Konventionen des SDK vertraut machen.</span><span class="sxs-lookup"><span data-stu-id="6098a-106">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="6098a-107">Am Ende dieses Schnellstarts verfügen Sie über eine aktive VM, an der Sie sich mit einem Benutzernamen und Kennwort anmelden können.</span><span class="sxs-lookup"><span data-stu-id="6098a-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="6098a-108">Falls Sie eine lokale Installation der Azure CLI verwenden, ist für diesen Schnellstart die CLI-Version 2.0.24 oder höher erforderlich.</span><span class="sxs-lookup"><span data-stu-id="6098a-108">If you use a local install of the Azure CLI, this quickstart requires CLI version 2.0.24 or later.</span></span> <span data-ttu-id="6098a-109">Führen Sie `az --version` aus, um sicherzustellen, dass Ihre CLI diese Anforderung erfüllt.</span><span class="sxs-lookup"><span data-stu-id="6098a-109">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="6098a-110">Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6098a-110">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="6098a-111">Installieren des Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="6098a-111">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="6098a-112">Erstellen eines Dienstprinzipals</span><span class="sxs-lookup"><span data-stu-id="6098a-112">Create a service principal</span></span>

<span data-ttu-id="6098a-113">Wenn Sie sich nicht interaktiv mit einer Anwendung anmelden möchten, benötigen Sie einen Dienstprinzipal.</span><span class="sxs-lookup"><span data-stu-id="6098a-113">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="6098a-114">Dienstprinzipale sind Teil der rollenbasierten Zugriffssteuerung (RBAC), bei der eine eindeutige Benutzeridentität erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="6098a-114">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="6098a-115">Führen Sie den folgenden Befehl aus, um mit der CLI einen neuen Dienstprinzipal zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="6098a-115">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

<span data-ttu-id="6098a-116">__Stellen Sie sicher__, dass Sie sich die in der Ausgabe enthaltenen Werte für `appId`, `password` und `tenant` notieren.</span><span class="sxs-lookup"><span data-stu-id="6098a-116">__Make sure__ to record the `appId`, `password`, and `tenant` values in the output.</span></span> <span data-ttu-id="6098a-117">Diese Werte werden von der Anwendung für die Azure-Authentifizierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="6098a-117">These values are used by the application to authenticate with Azure.</span></span>

<span data-ttu-id="6098a-118">Weitere Informationen zum Erstellen und Verwalten von Dienstprinzipalen per Azure CLI 2.0 finden Sie unter [Erstellen eines Azure-Dienstprinzipals mit Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6098a-118">For more information on creating and managing service principals with the Azure CLI 2.0, see [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="6098a-119">Abrufen des Codes</span><span class="sxs-lookup"><span data-stu-id="6098a-119">Get the code</span></span>

<span data-ttu-id="6098a-120">Sie können den Schnellstartcode und alle Abhängigkeiten mit `go get` abrufen.</span><span class="sxs-lookup"><span data-stu-id="6098a-120">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

<span data-ttu-id="6098a-121">Dieser Code kann kompiliert werden, aber er wird erst richtig ausgeführt, nachdem Sie Informationen zu Ihrem Azure-Konto und zum erstellten Dienstprinzipal angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="6098a-121">This code compiles, but doesn't run correctly until you provide it information about your Azure account and the created service principal.</span></span> <span data-ttu-id="6098a-122">`main.go` enthält die Variable `config` mit der Struktur `authInfo`.</span><span class="sxs-lookup"><span data-stu-id="6098a-122">In `main.go` there is a variable, `config`, which contains an `authInfo` struct.</span></span> <span data-ttu-id="6098a-123">Für diese Struktur müssen die Feldwerte ersetzt werden, um richtig authentifiziert werden zu können.</span><span class="sxs-lookup"><span data-stu-id="6098a-123">This struct needs to have its field values replaced in order to authenticate correctly.</span></span>

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* <span data-ttu-id="6098a-124">`SubscriptionID`: Ihre Abonnement-ID, die mit dem CLI-Befehl abgerufen werden kann.</span><span class="sxs-lookup"><span data-stu-id="6098a-124">`SubscriptionID`: Your subscription ID, which can be obtained from the CLI command</span></span>

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* <span data-ttu-id="6098a-125">`TenantID`: Ihre Mandanten-ID. Dies ist der Wert `tenant`, den Sie sich beim Erstellen des Dienstprinzipals notiert haben.</span><span class="sxs-lookup"><span data-stu-id="6098a-125">`TenantID`: Your tenant ID, the `tenant` value recorded when creating the service principal</span></span>
* <span data-ttu-id="6098a-126">`ServicePrincipalID`: Der Wert `appId`, den Sie sich beim Erstellen des Dienstprinzipals notiert haben.</span><span class="sxs-lookup"><span data-stu-id="6098a-126">`ServicePrincipalID`: The `appId` value recorded when creating the service principal</span></span>
* <span data-ttu-id="6098a-127">`ServicePrincipalSecret`: Der Wert `password`, den Sie sich beim Erstellen des Dienstprinzipals notiert haben.</span><span class="sxs-lookup"><span data-stu-id="6098a-127">`ServicePrincipalSecret`: The `password` value recorded when creating the service principal</span></span>

<span data-ttu-id="6098a-128">Außerdem müssen Sie einen Wert in der Datei `vm-quickstart-params.json` bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="6098a-128">You also need to edit a value in the `vm-quickstart-params.json` file.</span></span>

```json
    "vm_password": {
        "value": "_"
    }
```

* <span data-ttu-id="6098a-129">`vm_password`: Das Kennwort für das VM-Benutzerkonto.</span><span class="sxs-lookup"><span data-stu-id="6098a-129">`vm_password`: The password for the VM user account.</span></span> <span data-ttu-id="6098a-130">Es muss eine Länge von 12 bis 72 Zeichen haben und mindestens drei verschiedene Arten der folgenden Zeichen enthalten:</span><span class="sxs-lookup"><span data-stu-id="6098a-130">It must be 12-72 characters in length and contain 3 of the following characters:</span></span>
  * <span data-ttu-id="6098a-131">Kleinbuchstabe</span><span class="sxs-lookup"><span data-stu-id="6098a-131">A lowercase letter</span></span>
  * <span data-ttu-id="6098a-132">Großbuchstabe</span><span class="sxs-lookup"><span data-stu-id="6098a-132">An uppercase letter</span></span>
  * <span data-ttu-id="6098a-133">Ziffer</span><span class="sxs-lookup"><span data-stu-id="6098a-133">A number</span></span>
  * <span data-ttu-id="6098a-134">Symbol</span><span class="sxs-lookup"><span data-stu-id="6098a-134">A symbol</span></span>

## <a name="running-the-code"></a><span data-ttu-id="6098a-135">Ausführen des Codes</span><span class="sxs-lookup"><span data-stu-id="6098a-135">Running the code</span></span>

<span data-ttu-id="6098a-136">Führen Sie den Schnellstart mit dem Befehl `go run` aus.</span><span class="sxs-lookup"><span data-stu-id="6098a-136">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

<span data-ttu-id="6098a-137">Falls für die Bereitstellung ein Fehler auftritt, wird eine Meldung mit einem Hinweis zu einem Problem angezeigt, aber es werden keine Details angegeben.</span><span class="sxs-lookup"><span data-stu-id="6098a-137">If there is a failure in the deployment, you get a message indicating that there was an issue, but without any specific details.</span></span> <span data-ttu-id="6098a-138">Rufen Sie die Details zum Bereitstellungsfehler über die Azure CLI mit dem folgenden Befehl ab:</span><span class="sxs-lookup"><span data-stu-id="6098a-138">Using the Azure CLI, get the details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="6098a-139">Wenn die Bereitstellung erfolgreich ist, erhalten Sie eine Meldung mit dem Benutzernamen, der IP-Adresse und dem Kennwort für die Anmeldung am neu erstellten virtuellen Computer.</span><span class="sxs-lookup"><span data-stu-id="6098a-139">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="6098a-140">Greifen Sie per SSH auf diesen Computer zu, um zu bestätigen, dass er aktiv ist und ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6098a-140">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="6098a-141">Bereinigen</span><span class="sxs-lookup"><span data-stu-id="6098a-141">Cleaning up</span></span>

<span data-ttu-id="6098a-142">Bereinigen Sie die Ressourcen, die für diesen Schnellstart erstellt wurden, indem Sie die Ressourcengruppe über die CLI löschen.</span><span class="sxs-lookup"><span data-stu-id="6098a-142">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="6098a-143">Ausführliche Informationen zum Code</span><span class="sxs-lookup"><span data-stu-id="6098a-143">Code in depth</span></span>

<span data-ttu-id="6098a-144">Der Code des Schnellstarts wurde in einen Block mit Variablen und mehreren kleineren Funktionen unterteilt, die hier einzeln beschrieben sind.</span><span class="sxs-lookup"><span data-stu-id="6098a-144">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variable-assignments-and-structs"></a><span data-ttu-id="6098a-145">Variablenzuweisungen und Strukturen</span><span class="sxs-lookup"><span data-stu-id="6098a-145">Variable assignments and structs</span></span>

<span data-ttu-id="6098a-146">Da der Schnellstart eigenständig ist, werden anstelle von Befehlszeilenoptionen oder Umgebungsvariablen globale Variablen verwendet.</span><span class="sxs-lookup"><span data-stu-id="6098a-146">Since quickstart is self-contained, it uses global variables rather than command-line options or environment variables.</span></span>

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

<span data-ttu-id="6098a-147">Die Struktur `authInfo` wird deklariert, um alle Informationen zu kapseln, die für die Autorisierung für Azure-Dienste benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="6098a-147">The `authInfo` struct is declared to encapsulate all of the information needed for authorization with Azure services.</span></span>

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

<span data-ttu-id="6098a-148">Es werden Werte deklariert, mit denen die Namen der erstellten Ressourcen angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="6098a-148">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="6098a-149">Außerdem wird hier der Standort angegeben. Sie können ihn ändern, um zu ermitteln, wie sich Bereitstellungen in anderen Rechenzentren verhalten.</span><span class="sxs-lookup"><span data-stu-id="6098a-149">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="6098a-150">Nicht jedes Rechenzentrum verfügt über alle erforderlichen Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="6098a-150">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="6098a-151">Die Konstanten `templateFile` und `parametersFile` verweisen auf die Dateien, die für die Bereitstellung benötigt werden.</span><span class="sxs-lookup"><span data-stu-id="6098a-151">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="6098a-152">Das Dienstprinzipaltoken wird später behandelt, und die Variable `ctx` ist ein [Go-Kontext](https://blog.golang.org/context) für die Netzwerkvorgänge.</span><span class="sxs-lookup"><span data-stu-id="6098a-152">The service principal token is covered later, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="init-and-authorization"></a><span data-ttu-id="6098a-153">init() und Autorisierung</span><span class="sxs-lookup"><span data-stu-id="6098a-153">init() and authorization</span></span>

<span data-ttu-id="6098a-154">Mit der `init()`-Methode für den Code wird die Autorisierung eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="6098a-154">The `init()` method for the code sets up authorization.</span></span> <span data-ttu-id="6098a-155">Da die Autorisierung eine Vorbedingung für alle Elemente des Schnellstarts ist, ist es sinnvoll, sie als Teil der Initialisierung zu betrachten.</span><span class="sxs-lookup"><span data-stu-id="6098a-155">Since authorization is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> 

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

<span data-ttu-id="6098a-156">Mit diesem Code werden zwei Schritte für die Autorisierung ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="6098a-156">This code completes two steps for authorization:</span></span>

* <span data-ttu-id="6098a-157">OAuth-Konfigurationsinformationen für die `TenantID` werden abgerufen, indem Azure Active Directory genutzt wird.</span><span class="sxs-lookup"><span data-stu-id="6098a-157">OAuth configuration information for the `TenantID` is retrieved by interfacing with Azure Active Directory.</span></span> <span data-ttu-id="6098a-158">Das [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud)-Objekt enthält Endpunkte, die in der Azure-Standardkonfiguration verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6098a-158">The [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) object contains endpoints used in the standard Azure configuration.</span></span>
* <span data-ttu-id="6098a-159">Die Funktion [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="6098a-159">The [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) function is called.</span></span> <span data-ttu-id="6098a-160">Bei dieser Funktion werden die OAuth-Informationen zusammen mit der Dienstprinzipalanmeldung verwendet, und es wird angegeben, welche Art von Azure-Verwaltung genutzt wird.</span><span class="sxs-lookup"><span data-stu-id="6098a-160">This function takes the OAuth information along with the service principal login, as well as which style of Azure management is being used.</span></span> <span data-ttu-id="6098a-161">Dieser Wert sollte immer `.ResourceManagerEndpoint` lauten, sofern bei Ihnen nicht besondere Anforderungen gelten und Sie wissen, was zu tun ist.</span><span class="sxs-lookup"><span data-stu-id="6098a-161">Unless you have specific requirements and know what you're doing, this value should always be `.ResourceManagerEndpoint`.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="6098a-162">Vorgangsablauf in main()</span><span class="sxs-lookup"><span data-stu-id="6098a-162">Flow of operations in main()</span></span>

<span data-ttu-id="6098a-163">Die Funktion `main()` ist einfach aufgebaut. Sie gibt nur den Vorgangsablauf an und führt die Überprüfung auf Fehler durch.</span><span class="sxs-lookup"><span data-stu-id="6098a-163">The `main()` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="6098a-164">Im Code werden die folgenden Schritte ausgeführt (in dieser Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="6098a-164">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="6098a-165">Erstellen der Ressourcengruppe für die Bereitstellung (`createGroup()`)</span><span class="sxs-lookup"><span data-stu-id="6098a-165">Create the resource group to deploy to (`createGroup()`)</span></span>
* <span data-ttu-id="6098a-166">Erstellen der Bereitstellung in dieser Gruppe (`createDeployment()`)</span><span class="sxs-lookup"><span data-stu-id="6098a-166">Create the deployment within this group (`createDeployment()`)</span></span>
* <span data-ttu-id="6098a-167">Abrufen und Anzeigen von Anmeldeinformationen für die bereitgestellte VM (`getLogin()`)</span><span class="sxs-lookup"><span data-stu-id="6098a-167">Obtain and display login information for the deployed VM (`getLogin()`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="6098a-168">Erstellen der Ressourcengruppe</span><span class="sxs-lookup"><span data-stu-id="6098a-168">Creating the resource group</span></span>

<span data-ttu-id="6098a-169">Mit der Funktion `createGroup()` wird die Ressourcengruppe erstellt.</span><span class="sxs-lookup"><span data-stu-id="6098a-169">The `createGroup()` function creates the resource group.</span></span> <span data-ttu-id="6098a-170">Wenn Sie sich den Ablauf der Aufrufe und die Argumente ansehen, wird deutlich, wie Dienstinteraktionen im SDK strukturiert sind.</span><span class="sxs-lookup"><span data-stu-id="6098a-170">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="6098a-171">Dies ist der allgemeine Ablauf der Interaktion mit einem Azure-Dienst:</span><span class="sxs-lookup"><span data-stu-id="6098a-171">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="6098a-172">Erstellen Sie den Client mit der `service.NewXClient()`-Methode, wobei `X` der Ressourcentyp des `service`-Elements ist, mit dem Sie interagieren möchten.</span><span class="sxs-lookup"><span data-stu-id="6098a-172">Create the client using the `service.NewXClient()` method, where `X` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="6098a-173">Für diese Funktion wird immer eine Abonnement-ID verwendet.</span><span class="sxs-lookup"><span data-stu-id="6098a-173">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="6098a-174">Legen Sie die Autorisierungsmethode für den Client fest, damit dieser mit der Remote-API interagieren kann.</span><span class="sxs-lookup"><span data-stu-id="6098a-174">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="6098a-175">Führen Sie den Methodenaufruf auf dem Client gemäß der Remote-API durch.</span><span class="sxs-lookup"><span data-stu-id="6098a-175">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="6098a-176">Für Dienstclientmethoden werden normalerweise der Name der Ressource und ein Metadatenobjekt verwendet.</span><span class="sxs-lookup"><span data-stu-id="6098a-176">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="6098a-177">Die Funktion [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) wird verwendet, um hier eine Typkonvertierung durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="6098a-177">The [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="6098a-178">Für die Parameterstrukturen für Methoden des SDK werden fast ausschließlich Zeiger genutzt, sodass diese Methoden bereitgestellt werden, um die Typkonvertierungen zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="6098a-178">The parameters structs for methods of the SDK almost exclusively take pointers, so these methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="6098a-179">In der Dokumentation für das Modul [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) finden Sie die vollständige Liste und Informationen zum Verhalten von benutzerfreundlichen Konvertern.</span><span class="sxs-lookup"><span data-stu-id="6098a-179">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list and behavior of convenience converters.</span></span>

<span data-ttu-id="6098a-180">Der Vorgang `groupsClient.CreateOrUpdate()` gibt einen Zeiger auf die Datenstruktur zurück, die für die Ressourcengruppe steht.</span><span class="sxs-lookup"><span data-stu-id="6098a-180">The `groupsClient.CreateOrUpdate()` operation returns a pointer to a data struct representing the resource group.</span></span> <span data-ttu-id="6098a-181">Ein direkter Rückgabewert dieser Art weist auf einen Vorgang mit kurzer Ausführungsdauer hin, der synchron ablaufen soll.</span><span class="sxs-lookup"><span data-stu-id="6098a-181">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="6098a-182">Der nächste Abschnitt enthält ein Beispiel für einen Vorgang mit langer Ausführungsdauer und Informationen zur Interaktion.</span><span class="sxs-lookup"><span data-stu-id="6098a-182">In the next section, you'll see an example of a long-running operation and how to interact with them.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="6098a-183">Durchführen der Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="6098a-183">Performing the deployment</span></span>

<span data-ttu-id="6098a-184">Nachdem die Gruppe für die Ressourcen erstellt wurde, kann die Bereitstellung ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="6098a-184">Once the group to contain its resources is created, it's time to run the deployment.</span></span> <span data-ttu-id="6098a-185">Dieser Code ist in kleinere Abschnitte unterteilt, um unterschiedliche Teile der Logik herauszustellen.</span><span class="sxs-lookup"><span data-stu-id="6098a-185">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>


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

<span data-ttu-id="6098a-186">Die Bereitstellungsdateien werden mit `readJSON` geladen. Auf die Details hierzu wird hier nicht näher eingegangen.</span><span class="sxs-lookup"><span data-stu-id="6098a-186">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="6098a-187">Diese Funktion gibt ein `*map[string]interface{}`-Element zurück. Dieser Typ wird beim Erstellen der Metadaten für den Aufruf zur Ressourcenbereitstellung verwendet.</span><span class="sxs-lookup"><span data-stu-id="6098a-187">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span>

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

<span data-ttu-id="6098a-188">Der Code basiert auf dem gleichen Muster wie bei der Erstellung der Ressourcengruppe.</span><span class="sxs-lookup"><span data-stu-id="6098a-188">This code follows the same pattern as with creating the resource group.</span></span> <span data-ttu-id="6098a-189">Es wird ein neuer Client erstellt, für den die Authentifizierung mit Azure ermöglicht wird, und anschließend wird eine Methode aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="6098a-189">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="6098a-190">Die Methode hat sogar den gleichen Namen (`CreateOrUpdate`) wie die entsprechende Methode für Ressourcengruppen.</span><span class="sxs-lookup"><span data-stu-id="6098a-190">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="6098a-191">Dieses Muster ist im SDK häufiger zu beobachten.</span><span class="sxs-lookup"><span data-stu-id="6098a-191">This pattern is seen again and again in the SDK.</span></span> <span data-ttu-id="6098a-192">Methoden, die ähnliche Aufgaben haben, haben normalerweise auch die gleichen Namen.</span><span class="sxs-lookup"><span data-stu-id="6098a-192">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="6098a-193">Der größte Unterschied ist der Rückgabewert der `deploymentsClient.CreateOrUpdate()`-Methode.</span><span class="sxs-lookup"><span data-stu-id="6098a-193">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate()` method.</span></span> <span data-ttu-id="6098a-194">Dieser Wert ist ein `Future`-Objekt, das auf dem [Entwurfsmuster mit „Futures“](https://en.wikipedia.org/wiki/Futures_and_promises) basiert.</span><span class="sxs-lookup"><span data-stu-id="6098a-194">This value is a `Future` object, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="6098a-195">Futures stehen für einen Vorgang mit langer Ausführungsdauer in Azure, der ggf. von Zeit zu Zeit abgefragt wird, während Sie andere Arbeitsschritte ausführen.</span><span class="sxs-lookup"><span data-stu-id="6098a-195">Futures represent a long-running operation in Azure that you may want to occasionally poll while performing other work.</span></span>

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="6098a-196">In diesem Beispiel warten Sie am besten, bis der Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="6098a-196">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="6098a-197">Für das Warten auf ein Future-Objekt ist sowohl ein [context-Objekt](https://blog.golang.org/context) als auch der Client erforderlich, der das Future-Objekt erstellt hat.</span><span class="sxs-lookup"><span data-stu-id="6098a-197">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the Future object.</span></span> <span data-ttu-id="6098a-198">Hierbei gibt es zwei mögliche Fehlerquellen: Ein auf Clientseite verursachter Fehler, wenn versucht wird, die Methode aufzurufen, und eine Fehlerantwort vom Server.</span><span class="sxs-lookup"><span data-stu-id="6098a-198">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="6098a-199">Letztere wird als Teil des Aufrufs `deploymentFuture.Result()` zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="6098a-199">The latter is returned as part of the `deploymentFuture.Result()` call.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="6098a-200">Abrufen der zugewiesenen IP-Adresse</span><span class="sxs-lookup"><span data-stu-id="6098a-200">Obtaining the assigned IP address</span></span>

<span data-ttu-id="6098a-201">Sie benötigen die zugewiesene IP-Adresse, um die neu erstellte VM nutzen zu können.</span><span class="sxs-lookup"><span data-stu-id="6098a-201">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="6098a-202">IP-Adressen stellen eine eigene separate Azure-Ressource dar, die an NIC-Ressourcen (Network Interface Controller) gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="6098a-202">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="6098a-203">Für diese Methode werden die Informationen benötigt, die in der Parameterdatei gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="6098a-203">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="6098a-204">Mit dem Code kann die VM direkt abgefragt werden, um ihre NIC abzurufen, und dann die NIC zum Abrufen ihrer IP-Ressource und anschließend die IP-Ressource direkt abgefragt werden.</span><span class="sxs-lookup"><span data-stu-id="6098a-204">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="6098a-205">Dies ist eine lange Kette von Abhängigkeiten und Vorgängen, die abgearbeitet werden müssen und zu einem hohen Kostenaufwand führen.</span><span class="sxs-lookup"><span data-stu-id="6098a-205">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="6098a-206">Da die JSON-Informationen lokal vorliegen, können sie stattdessen geladen werden.</span><span class="sxs-lookup"><span data-stu-id="6098a-206">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="6098a-207">Die Werte für den VM-Benutzer und das dazugehörige Kennwort werden ebenfalls per JSON geladen.</span><span class="sxs-lookup"><span data-stu-id="6098a-207">The values for the VM user and password are likewise loaded from the JSON.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6098a-208">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="6098a-208">Next steps</span></span>

<span data-ttu-id="6098a-209">In diesem Schnellstart haben Sie eine vorhandene Vorlage verwendet und mit Go bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="6098a-209">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="6098a-210">Anschließend haben Sie per SSH eine Verbindung mit der neu erstellten VM hergestellt, um sicherzustellen, dass sie ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6098a-210">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="6098a-211">Wenn Sie sich weiter über die Arbeit mit virtuellen Computern in der Azure-Umgebung mit Go informieren möchten, helfen Ihnen die Informationen unter [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) (Azure-Computebeispiele für Go) und [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources) (Azure-Ressourcenverwaltungsbeispiele für Go) weiter.</span><span class="sxs-lookup"><span data-stu-id="6098a-211">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>
