---
title: Übersicht zum Verwalten von Updates in Azure Stack | Microsoft-Dokumentation
description: Informationen zur Updateverwaltung für integrierte Azure Stack-Systeme.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2019
ms.author: mabrigg
ms.lastreviewed: 01/22/2019
ms.openlocfilehash: 6cddb513249b424ef0df2ed2d1b5609de17c0744
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57894035"
---
# <a name="manage-updates-in-azure-stack-overview"></a>Übersicht zum Verwalten von Updates in Azure Stack

*Anwendungsbereich: Integrierte Azure Stack-Systeme*

Microsoft-Updatepakete für integrierte Azure Stack-Systeme werden in der Regel um den vierten Dienstag jedes Monats herausgegeben. Fragen Sie Ihren OEM nach seinem spezifischen Benachrichtigungsprozess, um sicherzustellen, dass Updatebenachrichtigungen Ihre Organisation erreichen. Sie können auch in dieser Dokumentationsbibliothek unter **Übersicht** > **Anmerkungen zu dieser Version** überprüfen, ob Informationen zu Releases vorliegen, die aktiv unterstützt werden. 

Jedes Release von Microsoft-Softwareupdates ist in einem einzelnen Updatepaket gebündelt. Als Azure Stack-Operator können Sie über das Administratorportal diese Updatepakete importieren, installieren und ihren Installationsstatus überwachen. 

Ihr Originalgerätehersteller-Hardwareanbieter (Original Equipment Manufacturer, OEM) veröffentlicht auch Updates, z.B. Treiber- und Firmwareupdates. Diese Updates werden zwar als separate Pakete von Ihrem OEM-Hardwarehersteller bereitgestellt, doch sie werden auf die gleiche Weise importiert, installiert und verwaltet wie Updatepakete von Microsoft.

Um den Support für Ihr System aufrechtzuerhalten, müssen Sie sicherstellen, dass Azure Stack stets bis zu einer bestimmten Version aktualisiert ist. Machen Sie sich unbedingt mit der [Azure Stack servicing policy](azure-stack-servicing-policy.md) (Azure Stack-Wartungsrichtlinie) vertraut.

> [!NOTE]
> Sie können Azure Stack-Updatepakete nicht auf Azure Stack Development Kit anwenden. Die Updatepakete sind für integrierte Systeme vorgesehen. Weitere Informationen finden Sie unter [Erneutes Bereitstellen des ASDK](https://docs.microsoft.com/azure/azure-stack/asdk).

## <a name="the-update-resource-provider"></a>Der Updateressourcenanbieter

Azure Stack umfasst einen Updateressourcenanbieter, der die Anwendung von Microsoft-Softwareupdates orchestriert. Dieser Ressourcenanbieter stellt sicher, dass Updates alle physischen Hosts, Service Fabric-Anwendungen und Laufzeiten sowie alle virtuellen Computer der Infrastruktur und die ihnen zugeordneten Dienste übergreifend angewandt werden.

Bei der Installation der Updates können Sie den allgemeinen Status anzeigen, während der Updateprozess die verschiedenen Subsysteme in Azure Stack (z.B. physische Hosts und virtuelle Computer der Infrastruktur) erfasst.

## <a name="plan-for-updates"></a>Planen für Updates

Es wird dringend empfohlen, dass Sie die Benutzer über alle Wartungsvorgänge unterrichten und normale Wartungsfenster möglichst außerhalb der Geschäftszeiten planen. Wartungsvorgänge können sowohl Mandantenworkloads als auch Portalvorgänge beeinträchtigen.

- Bevor Sie mit der Installation dieses Updates beginnen, führen Sie [Test-AzureStack](azure-stack-diagnostic-test.md) mit den folgenden Parametern aus, um den Status von Azure Stack zu überprüfen und alle gefundenen operativen Probleme (einschließlich aller Warnungen und Fehler) zu beheben. Überprüfen Sie auch aktive Warnungen, und lösen Sie solche auf, die eine Aktion erfordern.  

  ```PowerShell
  Test-AzureStack -Group UpdateReadiness
  ``` 

## <a name="using-the-update-tile-to-manage-updates"></a>Verwenden der Update-Kachel zum Verwalten von Updates
Sie können Updates über das Administratorportal verwalten. Als Azure Stack-Operator können Sie zu folgenden Zwecken die Update-Kachel im Dashboard verwenden:

- Anzeigen wichtiger Informationen wie z.B. der aktuellen Version.
- Installieren von Updates und Überwachen des Status.
- Überprüfen des Updateverlaufs der zuvor installierten Updates.
 
## <a name="determine-the-current-version"></a>Bestimmen der aktuellen Version

Die Update-Kachel zeigt die aktuelle Version von Azure Stack. Sie gelangen mithilfe einer der folgenden Methoden zum Administratorportal:

- Zeigen Sie auf dem Dashboard die aktuelle Version auf der **Update**-Kachel an.
 
   ![Kachel „Updates“ auf dem Standarddashboard](./media/azure-stack-updates/image1.png)
 
- Klicken Sie auf der Kachel **Regionsverwaltung** auf den Namen der Region. Zeigen Sie die aktuelle Version auf der **Update**-Kachel an.

## <a name="next-steps"></a>Nächste Schritte

- [Azure Stack-Wartungsrichtlinie](azure-stack-servicing-policy.md) 
- [Region management in Azure Stack](azure-stack-region-management.md) (Regionsverwaltung in Azure Stack)     


