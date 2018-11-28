---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 5df14f939efdd0550b49261c88c8dc6518ada459
ms.sourcegitcommit: 887b15afcdeaf926a5f3d21b64e4045167fd062c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2018
ms.locfileid: "52293507"
---
Das [Azure SDK für Go](https://github.com/Azure/azure-sdk-for-go) ist mit Go-Version 1.8 und höheren Versionen kompatibel. Bei Umgebungen mit [Azure Stack-Profilen](/azure/azure-stack/user/azure-stack-version-profiles-go) ist mindestens Go-Version 1.9 erforderlich.
Falls Sie Go installieren müssen, befolgen Sie die [Installationsanweisungen für Go](https://golang.org/doc/install).

Sie können das Azure SDK für Go und seine Abhängigkeiten über `go get` herunterladen.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Schreiben Sie `Azure` in der URL unbedingt groß. Andernfalls können bei der Verwendung des SDK Importprobleme aufgrund der Groß-/Kleinschreibung auftreten. `Azure` muss außerdem in den Importanweisungen großgeschrieben werden.
