---
title: Verwalten von Berechtigungen für Ressourcen pro Benutzer in Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie als Dienstadministrator oder Mandant RBAC-Berechtigungen verwalten.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.lastreviewed: 03/11/2019
ms.openlocfilehash: 3565bf4c4a19bcf1b136b4cbb781006658865a1c
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57767208"
---
# <a name="manage-access-to-resources-with-azure-stack-role-based-access-control"></a>Verwalten des Zugriffs auf Ressourcen mit der rollenbasierten Zugriffssteuerung in Azure Stack

*Anwendungsbereich: Integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Azure Stack unterstützt die rollenbasierte Zugriffssteuerung (RBAC), das gleiche von Microsoft Azure verwendete [Sicherheitsmodell für die Zugriffsverwaltung](https://docs.microsoft.com/azure/role-based-access-control/overview). Anhand der RBAC können Sie den Benutzer-, Gruppen- oder Anwendungszugriff auf Abonnements, Ressourcen und Dienste verwalten.

## <a name="basics-of-access-management"></a>Grundlagen zur Zugriffsverwaltung

Die rollenbasierte Zugriffssteuerung bietet eine präzise Zugriffssteuerung, mit der Sie Ihre Umgebung schützen können. Dabei erteilen Sie Benutzern genau die benötigten Berechtigungen, indem Sie eine RBAC-Rolle in einem bestimmten Bereich zuweisen. Der Bereich der Rollenzuweisung kann ein Abonnement, eine Ressourcengruppe oder eine einzelne Ressource sein. Ausführlichere Informationen zur Zugriffsverwaltung finden Sie im Artikel [Rollenbasierte Zugriffssteuerung im Azure-Portal](https://docs.microsoft.com/azure/role-based-access-control/overview).

### <a name="built-in-roles"></a>Integrierte Rollen

Azure Stack verfügt über drei grundlegende Rollen, die für alle Ressourcentypen gelten:

* **Besitzer** können alles verwalten, einschließlich des Zugriffs auf Ressourcen.
* **Mitwirkender** können alles mit Ausnahme des Zugriffs auf Ressourcen verwalten.
* **Leser** können alles anzeigen, jedoch keine Änderungen vornehmen.

### <a name="resource-hierarchy-and-inheritance"></a>Ressourcenhierarchie und Vererbung

Azure Stack verfügt über die folgende Ressourcenhierarchie:

* Jedes Abonnement gehört einem Verzeichnis.
* Jede Ressourcengruppe gehört einem Abonnement.
* Jede Ressource gehört einer Ressourcengruppe.

Der Zugriff, den Sie auf übergeordneter Ebene gewähren, wird auf untergeordneter Ebene geerbt. Beispiel: 

* Sie weisen die Rolle „Leser“ einer Azure AD-Gruppe im Abonnementbereich zu. Die Mitglieder dieser Gruppe können alle Ressourcengruppen und Ressourcen im Abonnement anzeigen.
* Sie weisen die Rolle „Mitwirkender“ einer Anwendung im Ressourcengruppenbereich zu. Damit kann die Anwendung Ressourcen aller Typen in dieser Ressourcengruppe verwalten, aber keine anderen Ressourcengruppen des Abonnements.

### <a name="assigning-roles"></a>Zuweisen von Rollen

Sie können einem Benutzer mehrere Rollen zuweisen, und jede Rolle kann einem anderen Bereich zugeordnet werden. Beispiel: 

* Sie weisen TestUser-A die Leserolle für Subscription-1 zu.
* Sie weisen TestUser-A die Besitzerrolle für TestVM-1 zu.

Der Artikel zu [Rollenzuweisungen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) in Azure enthält ausführliche Informationen zum Anzeigen, Zuweisen und Löschen von Rollen.

### <a name="resource-hierarchy-and-inheritance"></a>Ressourcenhierarchie und Vererbung

Azure Stack verfügt über die folgende Ressourcenhierarchie:

* Jedes Abonnement gehört einem Verzeichnis.
* Jede Ressourcengruppe gehört einem Abonnement.
* Jede Ressource gehört einer Ressourcengruppe.

Der Zugriff, den Sie auf übergeordneter Ebene gewähren, wird auf untergeordneter Ebene geerbt. Beispiel: 

* Sie weisen die Rolle **Leser** einer Azure AD-Gruppe im Abonnementbereich zu. Die Mitglieder dieser Gruppe können alle Ressourcengruppen und Ressourcen im Abonnement anzeigen.
* Sie weisen die Rolle **Mitwirkender** einer Anwendung im Ressourcengruppenbereich zu. Damit kann die Anwendung Ressourcen aller Typen in dieser Ressourcengruppe verwalten, aber keine anderen Ressourcengruppen des Abonnements.

### <a name="assigning-roles"></a>Zuweisen von Rollen

Sie können einem Benutzer mehrere Rollen zuweisen, und jede Rolle kann einem anderen Bereich zugeordnet werden. Beispiel: 

* Sie weisen TestUser-A die Leserolle für Subscription-1 zu.
* Sie weisen TestUser-A die Besitzerrolle für TestVM-1 zu.

Der Artikel zu [Rollenzuweisungen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) in Azure enthält ausführliche Informationen zum Anzeigen, Zuweisen und Löschen von Rollen.

## <a name="set-access-permissions-for-a-user"></a>Festlegen von Zugriffsberechtigungen für Benutzer

In den folgenden Schritte wird beschrieben, wie Berechtigungen für einen Benutzer konfiguriert werden.

1. Melden Sie sich mit einem Konto an, das Besitzerberechtigungen für die Ressource hat, die Sie verwalten möchten.
2. Wählen Sie im linken Navigationsbereich die Option **Ressourcengruppen** aus.
3. Wählen Sie den Namen der Ressourcengruppe, der Sie Berechtigungen erteilen möchten.
4. Wählen Sie im Navigationsbereich der Ressourcengruppe **Zugriffssteuerung (IAM)**. Die Ansicht **Rollenzuweisungen** enthält die Elemente, die über Zugriff auf die Ressourcengruppe verfügen. Sie können die Ergebnisse filtern und gruppieren.
5. Wählen Sie auf der Menüleiste **Zugriffssteuerung** die Option **Hinzufügen** aus.
6. Gehen Sie im Bereich **Berechtigungen hinzufügen** wie folgt vor:

   * Wählen Sie aus der Dropdownliste **Rolle** die Rolle aus, die Sie zugewiesen werden soll.
   * Wählen Sie aus der Dropdownliste **Zugriff zuweisen an** die Ressource aus, die Sie zugewiesen werden soll.
   * Wählen Sie den Benutzer, die Gruppe oder die Anwendung als das Element in Ihrem Verzeichnis aus, für das Sie Zugriff gewähren möchten. Sie können das Verzeichnis mit Anzeigenamen, E-Mail-Adressen und Objektbezeichnern durchsuchen.

7. Wählen Sie **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen von Dienstprinzipalen](azure-stack-create-service-principals.md)