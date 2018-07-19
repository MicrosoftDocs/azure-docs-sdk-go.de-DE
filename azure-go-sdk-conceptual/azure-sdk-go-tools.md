---
title: Tools für Go-Entwickler
description: Tools zur Verwendung mit dem Azure SDK für Go und Azure-Diensten
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 07/13/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: dfa3912ac13e6f6d52d607f9dcc150f3a5b57602
ms.sourcegitcommit: d1790b317a8fcb4d672c654dac2a925a976589d4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2018
ms.locfileid: "39039504"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="f949f-103">Tools für Entwickler, die das Azure SDK für Go nutzen</span><span class="sxs-lookup"><span data-stu-id="f949f-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="f949f-104">Hier finden Sie einige Empfehlungen für Tools, mit denen Sie Go-Code effizient schreiben und problemlos in Azure-Diensten nutzen können.</span><span class="sxs-lookup"><span data-stu-id="f949f-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="f949f-105">Azure-Befehlszeilenschnittstelle</span><span class="sxs-lookup"><span data-stu-id="f949f-105">Azure CLI</span></span>

<span data-ttu-id="f949f-106">Die Azure CLI stellt eine Befehlszeilenschnittstelle zum Erstellen und Konfigurieren von Azure-Ressourcen in Ihren Abonnements bereit.</span><span class="sxs-lookup"><span data-stu-id="f949f-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="f949f-107">Die CLI erleichtert Ihnen den schnellen Einstieg in die Erstellung allgemeiner und gemeinsam genutzter Azure-Ressourcen, sodass Sie sich auf die komplexere Nutzung von Diensten konzentrieren können.</span><span class="sxs-lookup"><span data-stu-id="f949f-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="f949f-108">Die CLI bietet Abfrage- und Filterfunktionen, sodass Sie Ausgaben direkt an Ihre bevorzugten Befehlszeilentools weiterreichen können.</span><span class="sxs-lookup"><span data-stu-id="f949f-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="f949f-109">Die CLI steht für die Installation auf Ihrem lokalen System, als Docker-Image oder über [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="f949f-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f949f-110">Installieren der Azure-Befehlszeilenschnittstelle</span><span class="sxs-lookup"><span data-stu-id="f949f-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="f949f-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f949f-111">Visual Studio Code</span></span>

<span data-ttu-id="f949f-112">Visual Studio Code ist ein einfacher Editor, der die Go-Sprache über Erweiterungen umfassend unterstützt.</span><span class="sxs-lookup"><span data-stu-id="f949f-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="f949f-113">Diese Erweiterungen beinhalten Unterstützung für Features wie AutoVervollständigen, `impl`-Vorlagen, Refactoring und Debuggen.</span><span class="sxs-lookup"><span data-stu-id="f949f-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="f949f-114">Visual Studio Code bietet außerdem zahlreiche Erweiterungen für allgemeine Entwicklertools (etwa Quellcodeverwaltung) sowie Erweiterungen für direkte Interaktionen mit Azure-Diensten.</span><span class="sxs-lookup"><span data-stu-id="f949f-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="f949f-115">Microsoft stellt eine offizielle Metaerweiterung mit diesen Azure-Erweiterungen bereit, die auch eine interaktiven Schnittstelle für die Azure CLI enthält.</span><span class="sxs-lookup"><span data-stu-id="f949f-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="f949f-116">Installieren von Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f949f-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="f949f-117">Abrufen der Visual Studio Code-Erweiterung für Go</span><span class="sxs-lookup"><span data-stu-id="f949f-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="f949f-118">Herunterladen der Erweiterung für Azure-Tools</span><span class="sxs-lookup"><span data-stu-id="f949f-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="f949f-119">CI/CD mit Azure DevOps-Projekt</span><span class="sxs-lookup"><span data-stu-id="f949f-119">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="f949f-120">Mit der Azure DevOps-Projektpipeline können Sie einen kontinuierlichen Build einrichten und für Ihre Go-Anwendungen bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="f949f-120">With the Azure DevOps Project pipeline, you can set up a continuous build and deploy for your Go applications.</span></span> <span data-ttu-id="f949f-121">Sie benötigen lediglich ein verfügbares Git-Repository, und Sie können die Bereitstellung und Tests direkt auf Ihren Azure-Ressourcen einrichten.</span><span class="sxs-lookup"><span data-stu-id="f949f-121">All you need is an available git repo, and you can set up to deploy and test directly on your Azure resources.</span></span> <span data-ttu-id="f949f-122">Die Konfigurationspipeline ist einfach zu erstellen und zu verwalten, und da sie direkt in Azure bereitgestellt ist, können Sie sie auf die gleiche Weise wie Ihre anderen Azure-Ressourcen steuern.</span><span class="sxs-lookup"><span data-stu-id="f949f-122">The configuration pipeline is easy to create and manage, and since it's provisioned directly on Azure, you can control it in the same way that you handle your other Azure resources.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f949f-123">Informationen zum Erstellen einer CI/CD-Pipeline mit Azure DevOps-Projekt</span><span class="sxs-lookup"><span data-stu-id="f949f-123">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="f949f-124">Verwaltung von Abhängigkeiten mit dep</span><span class="sxs-lookup"><span data-stu-id="f949f-124">Dependency management with dep</span></span>

<span data-ttu-id="f949f-125">Da es noch keine offizielle Lösung gibt, stehen für die Verwaltung Ihrer Paketabhängigkeiten sowie das Vendoring mit Go zahlreiche Methoden zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="f949f-125">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="f949f-126">Es wird empfohlen, für die Verwaltung den Abhängigkeits-Manager `dep` zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="f949f-126">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="f949f-127">Das Azure SDK für Go nutzt dep für das Vendoring. Bei Verwendung von dep werden Abhängigkeiten für andere Projekte garantiert korrekt abgerufen.</span><span class="sxs-lookup"><span data-stu-id="f949f-127">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f949f-128">Abrufen des dep-Abhängigkeit-Managers</span><span class="sxs-lookup"><span data-stu-id="f949f-128">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
