---
title: Entwickeln von Apps mit dem Speech SDK – Speech Services
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie Apps mit dem Speech SDK erstellen.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: f3522f065d22ce276174fbd165c37df3914e32b9
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2019
ms.locfileid: "56962025"
---
# <a name="ship-an-application"></a>Ausliefern einer Anwendung

Beachten Sie beim Verteilen des Azure Cognitive Services Speech SDK die [Lizenzbedingungen für das Speech SDK](https://aka.ms/csspeech/license201809) sowie die [Hinweise und Informationen zu Drittanbietersoftware](https://csspeechstorage.blob.core.windows.net/drop/1.0.0/ThirdPartyNotices.html). Lesen Sie außerdem die [Datenschutzerklärung von Microsoft](https://aka.ms/csspeech/privacy).

Je nach Plattform bestehen unterschiedliche Abhängigkeiten zum Ausführen Ihrer Anwendung.

## <a name="windows"></a>Windows

Das Cognitive Services Speech SDK wird unter Windows 10 und unter Windows Server 2016 getestet.

Für das Cognitive Services Speech SDK muss [Microsoft Visual C++ Redistributable für Visual Studio 2017](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) auf dem System installiert sein. Installationsprogramme für die neueste Version von `Microsoft Visual C++ Redistributable for Visual Studio 2017` können Sie hier herunterladen:

- [Win32](https://aka.ms/vs/15/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/15/release/vc_redist.x64.exe)

Wenn Ihre Anwendung verwalteten Code verwendet, muss `.NET Framework 4.6.1` oder höher auf dem Zielcomputer installiert sein.

Für die Mikrofoneingabe müssen die Media Foundation-Bibliotheken installiert sein. Diese Bibliotheken sind Bestandteil von Windows 10 und Windows Server 2016. Das Speech SDK kann ohne diese Bibliotheken verwendet werden, wenn als Audioeingabegerät kein Mikrofon verwendet wird.

Die erforderlichen Speech SDK-Dateien können im gleichen Verzeichnis wie die Anwendung bereitgestellt werden. Auf diese Weise kann die Anwendung direkt auf die Bibliotheken zugreifen. Stellen Sie sicher, dass Sie die richtige, der Anwendung entsprechende Version (Win32/x64) auswählen.

| NAME | Funktion
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | Core SDK, erforderlich für die native und verwaltete Bereitstellung
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Erforderlich für die verwaltete Bereitstellung

>[!NOTE]
> Ab Release 1.3.0 ist die Datei `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (im Lieferumfang von früheren Releases enthalten) nicht mehr erforderlich. Die Funktion ist jetzt in das Core-SDK integriert.

## <a name="linux"></a>Linux

Das Speech SDK unterstützt derzeit die Distributionen Ubuntu 16.04 und 18.04.
Für eine native Anwendung müssen Sie die Speech SDK-Bibliothek `libMicrosoft.CognitiveServices.Speech.core.so` ausliefern.
Stellen Sie sicher, dass Sie die der Anwendung entsprechende Version (x86, x64) auswählen. Je nach verwendeter Linux-Version müssen Sie möglicherweise auch die folgenden Abhängigkeiten einbinden:

* Freigegebene Bibliotheken der GNU C-Bibliothek (einschließlich der POSIX Threads Programming-Bibliothek `libpthreads`)
* OpenSSL-Bibliothek (`libssl.so.1.0.0`)
* Freigegebene Bibliothek für ALSA-Anwendungen (`libasound.so.2`)

Unter Ubuntu sollten die GNU C-Bibliotheken bereits standardmäßig installiert sein. Die letzten drei Bibliotheken können mit folgenden Befehlen installiert werden:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libasound2 wget
```

## <a name="next-steps"></a>Nächste Schritte

* [Abrufen Ihres Testabonnements für Speech](https://azure.microsoft.com/try/cognitive-services/)
* [Erkennen von Sprache in C#](quickstart-csharp-dotnet-windows.md)
