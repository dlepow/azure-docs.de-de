---
title: Was sind FPGAs, und was ist Project Brainwave?
titleSuffix: Azure Machine Learning service
description: Hier erfahren Sie, wie Sie Modelle und Deep Neural Networks mit FPGAs in Azure beschleunigen. Dieser Artikel enthält eine Einführung in FPGAs (Field-Programmable Gate Arrays) und erläutert, wie Azure Machine Learning Service bei der Bereitstellung Ihres Modells in einem Azure-FPGA künstliche Intelligenz in Echtzeit bietet.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: tedway
author: tedway
ms.reviewer: jmartens
ms.date: 1/10/2019
ms.custom: seodec18
ms.openlocfilehash: 716de6d491be753c5c32c0a2774404140010f72c
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2019
ms.locfileid: "56823313"
---
# <a name="what-are-fpgas-and-project-brainwave"></a>Was sind FPGAs, und was ist Project Brainwave?

Dieser Artikel enthält eine Einführung in FPGAs (Field-Programmable Gate Arrays) und erläutert, wie Azure Machine Learning Service bei der Bereitstellung Ihres Modells in einem Azure-FPGA künstliche Intelligenz in Echtzeit bietet.

FPGAs enthalten ein Array von programmierbaren Logikblöcken sowie eine Hierarchie von neu konfigurierbaren Interconnects. Durch die Interconnects können diese Blöcke nach der Fertigung auf verschiedene Weise konfiguriert werden. Im Vergleich zu anderen Chips bieten FPGAs eine Kombination aus Programmierbarkeit und Leistung.

## <a name="fpgas-vs-cpu-gpu-and-asic"></a>FPGAs im Vergleich zu CPU, GPU und ASIC

Der folgende Abbildung und die Tabelle zeigen FPGAs im Vergleich zu anderen Prozessoren.

![Abbildung zu Azure Machine Learning Service: FPGA-Vergleich](./media/concept-accelerate-with-fpgas/azure-machine-learning-fpga-comparison.png)

|Prozessor||BESCHREIBUNG|
|---|:-------:|------|
|Anwendungsspezifische integrierte Schaltkreise|ASICs|Benutzerdefinierte Schaltkreise, z.B. TPUs (TensorFlow-Prozessoren) von Google, bieten die höchste Effizienz. Sie können nicht neu konfiguriert werden, wenn sich Ihre Anforderungen ändern.|
|Field-Programmable Gate Arrays|FPGAs|FPGAs, wie sie in Azure verfügbar sind, stellen eine fast so gute Leistung wie ASICs bereit. Sie sind auch flexibel und können im Lauf der Zeit erneut konfiguriert werden, um neue Programmlogik zu implementieren.|
|Grafikprozessoren|GPUs|Eine beliebte Wahl für KI-Berechnungen. GPUs bieten Funktionen zur Parallelverarbeitung und sind beim Grafikrendering schneller als CPUs.|
|Zentralprozessoren|CPUs|CPUs sind Allzweckprozessoren, deren Leistung für die Grafik- und Videoverarbeitung nicht optimal ist.|

## <a name="project-brainwave-on-azure"></a>Project Brainwave für Azure

[Project Brainwave](https://www.microsoft.com/en-us/research/project/project-brainwave/) ist eine Hardwarearchitektur von Microsoft. Sie basiert auf FPGA-Geräten von Intel, die Data Scientists und Entwickler zu verwenden, um KI-Echtzeitberechnungen zu beschleunigen. Diese FPGA-fähige Architektur bietet Leistung, Flexibilität und Skalierbarkeit und ist in Azure verfügbar.

FPGAs ermöglichen eine geringe Latenzzeit für Echtzeit-Inferenzanforderungen. Asynchrone Anforderungen (Batchverarbeitung) sind nicht erforderlich. Die Batchverarbeitung kann zu Wartezeiten führen, weil mehr Daten verarbeitet werden müssen. Bei Project Brainwave-Implementierungen von neuronalen Prozessoren ist keine Batchverarbeitung erforderlich. Daher kann die Wartezeit im Vergleich zu CPU- und GPU-Prozessoren um ein Vielfaches geringer ausfallen.

### <a name="reconfigurable-power"></a>Neu konfigurierbare Leistung
Sie können FPGAs für verschiedene Arten von Machine Learning-Modellen neu konfigurieren. Durch diese Flexibilität ist es einfacher, die Anwendungen auf der Grundlage der optimalen numerischen Genauigkeit und des verwendeten Speichermodells zu beschleunigen. Da FPGAs neu konfigurierbar sind, können Sie mit den Anforderungen der sich schnell ändernden KI-Algorithmen Schritt halten.

### <a name="whats-supported-on-azure"></a>In Azure unterstützte Funktionen
Microsoft Azure ist die weltweit größte Cloudinvestition in FPGAs. Sie können Project Brainwave unter der Skalierungsinfrastruktur von Azure ausführen.

In Project Brainwave wird zurzeit Folgendes unterstützt:
+ Szenarien zur Bildklassifizierung und -erkennung
+ TensorFlow-Bereitstellung
+ DNNs: ResNet 50, ResNet 152, VGG-16, SSD-VGG und DenseNet-121
+ Intel FPGA-Hardware 

Mit dieser FPGA-fähigen Hardwarearchitektur werden trainierte neuronale Netze schnell und mit geringerer Wartezeit ausgeführt. Project Brainwave kann vortrainierte DNNs (Deep Neural Networks) FPGA-übergreifend parallelisieren, um Ihren Dienst horizontal hochzuskalieren. Die DNNs können als Deep Featurizer zum Transferlernen vortrainiert oder durch aktualisierte Gewichtungen optimiert werden.

### <a name="scenarios-and-applications"></a>Szenarien und Anwendungen

Project Brainwave ist in Azure Machine Learning integriert. Microsoft verwendet FPGAs für die DNN-Auswertung, die Rangfolge bei der Bing-Suche und die Beschleunigung von Software Defined Networking (SDN), um die Latenz zu verringern und gleichzeitig CPUs für andere Aufgaben freizugeben.

Die folgenden Szenarien verwenden FPGA unter der Project Brainwave-Architektur:
+ [Automatisiertes optisches Untersuchungssystem](https://blogs.microsoft.com/ai/build-2018-project-brainwave/)

+ [Landnutzungskartierung](https://blogs.technet.microsoft.com/machinelearning/2018/05/29/how-to-use-fpgas-for-deep-learning-inference-to-perform-land-cover-mapping-on-terabytes-of-aerial-images/)

## <a name="deploy-to-fpgas-on-azure"></a>Bereitstellen für FPGAs in Azure

Um einen Bilderkennungsdienst in Azure zu erstellen, können Sie unterstützte DNNs als Featurizer für die Bereitstellung in Azure-FPGAs verwenden:

1. Verwenden Sie das [Azure Machine Learning SDK für Python](https://aka.ms/aml-sdk), um eine Dienstdefinition zu erstellen. Eine Dienstdefinition ist eine Datei, die eine Pipeline mit auf TensorFlow basierenden Diagrammen (Eingabe, Featurizer und Klassifizierung) beschreibt. Durch den Bereitstellungsbefehl werden die Definition und die Diagramme automatisch in einer ZIP-Datei komprimiert, die dann in Azure Blob Storage hochgeladen wird. Das DNN wurde in Project Brainwave bereits zur Ausführung im FPGA bereitgestellt.

1. Registrieren Sie das Modell über das SDK anhand der ZIP-Datei in Azure Blob Storage.

1. Stellen Sie den Dienst mit dem registrierten Modell mithilfe des SDK bereit.

Um mit dem Bereitstellen trainierter DNN-Modelle in FPGAs in der Azure-Cloud zu beginnen, lesen Sie [Bereitstellen eines Modells als Webdienst in einem FPGA](how-to-deploy-fpga-web-service.md).


## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich diese Videos und Blogs an:

+ [Hyperscalehardware: ML at scale on top of Azure + FPGA (ML mit Skalierung über Azure und FPGA): Build 2018 (Video)](https://www.youtube.com/watch?v=BMgQAHIx2eY)

+ [Einblicke in die FPGA-basierte konfigurierbare Cloud von Microsoft (Video)](https://channel9.msdn.com/Events/Build/2017/B8063)

+ [Project Brainwave für KI in Echtzeit: Projektstartseite](https://www.microsoft.com/research/project/project-brainwave/)
