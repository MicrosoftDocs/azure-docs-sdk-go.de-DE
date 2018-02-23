Das Azure SDK für Go ist mit Go-Version 1.8 und höheren Versionen kompatibel. Bei Umgebungen mit [Azure Stack-Profilen](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles) ist mindestens Go-Version 1.9 erforderlich. Falls Go auf Ihrem System nicht verfügbar ist, befolgen Sie die [Anweisungen für die Installation von Go](https://golang.org/doc/install).

Sie können das Azure SDK für Go und seine Abhängigkeiten über `go get` abrufen.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Schreiben Sie `Azure` in der URL unbedingt groß. Andernfalls können bei der Verwendung des SDK Importprobleme aufgrund der Groß-/Kleinschreibung auftreten. `Azure` muss außerdem in den Importanweisungen großgeschrieben werden.
