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
<span data-ttu-id="7ff9e-101">Das [Azure SDK für Go](https://github.com/Azure/azure-sdk-for-go) ist mit Go-Version 1.8 und höheren Versionen kompatibel.</span><span class="sxs-lookup"><span data-stu-id="7ff9e-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="7ff9e-102">Bei Umgebungen mit [Azure Stack-Profilen](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles) ist mindestens Go-Version 1.9 erforderlich.</span><span class="sxs-lookup"><span data-stu-id="7ff9e-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="7ff9e-103">Falls Go auf Ihrem System nicht verfügbar ist, befolgen Sie die [Anweisungen für die Installation von Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="7ff9e-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="7ff9e-104">Sie können das Azure SDK für Go und seine Abhängigkeiten über `go get` abrufen.</span><span class="sxs-lookup"><span data-stu-id="7ff9e-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="7ff9e-105">Schreiben Sie `Azure` in der URL unbedingt groß.</span><span class="sxs-lookup"><span data-stu-id="7ff9e-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="7ff9e-106">Andernfalls können bei der Verwendung des SDK Importprobleme aufgrund der Groß-/Kleinschreibung auftreten.</span><span class="sxs-lookup"><span data-stu-id="7ff9e-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="7ff9e-107">`Azure` muss außerdem in den Importanweisungen großgeschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="7ff9e-107">You also need to capitalize `Azure` in your import statements.</span></span>
