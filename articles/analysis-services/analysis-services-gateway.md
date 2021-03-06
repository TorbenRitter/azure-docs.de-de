---
title: Lokales Datengateway | Microsoft Docs
description: Ein lokales Gateway ist erforderlich, wenn der Analysis Services-Server in Azure mit lokalen Datenquellen verbunden wird.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: cd596155-b608-4a34-935e-e45c95d884a9
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 01/20/2017
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: 193c939065979dc48243d31e7f97cd87d96bf9a8
ms.openlocfilehash: 90584f60864589744888817ea71d0eb0d4d170ff


---
# <a name="on-premises-data-gateway"></a>Lokales Datengateway
Das lokale Datengateway fungiert als Brücke für eine sichere Datenübertragung zwischen lokalen Datenquellen und dem Azure Analysis Services-Servers in der Cloud.

Ein Gateway wird auf einem Computer im Netzwerk installiert. Für jeden Azure Analysis Services-Server im Azure-Abonnement muss ein Gateway installiert werden. Wenn beispielsweise Ihr Azure-Abonnement zwei Server enthält, die mit lokalen Datenquellen verbunden werden, muss auf zwei verschiedenen Computern im Netzwerk ein Gateway installiert werden.

## <a name="requirements"></a>Anforderungen
**Mindestanforderungen.**

* .NET 4.5 Framework
* 64-Bit-Version von Windows 7/Windows Server 2008 R2 (oder höher)

**Empfohlen.**

* 8-Kern-CPU
* 8 GB Arbeitsspeicher
* 64-Bit-Version von Windows 2012 R2 (oder höher)

**Wichtige Hinweise:**

* Das Gateway darf nicht auf einem Domänencontroller installiert werden.
* Auf einem einzelnen Computer kann nur ein Gateway installiert werden.
* Installieren Sie das Gateway auf einem Computer, der eingeschaltet bleibt und nicht in den Ruhezustand versetzt wird. Wenn der Computer nicht in Betrieb ist, kann der Azure Analysis Services-Server keine Verbindung mit den lokalen Datenquellen herstellen, um Daten zu aktualisieren.
* Installieren Sie das Gateway nicht auf einem Computer, der über eine Drahtlosverbindung mit dem Netzwerk verfügt. Die Leistung kann beeinträchtigt werden.
* Zum Ändern des Servernamens für ein Gateway, das bereits konfiguriert wurde, müssen Sie das Gateway neu installieren und ein neues Gateway konfigurieren.
* Gelegentlich kann es vorkommen, dass tabellarische Modelle beim Herstellen einer Verbindung mit Datenquellen mithilfe von nativen Anbietern wie SQL Server Native Client (SQLNCLI11) einen Fehler zurückgeben. Weitere Informationen finden Sie unter [Datenquellenverbindungen](analysis-services-datasource.md).

## <a name="supported-on-premises-data-sources"></a>Unterstützte lokale Datenquellen
In der Vorschauversion unterstützt das Gateway Verbindungen zwischen dem Azure Analysis Services-Server und den folgenden lokalen Datenquellen:

* SQL Server
* SQL Data Warehouse
* APS
* Oracle
* Teradata

## <a name="download"></a>Download
 [Gateway herunterladen](https://aka.ms/azureasgateway)

## <a name="install-and-configure"></a>Installieren und konfigurieren
1. Führen Sie das Setup aus.
2. Wählen Sie einen Installationsspeicherort aus, und akzeptieren Sie die Lizenzbedingungen.
3. Melden Sie sich bei Azure an.
4. Geben Sie den Namen des Azure-Analysis-Servers an. Sie können nur einen Server pro Gateway angeben. Klicken Sie auf **Konfigurieren**, und Sie können loslegen.

    ![Bei Azure anmelden](./media/analysis-services-gateway/aas-gateway-configure-server.png)

## <a name="how-it-works"></a>So funktioniert's
Das Gateway wird als Windows-Dienst mit dem Namen **Lokales Datengateway** auf einem Computer im Netzwerk der Organisation ausgeführt. Das Gateway, das Sie für die Verwendung mit Azure Analysis Services installieren, basiert auf demselben Gateway, das für andere Dienste, z. B. Power BI, verwendet wird, es gibt jedoch einige Unterschiede bei der Konfiguration.

![So funktioniert's](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Funktion von Abfragen und Datenfluss:

1. Vom Clouddienst wird eine Abfrage mit den verschlüsselten Anmeldeinformationen für die lokale Datenquelle erstellt. Sie wird dann an eine Warteschlange für die Verarbeitung durch das Gateway gesendet.
2. Der Gatewayclouddienst analysiert die Abfrage und überträgt die Anforderung per Push an den [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).
3. Das lokale Datengateway fragt den Azure Service Bus nach ausstehenden Anforderungen ab.
4. Das Gateway ruft die Abfrage ab, entschlüsselt die Anmeldeinformationen und stellt anhand dieser Anmeldeinformationen eine Verbindung mit den Datenquellen her.
5. Das Gateway sendet die Abfrage zur Ausführung an die Datenquelle.
6. Die Ergebnisse werden aus der Datenquelle zurück an das Gateway und dann an den Clouddienst gesendet.

## <a name="windows-service-account"></a>Windows-Dienstkonto
Das lokale Datengateway ist zur Verwendung von *NT SERVICE\PBIEgwService* für die Anmeldeinformationen für den Windows-Dienst konfiguriert. Standardmäßig verfügt es über das Recht „Anmelden als Dienst“ im Kontext des Computers, auf dem Sie das Gateway installieren. Diese Anmeldeinformationen sind nicht mit denen des Kontos identisch, das zum Verbinden mit lokalen Datenquellen oder dem Azure-Konto verwendet wird.  

Wenn beim Proxyserver Authentifizierungsprobleme auftreten, sollten Sie das Windows-Dienstkonto in ein Domänenbenutzerkonto oder verwaltetes Dienstkonto ändern.

## <a name="ports"></a>Ports
Das Gateway stellt eine ausgehende Verbindung mit dem Azure Service Bus her. Es kommuniziert über ausgehende Ports: TCP 443 (Standard), 5671, 5672, 9350 bis 9354.  Das Gateway benötigt keine eingehenden Ports.

Es wird empfohlen, die IP-Adressen für Ihren Datenbereich in die Whitelist der Firewall aufzunehmen. Sie können die [Liste der IP-Bereiche des Microsoft Azure-Rechenzentrums](https://www.microsoft.com/download/details.aspx?id=41653) (in englischer Sprache) herunterladen. Diese Liste wird wöchentlich aktualisiert.

> [!NOTE]
> Die IP-Adressen in der Liste der IP-Bereiche des Azure-Rechenzentrums sind in der CIDR-Notation angegeben. Beispielsweise bedeutet 10.0.0.0/24 nicht 10.0.0.0 bis 10.0.0.24. Informieren Sie sich über die [CIDR-Notation](http://whatismyipaddress.com/cidr).
>
>

Nachfolgend sind die vollqualifizierten Domänennamen aufgeführt, die vom Gateway verwendet werden.

| Domänennamen | Ausgehende Ports | Beschreibung |
| --- | --- | --- |
| *.powerbi.com |80 |Zum Herunterladen des Installers wird HTTP verwendet. |
| *.powerbi.com |443 |HTTPS |
| *. analysis.windows.net |443 |HTTPS |
| *.login.windows.net |443 |HTTPS |
| *.servicebus.windows.net |5671-5672 |Advanced Message Queuing Protocol (AMQP) |
| *.servicebus.windows.net |443, 9350-9354 |Listener auf Service Bus Relay per TCP (443 für Access Control-Tokenbeschaffung erforderlich) |
| *.frontend.clouddatahub.net |443 |HTTPS |
| *.core.windows.net |443 |HTTPS |
| login.microsoftonline.com |443 |HTTPS |
| *.msftncsi.com |443 |Wird verwendet, um die Internetkonnektivität zu testen, falls der Power BI-Dienst das Gateway nicht erreicht. |
| *.microsoftonline-p.com |443 |Wird je nach Konfiguration für die Authentifizierung verwendet. |

### <a name="forcing-https-communication-with-azure-service-bus"></a>Erzwingen von HTTPS-Kommunikation mit Azure Service Bus
Sie können erzwingen, dass das Gateway für die Kommunikation mit Azure Service Bus HTTPS statt TCP verwendet; dies kann jedoch die Leistung erheblich beeinträchtigen. Hierzu müssen Sie die Datei *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* ändern. Ändern Sie den Wert von `AutoDetect` in `Https`. Diese Datei befindet sich standardmäßig in *C:\Programme\On-premises data gateway*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```


## <a name="troubleshooting"></a>Problembehandlung
De facto ist das lokale Datengateway, das zum Verbinden von Azure Analysis Services mit den lokalen Datenquellen verwendet wird, das gleiche Gateway, das für Power BI verwendet wird.

Wenn beim Installieren und Konfigurieren eines Gateways Probleme auftreten, konsultieren Sie [Power BI-Gateway – Problembehandlung](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem-tshoot/). Wenn Sie der Meinung sind, dass ein Problem bei der Firewall aufgetreten ist, konsultieren Sie die Abschnitte zu Firewall oder Proxy.

Wenn Sie meinen, dass beim Gateway Proxyprobleme vorliegen, konsultieren Sie [Konfigurieren von Proxyeinstellungen für Power BI-Gateways](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy.md).

## <a name="next-steps"></a>Nächste Schritte
* [Verwalten von Analysis Services](analysis-services-manage.md)
* [Abrufen von Daten aus Azure Analysis Services](analysis-services-connect.md)



<!--HONumber=Nov16_HO3-->


