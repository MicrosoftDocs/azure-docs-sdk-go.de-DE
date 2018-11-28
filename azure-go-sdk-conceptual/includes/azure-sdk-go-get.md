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
<span data-ttu-id="22ef9-101">Das [Azure SDK für Go](https://github.com/Azure/azure-sdk-for-go) ist mit Go-Version 1.8 und höheren Versionen kompatibel.</span><span class="sxs-lookup"><span data-stu-id="22ef9-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and higher.</span></span> <span data-ttu-id="22ef9-102">Bei Umgebungen mit [Azure Stack-Profilen](/azure/azure-stack/user/azure-stack-version-profiles-go) ist mindestens Go-Version 1.9 erforderlich.</span><span class="sxs-lookup"><span data-stu-id="22ef9-102">For environments using [Azure Stack Profiles](/azure/azure-stack/user/azure-stack-version-profiles-go), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="22ef9-103">Falls Sie Go installieren müssen, befolgen Sie die [Installationsanweisungen für Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="22ef9-103">If you need to install Go, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="22ef9-104">Sie können das Azure SDK für Go und seine Abhängigkeiten über `go get` herunterladen.</span><span class="sxs-lookup"><span data-stu-id="22ef9-104">You can download the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="22ef9-105">Schreiben Sie `Azure` in der URL unbedingt groß.</span><span class="sxs-lookup"><span data-stu-id="22ef9-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="22ef9-106">Andernfalls können bei der Verwendung des SDK Importprobleme aufgrund der Groß-/Kleinschreibung auftreten.</span><span class="sxs-lookup"><span data-stu-id="22ef9-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="22ef9-107">`Azure` muss außerdem in den Importanweisungen großgeschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="22ef9-107">You also need to capitalize `Azure` in your import statements.</span></span>
