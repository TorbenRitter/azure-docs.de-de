---
title: 'Abrufen von ARP-Tabellen: Resource Manager: Azure ExpressRoute-Problembehandlung | Microsoft-Dokumentation'
description: "Diese Seite enthält Anweisungen zum Abrufen der ARP-Tabellen für eine ExpressRoute-Verbindung."
documentationcenter: na
services: expressroute
author: ganesr
manager: carolz
editor: tysonn
ms.assetid: 0a6bf1d5-6baf-44dd-87d3-1ebd2fd08bdc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2017
ms.author: ganesr
translationtype: Human Translation
ms.sourcegitcommit: 1a4206c80bc3581034b140de0003c64556b90303
ms.openlocfilehash: 2a5a12899ea3bcc89be0244e252c552515f57150


---
# <a name="getting-arp-tables-in-the-resource-manager-deployment-model"></a>Abrufen von ARP-Tabellen im Resource Manager-Bereitstellungsmodell
> [!div class="op_single_selector"]
> * [PowerShell – Resource Manager](expressroute-troubleshooting-arp-resource-manager.md)
> * [PowerShell – klassisch](expressroute-troubleshooting-arp-classic.md)
> 
> 

In diesem Artikel werden Sie durch die Schritte zum Erlernen der Nutzung der ARP-Tabellen für Ihre ExpressRoute-Verbindung geführt. 

> [!IMPORTANT]
> Dieses Dokument soll Ihnen helfen, einfache Probleme zu untersuchen und zu beheben. Es dient nicht als Ersatz für Microsoft-Support. Sie müssen ein Supportticket beim [Microsoft-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) öffnen, wenn Sie Ihr Problem nicht anhand der folgenden Anleitung beheben können.
> 
> 

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Address Resolution Protocol (ARP) und ARP-Tabellen
Address Resolution Protocol (ARP) ist ein in [RFC 826](https://tools.ietf.org/html/rfc826)definiertes Schicht-2-Protokoll. ARP dient zum Zuordnen der Ethernet-Adresse (MAC-Adresse) zu einer IP-Adresse.

Die ARP-Tabelle ermöglicht eine Zuordnung der IPv4-Adresse und MAC-Adresse für ein bestimmtes Peering. Die ARP-Tabelle für ein ExpressRoute-Verbindungspeering enthält die folgenden Informationen für jede (primäre und sekundäre) Schnittstelle.

1. Zuordnung der IP-Adresse der lokalen Routerschnittstelle zur MAC-Adresse
2. Zuordnung der IP-Adresse der ExpressRoute-Routerschnittstelle zur MAC-Adresse
3. Alter der Zuordnung

ARP-Tabellen dienen zum Überprüfen der Layer-2-Konfiguration und Behandeln grundlegender Layer-2-Verbindungsprobleme. 

Beispiel einer ARP-Tabelle: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Der folgende Abschnitt bietet Informationen zum Anzeigen der ARP-Tabellen, die von ExpressRoute-Edge-Routern verwendet werden. 

## <a name="prerequisites-for-learning-arp-tables"></a>Voraussetzungen für das Erlernen von ARP-Tabellen
Stellen Sie sicher, dass Folgendes vorliegt, bevor Sie fortfahren:

* Eine gültige ExpressRoute-Verbindung, die mit mindestens einem Peering konfiguriert ist. Die Verbindung muss vom Konnektivitätsanbieter vollständig konfiguriert werden. Von Ihnen (oder Ihrem Konnektivitätsanbieter) muss mindestens eines der Peerings (Azure privat, Azure öffentlich und Microsoft) für diese Verbindung konfiguriert werden.
* IP-Adressbereiche für die Konfiguration der Peerings (Azure privat, Azure öffentlich und Microsoft). Überprüfen Sie die Beispielzuweisungen von IP-Adressen auf der Seite [ExpressRoute-Routinganforderungen](expressroute-routing.md) , um zu verstehen, wie IP-Adressen auf Ihrer Seite und auf ExpressRoute-Seite zugeordnet werden. Auf der Seite [Erstellen und Ändern des Routings für eine ExpressRoute-Verbindung](expressroute-howto-routing-arm.md)erhalten Sie Informationen zur Konfiguration des Peerings.
* Informationen von Ihrem Netzwerkteam/Konnektivitätsanbieter zu den MAC-Adressen von Schnittstellen, die mit diesen IP-Adressen verwendet werden.
* Sie benötigen das neueste PowerShell-Modul für Azure (mindestens Version 1.50).

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>Abrufen der ARP-Tabellen für Ihre ExpressRoute-Verbindung
Dieser Abschnitt bietet Anweisungen zum Anzeigen der ARP-Tabellen pro Peering mithilfe von PowerShell. Von Ihnen oder Ihrem Konnektivitätsanbieter muss das Peering konfiguriert worden sein, ehe Sie fortfahren können. Jede Verbindung hat einen primären und sekundären Pfad. Sie können die ARP-Tabelle für jeden Pfad unabhängig überprüfen.

### <a name="arp-tables-for-azure-private-peering"></a>IP-Adressen für privates Azure-Peering
Das folgende Cmdlet dient zum Abrufen der ARP-Tabellen für privates Azure-Peering.

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary

        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Nachstehend sehen Sie die Beispielausgabe für einen der Pfade.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP-Tabellen für öffentliches Azure-Peering
Das folgende Cmdlet dient zum Abrufen der ARP-Tabellen für öffentliches Azure-Peering.

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary

        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Nachstehend sehen Sie die Beispielausgabe für einen der Pfade.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP-Tabellen für Microsoft-Peering
Das folgende Cmdlet dient zum Abrufen der ARP-Tabellen für Microsoft-Peering.

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"

        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary

        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Nachstehend sehen Sie die Beispielausgabe für einen der Pfade.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Nutzen dieser Informationen
Die ARP-Tabelle eines Peerings dient zum Bestimmen und Überprüfen der Layer-2-Konfiguration und -Konnektivität. Dieser Abschnitt enthält eine Übersicht über ARP-Tabellen in verschiedenen Szenarien.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>ARP-Tabelle einer Verbindung im Betriebszustand (d.h. dem erwarteten Zustand)
* Die ARP-Tabelle hat einen Eintrag für die lokale Seite mit einer gültigen IP-Adresse und MAC-Adresse und einen ähnlichen Eintrag für die Microsoft-Seite. 
* Das letzte Oktett der lokalen IP-Adresse ist immer eine ungerade Zahl.
* Das letzte Oktett der IP-Adresse von Microsoft ist immer eine gerade Zahl.
* Dieselbe MAC-Adresse wird auf Microsoft-Seite für alle drei Peerings (primär/sekundär) angezeigt. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP-Tabelle, wenn auf der lokalen Seite bzw. beim Konnektivitätsanbieter Probleme auftreten
* In der ARP-Tabelle wird nur ein Eintrag angezeigt. Dieser zeigt die Zuordnung zwischen der MAC-Adresse und IP-Adresse auf der Microsoft-Seite. 
  
       Age InterfaceProperty IpAddress  MacAddress    
       --- ----------------- ---------  ----------    
         0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

> [!NOTE]
> Öffnen Sie eine Supportanfrage bei Ihrem Konnektivitätsanbieter, um solche Probleme zu beheben. 
> 
> 

### <a name="arp-table-when-microsoft-side-has-problems"></a>ARP-Tabelle, wenn auf der Microsoft-Seite Probleme auftreten
* Wenn auf der Microsoft-Seite Probleme vorliegen, wird für ein Peering keine ARP-Tabelle angezeigt. 
* Öffnen Sie ein Supportticket beim [Microsoft Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Geben Sie an, dass Sie ein Problem mit der Layer-2-Konnektivität haben. 

## <a name="next-steps"></a>Nächste Schritte
* Überprüfen der Layer-3-Konfigurationen Ihrer ExpressRoute-Verbindung
  * Abrufen der Routenübersicht zum Bestimmen des Status von BGP-Sitzungen 
  * Abrufen der Routentabelle zum Bestimmen der für ExpressRoute angekündigten Präfixe
* Überprüfen der Datenübertragung anhand der ein- und ausgehenden Bytes
* Öffnen Sie ein Supportticket beim [Microsoft-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , wenn weiterhin Probleme auftreten.




<!--HONumber=Jan17_HO5-->


