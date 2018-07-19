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
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Tools für Entwickler, die das Azure SDK für Go nutzen

Hier finden Sie einige Empfehlungen für Tools, mit denen Sie Go-Code effizient schreiben und problemlos in Azure-Diensten nutzen können.

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Die Azure CLI stellt eine Befehlszeilenschnittstelle zum Erstellen und Konfigurieren von Azure-Ressourcen in Ihren Abonnements bereit. Die CLI erleichtert Ihnen den schnellen Einstieg in die Erstellung allgemeiner und gemeinsam genutzter Azure-Ressourcen, sodass Sie sich auf die komplexere Nutzung von Diensten konzentrieren können. Die CLI bietet Abfrage- und Filterfunktionen, sodass Sie Ausgaben direkt an Ihre bevorzugten Befehlszeilentools weiterreichen können. Die CLI steht für die Installation auf Ihrem lokalen System, als Docker-Image oder über [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) zur Verfügung.

> [!div class="nextstepaction"]
> [Installieren der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code ist ein einfacher Editor, der die Go-Sprache über Erweiterungen umfassend unterstützt. Diese Erweiterungen beinhalten Unterstützung für Features wie AutoVervollständigen, `impl`-Vorlagen, Refactoring und Debuggen. Visual Studio Code bietet außerdem zahlreiche Erweiterungen für allgemeine Entwicklertools (etwa Quellcodeverwaltung) sowie Erweiterungen für direkte Interaktionen mit Azure-Diensten. Microsoft stellt eine offizielle Metaerweiterung mit diesen Azure-Erweiterungen bereit, die auch eine interaktiven Schnittstelle für die Azure CLI enthält.

* [Installieren von Visual Studio Code](https://code.visualstudio.com/Download)
* [Abrufen der Visual Studio Code-Erweiterung für Go](https://code.visualstudio.com/docs/languages/go)
* [Herunterladen der Erweiterung für Azure-Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a>CI/CD mit Azure DevOps-Projekt

Mit der Azure DevOps-Projektpipeline können Sie einen kontinuierlichen Build einrichten und für Ihre Go-Anwendungen bereitstellen. Sie benötigen lediglich ein verfügbares Git-Repository, und Sie können die Bereitstellung und Tests direkt auf Ihren Azure-Ressourcen einrichten. Die Konfigurationspipeline ist einfach zu erstellen und zu verwalten, und da sie direkt in Azure bereitgestellt ist, können Sie sie auf die gleiche Weise wie Ihre anderen Azure-Ressourcen steuern.

> [!div class="nextstepaction"]
> [Informationen zum Erstellen einer CI/CD-Pipeline mit Azure DevOps-Projekt](/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a>Verwaltung von Abhängigkeiten mit dep

Da es noch keine offizielle Lösung gibt, stehen für die Verwaltung Ihrer Paketabhängigkeiten sowie das Vendoring mit Go zahlreiche Methoden zur Verfügung. Es wird empfohlen, für die Verwaltung den Abhängigkeits-Manager `dep` zu verwenden. Das Azure SDK für Go nutzt dep für das Vendoring. Bei Verwendung von dep werden Abhängigkeiten für andere Projekte garantiert korrekt abgerufen.

> [!div class="nextstepaction"]
> [Abrufen des dep-Abhängigkeit-Managers](https://github.com/golang/dep)
