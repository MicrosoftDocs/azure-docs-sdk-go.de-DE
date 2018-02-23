---
title: "Installieren des Azure SDK für Go"
description: "Es wird beschrieben, wie Sie das Azure SDK für Go installieren und konfigurieren und das Vendoring dafür durchführen."
keywords: Azure, SDK, Go, Golang, Azure SDK
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: routlaw
ms.openlocfilehash: f822a9304a4744e0b0e93286303aa8bb80fec852
ms.sourcegitcommit: aaa8c37880332625f858a38f5918e6cf581bf48d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2018
---
# <a name="installing-the-azure-sdk-for-go"></a>Installieren des Azure SDK für Go

Willkommen beim Azure SDK für Go! Dieses SDK ermöglicht Ihnen das Verwalten und Interagieren mit Azure-Diensten über Ihre Go-Anwendungen.

## <a name="get-the-azure-sdk-for-go"></a>Abrufen des Azure SDK für Go

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

Für die Arbeit mit Azure Storage Blobs ist ein separates SDK erforderlich.

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendoring-the-azure-sdk-for-go"></a>Durchführen des Vendoring für das Azure SDK für Go

Das Vendoring für das Azure SDK für Go kann mit [dep](https://github.com/golang/dep) durchgeführt werden. Das Vendoring wird aus Stabilitätsgründen empfohlen. Fügen Sie `gitub.com/Azure/azure-sdk-for-go` einem `[[constraint]]`-Abschnitt der Datei `Gopkg.toml` hinzu, um die Unterstützung für `dep` zu verwenden. Fügen Sie beispielsweise den folgenden Eintrag hinzu, um das Vendoring für Version `14.0.0` durchzuführen:

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="including-the-azure-sdk-for-go-in-your-project"></a>Einbinden des Azure SDK für Go in Ihr Projekt

Importieren Sie beliebige Dienste, mit denen Sie interagieren, und die erforderlichen `autorest`-Module, um Azure-Dienste in Ihrem Go-Code zu nutzen.
Sie erhalten eine vollständige Liste mit den verfügbaren Modulen von GoDoc für [verfügbare Dienste](https://godoc.org/github.com/Azure/azure-sdk-for-go) und [AutoRest-Pakete](https://godoc.org/github.com/Azure/go-autorest). Die am häufigsten verwendeten Pakete, die Sie für `go-autorest` benötigen, sind:

| Paket | Beschreibung |
|---------|-------------|
| [github.com/Azure/go-autorest/autorest][autorest] | Objekte zur Verarbeitung der Dienstclientauthentifizierung |
| [github.com/Azure/go-autorest/autorest/azure][autorest/azure] | Konstanten für Interaktionen mit Azure-Diensten |
| [github.com/Azure/go-autorest/autorest/adal][autorest/adal] | Authentifizierungsmechanismen für den Zugriff auf Azure-Dienste |
| [github.com/Azure/go-autorest/autorest/to][autorest/to] | Hilfsprogramme für die Typassertion für die Arbeit mit Azure SDK-Datenstrukturen |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

Module für Azure-Dienste werden unabhängig von den dazugehörigen SDK-APIs versioniert. Diese Versionen sind Teil des Modulimportpfads und stammen entweder aus einer _Dienstversion_ oder einem _Profil_. Derzeit lautet die Empfehlung, sowohl für die Entwicklung als auch für das Release eine bestimmte Dienstversion zu nutzen. Dienste befinden sich unter dem `services`-Modul. Der vollständige Pfad für den Import ist der Name des Diensts, gefolgt von der Version im Format `YYYY-MM-DD`, worauf erneut der Dienstname folgt. Beispiel für die Einbindung der Version `2017-03-30` des Computediensts:

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

Derzeit wird empfohlen, die aktuelle Version eines Diensts zu verwenden, sofern nicht ein triftiger Grund für eine andere Vorgehensweise vorliegt.

Wenn Sie eine gemeinsame Momentaufnahme aller Dienste benötigen, können Sie auch eine einzelne Profilversion wählen. Das einzige gesperrte Profil ist derzeit Version `2017-03-30`, die unter Umständen nicht über die aktuellen Features der Dienste verfügt. Profile befinden sich unter dem Modul `profiles`, und die Version wird im Format `YYYY-MM-DD` angegeben. Dienste werden unter ihrer Profilversion gruppiert. Beispiel für den Import des Verwaltungsmoduls für Azure-Ressourcen aus dem Profil `2017-03-09`:

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> Außerdem sind die Profile `preview` und `latest` verfügbar. Ihre Verwendung ist nicht empfehlenswert. Diese Profile sind fortlaufende Versionen, und das Dienstverhalten kann sich im Laufe der Zeit ändern.

## <a name="next-steps"></a>Nächste Schritte

Probieren Sie einen Schnellstart aus, um mit der Verwendung des Azure SDK für Go zu beginnen.

* [Bereitstellen eines virtuellen Azure-Computers über eine Vorlage mit dem Azure SDK für Go](azure-sdk-go-qs-vm.md)
* [Übertragen von Objekten nach/aus Azure Blob Storage mit Go](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [Herstellen einer Verbindung mit Azure Database for PostgreSQL](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

Wenn Sie sofort mit anderen Diensten im Go SDK starten möchten, ist es ratsam, sich den verfügbaren Beispielcode anzusehen.

* [Authenticate with Azure services](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam) (Authentifizieren mit Azure-Diensten)
* [Deploy new virtual machines with SSH authentication](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) (Bereitstellen neuer virtueller Computer mit SSH-Authentifizierung)
* [Deploy a container image to Azure Container Instances](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance) (Bereitstellen eines Containerimages für Azure Container Instances)
* [Create a cluster in Azure Kubernetes Service](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice) (Erstellen eines Clusters im Azure Kubernetes-Dienst)
* [Work with Azure Storage services](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage) (Arbeiten mit Azure Storage-Diensten)
* [All samples for the Azure SDK for Go](https://github.com/azure-samples/azure-sdk-for-go-samples) (Alle Beispiele für das Azure SDK für Go)
