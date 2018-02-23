<span data-ttu-id="7861b-101">Das Azure SDK für Go ist mit Go-Version 1.8 und höheren Versionen kompatibel.</span><span class="sxs-lookup"><span data-stu-id="7861b-101">The Azure SDK for Go is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="7861b-102">Bei Umgebungen mit [Azure Stack-Profilen](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles) ist mindestens Go-Version 1.9 erforderlich.</span><span class="sxs-lookup"><span data-stu-id="7861b-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span> <span data-ttu-id="7861b-103">Falls Go auf Ihrem System nicht verfügbar ist, befolgen Sie die [Anweisungen für die Installation von Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="7861b-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="7861b-104">Sie können das Azure SDK für Go und seine Abhängigkeiten über `go get` abrufen.</span><span class="sxs-lookup"><span data-stu-id="7861b-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="7861b-105">Schreiben Sie `Azure` in der URL unbedingt groß.</span><span class="sxs-lookup"><span data-stu-id="7861b-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="7861b-106">Andernfalls können bei der Verwendung des SDK Importprobleme aufgrund der Groß-/Kleinschreibung auftreten.</span><span class="sxs-lookup"><span data-stu-id="7861b-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="7861b-107">`Azure` muss außerdem in den Importanweisungen großgeschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="7861b-107">You also need to capitalize `Azure` in your import statements.</span></span>

