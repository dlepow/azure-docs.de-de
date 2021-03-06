---
title: Verwalten von Verbindungen in Azure Functions
description: Erfahren Sie, wie Sie durch die Verwendung statischer Verbindungsclients Leistungsprobleme in Azure Functions vermeiden.
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: glenga
ms.openlocfilehash: 4246259445cf096b5353ab87a9ed83f87332dc78
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2019
ms.locfileid: "56299325"
---
# <a name="how-to-manage-connections-in-azure-functions"></a>Verwalten von Verbindungen in Azure Functions

Azure Functions nutzt in einer Funktionen-App gemeinsame Ressourcen. Zu diesen gemeinsam genutzten Ressourcen zählen Verbindungen &mdash; HTTP-Verbindungen, Datenbankverbindungen und Verbindungen zu Azure-Diensten wie Azure Storage. Wenn viele Funktionen gleichzeitig ausgeführt werden, kann es sein, dass nicht mehr genügend Verbindungen zur Verfügung stehen. In diesem Artikel erfahren Sie, wie Sie Ihre Funktionen codieren, um zu vermeiden, dass Sie mehr Verbindungen als eigentlich erforderlich verwenden.

## <a name="connections-limit"></a>Verbindungslimit

Die Anzahl der verfügbaren Verbindungen ist teilweise begrenzt, da eine Funktionen-App im [Azure App Service-Sandkasten](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) ausgeführt wird. Eine Einschränkung, die der Sandkasten Ihrem Code auferlegt, ist eine [Obergrenze für die Anzahl der Verbindungen (derzeit 300)](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#numerical-sandbox-limits). Wenn dieses Limit erreicht ist, erstellt Functions Runtime ein Protokoll mit folgender Meldung: `Host thresholds exceeded: Connections`.

Die Wahrscheinlichkeit, dass das Limit überschritten wird, steigt, wenn der [Skalierungscontroller Funktions-App-Instanzen hinzufügt](functions-scale.md#how-the-consumption-plan-works), um mehr Anforderungen zu verarbeiten. Jede Funktions-App-Instanz kann mehrere Funktionen auf einmal ausführen, die alle Verbindungen nutzen, die in die Berechnung des Grenzwerts „300“ einbezogen werden.

## <a name="use-static-clients"></a>Verwenden von statischen Clients

Um zu vermeiden, dass mehr Verbindungen als nötig gehalten werden, erstellen Sie bei jedem Funktionsaufruf keine neuen Instanzen, sondern verwenden die Clientinstanzen erneut. .NET-Clients wie [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) und [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
) sowie Azure Storage-Clients können Verbindungen verwalten, wenn Sie einen einzelnen, statischen Client verwenden.

Es folgen einige Richtlinien, die zu beachten sind, wenn Sie einen dienstspezifischen Client in einer Azure Functions-Anwendung verwenden:

- **NEIN** Erstellen Sie keinen neuen Client bei jedem Funktionsaufruf.
- **JA** Erstellen Sie einen einzelnen, statischen Client, der von jedem Funktionsaufruf verwendet werden kann.
- **VIELLEICHT** Denken Sie über die Erstellung eines einzelnen, statischen Clients in einer freigegebenen Hilfsprogrammklasse nach, wenn verschiedene Funktionen den gleichen Dienst verwenden.

## <a name="client-code-examples"></a>Clientcodebeispiele

Dieser Abschnitt veranschaulicht Best Practices für die Erstellung und Verwendung von Clients über den Funktionscode.

### <a name="httpclient-example-c"></a>HttpClient-Beispiel (C#)

Es folgt ein Beispiel für einen C#-Funktionscode, der einen statischen [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) erstellt:

```cs
// Create a single, static HttpClient
private static HttpClient httpClient = new HttpClient();

public static async Task Run(string input)
{
    var response = await httpClient.GetAsync("https://example.com");
    // Rest of function
}
```

Eine häufig gestellte Frage zum .NET-[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx) lautet: „Soll ich meinen Client löschen?“. Im Allgemeinen löschen Sie Objekte, die `IDisposable` implementieren, wenn Sie sie nicht mehr verwenden. Sie löschen jedoch keinen statischen Client, da dessen Verwendung mit dem Funktionsende nicht abgeschlossen ist. Der statische Client soll für die Dauer der Anwendung gültig sein.

### <a name="http-agent-examples-nodejs"></a>HTTP-Agent-Beispiele (Node.js)

Da die native [`http.agent`](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_class_http_agent)-Klasse bessere Optionen für die Verbindungsverwaltung bietet, sollten Sie diese anstelle von nicht nativen Methoden wie etwa dem `node-fetch`-Modul verwenden. Verbindungsparameter werden mithilfe von Optionen in der `http.agent`-Klasse konfiguriert. Die genauen Optionen, die mit dem HTTP-Agent verfügbar sind, finden Sie unter [new Agent(\[options\])](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_new_agent_options).

Für den globalen `http.globalAgent`, der von `http.request()` verwendet wird, sind all diese Werte auf ihre jeweiligen Standardwerte festgelegt. Die empfohlene Vorgehensweise zum Konfigurieren von Verbindungsbeschränkungen in Functions besteht darin, global eine maximale Anzahl festzulegen. Das folgende Beispiel legt die maximale Anzahl von Sockets für die Funktions-App fest:

```js
http.globalAgent.maxSockets = 200;
```

 Das folgende Beispiel erstellt eine neue HTTP-Anforderung mit einem benutzerdefinierten HTTP-Agent nur für diese Anforderung.

```js
var http = require('http');
var httpAgent = new http.Agent();
httpAgent.maxSockets = 200;
options.agent = httpAgent;
http.request(options, onResponseCallback);
```

### <a name="documentclient-code-example-c"></a>Codebeispiel für DocumentClient (C#)

[DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
) stellt eine Verbindung mit einer Azure Cosmos DB-Instanz her. In der Azure Cosmos DB-Dokumentation wird empfohlen, dass Sie [einen Singleton-Azure Cosmos DB-Client für die Lebensdauer der Anwendung verwenden](https://docs.microsoft.com/azure/cosmos-db/performance-tips#sdk-usage). Im folgenden Beispiel wird ein Muster dafür in einer Funktion gezeigt:

```cs
#r "Microsoft.Azure.Documents.Client"
using Microsoft.Azure.Documents.Client;

private static Lazy<DocumentClient> lazyClient = new Lazy<DocumentClient>(InitializeDocumentClient);
private static DocumentClient documentClient => lazyClient.Value;

private static DocumentClient InitializeDocumentClient()
{
    // Perform any initialization here
    var uri = new Uri("example");
    var authKey = "authKey";
    
    return new DocumentClient(uri, authKey);
}

public static async Task Run(string input)
{
    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("database", "collection");
    object document = new { Data = "example" };
    await documentClient.UpsertDocumentAsync(collectionUri, document);
    
    // Rest of function
}
```

## <a name="sqlclient-connections"></a>SqlClient-Verbindungen

Funktionscode kann den .NET Framework-Datenanbieter für SQL Server ([SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx)) verwenden, um Verbindungen zu einer relationalen SQL-Datenbank herzustellen. Dies ist auch der zugrunde liegende Anbieter für Daten-Frameworks, die ADO.NET verwenden, z.B. Entity Framework. Im Gegensatz zu [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)- und [DocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient
)-Verbindungen implementiert ADO.NET standardmäßig das Verbindungspooling. Da jedoch noch immer nicht genügend Verbindungen verfügbar sein können, sollten Sie die Verbindungen mit der Datenbank optimieren. Weitere Informationen finden Sie unter [SQL Server-Verbindungspooling (ADO.NET)](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling).

> [!TIP]
> Einige Daten-Frameworks, z.B. [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx), rufen Verbindungszeichenfolgen üblicherweise aus dem Abschnitt **ConnectionStrings** einer Konfigurationsdatei ab. In diesem Fall müssen Sie der Sammlung **Verbindungszeichenfolgen** der Funktions-App-Einstellungen und der Datei [local.settings.json](functions-run-local.md#local-settings-file) im lokalen Projekt explizit SQL-Datenbank-Verbindungszeichenfolgen hinzufügen. Bei der Erstellung einer [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx)-Klasse in Ihrem Funktionscode sollten Sie den Verbindungszeichenfolgenwert zusammen mit den anderen Verbindungen in den **Anwendungseinstellungen** speichern.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Empfehlung von statischen Clients finden Sie unter [Antimuster für ungeeignete Instanziierung](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/).

Weitere Tipps zur Leistungssteigerung von Azure Functions finden Sie unter [Optimieren der Leistung und Zuverlässigkeit von Azure Functions](functions-best-practices.md).
