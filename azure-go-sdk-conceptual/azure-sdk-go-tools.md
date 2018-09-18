---
title: Tools für Entwickler, die das Azure SDK für Go nutzen
description: Tools zur Verwendung mit dem Azure SDK für Go und Azure-Diensten
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 70cf7d645f47df29e8e42599a0acd75858144783
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059202"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="e9dbf-103">Tools für Entwickler, die das Azure SDK für Go nutzen</span><span class="sxs-lookup"><span data-stu-id="e9dbf-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="e9dbf-104">Hier finden Sie einige Empfehlungen für Tools, mit denen Sie Go-Code effizient schreiben und problemlos in Azure-Diensten nutzen können.</span><span class="sxs-lookup"><span data-stu-id="e9dbf-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="e9dbf-105">Azure-Befehlszeilenschnittstelle</span><span class="sxs-lookup"><span data-stu-id="e9dbf-105">Azure CLI</span></span>

<span data-ttu-id="e9dbf-106">Die Azure CLI stellt eine Befehlszeilenschnittstelle zum Erstellen und Konfigurieren von Azure-Ressourcen in Ihren Abonnements bereit.</span><span class="sxs-lookup"><span data-stu-id="e9dbf-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="e9dbf-107">Die CLI erleichtert Ihnen den schnellen Einstieg in die Erstellung allgemeiner und gemeinsam genutzter Azure-Ressourcen, sodass Sie sich auf die komplexere Nutzung von Diensten konzentrieren können.</span><span class="sxs-lookup"><span data-stu-id="e9dbf-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="e9dbf-108">Die CLI bietet Abfrage- und Filterfunktionen, sodass Sie Ausgaben direkt an Ihre bevorzugten Befehlszeilentools weiterreichen können.</span><span class="sxs-lookup"><span data-stu-id="e9dbf-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="e9dbf-109">Die CLI steht für die Installation auf Ihrem lokalen System, als Docker-Image oder über [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="e9dbf-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e9dbf-110">Installieren der Azure-Befehlszeilenschnittstelle</span><span class="sxs-lookup"><span data-stu-id="e9dbf-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="e9dbf-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e9dbf-111">Visual Studio Code</span></span>

<span data-ttu-id="e9dbf-112">Visual Studio Code ist ein einfacher Editor, der Go unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e9dbf-112">Visual Studio Code is a lightweight editor that offers Go support.</span></span> <span data-ttu-id="e9dbf-113">Diese Erweiterung bietet Features wie AutoVervollständigen, `impl`-Vorlagen, Refactoring und Debuggen.</span><span class="sxs-lookup"><span data-stu-id="e9dbf-113">This extension offers features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="e9dbf-114">Visual Studio Code bietet außerdem Unterstützung für den Zugriff auf die Quellcodeverwaltung im Editor sowie Erweiterungen für die Verwendung von Azure-Diensten.</span><span class="sxs-lookup"><span data-stu-id="e9dbf-114">Visual Studio Code also offers support for in-editor access to source control, and extensions for working with Azure services.</span></span>

* [<span data-ttu-id="e9dbf-115">Installieren von Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e9dbf-115">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="e9dbf-116">Abrufen der Visual Studio Code-Erweiterung für Go</span><span class="sxs-lookup"><span data-stu-id="e9dbf-116">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="e9dbf-117">Abrufen der Visual Studio Code-Erweiterung für Azure-Tools</span><span class="sxs-lookup"><span data-stu-id="e9dbf-117">Get the Visual Studio Code Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="e9dbf-118">CI/CD mit Azure DevOps-Projekt</span><span class="sxs-lookup"><span data-stu-id="e9dbf-118">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="e9dbf-119">Azure DevOps-Projektpipelines ermöglichen die Einrichtung eines Continuous Integration-Systems für Ihre Go-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="e9dbf-119">Azure DevOps Project pipelines allow you to set up a continuous integration system for your Go applications.</span></span> <span data-ttu-id="e9dbf-120">Dafür ist lediglich ein Git-Repository erforderlich, und Sie können direkt in Azure bereitstellen und testen.</span><span class="sxs-lookup"><span data-stu-id="e9dbf-120">All it takes is a git repo, and you can deploy and test directly on Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e9dbf-121">Informationen zum Erstellen einer CI/CD-Pipeline mit Azure DevOps-Projekt</span><span class="sxs-lookup"><span data-stu-id="e9dbf-121">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="e9dbf-122">Verwaltung von Abhängigkeiten mit dep</span><span class="sxs-lookup"><span data-stu-id="e9dbf-122">Dependency management with dep</span></span>

<span data-ttu-id="e9dbf-123">Das Azure SDK für Go nutzt dep für die Verwaltung von Abhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="e9dbf-123">The Azure SDK for Go uses dep for dependency management.</span></span> <span data-ttu-id="e9dbf-124">Der dep-Befehl ermöglicht das Abrufen und Vendoring von Anforderungen für Ihre Go-Anwendung, um Versionskonflikte zu vermeiden und die ordnungsgemäße Funktion Ihres Projekts sicherzustellen.</span><span class="sxs-lookup"><span data-stu-id="e9dbf-124">The dep command allows you to pull and vendor requirements for your Go application, avoiding version conflicts and ensuring that your project works correctly.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e9dbf-125">Abrufen des dep-Abhängigkeit-Managers</span><span class="sxs-lookup"><span data-stu-id="e9dbf-125">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
