---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: ddd58efdbc0c2d3ded068a9bebf2466db702566f
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
<span data-ttu-id="44379-101">Das [Azure SDK für Go](https://github.com/Azure/azure-sdk-for-go) ist mit Go-Version 1.8 und höheren Versionen kompatibel.</span><span class="sxs-lookup"><span data-stu-id="44379-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="44379-102">Bei Umgebungen mit [Azure Stack-Profilen](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles) ist mindestens Go-Version 1.9 erforderlich.</span><span class="sxs-lookup"><span data-stu-id="44379-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="44379-103">Falls Go auf Ihrem System nicht verfügbar ist, befolgen Sie die [Anweisungen für die Installation von Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="44379-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="44379-104">Sie können das Azure SDK für Go und seine Abhängigkeiten über `go get` abrufen.</span><span class="sxs-lookup"><span data-stu-id="44379-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="44379-105">Schreiben Sie `Azure` in der URL unbedingt groß.</span><span class="sxs-lookup"><span data-stu-id="44379-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="44379-106">Andernfalls können bei der Verwendung des SDK Importprobleme aufgrund der Groß-/Kleinschreibung auftreten.</span><span class="sxs-lookup"><span data-stu-id="44379-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="44379-107">`Azure` muss außerdem in den Importanweisungen großgeschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="44379-107">You also need to capitalize `Azure` in your import statements.</span></span>

