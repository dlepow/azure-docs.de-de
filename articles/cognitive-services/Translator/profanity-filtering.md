---
title: Filtern von Obszönitäten – Textübersetzungs-API
titlesuffix: Azure Cognitive Services
description: Verwenden Sie die Filterung von Obszönitäten in der Textübersetzungs-API.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: v-jansko
ms.openlocfilehash: ab54f34dbdbda5af94c6ebc2bc8c1818ebf3d601
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729881"
---
# <a name="add-profanity-filtering-with-the-translator-text-api"></a>Hinzufügen der Filterung von Obszönitäten mit der Textübersetzungs-API

In der Regel behält der Übersetzungsdienst Obszönitäten, die im Quelltext vorhanden sind, in der Übersetzung bei. Der Grad an Obszönitäten und der Kontext, bei dem Wörter als obszön angesehen werden, unterscheiden sich je nach Kultur. Daher kann der Obszönitätsgrad in der Zielsprache unter Umständen verstärkt oder reduziert werden.

Wenn Sie keine Obszönitäten in der Übersetzung wünschen (unabhängig davon, ob diese im Quelltext vorhanden sind), können Sie die Obszönitäten-Filteroption in der Translate()-Methode verwenden. Mit dieser Option können Sie auswählen, ob Obszönitäten gelöscht oder entsprechend gekennzeichnet werden sollen oder ob keine Aktion durchgeführt werden soll.

Für die Translate()-Methode wird der Parameter „options“ verwendet, der das neue Element „ProfanityAction“ enthält. Die akzeptierten Werte für ProfanityAction sind „NoAction“, „Marked“ und „Deleted“.

## <a name="accepted-values-of-profanityaction-and-examples"></a>Akzeptierte Werte für ProfanityAction und Beispiele
|ProfanityAction-Wert | Aktion | Beispiel: Ausgangssprache – Japanisch | Beispiel: Zielsprache – Englisch|
| :---|:---|:---|:---|
| NoAction | Standard. Entspricht dem Fall, in dem die Option nicht festgelegt wird. Die Obszönität wird aus der Ausgangs- in die Zielsprache übernommen. | 彼は変態です。 | He is a jerk. (Er ist ein Vollidiot.) |
| Marked | Obszöne Wörter werden in die XML-Tags \<profanity> ... \</profanity> eingeschlossen. | 彼は変態です。 | He is a \<profanity>jerk\</profanity>. |
| Deleted | Obszöne Begriffe werden aus der Ausgabe entfernt, und es wird kein Ersatzbegriff bereitgestellt. | 彼は。 | He is a. (Er ist ein.) |

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Anwenden der Filterung von Obszönitäten in Ihrem Aufruf der Translator-API](reference/v3-0-translate.md)
