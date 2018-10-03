---
title: Authentifizierung mit dem Azure SDK für Go
description: Hier erfahren Sie, welche Authentifizierungsmethoden im Azure SDK für Go zur Verfügung stehen und wie Sie sie verwenden.
services: azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.technology: azure-sdk-go
ms.devlang: go
ms.component: authentication
ms.openlocfilehash: f5c2c56e43828f0bedad0b5781dc71991ce1fd3e
ms.sourcegitcommit: 172f81dd6e4c6a275dc8031815aa87cdb488cbf0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2018
ms.locfileid: "47231674"
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a>Authentifizierungsmethoden im Azure SDK für Go

Das Azure SDK für Go bietet mehrere Möglichkeiten für die Authentifizierung über Azure. Diese _Authentifizierungsarten_ können mit verschiedenen _Authentifizierungsmethoden_ aufgerufen werden. Dieser Artikel behandelt die verfügbaren Typen und Methoden und unterstützt Sie bei der Wahl der optimalen Typen und Methoden für Ihre Anwendung.

## <a name="available-authentication-types-and-methods"></a>Verfügbare Authentifizierungsarten und -methoden

Das Azure SDK für Go bietet verschiedene Authentifizierungsarten mit unterschiedlichen Anmeldeinformationen. Alle Authentifizierungsarten können jeweils über verschiedene Authentifizierungsmethoden verwendet werden, die dazu dienen, die Anmeldeinformationen als Eingabe an das SDK zu übergeben. Die folgende Tabelle enthält Informationen zu den verfügbaren Authentifizierungsarten sowie Verwendungsempfehlungen:

| Authentifizierungsart | Verwendungsempfehlung |
|---------------------|---------------------|
| Zertifikatbasierte Authentifizierung | Sie verfügen über ein X.509-Zertifikat, das für einen AAD-Benutzer oder -Dienstprinzipal (Azure Active Directory) konfiguriert wurde. Weitere Informationen finden Sie unter [Erste Schritte mit zertifikatbasierter Authentifizierung in Azure Active Directory]. |
| Client credentials (Clientanmeldeinformationen) | Sie verfügen über einen konfigurierten Dienstprinzipal, der für diese Anwendung oder für eine Klasse von Anwendungen konfiguriert ist, der sie angehört. Weitere Informationen finden Sie unter [Erstellen eines Dienstprinzipals über die Azure CLI]. |
| Verwaltete Identitäten für Azure-Ressourcen | Ihre Anwendung wird auf einer Azure-Ressource ausgeführt, die mit einer verwalteten Identität konfiguriert wurde. Weitere Informationen finden Sie unter [Verwaltete Identitäten für Azure-Ressourcen] (Worum handelt es sich bei verwalteten Identitäten für Azure-Ressourcen?). |
| Gerätetoken | Ihre Anwendung ist __nur__ für die interaktive Verwendung vorgesehen. Benutzer haben unter Umständen die mehrstufige Authentifizierung aktiviert. Benutzer können sich über einen Webbrowser anmelden. Weitere Informationen finden Sie unter [Verwenden der Gerätetokenauthentifizierung](#use-device-token-authentication).|
| Benutzername/Kennwort | Sie verfügen über eine interaktive Anwendung, die keine andere Authentifizierungsmethode verwenden kann. Für Ihre Benutzer ist die mehrstufige Authentifizierung für die AAD-Anmeldung nicht aktiviert. |

> [!IMPORTANT]
> Wenn Sie als Authentifizierungsart keine Clientanmeldeinformationen verwenden, muss Ihre Anwendung in Azure Active Directory registriert werden. Eine entsprechende Anleitung finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).
>
> [!NOTE]
> Verwenden Sie die Authentifizierung mit Benutzername/Kennwort nur, wenn dies zur Erfüllung besonderer Anforderungen erforderlich ist. In Situationen, in denen eine benutzerbasierte Anmeldung geeignet ist, kann in der Regel auch die Gerätetokenauthentifizierung verwendet werden.

[Erste Schritte mit zertifikatbasierter Authentifizierung in Azure Active Directory]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Erstellen eines Dienstprinzipals über die Azure CLI]: /cli/azure/create-an-azure-service-principal-azure-cli
[Verwaltete Identitäten für Azure-Ressourcen]: /azure/active-directory/managed-identities-azure-resources/overview

Diese Authentifizierungsarten können mit verschiedenen Methoden verwendet werden.

* Bei der [_umgebungsbasierten Authentifizierung_](#use-environment-based-authentication) werden die Anmeldeinformationen direkt aus der Umgebung des Programms gelesen.
* Bei der [_dateibasierten Authentifizierung_](#use-file-based-authentication) wird eine Datei mit Dienstprinzipal-Anmeldeinformationen geladen.
* Bei der [_clientbasierten Authentifizierung_](#use-an-authentication-client) wird ein Objekt im Code verwendet, und die Anmeldeinformationen müssen von Ihnen während der Programmausführung angegeben werden.
* Bei der [_Gerätetokenauthentifizierung_](#use-device-token-authentication) müssen sich Benutzer interaktiv mit einem Token über einen Webbrowser anmelden.

Alle Authentifizierungsfunktionen und -arten stehen im Paket `github.com/Azure/go-autorest/autorest/azure/auth` zur Verfügung.

> [!NOTE]
> Verwenden Sie die clientbasierte Authentifizierung nur, wenn dies zur Erfüllung besonderer Anforderungen erforderlich ist. Diese Authentifizierungsmethode verleitet zu nicht empfehlenswerten Vorgehensweisen. Dazu zählt in erster Linie die Verwendung hartcodierter Anmeldeinformationen. Bei benutzerdefiniertem Code für die Authentifizierung besteht die Gefahr, dass er in späteren SDK-Versionen nicht mehr funktioniert, falls sich die Authentifizierungsanforderungen ändern.

## <a name="use-environment-based-authentication"></a>Verwenden der umgebungsbasierten Authentifizierung

Wenn Sie Ihre Anwendung in einer kontrollierten Umgebung ausführen, ist die umgebungsbasierte Authentifizierung eine naheliegende Option. Bei dieser Authentifizierungsmethode konfigurieren Sie die Shell-Umgebung vor dem Ausführen Ihrer Anwendung. Zur Laufzeit liest das Go SDK diese Umgebungsvariablen für die Authentifizierung bei Azure.

Die umgebungsbasierte Authentifizierung unterstützt alle Authentifizierungsmethoden (mit Ausnahme von Gerätetoken). Die Auswertung erfolgt in der folgenden Reihenfolge:

* Client credentials (Clientanmeldeinformationen)
* X509-Zertifikate
* Benutzername/Kennwort
* Verwaltete Identitäten für Azure-Ressourcen

Sind für einen Authentifizierungstyp keine Werte festgelegt oder wird der Authentifizierungstyp verweigert, probiert das SDK automatisch den nächsten Authentifizierungstyp aus. Wenn keine weitere Typen mehr übrig sind, gibt das SDK einen Fehler zurück.

Die folgende Tabelle gibt Aufschluss über die Umgebungsvariablen, die für die einzelnen Authentifizierungsarten festgelegt werden müssen, die von der umgebungsbasierten Authentifizierung unterstützt werden.

| Authentifizierungsart | Umgebungsvariable | BESCHREIBUNG |
| ------------------- | -------------------- | ----------- |
| __Clientanmeldeinformationen__ | `AZURE_TENANT_ID` | Die ID für den Active Directory-Mandanten, zu dem der Dienstprinzipal gehört. |
| | `AZURE_CLIENT_ID` | Der Name oder die ID des Dienstprinzipals. |
| | `AZURE_CLIENT_SECRET` | Das dem Dienstprinzipal zugeordnete Geheimnis |
| __Certificate__ | `AZURE_TENANT_ID` | Die ID für den Active Directory-Mandanten, bei dem das Zertifikat registriert ist. |
| | `AZURE_CLIENT_ID` | Die dem Zertifikat zugeordnete Anwendungsclient-ID. |
| | `AZURE_CERTIFICATE_PATH` | Der Pfad der Clientzertifikatdatei. |
| | `AZURE_CERTIFICATE_PASSWORD` | Das Kennwort für das Clientzertifikat. |
| __Benutzername/Kennwort__ | `AZURE_TENANT_ID` | Die ID für den Active Directory-Mandanten, zu dem der Benutzer gehört. |
| | `AZURE_CLIENT_ID` | Die Anwendungsclient-ID. |
| | `AZURE_USERNAME` | Der Benutzername für die Anmeldung. |
| | `AZURE_PASSWORD` | Das Kennwort für die Anmeldung. |
| __Verwaltete Identität__ | | Für die Authentifizierung der verwalteten Identität sind keine Anmeldeinformationen erforderlich. Die Anwendung muss auf einer Azure-Ressource ausgeführt werden, die für die Verwendung von verwalteten Identitäten konfiguriert ist. Ausführliche Informationen finden Sie unter [Verwaltete Identitäten für Azure-Ressourcen] (Worum handelt es sich bei verwalteten Identitäten für Azure-Ressourcen?). |

Wenn Sie eine Verbindung mit einem Cloud- oder Verwaltungsendpunkt herstellen möchten, bei dem es sich nicht um die standardmäßige öffentliche Azure-Cloud handelt, legen Sie die folgenden Umgebungsvariablen fest. Diese werden häufig bei Verwendung von Azure Stack, bei Verwendung einer Cloud in einer anderen geografischen Region oder bei Verwendung des klassischen Bereitstellungsmodells festgelegt.

| Umgebungsvariable | BESCHREIBUNG  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | Der Name der Cloudumgebung, mit der eine Verbindung hergestellt werden soll. |
| `AZURE_AD_RESOURCE` | Die Active Directory-Ressourcen-ID, die beim Herstellen einer Verbindung mit dem Verwaltungsendpunkt als URI verwendet wird |

Rufen Sie bei Verwendung der umgebungsbasierten Authentifizierung die Funktion [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) auf, um Ihr Authorizer-Objekt abzurufen. Dieses Objekt wird dann für die Eigenschaft `Authorizer` von Clients festgelegt, um ihnen den Zugriff auf Azure zu ermöglichen.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

### <a name="authentication-on-azure-stack"></a>Authentifizierung in Azure Stack

Für die Authentifizierung in Azure Stack müssen Sie die folgenden Variablen festlegen:

| Umgebungsvariable | BESCHREIBUNG  |
|----------------------|--------------|
| `AZURE_AD_ENDPOINT` | Active Directory-Endpunkt |
| `AZURE_AD_RESOURCE` | Active Directory-Ressourcen-ID |

Diese Variablen können aus den Azure Stack-Metadateninformationen abgerufen werden. Öffnen Sie zum Abrufen der Metadaten einen Webbrowser in Ihrer Azure Stack-Umgebung, und geben Sie die folgende URL ein: `(ResourceManagerURL)/metadata/endpoints?api-version=1.0`.

`ResourceManagerURL` variiert basierend auf dem Regionsnamen, dem Computernamen und dem externen vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) der Azure Stack-Bereitstellung:

| Environment | ResourceManagerURL |
|----------------------|--------------|
| Development Kit | `https://management.local.azurestack.external/` |
| Integrierte Systeme | `https://management.(region).ext-(machine-name).(FQDN)` |

Weitere Informationen zur Verwendung des Azure SDK für Go in Azure Stack finden Sie unter [Verwenden von API-Versionsprofilen mit Go in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-go).

## <a name="use-file-based-authentication"></a>Verwenden der dateibasierten Authentifizierung

Bei der dateibasierten Authentifizierung wird ein von der [Azure CLI](/cli/azure) generiertes Dateiformat verwendet. Diese Datei können Sie im Rahmen der Erstellung eines neuen Dienstprinzipals ganz einfach mit dem Parameter `--sdk-auth` erstellen. Wenn Sie die dateibasierte Authentifizierung verwenden möchten, achten Sie darauf, dass dieses Argument bei der Dienstprinzipalerstellung angegeben ist. Da die CLI-Ausgabe in `stdout` erfolgt, leiten Sie die Ausgabe in eine Datei um.

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

Legen Sie die Umgebungsvariable `AZURE_AUTH_LOCATION` auf den Speicherort der Autorisierungsdatei fest. Diese Umgebungsvariable wird von der Anwendung gelesen, und die darin enthaltenen Anmeldeinformationen werden analysiert. Wenn Sie die Autorisierungsdatei zur Laufzeit auswählen müssen, bearbeiten Sie die Programmumgebung mithilfe der Funktion [os.Setenv](https://golang.org/pkg/os/#Setenv).

Rufen Sie zum Laden der Authentifizierungsinformationen die Funktion [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) auf. Im Gegensatz zur umgebungsbasierten Autorisierung benötigt die dateibasierte Autorisierung einen Ressourcenendpunkt.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

Weitere Informationen zur Verwendung von Dienstprinzipalen sowie zur Verwaltung ihrer Zugriffsberechtigungen finden Sie unter [Erstellen eines Dienstprinzipals über die Azure CLI].

## <a name="use-device-token-authentication"></a>Verwenden der Gerätetokenauthentifizierung

Wenn sich Benutzer interaktiv anmelden sollen, empfiehlt sich die Verwendung der Gerätetokenauthentifizierung. Bei diesem Authentifizierungsablauf wird ein Token an den Benutzer übergeben, das dieser auf einer Anmeldewebsite von Microsoft einfügt, auf der er sich bei einem AAD-Konto (Azure Active Directory) authentifiziert. Im Gegensatz zur Standardauthentifizierung mit Benutzername/Kennwort unterstützt diese Authentifizierungsmethode Konten mit aktivierter mehrstufiger Authentifizierung.

Erstellen Sie zur Verwendung der Gerätetokenauthentifizierung mithilfe der Funktion [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig) ein Authorizer-Objekt vom Typ [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig). Rufen Sie [Authorizer](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) für das resultierende Objekt auf, um den Authentifizierungsprozess zu starten. Die Geräteauthentifizierung blockiert die Programmausführung, bis der gesamte Authentifizierungsablauf abgeschlossen ist.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a>Verwenden eines Authentifizierungsclients

Wenn Sie eine bestimmte Art von Authentifizierung benötigen und das Laden der Authentifizierungsinformationen des Benutzers Ihrem Programm überlassen möchten, können Sie einen beliebigen Client verwenden, der mit der Schnittstelle [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig) kompatibel ist. Verwenden Sie einen Typ, der diese Schnittstelle in den folgenden Fällen implementiert:

* Schreiben eines interaktiven Programms
* Verwenden spezieller Konfigurationsdateien
* Anforderung, die die Verwendung einer integrierten Authentifizierungsmethode verhindert

> [!WARNING]
> Verwenden Sie in einer Anwendung niemals hartcodierte Azure-Anmeldeinformationen. In der Binärdatei einer Anwendung enthaltene Geheimnisse können von Angreifern leichter extrahiert werden. Dabei spielt es keine Rolle, ob die Anwendung ausgeführt wird oder nicht. Dies gefährdet alle Azure-Ressourcen, für die die Anmeldeinformationen autorisiert sind.

Die folgende Tabelle enthält die mit der Schnittstelle `AuthorizerConfig` kompatiblen Arten aus dem SDK.

| Authentifizierungsart | Authorizer-Typ |
|---------------------|-----------------------|
| Zertifikatbasierte Authentifizierung | [ClientCertificateConfig] |
| Client credentials (Clientanmeldeinformationen) | [ClientCredentialsConfig] |
| Verwaltete Identitäten für Azure-Ressourcen | [MSIConfig] |
| Benutzername/Kennwort | [UsernamePasswordConfig] |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

Erstellen Sie einen Authentifikator mit der entsprechenden `New`-Funktion, und rufen Sie dann `Authorize` für das resultierende Objekt auf, um die Authentifizierung durchzuführen. Beispiel für die zertifikatbasierte Authentifizierung:

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
