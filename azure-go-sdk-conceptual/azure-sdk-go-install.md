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
ms.openlocfilehash: ad77bdff881770512a828b19dc7af4821f4a55ad
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="0baa4-103">Installieren des Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="0baa4-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="0baa4-104">Willkommen beim Azure SDK für Go!</span><span class="sxs-lookup"><span data-stu-id="0baa4-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="0baa4-105">Mit diesem SDK können Sie Azure-Dienste über Ihre Go-Anwendungen verwalten und mit ihnen interagieren.</span><span class="sxs-lookup"><span data-stu-id="0baa4-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="0baa4-106">Abrufen des Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="0baa4-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="0baa4-107">Für die Arbeit mit Azure Storage Blobs ist ein separates SDK erforderlich.</span><span class="sxs-lookup"><span data-stu-id="0baa4-107">Working with Azure Storage Blobs requires a separate SDK.</span></span>

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="0baa4-108">Durchführen des Vendorings für das Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="0baa4-108">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="0baa4-109">Das Vendoring für das Azure SDK für Go kann mit [dep](https://github.com/golang/dep) durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="0baa4-109">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="0baa4-110">Das Vendoring wird aus Stabilitätsgründen empfohlen.</span><span class="sxs-lookup"><span data-stu-id="0baa4-110">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="0baa4-111">Fügen Sie `github.com/Azure/azure-sdk-for-go` einem `[[constraint]]`-Abschnitt der Datei `Gopkg.toml` hinzu, um die Unterstützung für `dep` zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="0baa4-111">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="0baa4-112">Fügen Sie beispielsweise den folgenden Eintrag hinzu, um das Vendoring für Version `14.0.0` durchzuführen:</span><span class="sxs-lookup"><span data-stu-id="0baa4-112">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="0baa4-113">Integrieren des Azure SDK für Go in Ihr Projekt</span><span class="sxs-lookup"><span data-stu-id="0baa4-113">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="0baa4-114">Importieren Sie beliebige Dienste, mit denen Sie interagieren, und die erforderlichen `autorest`-Module, um Azure-Dienste in Ihrem Go-Code zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="0baa4-114">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="0baa4-115">Sie erhalten eine vollständige Liste mit den verfügbaren Modulen von GoDoc für [verfügbare Dienste](https://godoc.org/github.com/Azure/azure-sdk-for-go) und [AutoRest-Pakete](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="0baa4-115">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="0baa4-116">Die am häufigsten verwendeten Pakete, die Sie für `go-autorest` benötigen, sind:</span><span class="sxs-lookup"><span data-stu-id="0baa4-116">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="0baa4-117">Paket</span><span class="sxs-lookup"><span data-stu-id="0baa4-117">Package</span></span> | <span data-ttu-id="0baa4-118">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="0baa4-118">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="0baa4-119">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="0baa4-119">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="0baa4-120">Objekte zur Verarbeitung der Dienstclientauthentifizierung</span><span class="sxs-lookup"><span data-stu-id="0baa4-120">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="0baa4-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="0baa4-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="0baa4-122">Konstanten für Interaktionen mit Azure-Diensten</span><span class="sxs-lookup"><span data-stu-id="0baa4-122">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="0baa4-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="0baa4-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="0baa4-124">Authentifizierungsmechanismen für den Zugriff auf Azure-Dienste</span><span class="sxs-lookup"><span data-stu-id="0baa4-124">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="0baa4-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="0baa4-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="0baa4-126">Hilfsprogramme für die Typassertion für die Arbeit mit Azure SDK-Datenstrukturen</span><span class="sxs-lookup"><span data-stu-id="0baa4-126">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="0baa4-127">Module für Azure-Dienste werden unabhängig von den dazugehörigen SDK-APIs versioniert.</span><span class="sxs-lookup"><span data-stu-id="0baa4-127">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="0baa4-128">Diese Versionen sind Teil des Modulimportpfads und stammen entweder aus einer _Dienstversion_ oder einem _Profil_.</span><span class="sxs-lookup"><span data-stu-id="0baa4-128">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="0baa4-129">Derzeit lautet die Empfehlung, sowohl für die Entwicklung als auch für das Release eine bestimmte Dienstversion zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="0baa4-129">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="0baa4-130">Dienste befinden sich unter dem `services`-Modul.</span><span class="sxs-lookup"><span data-stu-id="0baa4-130">Services are located under the `services` module.</span></span> <span data-ttu-id="0baa4-131">Der vollständige Pfad für den Import ist der Name des Diensts, gefolgt von der Version im Format `YYYY-MM-DD`, worauf erneut der Dienstname folgt.</span><span class="sxs-lookup"><span data-stu-id="0baa4-131">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="0baa4-132">Beispiel für die Einbindung der Version `2017-03-30` des Computediensts:</span><span class="sxs-lookup"><span data-stu-id="0baa4-132">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="0baa4-133">Derzeit wird empfohlen, die aktuelle Version eines Diensts zu verwenden, sofern nicht ein triftiger Grund für eine andere Vorgehensweise vorliegt.</span><span class="sxs-lookup"><span data-stu-id="0baa4-133">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="0baa4-134">Wenn Sie eine gemeinsame Momentaufnahme aller Dienste benötigen, können Sie auch eine einzelne Profilversion wählen.</span><span class="sxs-lookup"><span data-stu-id="0baa4-134">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="0baa4-135">Das einzige gesperrte Profil ist derzeit Version `2017-03-09`, die unter Umständen nicht über die aktuellen Features der Dienste verfügt.</span><span class="sxs-lookup"><span data-stu-id="0baa4-135">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="0baa4-136">Profile befinden sich unter dem Modul `profiles`, und die Version wird im Format `YYYY-MM-DD` angegeben.</span><span class="sxs-lookup"><span data-stu-id="0baa4-136">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="0baa4-137">Dienste werden unter ihrer Profilversion gruppiert.</span><span class="sxs-lookup"><span data-stu-id="0baa4-137">Services are grouped under their profile version.</span></span> <span data-ttu-id="0baa4-138">Beispiel für den Import des Verwaltungsmoduls für Azure-Ressourcen aus dem Profil `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="0baa4-138">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="0baa4-139">Außerdem sind die Profile `preview` und `latest` verfügbar.</span><span class="sxs-lookup"><span data-stu-id="0baa4-139">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="0baa4-140">Ihre Verwendung ist nicht empfehlenswert.</span><span class="sxs-lookup"><span data-stu-id="0baa4-140">Using them is not recommended.</span></span> <span data-ttu-id="0baa4-141">Diese Profile sind fortlaufende Versionen, und das Dienstverhalten kann sich im Laufe der Zeit ändern.</span><span class="sxs-lookup"><span data-stu-id="0baa4-141">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0baa4-142">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="0baa4-142">Next steps</span></span>

<span data-ttu-id="0baa4-143">Probieren Sie einen Schnellstart aus, um mit der Verwendung des Azure SDK für Go zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="0baa4-143">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="0baa4-144">Bereitstellen eines virtuellen Azure-Computers über eine Vorlage mit dem Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="0baa4-144">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="0baa4-145">Übertragen von Objekten nach/aus Azure Blob Storage mit Go</span><span class="sxs-lookup"><span data-stu-id="0baa4-145">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="0baa4-146">Herstellen einer Verbindung mit Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="0baa4-146">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="0baa4-147">Wenn Sie sofort mit anderen Diensten im Go SDK starten möchten, ist es ratsam, sich den verfügbaren Beispielcode anzusehen.</span><span class="sxs-lookup"><span data-stu-id="0baa4-147">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* <span data-ttu-id="0baa4-148">[Authenticate with Azure services](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam) (Authentifizieren mit Azure-Diensten)</span><span class="sxs-lookup"><span data-stu-id="0baa4-148">[Authenticate with Azure services](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)</span></span>
* <span data-ttu-id="0baa4-149">[Deploy new virtual machines with SSH authentication](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) (Bereitstellen neuer virtueller Computer mit SSH-Authentifizierung)</span><span class="sxs-lookup"><span data-stu-id="0baa4-149">[Deploy new virtual machines with SSH authentication](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)</span></span>
* <span data-ttu-id="0baa4-150">[Deploy a container image to Azure Container Instances](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance) (Bereitstellen eines Containerimages für Azure Container Instances)</span><span class="sxs-lookup"><span data-stu-id="0baa4-150">[Deploy a container image to Azure Container Instances](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)</span></span>
* <span data-ttu-id="0baa4-151">[Create a cluster in Azure Kubernetes Service](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice) (Erstellen eines Clusters im Azure Kubernetes-Dienst)</span><span class="sxs-lookup"><span data-stu-id="0baa4-151">[Create a cluster in Azure Kubernetes Service](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)</span></span>
* <span data-ttu-id="0baa4-152">[Work with Azure Storage services](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage) (Arbeiten mit Azure Storage-Diensten)</span><span class="sxs-lookup"><span data-stu-id="0baa4-152">[Work with Azure Storage services](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)</span></span>
* <span data-ttu-id="0baa4-153">[All samples for the Azure SDK for Go](https://github.com/azure-samples/azure-sdk-for-go-samples) (Alle Beispiele für das Azure SDK für Go)</span><span class="sxs-lookup"><span data-stu-id="0baa4-153">[All samples for the Azure SDK for Go](https://github.com/azure-samples/azure-sdk-for-go-samples)</span></span>
