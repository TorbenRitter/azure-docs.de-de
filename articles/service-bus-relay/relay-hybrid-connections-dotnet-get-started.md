---
title: Erste Schritte mit Azure Relay-Hybridverbindungen in .NET | Microsoft-Dokumentation
description: "Gewusst wie: Schreiben einer C#-Konsolenanwendung für Hybridverbindungen"
services: service-bus-relay
documentationcenter: .net
author: jtaubensee
manager: timlt
editor: 
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 02/03/2017
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: 1ee1e7d0c6f239abfda474d51c8d02d5338dabc7
ms.openlocfilehash: ec8d7cf64786a3347998f243fc7e4f9550fd9f08


---
# <a name="get-started-with-relay-hybrid-connections"></a>Erste Schritte mit Relay-Hybridverbindungen
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Dieses Tutorial bietet eine Einführung in [Azure Relay-Hybridverbindungen](relay-what-is-it.md#hybrid-connections) und zeigt die Erstellung einer Clientanwendung, die Nachrichten an eine entsprechende Listener-Anwendung sendet. 

## <a name="what-will-be-accomplished"></a>Ziele
Da für Hybridverbindungen sowohl eine Client- als auch eine Serverkomponente erforderlich ist, werden im Rahmen des Tutorials zwei Konsolenanwendungen erstellt. Vorgehensweise:

1. Erstellen eines Relay-Namespace mit dem Azure-Portal
2. Erstellen einer Hybridverbindung mit dem Azure-Portal
3. Erstellen einer Serverkonsolenanwendung (Listener) zum Empfangen von Nachrichten
4. Erstellen einer Clientkonsolenanwendung (Absender) zum Senden von Nachrichten

## <a name="prerequisites"></a>Voraussetzungen
1. [Visual Studio 2013 oder Visual Studio 2015](http://www.visualstudio.com). Für die Beispiele in diesem Tutorial wird Visual Studio 2015 verwendet.
2. Ein Azure-Abonnement.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Erstellen eines Namespace mithilfe des Azure-Portals
Falls Sie bereits einen Relay-Namespace erstellt haben, fahren Sie mit dem Abschnitt [Erstellen einer Hybridverbindung mit dem Azure-Portal](#2-create-a-hybrid-connection-using-the-azure-portal) fort.

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Erstellen einer Hybridverbindung mit dem Azure-Portal
Falls Sie bereits eine Hybridverbindung erstellt haben, fahren Sie mit dem Abschnitt [Erstellen einer Serveranwendung](#3-create-a-server-application-listener) fort.

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Erstellen einer Serveranwendung (Listener)
Zum Lauschen und Empfangen von Nachrichten vom Relay schreiben wir mit Visual Studio eine C#-Konsolenanwendung.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Erstellen einer Clientanwendung (Absender)
Um Nachrichten an das Relay senden zu können, erstellen wir mithilfe von Visual Studio eine C#-Konsolenanwendung.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Ausführen der Anwendungen
1. Führen Sie die Serveranwendung aus.
2. Führen Sie die Clientanwendung aus, und geben Sie Text ein.
3. Stellen Sie sicher, dass von der Konsole der Serveranwendung der Text ausgegeben wird, der in die Clientanwendung eingegeben wurde.

![Ausführen von Anwendungen](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

Glückwunsch! Sie haben eine Anwendung für End-to-End-Hybridverbindungen erstellt.

## <a name="next-steps"></a>Nächste Schritte:
* [Relay – Häufig gestellte Fragen](relay-faq.md)
* [Erstellen eines Namespace](relay-create-namespace-portal.md)
* [Erste Schritte mit Node](relay-hybrid-connections-node-get-started.md)




<!--HONumber=Feb17_HO1-->


