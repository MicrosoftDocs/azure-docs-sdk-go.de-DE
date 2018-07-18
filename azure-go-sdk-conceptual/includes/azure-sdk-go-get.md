---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: d021dd8ef4744b7c50b296b231bf63481f92411a
ms.sourcegitcommit: 2a3bd491e087a1d0e7d269bed896c029357d62a6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988061"
---
Das [Azure SDK für Go](https://github.com/Azure/azure-sdk-for-go) ist mit Go-Version 1.8 und höheren Versionen kompatibel. Bei Umgebungen mit [Azure Stack-Profilen](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles) ist mindestens Go-Version 1.9 erforderlich.
Falls Go auf Ihrem System nicht verfügbar ist, befolgen Sie die [Anweisungen für die Installation von Go](https://golang.org/doc/install).

Sie können das Azure SDK für Go und seine Abhängigkeiten über `go get` abrufen.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Schreiben Sie `Azure` in der URL unbedingt groß. Andernfalls können bei der Verwendung des SDK Importprobleme aufgrund der Groß-/Kleinschreibung auftreten. `Azure` muss außerdem in den Importanweisungen großgeschrieben werden.
