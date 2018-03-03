---
title: "Installieren des Azure SDK für Go"
description: "Es wird beschrieben, wie Sie das Azure SDK für Go installieren und konfigurieren und das Vendoring dafür durchführen."
keywords: Azure, SDK, Go, Golang, Azure SDK
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: routlaw
ms.openlocfilehash: 7fc0a3ff71b0b06f616ae43cff311352fe873345
ms.sourcegitcommit: 890f5f01a70e7e376e6bb98a2030afbfc016f538
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/01/2018
---
# <a name="installing-the-azure-sdk-for-go"></a><span data-ttu-id="1af98-104">Installieren des Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="1af98-104">Installing the Azure SDK for Go</span></span>

<span data-ttu-id="1af98-105">Willkommen beim Azure SDK für Go!</span><span class="sxs-lookup"><span data-stu-id="1af98-105">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="1af98-106">Dieses SDK ermöglicht Ihnen das Verwalten und Interagieren mit Azure-Diensten über Ihre Go-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="1af98-106">This SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="1af98-107">Abrufen des Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="1af98-107">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="1af98-108">Für die Arbeit mit Azure Storage Blobs ist ein separates SDK erforderlich.</span><span class="sxs-lookup"><span data-stu-id="1af98-108">Working with Azure Storage Blobs requires a separate SDK.</span></span>

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendoring-the-azure-sdk-for-go"></a><span data-ttu-id="1af98-109">Durchführen des Vendoring für das Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="1af98-109">Vendoring the Azure SDK for Go</span></span>

<span data-ttu-id="1af98-110">Das Vendoring für das Azure SDK für Go kann mit [dep](https://github.com/golang/dep) durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="1af98-110">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="1af98-111">Das Vendoring wird aus Stabilitätsgründen empfohlen.</span><span class="sxs-lookup"><span data-stu-id="1af98-111">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="1af98-112">Fügen Sie `github.com/Azure/azure-sdk-for-go` einem `[[constraint]]`-Abschnitt der Datei `Gopkg.toml` hinzu, um die Unterstützung für `dep` zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="1af98-112">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="1af98-113">Fügen Sie beispielsweise den folgenden Eintrag hinzu, um das Vendoring für Version `14.0.0` durchzuführen:</span><span class="sxs-lookup"><span data-stu-id="1af98-113">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="including-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="1af98-114">Einbinden des Azure SDK für Go in Ihr Projekt</span><span class="sxs-lookup"><span data-stu-id="1af98-114">Including the Azure SDK for Go in your project</span></span>

<span data-ttu-id="1af98-115">Importieren Sie beliebige Dienste, mit denen Sie interagieren, und die erforderlichen `autorest`-Module, um Azure-Dienste in Ihrem Go-Code zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="1af98-115">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="1af98-116">Sie erhalten eine vollständige Liste mit den verfügbaren Modulen von GoDoc für [verfügbare Dienste](https://godoc.org/github.com/Azure/azure-sdk-for-go) und [AutoRest-Pakete](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="1af98-116">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="1af98-117">Die am häufigsten verwendeten Pakete, die Sie für `go-autorest` benötigen, sind:</span><span class="sxs-lookup"><span data-stu-id="1af98-117">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="1af98-118">Paket</span><span class="sxs-lookup"><span data-stu-id="1af98-118">Package</span></span> | <span data-ttu-id="1af98-119">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="1af98-119">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="1af98-120">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="1af98-120">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="1af98-121">Objekte zur Verarbeitung der Dienstclientauthentifizierung</span><span class="sxs-lookup"><span data-stu-id="1af98-121">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="1af98-122">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="1af98-122">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="1af98-123">Konstanten für Interaktionen mit Azure-Diensten</span><span class="sxs-lookup"><span data-stu-id="1af98-123">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="1af98-124">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="1af98-124">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="1af98-125">Authentifizierungsmechanismen für den Zugriff auf Azure-Dienste</span><span class="sxs-lookup"><span data-stu-id="1af98-125">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="1af98-126">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="1af98-126">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="1af98-127">Hilfsprogramme für die Typassertion für die Arbeit mit Azure SDK-Datenstrukturen</span><span class="sxs-lookup"><span data-stu-id="1af98-127">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="1af98-128">Module für Azure-Dienste werden unabhängig von den dazugehörigen SDK-APIs versioniert.</span><span class="sxs-lookup"><span data-stu-id="1af98-128">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="1af98-129">Diese Versionen sind Teil des Modulimportpfads und stammen entweder aus einer _Dienstversion_ oder einem _Profil_.</span><span class="sxs-lookup"><span data-stu-id="1af98-129">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="1af98-130">Derzeit lautet die Empfehlung, sowohl für die Entwicklung als auch für das Release eine bestimmte Dienstversion zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="1af98-130">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="1af98-131">Dienste befinden sich unter dem `services`-Modul.</span><span class="sxs-lookup"><span data-stu-id="1af98-131">Services are located under the `services` module.</span></span> <span data-ttu-id="1af98-132">Der vollständige Pfad für den Import ist der Name des Diensts, gefolgt von der Version im Format `YYYY-MM-DD`, worauf erneut der Dienstname folgt.</span><span class="sxs-lookup"><span data-stu-id="1af98-132">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="1af98-133">Beispiel für die Einbindung der Version `2017-03-30` des Computediensts:</span><span class="sxs-lookup"><span data-stu-id="1af98-133">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="1af98-134">Derzeit wird empfohlen, die aktuelle Version eines Diensts zu verwenden, sofern nicht ein triftiger Grund für eine andere Vorgehensweise vorliegt.</span><span class="sxs-lookup"><span data-stu-id="1af98-134">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="1af98-135">Wenn Sie eine gemeinsame Momentaufnahme aller Dienste benötigen, können Sie auch eine einzelne Profilversion wählen.</span><span class="sxs-lookup"><span data-stu-id="1af98-135">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="1af98-136">Das einzige gesperrte Profil ist derzeit Version `2017-03-30`, die unter Umständen nicht über die aktuellen Features der Dienste verfügt.</span><span class="sxs-lookup"><span data-stu-id="1af98-136">Right now, the only locked profile is version `2017-03-30`, which may not have the latest features of services.</span></span> <span data-ttu-id="1af98-137">Profile befinden sich unter dem Modul `profiles`, und die Version wird im Format `YYYY-MM-DD` angegeben.</span><span class="sxs-lookup"><span data-stu-id="1af98-137">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="1af98-138">Dienste werden unter ihrer Profilversion gruppiert.</span><span class="sxs-lookup"><span data-stu-id="1af98-138">Services are grouped under their profile version.</span></span> <span data-ttu-id="1af98-139">Beispiel für den Import des Verwaltungsmoduls für Azure-Ressourcen aus dem Profil `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="1af98-139">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="1af98-140">Außerdem sind die Profile `preview` und `latest` verfügbar.</span><span class="sxs-lookup"><span data-stu-id="1af98-140">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="1af98-141">Ihre Verwendung ist nicht empfehlenswert.</span><span class="sxs-lookup"><span data-stu-id="1af98-141">Using them is not recommended.</span></span> <span data-ttu-id="1af98-142">Diese Profile sind fortlaufende Versionen, und das Dienstverhalten kann sich im Laufe der Zeit ändern.</span><span class="sxs-lookup"><span data-stu-id="1af98-142">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1af98-143">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="1af98-143">Next steps</span></span>

<span data-ttu-id="1af98-144">Probieren Sie einen Schnellstart aus, um mit der Verwendung des Azure SDK für Go zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="1af98-144">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="1af98-145">Bereitstellen eines virtuellen Azure-Computers über eine Vorlage mit dem Azure SDK für Go</span><span class="sxs-lookup"><span data-stu-id="1af98-145">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="1af98-146">Übertragen von Objekten nach/aus Azure Blob Storage mit Go</span><span class="sxs-lookup"><span data-stu-id="1af98-146">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="1af98-147">Herstellen einer Verbindung mit Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="1af98-147">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="1af98-148">Wenn Sie sofort mit anderen Diensten im Go SDK starten möchten, ist es ratsam, sich den verfügbaren Beispielcode anzusehen.</span><span class="sxs-lookup"><span data-stu-id="1af98-148">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* <span data-ttu-id="1af98-149">[Authenticate with Azure services](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam) (Authentifizieren mit Azure-Diensten)</span><span class="sxs-lookup"><span data-stu-id="1af98-149">[Authenticate with Azure services](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)</span></span>
* <span data-ttu-id="1af98-150">[Deploy new virtual machines with SSH authentication](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) (Bereitstellen neuer virtueller Computer mit SSH-Authentifizierung)</span><span class="sxs-lookup"><span data-stu-id="1af98-150">[Deploy new virtual machines with SSH authentication](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)</span></span>
* <span data-ttu-id="1af98-151">[Deploy a container image to Azure Container Instances](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance) (Bereitstellen eines Containerimages für Azure Container Instances)</span><span class="sxs-lookup"><span data-stu-id="1af98-151">[Deploy a container image to Azure Container Instances](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)</span></span>
* <span data-ttu-id="1af98-152">[Create a cluster in Azure Kubernetes Service](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice) (Erstellen eines Clusters im Azure Kubernetes-Dienst)</span><span class="sxs-lookup"><span data-stu-id="1af98-152">[Create a cluster in Azure Kubernetes Service](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)</span></span>
* <span data-ttu-id="1af98-153">[Work with Azure Storage services](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage) (Arbeiten mit Azure Storage-Diensten)</span><span class="sxs-lookup"><span data-stu-id="1af98-153">[Work with Azure Storage services](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)</span></span>
* <span data-ttu-id="1af98-154">[All samples for the Azure SDK for Go](https://github.com/azure-samples/azure-sdk-for-go-samples) (Alle Beispiele für das Azure SDK für Go)</span><span class="sxs-lookup"><span data-stu-id="1af98-154">[All samples for the Azure SDK for Go](https://github.com/azure-samples/azure-sdk-for-go-samples)</span></span>
