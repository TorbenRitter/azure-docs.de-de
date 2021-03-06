---
title: "Azure IoT Hub – C2D-Optionen (Cloud-zu-Gerät) | Microsoft Docs"
description: "Entwicklerhandbuch – Leitfaden, der angibt, wann direkte Methoden, gewünschte Eigenschaften von Gerätezwillingen oder C2D-Nachrichten für C2D-Kommunikationen verwendet werden sollen."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: 1ac90923-1edf-4134-bbd4-77fee9b68d24
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2016
ms.author: elioda
translationtype: Human Translation
ms.sourcegitcommit: e223d0613cd48994315451da87e6b7066585bdb6
ms.openlocfilehash: 17b338ab15ae7cf46f6732e7e7a1005eec98990b


---
# <a name="cloud-to-device-communications-guidance"></a>Leitfaden zur C2D-Kommunikation
IoT Hub bietet drei Optionen für Geräte-Apps, um Funktionen einer Back-End-App verfügbar zu machen:

* [Direkte Methoden][lnk-methods] für Kommunikation, die sofortige Bestätigung ihres Ergebnisses erfordert, in der Regel über interaktive Steuerung des Geräts, z.B. Einschalten eines Lüfters;
* [Gewünschte Eigenschaften von Gerätezwillingen][lnk-twins] für Befehle mit langer Ausführungszeit, die das Gerät in einen bestimmten gewünschten Zustand versetzen sollen. Legen Sie das Telemetriesendeintervall z.B. auf 30 Minuten fest;
* [Cloud-zu-Gerät-Nachrichten (Cloud-to-Device, C2D)][lnk-c2d] zum Senden unidirektionaler Benachrichtigungen an die Geräte-App.

Hier finden Sie einen detaillierten Vergleich verschiedener Optionen für die C2D-Kommunikation.

|  | Direkte Methoden | Gewünschte Eigenschaften von Gerätezwillingen | C2D-Nachrichten |
| ---- | ------- | ---------- | ---- |
| Szenario | Befehle, die sofortige Bestätigung erfordern, z.B. Einschalten eines Lüfters. | Befehle mit langer Ausführungszeit, die das Gerät in einen bestimmten gewünschten Zustand versetzen sollen. Legen Sie das Telemetriesendeintervall z.B. auf 30 Minuten fest. | Senden unidirektionaler Benachrichtigungen an die Geräte-App. |
| Datenfluss | Bidirektional. Die Geräte-App kann sofort auf die Methode reagieren. Das Lösungs-Back-End empfängt ein auf die Anforderung kontextbezogenes Ergebnis. | Unidirektional. Die Geräte-App empfängt eine Benachrichtigung mit der Eigenschaftenänderung. | Unidirektional. Die Geräte-App empfängt die Nachricht.
| Dauerhaftigkeit | Mit getrennten Geräten wird kein Kontakt hergestellt. Dem Back-End wird gemeldet, dass das Gerät nicht verbunden ist. | Eigenschaftswerte werden im Gerätezwilling beibehalten. Das Gerät wird diese bei der nächsten erneuten Verbindung lesen. Eigenschaftswerte sind mit der [IoT Hub-Abfragesprache][lnk-query] erneut abrufbar. | Nachrichten können durch IoT Hub bis zu 48 Stunden aufbewahrt werden. |
| Ziele | Einzelgerät mit **deviceId** oder mehrere Geräte mit [Aufträgen][lnk-jobs]. | Einzelgerät mit **deviceId** oder mehrere Geräte mit [Aufträgen][lnk-jobs]. | Einzelgerät nach **deviceId**. |
| Größe | Anforderungen und Antworten bis zu jeweils 8KB. | Die Maximalgröße gewünschter Eigenschaften beträgt 8KB. | Nachrichten bis zu 256KB. |
| Frequency | Hoch. Weitere Informationen finden Sie unter [IoT Hub-Grenzen][lnk-quotas]. | Mittel. Weitere Informationen finden Sie unter [IoT Hub-Grenzen][lnk-quotas]. | Niedrig. Weitere Informationen finden Sie unter [IoT Hub-Grenzen][lnk-quotas]. |
| Protocol | Mit MQTT und AMQP verfügbar. | Derzeit nur mit MQTT verfügbar. | Mit allen Protokollen verfügbar. Gerät muss bei Verwendung von HTTP einen Abruf tätigen. |

Erfahren Sie in den folgenden Tutorials, wie Sie direkte Methoden, gewünschte Eigenschaften und C2D-Nachrichten einsetzen:

* [Use direct methods][lnk-methods-tutorial] (Verwenden von direkten Methoden);
* [Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten][lnk-twin-properties] zu den gewünschten Eigenschaften von Gerätezwillingen; 
* [Tutorial: Senden von C2D-Nachrichten mithilfe von IoT Hub und „Node.js“][lnk-c2d-tutorial] für C2D-Nachrichten.

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-c2d]: iot-hub-devguide-messaging.md#cloud-to-device-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-tutorial]: iot-hub-node-node-c2d.md


<!--HONumber=Dec16_HO1-->


