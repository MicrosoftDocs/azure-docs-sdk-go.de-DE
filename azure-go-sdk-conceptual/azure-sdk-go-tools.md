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
ms.openlocfilehash: 1e122ab161766023ea146329a5edb13143699b8b
ms.sourcegitcommit: b81b17cbb934399c195bfdcb87137aee935f5234
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34755531"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="11498-103">Tools für Entwickler, die das Azure SDK für Go nutzen</span><span class="sxs-lookup"><span data-stu-id="11498-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="11498-104">Hier finden Sie einige Empfehlungen für Tools, mit denen Sie Go-Code effizient schreiben und problemlos in Azure-Diensten nutzen können.</span><span class="sxs-lookup"><span data-stu-id="11498-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="11498-105">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="11498-105">Azure CLI 2.0</span></span>

<span data-ttu-id="11498-106">Die Azure 2.0 CLI stellt eine Befehlszeilenschnittstelle zum Erstellen und Konfigurieren von Azure-Ressourcen in Ihren Abonnements bereit.</span><span class="sxs-lookup"><span data-stu-id="11498-106">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="11498-107">Die CLI erleichtert Ihnen den schnellen Einstieg in die Erstellung allgemeiner und gemeinsam genutzter Azure-Ressourcen, sodass Sie sich auf die komplexere Nutzung von Diensten konzentrieren können.</span><span class="sxs-lookup"><span data-stu-id="11498-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="11498-108">Die CLI bietet Abfrage- und Filterfunktionen, sodass Sie Ausgaben direkt an Ihre bevorzugten Befehlszeilentools weiterreichen können.</span><span class="sxs-lookup"><span data-stu-id="11498-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="11498-109">Die CLI steht für die Installation auf Ihrem lokalen System, als Docker-Image oder über [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="11498-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="11498-110">Installieren der Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="11498-110">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="11498-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="11498-111">Visual Studio Code</span></span>

<span data-ttu-id="11498-112">Visual Studio Code ist ein einfacher Editor, der die Go-Sprache über Erweiterungen umfassend unterstützt.</span><span class="sxs-lookup"><span data-stu-id="11498-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="11498-113">Diese Erweiterungen beinhalten Unterstützung für Features wie AutoVervollständigen, `impl`-Vorlagen, Refactoring und Debuggen.</span><span class="sxs-lookup"><span data-stu-id="11498-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="11498-114">Visual Studio Code bietet außerdem zahlreiche Erweiterungen für allgemeine Entwicklertools (etwa Quellcodeverwaltung) sowie Erweiterungen für direkte Interaktionen mit Azure-Diensten.</span><span class="sxs-lookup"><span data-stu-id="11498-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="11498-115">Microsoft stellt eine offizielle Metaerweiterung mit diesen Azure-Erweiterungen bereit, die auch eine interaktiven Schnittstelle für die Azure CLI enthält.</span><span class="sxs-lookup"><span data-stu-id="11498-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="11498-116">Installieren von Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="11498-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="11498-117">Abrufen der Visual Studio Code-Erweiterung für Go</span><span class="sxs-lookup"><span data-stu-id="11498-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="11498-118">Herunterladen der Erweiterung für Azure-Tools</span><span class="sxs-lookup"><span data-stu-id="11498-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="11498-119">Verwaltung von Abhängigkeiten mit dep</span><span class="sxs-lookup"><span data-stu-id="11498-119">Dependency management with dep</span></span>

<span data-ttu-id="11498-120">Da es noch keine offizielle Lösung gibt, stehen für die Verwaltung Ihrer Paketabhängigkeiten sowie das Vendoring mit Go zahlreiche Methoden zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="11498-120">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="11498-121">Es wird empfohlen, für die Verwaltung den Abhängigkeits-Manager `dep` zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="11498-121">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="11498-122">Das Azure SDK für Go nutzt dep für das Vendoring. Bei Verwendung von dep werden Abhängigkeiten für andere Projekte garantiert korrekt abgerufen.</span><span class="sxs-lookup"><span data-stu-id="11498-122">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="11498-123">Abrufen des dep-Abhängigkeit-Managers</span><span class="sxs-lookup"><span data-stu-id="11498-123">Get the dep dependency manager</span></span>](https://github.com/tools/godep)