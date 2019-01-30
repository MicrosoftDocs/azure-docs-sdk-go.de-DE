---
title: Installieren des Azure SDK für Go
description: Es wird beschrieben, wie Sie das Azure SDK für Go installieren und konfigurieren und das Vendoring dafür durchführen.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/14/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 2799e3a6c637036eeaf7b20adf8aa55a8a4ab400
ms.sourcegitcommit: 4db332f5e43a5b43032ff9017805d5fd5a650d86
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2019
ms.locfileid: "55145531"
---
# <a name="install-the-azure-sdk-for-go"></a>Installieren des Azure SDK für Go

Willkommen beim Azure SDK für Go! Mit diesem SDK können Sie Azure-Dienste über Ihre Go-Anwendungen verwalten und mit ihnen interagieren.

## <a name="get-the-azure-sdk-for-go"></a>Abrufen des Azure SDK für Go

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

Einige Azure-Dienste verfügen über ein eigenes Go SDK und sind nicht im Kernpaket von Azure SDK für Go enthalten. In der folgenden Tabelle sind die Dienste mit eigenen SDKs und deren Paketnamen aufgeführt. Alle diese Pakete gelten als Vorschauversion.

| Dienst | Paket |
|---------|---------|
| Blob Storage | [github.com/Azure/azure-storage-blob-go](https://github.com/Azure/azure-storage-blob-go) |
| File Storage | [github.com/Azure/azure-storage-file-go](https://github.com/Azure/azure-storage-file-go) |
| Speicherwarteschlange | [github.com/Azure/azure-storage-queue-go](https://github.com/Azure/azure-storage-queue-go) |
| Event Hub | [github.com/Azure/azure-event-hubs-go](https://github.com/Azure/azure-event-hubs-go) |
| Service Bus | [github.com/Azure/azure-service-bus-go](https://github.com/Azure/azure-service-bus-go) |
| Application Insights | [github.com/Microsoft/ApplicationInsights-go](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a>Durchführen des Vendorings für das Azure SDK für Go

Das Vendoring für das Azure SDK für Go kann mit [dep](https://github.com/golang/dep) durchgeführt werden. Das Vendoring wird aus Stabilitätsgründen empfohlen. Fügen Sie zur Verwendung von `dep` in Ihrem eigenen Projekt `github.com/Azure/azure-sdk-for-go` zum Abschnitt `[[constraint]]` in `Gopkg.toml` hinzu. Fügen Sie beispielsweise den folgenden Eintrag hinzu, um das Vendoring für Version `14.0.0` durchzuführen:

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a>Integrieren des Azure SDK für Go in Ihr Projekt

Importieren Sie beliebige Dienste, mit denen Sie interagieren, und die erforderlichen `autorest`-Module, um Azure-Dienste in Ihrem Go-Code zu nutzen.
Sie erhalten eine vollständige Liste mit den verfügbaren Modulen von GoDoc für [verfügbare Dienste](https://godoc.org/github.com/Azure/azure-sdk-for-go) und [AutoRest-Pakete](https://godoc.org/github.com/Azure/go-autorest). Die am häufigsten verwendeten Pakete, die Sie für `go-autorest` benötigen, sind:

| Paket | BESCHREIBUNG |
|---------|-------------|
| [github.com/Azure/go-autorest/autorest][autorest] | Objekte zur Verarbeitung der Dienstclientauthentifizierung |
| [github.com/Azure/go-autorest/autorest/azure][autorest/azure] | Konstanten für Interaktionen mit Azure-Diensten |
| [github.com/Azure/go-autorest/autorest/adal][autorest/adal] | Authentifizierungsmechanismen für den Zugriff auf Azure-Dienste |
| [github.com/Azure/go-autorest/autorest/to][autorest/to] | Hilfsprogramme für die Typassertion für die Arbeit mit Azure SDK-Datenstrukturen |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

Die Versionsangaben von Go-Paketen und Azure-Diensten sind voneinander unabhängig. Die Dienstversionen sind Teil des Modulimportpfads im Modul `services`. Der vollständige Pfad für das Modul ist der Name des Diensts, gefolgt von der Version im Format `YYYY-MM-DD`, worauf erneut der Dienstname folgt. Beispiel für den Import der Version `2017-03-30` des Computediensts:

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

Es wird empfohlen, die aktuelle Version eines Diensts zu verwenden, wenn Sie mit der Entwicklung beginnen, und die Versionen konsistent zu halten.
Dienstanforderungen können sich zwischen Versionen ändern und zu Codefehlern führen, obwohl das Go SDK gar nicht aktualisiert wurde.

Wenn Sie eine gemeinsame Momentaufnahme aller Dienste benötigen, können Sie auch eine einzelne Profilversion wählen. Das einzige gesperrte Profil ist derzeit Version `2017-03-09`, die unter Umständen nicht über die aktuellen Features der Dienste verfügt. Profile befinden sich unter dem Modul `profiles`, und die Version wird im Format `YYYY-MM-DD` angegeben. Dienste werden unter ihrer Profilversion gruppiert. Beispiel für den Import des Verwaltungsmoduls für Azure-Ressourcen aus dem Profil `2017-03-09`:

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

* [Authentifizieren mit Azure-Diensten](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/internal/iam)
* [Bereitstellen neuer virtueller Computer mit SSH-Authentifizierung](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [Bereitstellen eines Containerimages für Azure Container Instances](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [Erstellen eines Clusters im Azure Kubernetes-Dienst](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute)
* [Arbeiten mit Azure Storage-Diensten](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [Alle Beispiele für das Azure SDK für Go](https://github.com/azure-samples/azure-sdk-for-go-samples)
