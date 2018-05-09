---
title: Tools für Go-Entwickler
description: Tools zur Verwendung mit dem Azure SDK für Go und Azure-Diensten
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 2ea44fb8a4fdd6098bb44d3b5092cfbc352b424d
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="c9555-103">Tools für Entwickler, die das Azure SDK für Go nutzen</span><span class="sxs-lookup"><span data-stu-id="c9555-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="c9555-104">Hier finden Sie einige Empfehlungen für Tools, mit denen Sie Go-Code effizient schreiben und problemlos in Azure-Diensten nutzen können.</span><span class="sxs-lookup"><span data-stu-id="c9555-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="c9555-105">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c9555-105">Azure CLI 2.0</span></span>

<span data-ttu-id="c9555-106">Die Azure 2.0 CLI stellt eine Befehlszeilenschnittstelle zum Erstellen und Konfigurieren von Azure-Ressourcen in Ihren Abonnements bereit.</span><span class="sxs-lookup"><span data-stu-id="c9555-106">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="c9555-107">Die CLI erleichtert Ihnen den schnellen Einstieg in die Erstellung allgemeiner und gemeinsam genutzter Azure-Ressourcen, sodass Sie sich auf die komplexere Nutzung von Diensten konzentrieren können.</span><span class="sxs-lookup"><span data-stu-id="c9555-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="c9555-108">Die CLI bietet Abfrage- und Filterfunktionen, sodass Sie Ausgaben direkt an Ihre bevorzugten Befehlszeilentools weiterreichen können.</span><span class="sxs-lookup"><span data-stu-id="c9555-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="c9555-109">Die CLI steht für die Installation auf Ihrem lokalen System, als Docker-Image oder über [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="c9555-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c9555-110">Installieren der Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c9555-110">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="c9555-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c9555-111">Visual Studio Code</span></span>

<span data-ttu-id="c9555-112">Visual Studio Code ist ein einfacher Editor, der die Go-Sprache über Erweiterungen umfassend unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c9555-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="c9555-113">Diese Erweiterungen beinhalten Unterstützung für Features wie AutoVervollständigen, `impl`-Vorlagen, Refactoring und Debuggen.</span><span class="sxs-lookup"><span data-stu-id="c9555-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="c9555-114">Visual Studio Code bietet außerdem zahlreiche Erweiterungen für allgemeine Entwicklertools (etwa Quellcodeverwaltung) sowie Erweiterungen für direkte Interaktionen mit Azure-Diensten.</span><span class="sxs-lookup"><span data-stu-id="c9555-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="c9555-115">Microsoft stellt eine offizielle Metaerweiterung mit diesen Azure-Erweiterungen bereit, die auch eine interaktiven Schnittstelle für die Azure CLI enthält.</span><span class="sxs-lookup"><span data-stu-id="c9555-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="c9555-116">Installieren von Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c9555-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="c9555-117">Abrufen der Visual Studio Code-Erweiterung für Go</span><span class="sxs-lookup"><span data-stu-id="c9555-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="c9555-118">Herunterladen der Erweiterung für Azure-Tools</span><span class="sxs-lookup"><span data-stu-id="c9555-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="c9555-119">Verwaltung von Abhängigkeiten mit dep</span><span class="sxs-lookup"><span data-stu-id="c9555-119">Dependency management with dep</span></span>

<span data-ttu-id="c9555-120">Da es noch keine offizielle Lösung gibt, stehen für die Verwaltung Ihrer Paketabhängigkeiten sowie das Vendoring mit Go zahlreiche Methoden zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="c9555-120">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="c9555-121">Es wird empfohlen, für die Verwaltung den Abhängigkeits-Manager `dep` zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="c9555-121">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="c9555-122">Das Azure SDK für Go nutzt dep für das Vendoring. Bei Verwendung von dep werden Abhängigkeiten für andere Projekte garantiert korrekt abgerufen.</span><span class="sxs-lookup"><span data-stu-id="c9555-122">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c9555-123">Abrufen des dep-Abhängigkeit-Managers</span><span class="sxs-lookup"><span data-stu-id="c9555-123">Get the dep dependency manager</span></span>](https://github.com/tools/godep)

## <a name="telemetry-with-application-insights"></a><span data-ttu-id="c9555-124">Telemetrie mit Application Insights</span><span class="sxs-lookup"><span data-stu-id="c9555-124">Telemetry with Application Insights</span></span>

<span data-ttu-id="c9555-125">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) ist ein Analyseprodukt, mit dem Sie ganz einfach Telemetrieinformationen aus Ihren Anwendungen erfassen können. Es lässt sich in das Azure-Ökosystem, in Visual Studio Team Services und GitHub integrieren.</span><span class="sxs-lookup"><span data-stu-id="c9555-125">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) is an analytics product that allows you to easily collect telemetry information from your applications and integrates with the Azure ecosystem, Visual Studio Team Services, and GitHub.</span></span> <span data-ttu-id="c9555-126">Das Produkt kann in zahlreichen Anwendungen verwendet werden, und Microsoft bietet ein Go SDK für die Nutzung mit Application Insights.</span><span class="sxs-lookup"><span data-stu-id="c9555-126">It can be used in many applications, and Microsoft offers a Go SDK for working with Application Insights.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c9555-127">Abrufen des Application Insights SDK für Go</span><span class="sxs-lookup"><span data-stu-id="c9555-127">Get the Application Insights for Go SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Go) 
