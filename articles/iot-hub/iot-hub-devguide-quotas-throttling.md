---
title: Informationen zu Kontingenten und Drosselung bei Azure IoT Hub | Microsoft Docs
description: 'Entwicklerhandbuch: Beschreibung der für IoT Hub geltenden Kontingente und des erwarteten Drosselungsverhaltens'
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: dobett
ms.openlocfilehash: d75a2cef96eaafb606c66d469b0e27fed8bb3573
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2019
ms.locfileid: "55466811"
---
# <a name="reference---iot-hub-quotas-and-throttling"></a>Referenz: IoT Hub-Kontingente und -Drosselung

## <a name="quotas-and-throttling"></a>Kontingente und Drosselung

Jedes Azure-Abonnement kann maximal 50 IoT Hubs und höchstens einen Hub vom Typ „Free“ enthalten.

Jede IoT Hub-Instanz wird mit einer bestimmten Anzahl von Einheiten zu einem spezifischen Tarif bereitgestellt. Der Tarif und die Anzahl der Einheiten bestimmen das maximale tägliche Kontingent von Nachrichten, die Sie senden können. Die zum Berechnen des täglichen Kontingents verwendete Nachrichtengröße beträgt 0,5 KB für einen Hub mit dem Tarif „Free“ (kostenlos) und 4 KB für alle anderen Tarife. Weitere Informationen finden Sie unter [IoT Hub – Preise](https://azure.microsoft.com/pricing/details/iot-hub/).

Der Tarif legt auch die Drosselungslimits fest, die IoT Hub für alle Vorgänge erzwingt.

## <a name="operation-throttles"></a>Vorgangsdrosselung

Bei der Vorgangsdrosselung wird die Datenübertragungsrate pro Minute begrenzt, um einen Missbrauch zu verhindern. IoT Hub versucht, möglichst keine Fehler zurückzugeben, es werden jedoch `429 ThrottlingException` ausgelöst, wenn die Drosselungsgrenze zu lange überschritten wird.

Sie können die Kontingente oder Drosselungsgrenzwerte jederzeit erhöhen, indem Sie die Anzahl von bereitgestellten Einheiten in einem IoT Hub erhöhen.

Die folgende Tabelle zeigt die erzwungenen Drosselungen. Die Werte beziehen sich auf einen einzelnen Hub.

| Drosselung | Kostenlos, B1 und S1 | B2 und S2 | B3 und S3 | 
| -------- | ------- | ------- | ------- |
| Identitätsregistrierungsvorgänge (Erstellen, Abrufen, Aktualisieren, Löschen) | 1,67/Sekunde/Einheit (100/Minute/Einheit) | 1,67/Sekunde/Einheit (100/Minute/Einheit) | 83,33/Sekunde/Einheit (5.000/Minute/Einheit) |
| Neue Geräteverbindungen (diese Begrenzung gilt für die Rate, mit der _neue Verbindungen_ hergestellt werden, nicht die Gesamtzahl der Verbindungen) | 100/Sekunde oder 12/Sekunde/Einheit – je nachdem, was höher ist <br/> Beispielsweise ergeben zwei S1-Einheiten 2\*12 = 24 neue Verbindungen/Sek., Sie erhalten jedoch mindestens 100 neue Verbindungen/Sek. für Ihre Einheiten. Mit neun S1-Einheiten erhalten Sie 108 neue Verbindungen/Sek. (9\*12) für all Ihre Einheiten. | 120 neue Verbindungen/Sek./Einheit | 6000 neue Verbindungen/Sek./Einheit |
| Senden von Nachrichten von Geräten an die Cloud | 100/Sekunde oder 12/Sekunde/Einheit – je nachdem, was höher ist <br/> Zwei S1-Einheiten entsprechen beispielsweise 2\*12 = 24 Sekunden. Es sind jedoch mindestens 100 Sekunden auf die Einheiten verteilt vorhanden. Mit neun S1-Einheiten erhalten Sie 108 Sekunden (9\*12) über alle Einheiten. | 120/Sekunde/Einheit | 6.000/Sekunde/Einheit |
| C2D-Sendevorgänge<sup>1</sup> | 1,67/Sekunde/Einheit (100/Minute/Einheit) | 1,67/Sekunde/Einheit (100/Minute/Einheit) | 83,33/Sekunde/Einheit (5.000/Minute/Einheit) |
| C2D-Empfangsvorgänge<sup>1</sup> <br/> (nur bei Verwendung von HTTPS durch das Gerät)| 16,67/Sekunde/Einheit (1.000/Minute/Einheit) | 16,67/Sekunde/Einheit (1.000/Minute/Einheit) | 833,33/Sekunde/Einheit (50.000/Minute/Einheit) |
| Dateiupload | 1,67 Dateiuploadbenachrichtigungen/Sekunde/Einheit (100/Minute/Einheit) | 1,67 Dateiuploadbenachrichtigungen/Sekunde/Einheit (100/Minute/Einheit) | 83,33 Dateiuploadbenachrichtigungen/Sekunde/Einheit (5.000/Minute/Einheit) |
| Direkte Methoden<sup>1</sup> | 160KB/s/Einheit<sup>2</sup> | 480KB/s/Einheit<sup>2</sup> | 24MB/s/Einheit<sup>2</sup> | 
| Zwillingslesevorgänge (Gerät und Modul)<sup>1</sup> | 10/Sekunde | 10/Sekunde oder 1/Sekunde/Einheit – je nachdem, was höher ist | 50/Sekunde/Einheit |
| Zwillingsupdates (Gerät und Modul)<sup>1</sup> | 10/Sekunde | 10/Sekunde oder 1/Sekunde/Einheit – je nachdem, was höher ist | 50/Sekunde/Einheit |
| Auftragsvorgänge<sup>1,3</sup> <br/> (Erstellen, Aktualisieren, Auflisten, Löschen) | 1,67/Sekunde/Einheit (100/Minute/Einheit) | 1,67/Sekunde/Einheit (100/Minute/Einheit) | 83,33/Sekunde/Einheit (5.000/Minute/Einheit) |
| Aufträgegerätevorgänge<sup>1</sup> <br/> (Gerätezwilling aktualisieren, direkte Methode aufrufen) | 10/Sekunde | 10/Sekunde oder 1/Sekunde/Einheit – je nachdem, was höher ist | 50/Sekunde/Einheit |
| Konfigurationen und Edgebereitstellungen<sup>1</sup> <br/> (Erstellen, Aktualisieren, Auflisten, Löschen) | 0,33/Sek./Einheit (20/Min./Einheit) | 0,33/Sek./Einheit (20/Min./Einheit) | 0,33/Sek./Einheit (20/Min./Einheit) |
| Initiierungsrate für Gerätedatenstrom<sup>4</sup> | 5 neue Streams/Sekunde | 5 neue Streams/Sekunde | 5 neue Streams/Sekunde |
| Maximale Anzahl der gleichzeitig verbundenen Gerätedatenströme<sup>4</sup> | 50 | 50 | 50 |
| Maximale Gerätedatenstrom-Datenübertragung<sup>4</sup> (aggregiertes Volumen pro Tag) | 300MB | 300MB | 300MB |


<sup>1</sup>Dieses Feature ist im Tarif „Basic“ von IoT Hub nicht verfügbar. Weitere Informationen finden Sie unter [Wählen des richtigen IoT Hub-Tarifs für Ihre Lösung](iot-hub-scaling.md). <br/><sup>2</sup> Die Größe der Verbrauchseinheit für die Drosselung beträgt 8KB. <br/><sup>3</sup>Es kann jederzeit nur ein aktiver Geräteimport-/-exportauftrag vorhanden sein. <br/><sup>4</sup>IoT Hub-Gerätedatenströme sind nur für S1-, S2-, S3- und F1-SKUs verfügbar.

Die Drosselung der *Geräteverbindungen* bestimmt die Rate, mit der neue Geräteverbindungen mit einem IoT Hub hergestellt werden können. Die Drosselung der *Geräteverbindungen* steuert nicht die maximale Anzahl gleichzeitig verbundener Geräte. Die Drosselung der *Geräteverbindungsrate* ist abhängig von der Anzahl der Einheiten, die für den IoT-Hub bereitgestellt werden.

Wenn Sie beispielsweise eine S1-Einheit erwerben, erhalten Sie eine Drosselung von 100 Verbindungen pro Sekunde. Darum dauert das Herstellen einer Verbindung mit 100.000 Geräten mindestens 1.000 Sekunden (ca. 16 Minuten). Es können jedoch so viele Geräte gleichzeitig verbunden sein, wie in der Identitätsregistrierung registriert sind.

Eine ausführliche Erläuterung der IoT Hub-Drosselung finden Sie in dem Blogbeitrag [IoT Hub throttling and you](https://azure.microsoft.com/blog/iot-hub-throttling-and-you/) (Was habe ich mit der IoT Hub-Drosselung zu tun?).

> [!IMPORTANT]
> Identitätsregistrierungsvorgänge sind für die Verwendung zur Laufzeit in Szenarien für die Geräteverwaltung und -bereitstellung vorgesehen. Das Lesen oder Aktualisieren einer großen Anzahl von Geräteidentitäten wird durch [Import-/Exportaufträge](iot-hub-devguide-identity-registry.md#import-and-export-device-identities) unterstützt.
> 
> 

## <a name="other-limits"></a>Andere Limits

IoT Hub erzwingt andere Funktionsbegrenzungen:

| Vorgang | Begrenzung |
| --------- | ----- |
| Dateiupload-URIs | 10.000 SAS-URIs können gleichzeitig für ein Speicherkonto geöffnet sein. <br/>  10 SAS-URIs/Gerät können gleichzeitig geöffnet sein. |
| Aufträge<sup>1</sup> | Der Auftragsverlauf wird bis zu 30 Tage lang gespeichert. <br/> Maximale Anzahl gleichzeitiger Aufträge: 1 (für Free und S1), 5 (für S2), 10 (für S3) |
| Zusätzliche Endpunkte | Kostenpflichtige SKU-Hubs haben möglicherweise 10 zusätzliche Endpunkte. Kostenfreie SKU-Hubs haben möglicherweise einen zusätzlichen Endpunkt. |
| Regeln für die Nachrichtenweiterleitung | Kostenpflichtige SKU-Hubs haben möglicherweise 100 Weiterleitungsregeln. Kostenfreie SKU-Hubs haben möglicherweise fünf Weiterleitungsregeln. |
| Nachrichten, die von Geräten an die Cloud gesendet werden | Maximale Nachrichtengröße 256 KB |
| Cloud-zu-Gerät-Messaging<sup>1</sup> | Maximale Nachrichtengröße 64KB. Maximale Anzahl ausstehender Nachrichten für die Übermittlung: 50. |
| Direkte Methode<sup>1</sup> | Die maximale Nutzlast für direkte Methoden beträgt 128KB. |
| Automatische Gerätekonfigurationen<sup>1</sup> | 100 Konfigurationen pro kostenpflichtigem SKU-Hub. 20 Konfigurationen pro kostenfreiem SKU-Hub. |
| Automatische Edgebereitstellungen<sup>1</sup> | 20 Module pro Bereitstellung 100 Bereitstellungen pro kostenpflichtigem SKU-Hub. 20 Bereitstellungen pro kostenfreiem SKU-Hub. |
| Zwillinge<sup>1</sup> | Die maximale Größe pro Zwillingsabschnitt (Tags, gewünschte Eigenschaften, gemeldete Eigenschaften) beträgt 8 KB. |

<sup>1</sup>Dieses Feature ist im Tarif „Basic“ von IoT Hub nicht verfügbar. Weitere Informationen finden Sie unter [Wählen des richtigen IoT Hub-Tarifs für Ihre Lösung](iot-hub-scaling.md).

> [!NOTE]
> Zurzeit können höchstens 1.000.000 Geräte mit einem einzelnen IoT Hub verbunden werden. Wenn Sie diesen Grenzwert erhöhen möchten, wenden Sie sich an [Microsoft-Support](https://azure.microsoft.com/support/options/).

## <a name="latency"></a>Latency
IoT Hub sorgt bei allen Vorgängen für eine möglichst niedrige Latenz. Abhängig von den Netzwerkbedingungen und aufgrund weiterer unvorhersehbarer Faktoren kann eine maximale Latenzzeit jedoch nicht garantiert werden. Beim Entwerfen Ihrer Lösung sollten Sie Folgendes beachten:

* Treffen Sie keine Annahmen über die maximale Latenz von IoT Hub-Vorgängen.
* Stellen Sie Ihren IoT Hub in der Azure-Region in möglichst geringer Entfernung zu Ihren Geräten bereit.
* Erwägen Sie den Einsatz von Azure IoT Edge, um Vorgänge, für die die Latenz von Bedeutung ist, auf dem Gerät oder auf einem Gateway in der Nähe des Geräts auszuführen.

Mehrere IoT Hub-Einheiten wirken sich wie zuvor beschrieben auf die Drosselung aus, bieten jedoch keine zusätzlichen Vorteile oder Garantien in Bezug auf die Latenz.

Wenden Sie sich im Falle eines unerwarteten Anstiegs der Vorgangslatenz an den [Microsoft-Support](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Nächste Schritte

Weitere Referenzthemen in diesem IoT Hub-Entwicklungsleitfaden:

* [IoT Hub-Endpunkte](iot-hub-devguide-endpoints.md)
