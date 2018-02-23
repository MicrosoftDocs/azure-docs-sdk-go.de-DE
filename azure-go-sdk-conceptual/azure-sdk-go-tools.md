---
title: "Tools für Go-Entwickler"
description: "Tools zur Verwendung mit dem Azure SDK für Go und Azure-Diensten"
keywords: Azure, Go, Golang, Azure, Visual Studio, Visual Studio Code
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: routlaw
ms.openlocfilehash: 4753775e608b39c6da43d64fd08c1532e03d5810
ms.sourcegitcommit: aaa8c37880332625f858a38f5918e6cf581bf48d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2018
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="8bec7-104">Tools für Entwickler, die das Azure SDK für Go nutzen</span><span class="sxs-lookup"><span data-stu-id="8bec7-104">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="8bec7-105">Hier finden Sie einige Empfehlungen für Tools, mit denen Sie Go-Code effizient schreiben und problemlos in Azure-Diensten nutzen können.</span><span class="sxs-lookup"><span data-stu-id="8bec7-105">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="8bec7-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8bec7-106">Azure CLI 2.0</span></span>

<span data-ttu-id="8bec7-107">Die Azure 2.0 CLI stellt eine Befehlszeilenschnittstelle zum Erstellen und Konfigurieren von Azure-Ressourcen in Ihren Abonnements bereit.</span><span class="sxs-lookup"><span data-stu-id="8bec7-107">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="8bec7-108">Die CLI erleichtert Ihnen den schnellen Einstieg in die Erstellung allgemeiner und gemeinsam genutzter Azure-Ressourcen, sodass Sie sich auf die komplexere Nutzung von Diensten konzentrieren können.</span><span class="sxs-lookup"><span data-stu-id="8bec7-108">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="8bec7-109">Die CLI bietet Abfrage- und Filterfunktionen, sodass Sie Ausgaben direkt an Ihre bevorzugten Befehlszeilentools weiterreichen können.</span><span class="sxs-lookup"><span data-stu-id="8bec7-109">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="8bec7-110">Die CLI steht für die Installation auf Ihrem lokalen System, als Docker-Image oder über [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="8bec7-110">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8bec7-111">Installieren der Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8bec7-111">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="8bec7-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8bec7-112">Visual Studio Code</span></span>

<span data-ttu-id="8bec7-113">Visual Studio Code ist ein einfacher Editor, der die Go-Sprache über Erweiterungen umfassend unterstützt.</span><span class="sxs-lookup"><span data-stu-id="8bec7-113">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="8bec7-114">Diese Erweiterungen beinhalten Unterstützung für Features wie AutoVervollständigen, `impl`-Vorlagen, Refactoring und Debuggen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-114">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="8bec7-115">Visual Studio Code bietet außerdem zahlreiche Erweiterungen für allgemeine Entwicklertools (etwa Quellcodeverwaltung) sowie Erweiterungen für direkte Interaktionen mit Azure-Diensten.</span><span class="sxs-lookup"><span data-stu-id="8bec7-115">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="8bec7-116">Microsoft stellt eine offizielle Metaerweiterung mit diesen Azure-Erweiterungen bereit, die auch eine interaktiven Schnittstelle für die Azure CLI enthält.</span><span class="sxs-lookup"><span data-stu-id="8bec7-116">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="8bec7-117">Installieren von Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8bec7-117">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="8bec7-118">Abrufen der Visual Studio Code-Erweiterung für Go</span><span class="sxs-lookup"><span data-stu-id="8bec7-118">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="8bec7-119">Herunterladen der Erweiterung für Azure-Tools</span><span class="sxs-lookup"><span data-stu-id="8bec7-119">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="8bec7-120">Verwaltung von Abhängigkeiten mit dep</span><span class="sxs-lookup"><span data-stu-id="8bec7-120">Dependency management with dep</span></span>

<span data-ttu-id="8bec7-121">Da es noch keine offizielle Lösung gibt, stehen für die Verwaltung Ihrer Paketabhängigkeiten sowie das Vendoring mit Go zahlreiche Methoden zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="8bec7-121">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="8bec7-122">Es wird empfohlen, für die Verwaltung den Abhängigkeits-Manager `dep` zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="8bec7-122">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="8bec7-123">Das Azure SDK für Go nutzt dep für das Vendoring. Bei Verwendung von dep werden Abhängigkeiten für andere Projekte garantiert korrekt abgerufen.</span><span class="sxs-lookup"><span data-stu-id="8bec7-123">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8bec7-124">Abrufen des dep-Abhängigkeit-Managers</span><span class="sxs-lookup"><span data-stu-id="8bec7-124">Get the dep dependency manager</span></span>](https://github.com/tools/godep)

## <a name="telemetry-with-application-insights"></a><span data-ttu-id="8bec7-125">Telemetrie mit Application Insights</span><span class="sxs-lookup"><span data-stu-id="8bec7-125">Telemetry with Application Insights</span></span>

<span data-ttu-id="8bec7-126">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) ist ein Analyseprodukt, mit dem Sie ganz einfach Telemetrieinformationen aus Ihren Anwendungen erfassen können. Es lässt sich in das Azure-Ökosystem, in Visual Studio Team Services und GitHub integrieren.</span><span class="sxs-lookup"><span data-stu-id="8bec7-126">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) is an analytics product that allows you to easily collect telemetry information from your applications and integrates with the Azure ecosystem, Visual Studio Team Services, and GitHub.</span></span> <span data-ttu-id="8bec7-127">Das Produkt kann in zahlreichen Anwendungen verwendet werden, und Microsoft bietet ein Go SDK für die Nutzung mit Application Insights.</span><span class="sxs-lookup"><span data-stu-id="8bec7-127">It can be used in many applications, and Microsoft offers a Go SDK for working with Application Insights.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8bec7-128">Abrufen des Application Insights SDK für Go</span><span class="sxs-lookup"><span data-stu-id="8bec7-128">Get the Application Insights for Go SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Go) 
