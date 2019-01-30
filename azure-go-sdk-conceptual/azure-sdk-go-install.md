---
title: Installieren des Azure SDK für Go
description: Es wird beschrieben, wie Sie das Azure SDK für Go installieren und konfigurieren und das Vendoring dafür durchführen.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/14/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 2799e3a6c637036eeaf7b20adf8aa55a8a4ab400
ms.sourcegitcommit: 4db332f5e43a5b43032ff9017805d5fd5a650d86
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55145531"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="0a3d4-103">Installieren des Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="0a3d4-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="0a3d4-104">Willkommen beim Azure SDK für Go!</span><span class="sxs-lookup"><span data-stu-id="0a3d4-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="0a3d4-105">Mit diesem SDK können Sie Azure-Dienste über Ihre Go-Anwendungen verwalten und mit ihnen interagieren.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="0a3d4-106">Abrufen des Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="0a3d4-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="0a3d4-107">Einige Azure-Dienste verfügen über ein eigenes Go SDK und sind nicht im Kernpaket von Azure SDK für Go enthalten.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="0a3d4-108">In der folgenden Tabelle sind die Dienste mit eigenen SDKs und deren Paketnamen aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="0a3d4-109">Alle diese Pakete gelten als Vorschauversion.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="0a3d4-110">Dienst</span><span class="sxs-lookup"><span data-stu-id="0a3d4-110">Service</span></span> | <span data-ttu-id="0a3d4-111">Paket</span><span class="sxs-lookup"><span data-stu-id="0a3d4-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="0a3d4-112">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="0a3d4-112">Blob Storage</span></span> | [<span data-ttu-id="0a3d4-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="0a3d4-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="0a3d4-114">File Storage</span><span class="sxs-lookup"><span data-stu-id="0a3d4-114">File Storage</span></span> | [<span data-ttu-id="0a3d4-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="0a3d4-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="0a3d4-116">Speicherwarteschlange</span><span class="sxs-lookup"><span data-stu-id="0a3d4-116">Storage Queue</span></span> | [<span data-ttu-id="0a3d4-117">github.com/Azure/azure-storage-queue-go</span><span class="sxs-lookup"><span data-stu-id="0a3d4-117">github.com/Azure/azure-storage-queue-go</span></span>](https://github.com/Azure/azure-storage-queue-go) |
| <span data-ttu-id="0a3d4-118">Event Hub</span><span class="sxs-lookup"><span data-stu-id="0a3d4-118">Event Hub</span></span> | [<span data-ttu-id="0a3d4-119">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="0a3d4-119">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="0a3d4-120">Service Bus</span><span class="sxs-lookup"><span data-stu-id="0a3d4-120">Service Bus</span></span> | [<span data-ttu-id="0a3d4-121">github.com/Azure/azure-service-bus-go</span><span class="sxs-lookup"><span data-stu-id="0a3d4-121">github.com/Azure/azure-service-bus-go</span></span>](https://github.com/Azure/azure-service-bus-go) |
| <span data-ttu-id="0a3d4-122">Application Insights</span><span class="sxs-lookup"><span data-stu-id="0a3d4-122">Application Insights</span></span> | [<span data-ttu-id="0a3d4-123">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="0a3d4-123">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="0a3d4-124">Durchführen des Vendorings für das Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="0a3d4-124">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="0a3d4-125">Das Vendoring für das Azure SDK für Go kann mit [dep](https://github.com/golang/dep) durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-125">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="0a3d4-126">Das Vendoring wird aus Stabilitätsgründen empfohlen.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-126">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="0a3d4-127">Fügen Sie zur Verwendung von `dep` in Ihrem eigenen Projekt `github.com/Azure/azure-sdk-for-go` zum Abschnitt `[[constraint]]` in `Gopkg.toml` hinzu.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-127">To use `dep` in your own project, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="0a3d4-128">Fügen Sie beispielsweise den folgenden Eintrag hinzu, um das Vendoring für Version `14.0.0` durchzuführen:</span><span class="sxs-lookup"><span data-stu-id="0a3d4-128">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="0a3d4-129">Integrieren des Azure SDK für Go in Ihr Projekt</span><span class="sxs-lookup"><span data-stu-id="0a3d4-129">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="0a3d4-130">Importieren Sie beliebige Dienste, mit denen Sie interagieren, und die erforderlichen `autorest`-Module, um Azure-Dienste in Ihrem Go-Code zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-130">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="0a3d4-131">Sie erhalten eine vollständige Liste mit den verfügbaren Modulen von GoDoc für [verfügbare Dienste](https://godoc.org/github.com/Azure/azure-sdk-for-go) und [AutoRest-Pakete](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="0a3d4-131">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="0a3d4-132">Die am häufigsten verwendeten Pakete, die Sie für `go-autorest` benötigen, sind:</span><span class="sxs-lookup"><span data-stu-id="0a3d4-132">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="0a3d4-133">Paket</span><span class="sxs-lookup"><span data-stu-id="0a3d4-133">Package</span></span> | <span data-ttu-id="0a3d4-134">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="0a3d4-134">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="0a3d4-135">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="0a3d4-135">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="0a3d4-136">Objekte zur Verarbeitung der Dienstclientauthentifizierung</span><span class="sxs-lookup"><span data-stu-id="0a3d4-136">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="0a3d4-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="0a3d4-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="0a3d4-138">Konstanten für Interaktionen mit Azure-Diensten</span><span class="sxs-lookup"><span data-stu-id="0a3d4-138">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="0a3d4-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="0a3d4-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="0a3d4-140">Authentifizierungsmechanismen für den Zugriff auf Azure-Dienste</span><span class="sxs-lookup"><span data-stu-id="0a3d4-140">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="0a3d4-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="0a3d4-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="0a3d4-142">Hilfsprogramme für die Typassertion für die Arbeit mit Azure SDK-Datenstrukturen</span><span class="sxs-lookup"><span data-stu-id="0a3d4-142">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="0a3d4-143">Die Versionsangaben von Go-Paketen und Azure-Diensten sind voneinander unabhängig.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-143">Go packages and Azure services are versioned independently.</span></span> <span data-ttu-id="0a3d4-144">Die Dienstversionen sind Teil des Modulimportpfads im Modul `services`.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-144">The service versions are part of the module import path, underneath the `services` module.</span></span> <span data-ttu-id="0a3d4-145">Der vollständige Pfad für das Modul ist der Name des Diensts, gefolgt von der Version im Format `YYYY-MM-DD`, worauf erneut der Dienstname folgt.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-145">The full path for the module is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="0a3d4-146">Beispiel für den Import der Version `2017-03-30` des Computediensts:</span><span class="sxs-lookup"><span data-stu-id="0a3d4-146">For example, to import the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="0a3d4-147">Es wird empfohlen, die aktuelle Version eines Diensts zu verwenden, wenn Sie mit der Entwicklung beginnen, und die Versionen konsistent zu halten.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-147">It's recommended that you use the latest version of a service when starting development and keep it consistent.</span></span>
<span data-ttu-id="0a3d4-148">Dienstanforderungen können sich zwischen Versionen ändern und zu Codefehlern führen, obwohl das Go SDK gar nicht aktualisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-148">Service requirements may change between versions that could break your code, even if there are no Go SDK updates during that time.</span></span>

<span data-ttu-id="0a3d4-149">Wenn Sie eine gemeinsame Momentaufnahme aller Dienste benötigen, können Sie auch eine einzelne Profilversion wählen.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-149">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="0a3d4-150">Das einzige gesperrte Profil ist derzeit Version `2017-03-09`, die unter Umständen nicht über die aktuellen Features der Dienste verfügt.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-150">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="0a3d4-151">Profile befinden sich unter dem Modul `profiles`, und die Version wird im Format `YYYY-MM-DD` angegeben.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-151">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="0a3d4-152">Dienste werden unter ihrer Profilversion gruppiert.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-152">Services are grouped under their profile version.</span></span> <span data-ttu-id="0a3d4-153">Beispiel für den Import des Verwaltungsmoduls für Azure-Ressourcen aus dem Profil `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="0a3d4-153">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="0a3d4-154">Außerdem sind die Profile `preview` und `latest` verfügbar.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-154">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="0a3d4-155">Ihre Verwendung ist nicht empfehlenswert.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-155">Using them is not recommended.</span></span> <span data-ttu-id="0a3d4-156">Diese Profile sind fortlaufende Versionen, und das Dienstverhalten kann sich im Laufe der Zeit ändern.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-156">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a3d4-157">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="0a3d4-157">Next steps</span></span>

<span data-ttu-id="0a3d4-158">Probieren Sie einen Schnellstart aus, um mit der Verwendung des Azure SDK für Go zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-158">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="0a3d4-159">Bereitstellen eines virtuellen Azure-Computers über eine Vorlage mit dem Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="0a3d4-159">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="0a3d4-160">Übertragen von Objekten nach/aus Azure Blob Storage mit Go</span><span class="sxs-lookup"><span data-stu-id="0a3d4-160">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="0a3d4-161">Herstellen einer Verbindung mit Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="0a3d4-161">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="0a3d4-162">Wenn Sie sofort mit anderen Diensten im Go SDK starten möchten, ist es ratsam, sich den verfügbaren Beispielcode anzusehen.</span><span class="sxs-lookup"><span data-stu-id="0a3d4-162">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="0a3d4-163">Authentifizieren mit Azure-Diensten</span><span class="sxs-lookup"><span data-stu-id="0a3d4-163">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/internal/iam)
* [<span data-ttu-id="0a3d4-164">Bereitstellen neuer virtueller Computer mit SSH-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="0a3d4-164">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="0a3d4-165">Bereitstellen eines Containerimages für Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="0a3d4-165">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="0a3d4-166">Erstellen eines Clusters im Azure Kubernetes-Dienst</span><span class="sxs-lookup"><span data-stu-id="0a3d4-166">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute)
* [<span data-ttu-id="0a3d4-167">Arbeiten mit Azure Storage-Diensten</span><span class="sxs-lookup"><span data-stu-id="0a3d4-167">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="0a3d4-168">Alle Beispiele für das Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="0a3d4-168">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
