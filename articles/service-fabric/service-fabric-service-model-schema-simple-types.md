---
title: Einfache Typen im XML-Schema des Azure Service Fabric-Dienstmodells | Microsoft-Dokumentation
description: Hier werden einfache Typen im XML-Schema des Service Fabric-Dienstmodells beschrieben.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: xml
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/10/2018
ms.author: ryanwi
ms.openlocfilehash: d50cdf72cd4de69f65cdc25e3228f6142e75a46f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "57877104"
---
<!-- This article was generated by the Python script found in the service-fabric-service-model-schema.md file -->

# <a name="service-model-xml-schema-simple-types"></a>Einfache Typen im XML-Schema des Dienstmodells

## <a name="fabricuri-simpletype"></a>simpleType: FabricUri
Ein URI, der als stabiler Bezeichner von Diensten vom Microsoft Azure Service Fabric-Namensystem verwendet wird. 

### <a name="xml-source"></a>XML-Quelle
```xml
<xs:simpleType xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="FabricUri">
    <xs:annotation>
      <xs:documentation>A URI that is used as a stable identifier of services by Microsoft Azure Service Fabric Naming system. </xs:documentation>
    </xs:annotation>
    <xs:restriction base="xs:anyURI"/>
  </xs:simpleType>
  
```
