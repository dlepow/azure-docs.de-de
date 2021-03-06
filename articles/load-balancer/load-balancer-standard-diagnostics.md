---
title: Diagnose von Azure Load Balancer Standard
titlesuffix: Azure Load Balancer
description: Verwenden Sie die verfügbaren Metriken und Integritätsinformationen zur Diagnose für Azure Standard Load Balancer.
services: load-balancer
documentationcenter: na
author: KumudD
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2018
ms.author: Kumud
ms.openlocfilehash: 77c3c595994092ff2ca68f3cefa5eb3c8a54bcd6
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53189046"
---
# <a name="metrics-and-health-diagnostics-for-standard-load-balancer"></a>Metriken und Integritätsdiagnosen für Standard Load Balancer

Azure Load Balancer Standard bietet Ihren Ressourcen die folgenden Diagnosefunktionen:
* **Mehrdimensionale Metriken**: Stellt neue mehrdimensionale Diagnosefunktionen über [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) sowohl für öffentliche als auch für interne Load Balancer-Konfigurationen bereit. Sie können Ihre Load Balancer-Ressourcen überwachen, verwalten und hinsichtlich Fehlern behandeln.

* **Ressourcenintegrität**: Die Seite „Load Balancer“ im Azure-Portal und die Seite „Ressourcenintegrität“ (unter „Monitor“) stellen den Abschnitt „Ressourcenintegrität“ für die öffentliche Load Balancer-Konfiguration von Load Balancer Standard bereit.

Dieser Artikel enthält einen kurzen Überblick über diese Funktionen und zeigt Möglichkeiten auf, wie diese für Standard Load Balancer verwendet werden können.

## <a name = "MultiDimensionalMetrics"></a>Mehrdimensionale Metriken

Azure Load Balancer stellt neue mehrdimensionale Metriken über die neuen Azure-Metriken (Vorschau) im Azure-Portal zur Verfügung und ermöglicht es Ihnen, in Echtzeit diagnostische Einblicke in Ihre Load Balancer-Ressourcen zu erhalten. 

Die verschiedenen Standard Load Balancer-Konfigurationen bieten die folgenden Metriken:

| Metrik | Ressourcentyp | BESCHREIBUNG | Empfohlene Aggregation |
| --- | --- | --- | --- |
| VIP-Verfügbarkeit (Datenpfadverfügbarkeit) | Öffentlicher Load Balancer | Standard Load Balancer wendet kontinuierlich den Datenpfad aus einer Region auf das Load Balancer-Front-End bis hin zum SDN-Stapel an, der Ihren virtuellen Computer unterstützt. Solange integre Instanzen verbleiben, folgt die Messung demselben Pfad wie der Datenverkehr mit Lastenausgleich Ihrer Anwendungen. Der Datenpfad, der von Ihren Kunden verwendet wird, wird ebenfalls überprüft. Die Messung ist für Ihre Anwendung nicht sichtbar und bewirkt keine Beeinträchtigung bei anderen Vorgängen.| Durchschnitt |
| DIP-Verfügbarkeit (Integritätsteststatus) |  Öffentlicher und interner Load Blancer | Standard Load Balancer verwendet einen verteilten Integritätsprüfungsdienst, der die Integrität Ihres Anwendungsendpunkts gemäß Ihren Konfigurationseinstellungen überwacht. Diese Metrik stellt eine Aggregat- oder nach Endpunkt gefilterte Ansicht jedes Instanzendpunkts im Load Balancer-Pool bereit. Sie können sehen, wie Load Balancer die Integrität Ihrer Anwendung gemäß Ihrer Integritätsprüfungskonfiguration beurteilt. |  Durchschnitt |
| SYN-Pakete (Synchronisierung) |  Öffentlicher Load Balancer | Standard Load Balancer beendet keine TCP-Verbindungen (Transmission Control Protocol) und interagiert nicht mit TCP- oder UDP-Paketdatenflüssen. Datenflüsse und deren Handshakes erfolgen immer zwischen der Quelle und der VM-Instanz. Für eine bessere Problembehandlung Ihrer TCP-Protokollszenarien können Sie SYN-Paketzähler verwenden, um zu verstehen, wie viele TCP-Verbindungsversuche vorgenommen werden. Die Metrik gibt die Anzahl der TCP-SYN-Pakete an, die empfangen wurden.| Durchschnitt |
| SNAT-Verbindungen |  Öffentlicher Load Balancer |Standard Load Balancer meldet die Anzahl von maskierten ausgehenden Datenflüssen an das Front-End mit der öffentlichen IP-Adresse. SNAT-Ports (Source Network Address Translation) stellen eine erschöpfbare Ressource dar. Diese Metrik kann einen Hinweis darauf geben, in welchem Umfang Ihre Anwendung SNAT für ausgehende Flows nutzt. Die Zählerstände für erfolgreiche und fehlgeschlagene ausgehende SNAT-Flows werden gemeldet und können zur Problembehebung und Analyse Ihrer ausgehenden Flows verwendet werden.| Durchschnitt |
| Byteleistungsindikatoren |  Öffentlicher und interner Load Blancer | Standard Load Balancer meldet die pro Front-End verarbeiteten Daten.| Durchschnitt |
| Paketleistungsindikatoren |  Öffentlicher und interner Load Blancer | Standard Load Balancer meldet die pro Front-End verarbeiteten Pakete.| Durchschnitt |

### <a name="view-your-load-balancer-metrics-in-the-azure-portal"></a>Anzeigen Ihrer Load Balancer-Metriken im Azure-Portal

Das Azure-Portal macht die Load Balancer Metriken über die Seite „Metriken (Vorschau)“ verfügbar, die sowohl auf der Seite „Load Balancer-Ressource“ für eine bestimmte Ressource als auch auf der Seite „Azure Monitor“ verfügbar ist. 

So zeigen Sie die Metriken für Ihre Standard Load Balancer-Ressourcen an
1. Wechseln Sie zur Seite „Metriken (Vorschau)"“ und führen Sie einen der folgenden Schritte aus:
   * Wählen Sie auf der Seite „Load Balancer-Ressource“ den Metriktyp in der Dropdownliste aus.
   * Wählen Sie auf der Seite „Azure Monitor“ die Load Balancer-Ressource aus.
2. Legen Sie den entsprechenden Aggregationstyp fest.
3. Konfigurieren Sie optional die erforderliche Filterung und Gruppierung.

![Metrikenvorschau für Standard Load Balancer](./media/load-balancer-standard-diagnostics/LBMetrics1.png)

*Abbildung: Metrik für DIP-Verfügbarkeit und Integritätsteststatus für Load Balancer Standard*

### <a name="retrieve-multi-dimensional-metrics-programmatically-via-apis"></a>Programmgesteuertes Abrufen von mehrdimensionalen Metriken über APIs

Eine API-Anleitung zum Abrufen von Definitionen und Werten für multidimensionale Metriken finden Sie unter [Exemplarische Vorgehensweise für die Azure Monitor-REST-API](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough#retrieve-metric-definitions-multi-dimensional-api).

### <a name = "DiagnosticScenarios"></a>Allgemeine Diagnoseszenarien und empfohlene Ansichten

#### <a name="is-the-data-path-up-and-available-for-my-load-balancer-vip"></a>Ist der Datenpfad aktiv und für meine Load Balancer-VIP verfügbar?

Die VIP-Verfügbarkeitsmetrik beschreibt die Integrität des Datenpfads innerhalb des Bereichs zu dem Computehost, auf dem sich Ihre virtuellen Computer befinden. Die Metrik reflektiert die Integrität der Azure-Infrastruktur. Anhand der Metrik können Sie:
- Die externe Verfügbarkeit Ihres Diensts überwachen
- Tiefer einsteigen und ermitteln, ob die Plattform, auf der Ihr Dienst bereitgestellt wird, fehlerfrei ist, oder ob Ihr Gastbetriebssystem oder die Anwendungsinstanz fehlerfrei ist.
- Bestimmen, ob sich ein Ereignis auf Ihren Dienst oder die zugrunde liegende Datenebene bezieht. Verwechseln Sie diese Metrik nicht mit dem Integritätsteststatus („DIP-Verfügbarkeit“).

So rufen Sie die VIP-Verfügbarkeit für Ihre Standard Load Balancer-Ressourcen ab
1. Vergewissern Sie sich, dass die richtige Load Balancer-Ressource ausgewählt ist. 
2. Wählen Sie in der Dropdownliste **Metrik** die Option **VIP-Verfügbarkeit** aus. 
3. Wählen in der Dropdownliste **Aggregation** die Option **Mittelwert** aus. 
4. Fügen Sie außerdem einen Filter für die VIP-Adresse oder den VIP-Port als die Dimension mit der erforderlichen Front-End-IP-Adresse oder dem erforderlichen Front-End-Port hinzu, und gruppieren Sie diese dann nach der ausgewählten Dimension.

![VIP-Sondierung](./media/load-balancer-standard-diagnostics/LBMetrics-VIPProbing.png)

*Abbildung: VIP-Sondierungsdetails für Load Balancer*

Die Metrik wird durch eine aktive bandinterne Messung generiert. Ein Sondierungsdienst innerhalb der Region löst Datenverkehr für diese Messung aus. Der Dienst wird aktiviert, sobald Sie eine Bereitstellung mit einem öffentliche Front-End erstellen, und wird weiter ausgeführt, bis Sie das Front-End entfernen. 

>[!NOTE]
>Interne Front-Ends werden zu diesem Zeitpunkt nicht unterstützt. 

Ein Paket, das dem Front-End und der Regel Ihrer Bereitstellung entspricht, wird regelmäßig generiert. Es durchläuft die Region von der Quelle bis zu dem Host, auf dem sich ein virtueller Computer im Back-End-Pool befindet. Die Load Balancer-Infrastruktur führt dieselben Lastenausgleichs- und Übersetzungsvorgänge aus, wie sie dies für jeden anderen Datenverkehr tut. Diese Sondierung erfolgt bandintern auf Ihrem Endpunkt mit Lastenausgleich. Sobald der Test (Sondierung) auf dem Computehost eingeht, auf dem sich ein fehlerfreier virtueller Computer im Back-End-Pool befindet, generiert der Computehost eine Antwort an den Sondierungsdienst. Ihr virtueller Computer „sieht“ diesen Datenverkehr nicht.

VIP-Verfügbarkeit kann aus folgenden Gründen fehlschlagen:
- Ihre Bereitstellung hat keine fehlerfreien virtuellen Computer, die im Back-End-Pool verblieben sind. 
- Es ist ein Infrastrukturausfall aufgetreten.

Zu Diagnosezwecken können Sie die [VIP-Verfügbarkeitsmetrik zusammen mit dem Integritätsteststatus](#vipavailabilityandhealthprobes) verwenden.

Verwenden Sie **Mittelwert** als Aggregation für die meisten Szenarien.

#### <a name="are-the-back-end-instances-for-my-vip-responding-to-probes"></a>Antworten die Back-End-Instanzen für meine VIP auf Tests?

Die Integritätsteststatus-Metrik beschreibt die Integrität Ihrer Anwendungsbereitstellung, wie Sie diese konfiguriert haben, als Sie den Integritätstest Ihres Load Balancers konfiguriert haben. Der Load Balancer verwendet den Status des Integritätstests, um zu bestimmen, wohin neue Datenflüsse gesendet werden sollen. Integritätstests stammen aus einer Azure-Infrastrukturadresse und können im Gastbetriebssystem des virtuellen Computers angezeigt werden.

So rufen Sie die DIP-Verfügbarkeit für Ihre Standard Load Balancer-Ressourcen ab
1. Wählen Sie die Metrik **DIP-Verfügbarkeit** mit dem Aggregationstyp **Mittelwert** aus. 
2. Wenden Sie einen Filter auf die erforderliche VIP-IP-Adresse oder den erforderlichen VIP-Port (oder auf beides) an.

![DIP-Verfügbarkeit](./media/load-balancer-standard-diagnostics/LBMetrics-DIPAvailability.png)

*Abbildung: VIP-Verfügbarkeit des Load Balancers*

Integritätstests schlagen aus den folgenden Gründen fehl:
- Sie konfigurieren einen Integritätstest für einen Port, der nicht lauscht oder nicht reagiert oder das falsche Protokoll verwendet. Wenn für Ihren Dienst DSR-Regeln (Direct Server Return oder Floating IP-Regeln) verwendet werden, vergewissern Sie sich, dass der Dienst auf die IP-Adresse der IP-Konfiguration der Netzwerkkarte und nicht nur auf den Loopback lauscht, der mit der Front-End-IP-Adresse konfiguriert ist.
- Ihr Test wird von der Netzwerksicherheitsgruppe, der Firewall des Gastbetriebssystems des virtuellen Computers oder den Anwendungsschichtfilter nicht zugelassen.

Verwenden Sie **Mittelwert** als Aggregation für die meisten Szenarien.

#### <a name="how-do-i-check-my-outbound-connection-statistics"></a>Wie überprüfe ich meine Statistiken für ausgehende Verbindungen? 

Die SNAT-Verbindungen-Metrik beschreibt das Volumen erfolgreicher und fehlgeschlagener Verbindungen für [ausgehende Datenflüsse](https://aka.ms/lboutbound).

Ein Volumen für fehlgeschlagene Verbindungen, das größer als 0 (null) ist, kennzeichnet eine SNAT-Portüberlastung. Sie müssen weitere Untersuchungen vornehmen, um festzustellen, wodurch diese Fehler verursacht werden können. Eine SNAT-Portüberlastung zeigt sich dadurch, dass kein [ausgehender Datenfluss](https://aka.ms/lboutbound) eingerichtet werden kann. Lesen Sie den Artikel über ausgehende Verbindungen, um die Szenarien und greifenden Mechanismen zu verstehen und Informationen über die Maßnahmen für Abmildern und Entwurf zu erhalten, um so eine SNAT-Portüberlastung zu verhindern. 

So rufen Sie SNAT-Verbindungsstatistiken ab
1. Wählen Sie den Metriktyp **SNAT-Verbindungen** und **Summe** als Aggregation aus. 
2. Gruppieren Sie nach dem **Verbindungsstatus** für die Anzahl erfolgreicher und die Anzahl fehlgeschlagener SNAT-Verbindungen, die durch unterschiedliche Linien dargestellt werden. 

![SNAT-Verbindung](./media/load-balancer-standard-diagnostics/LBMetrics-SNATConnection.png)

*Abbildung: Anzahl von SNAT-Verbindungen des Load Balancers*


#### <a name="how-do-i-check-inboundoutbound-connection-attempts-for-my-service"></a>Wie überprüfe ich Versuche für eingehende/ausgehende Verbindungen für meinen Dienst?

Eine SYN-Pakete-Metrik beschreibt das Volumen der TCP-SYN-Pakete, die angekommen sind oder gesendet wurden (für [ausgehende Datenflüsse](https://aka.ms/lboutbound)) und einem bestimmten Front-End zugeordnet sind. Mit dieser Metrik können TCP-Verbindungsversuche für Ihren Dienst ausgewertet werden.

Verwenden Sie **Gesamt** als Aggregation für die meisten Szenarien.

![SYN-Verbindung](./media/load-balancer-standard-diagnostics/LBMetrics-SYNCount.png)

*Abbildung: SYN-Anzahl des Load Balancers*


#### <a name="how-do-i-check-my-network-bandwidth-consumption"></a>Wie überprüfe ich meine Netzwerkbandbreitennutzung? 

Die Byte- und Paketleistungsindikatoren-Metrik beschreibt das Volumen der Bytes und Pakete, die jeweils pro Front-End von Ihrem Dienst gesendet oder empfangen wurden.

Verwenden Sie **Gesamt** als Aggregation für die meisten Szenarien.

So rufen Sie eine Byteanzahl- oder Paketzahlstatistik ab
1. Wählen Sie die Metriktypen **Byteanzahl** und/oder **Paketzahl** mit **Mittelwert** als Aggregation aus. 
2. Führen Sie einen der folgenden Schritte aus:
   * Wenden Sie einen Filter auf eine bestimmte Front-End-IP, einen Front-End-Port, eine Back-End-IP oder einen Back-End-Port an.
   * Rufen Sie die Gesamtstatistik für Ihre Load Balancer-Ressource ohne jegliche Filterung ab.

![Byteanzahl](./media/load-balancer-standard-diagnostics/LBMetrics-ByteCount.png)

*Abbildung: Byteanzahl des Load Balancers*

#### <a name = "vipavailabilityandhealthprobes"></a>Wie führe ich eine Diagnose meiner Load Balancer-Bereitstellung durch?

Anhand einer Kombination der Metriken „VIP-Verfügbarkeit“ und „Integritätstests“ in einem einzigen Diagramm können Sie erkennen, wo das Problem zu suchen ist und wie es sich beheben lässt. Sie können sich davon überzeugen, dass Azure ordnungsgemäß funktioniert, und auf dieser Grundlage eindeutig feststellen, dass die Konfiguration oder die Anwendung die Hauptursache ist.

Sie können Integritätstestmetriken verwenden, um zu erkennen, wie Azure die Integrität Ihrer Bereitstellung entsprechend der Konfiguration beurteilt, die Sie bereitgestellt haben. Ein Anzeigen der Integritätstests ist immer ein hervorragender erster Schritt beim Überwachen oder Ermitteln einer Ursache.

Sie können einen Schritt weiter gehen und VIP-Verfügbarkeit-Metriken verwenden, um zu erfahren, wie Azure die Integrität der zugrunde liegenden Datenebene beurteilt, die für Ihre spezielle Bereitstellung zuständig ist. Wenn Sie beide Metriken kombinieren, können Sie, wie in diesem Beispiel veranschaulicht, gezielt feststellen, wo sich der Fehler befindet:

![VIP-Diagnose](./media/load-balancer-standard-diagnostics/LBMetrics-DIPnVIPAvailability.png)

*Abbildung: Kombinieren von DIP- und VIP-Verfügbarkeitsmetriken*

Im Diagramm werden die folgenden Informationen angezeigt:
- Die Infrastruktur selbst war fehlerfrei, die Infrastruktur, in der Ihr virtueller Computer gehostet wird, war erreichbar, und im Back-End waren mehrere virtuelle Computer platziert. Diese Informationen sind durch die blaue Ablaufverfolgung für VIP-Verfügbarkeit gekennzeichnet, die 100 Prozent beträgt. 
- Der Integritätsteststatus (DIP-Verfügbarkeit) liegt jedoch am Anfang des Diagramms bei 0 Prozent, wie es durch die orangefarbene Ablaufverfolgung gekennzeichnet ist. Der eingekreiste Bereich in Grün markiert, ab wann der Status (DIP-Verfügbarkeit) fehlerfrei wurde und an welchem Punkt die Bereitstellung des Kunden neue Datenflüsse akzeptieren konnte.

Anhand des Diagramms kann der Kunde eigenständig eine Fehlerbehebung für die Bereitstellung vornehmen, ohne zu erraten oder beim Support zu erfragen, ob andere Probleme auftreten. Der Dienst war nicht verfügbar, weil Integritätstests wegen einer Fehlkonfiguration oder einer fehlerhaften Anwendung fehlgeschlagen sind.

### <a name = "Limitations"></a>Einschränkungen

VIP-Verfügbarkeit ist derzeit nur für öffentliche Front-Ends verfügbar.

## <a name = "ResourceHealth"></a>Ressourcenintegritätsstatus

Der Integritätsstatus für die Standard Load Balancer-Ressourcen wird über die vorhandene **Ressourcenintegrität** unter **Monitor > Service Health** verfügbar gemacht.

>[!NOTE]
>Der Ressourcenintegritätsstatus für Load Balancer ist derzeit nur für die öffentliche Konfiguration von Standard Load Balancer verfügbar. Interne Load Balancer-Ressourcen oder Basic-SKUs von Load Balancer-Ressourcen machen keine Ressourcenintegrität verfügbar.

So zeigen Sie die Integrität Ihrer öffentlichen Standard Load Balancer-Ressourcen an
1. Wählen Sie **Monitor** > **Dienstintegrität** aus.

   ![Seite „Monitor“](./media/load-balancer-standard-diagnostics/LBHealth1.png)

   *Abbildung: Link „Dienstintegrität“ in Azure Monitor*

2. Wählen Sie **Dienstintegrität** aus, und stellen Sie dann sicher, dass **Abonnement-ID** und **Ressourcentyp = Load Balancer** ausgewählt sind.

   ![Ressourcenintegritätsstatus](./media/load-balancer-standard-diagnostics/LBHealth3.png)

   *Abbildung: Ressource für Integritätsanzeige auswählen*

3. Wählen Sie in der Liste die Load Balancer-Ressource aus, um deren Verlauf des Integritätsstatus anzuzeigen.

    ![Load Balancer-Integritätsstatus](./media/load-balancer-standard-diagnostics/LBHealth4.png)

   *Abbildung: Load Balancer-Integritätsanzeige für Ressource*
 
In der folgenden Tabelle sind die verschiedenen Ressourcenintegritätsstatus und deren Beschreibungen aufgeführt: 

| Ressourcenintegritätsstatus | BESCHREIBUNG |
| --- | --- |
| Verfügbar | Ihre öffentliche Standard Load Balancer-Ressource ist fehlerfrei und verfügbar. |
| Nicht verfügbar | Ihre öffentliche Standard Load Balancer-Ressource ist nicht fehlerfrei. Wählen Sie **Azure Monitor** > **Metriken** aus, um eine Diagnose der Integrität auszuführen.<br>(Der Status *Nicht verfügbar* kann auch bedeuten, dass die Ressource nicht mit Ihrem öffentliche Standard Load Balancer verbunden ist.) |
| Unknown | Der Ressourcenintegritätsstatus für Ihre öffentliche Standard Load Balancer-Ressource wurde noch nicht aktualisiert.<br>(Der Status *Unbekannt* kann auch bedeuten, dass die Ressource nicht mit Ihrem öffentliche Standard Load Balancer verbunden ist.)  |

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Load Balancer Standard](load-balancer-standard-overview.md).
- Weitere Informationen zu Ihren [ausgehenden Verbindungen für Load Balancer](https://aka.ms/lboutbound)
- Weitere Informationen finden Sie unter [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview).
- Weitere Informationen finden Sie unter [Azure Monitor REST-API](https://docs.microsoft.com/rest/api/monitor/) und [Abrufen von Metriken über die REST-API](/rest/api/monitor/metrics/list).


