---
title: "REST-API: Firewallregeln auf Serverebene für Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Firewallregeln auf Serverebene für IP-Adressen konfigurieren, mit denen per REST-API auf Azure SQL-Datenbanken zugegriffen wird."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: fc874f3c-c623-4924-8cb7-32a8c31e666c
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/09/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 86bc7d89bb5725add8ba05b6f0978467147fd3ca
ms.openlocfilehash: 6ac9bdf34e8cf0a8ca8e06ae3ed6cd2dd1cf408b


---
# <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>Konfigurieren von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe der REST-API
> [!div class="op_single_selector"]
> * [Übersicht](sql-database-firewall-configure.md)
> * [Azure-Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [REST-API](sql-database-configure-firewall-settings-rest.md)
> 
> 

Microsoft Azure SQL-Datenbank verwendet Firewallregeln, um Verbindungen mit Ihren Servern und Datenbanken zu erlauben. Sie können auf Ihrem Azure SQL-Datenbank-Server auf Serverebene und Datenbankebene Firewalleinstellungen für die Masterdatenbank oder eine Benutzerdatenbank definieren, um den selektiven Zugriff auf die Datenbank zu ermöglichen.

> [!IMPORTANT]
> Um Anwendungen von Azure die Verbindung mit dem Datenbankserver zu ermöglichen, müssen Azure-Verbindungen aktiviert sein. Weitere Informationen zu Firewallregeln und dem Aktivieren von Verbindungen aus Azure finden Sie unter [Firewall für Azure SQL-Datenbank](sql-database-firewall-configure.md). Wenn Sie Verbindungen innerhalb der Grenzen der Azure-Cloud herstellen möchten, müssen Sie möglicherweise einige zusätzliche TCP-Ports öffnen. Weitere Informationen finden Sie im Abschnitt **SQL-Datenbank V12: "Außerhalb" im Vergleich zu "Innerhalb"** im Artikel [Andere Ports als 1433 für ADO.NET 4.5 und SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md)
> 
> 

## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Verwalten von Firewallregeln auf Serverebene über die REST-API
1. Beim Verwalten von Firewallregeln über die REST-API muss eine Authentifizierung erfolgen. Informationen finden Sie im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](../azure-resource-manager/resource-manager-api-authentication.md).
2. Regeln auf Serverebene können über die REST-API erstellt, aktualisiert oder gelöscht werden.
   
    Um eine Firewallregel auf Serverebene zu erstellen oder zu aktualisieren, führen Sie die PUT-Methode mit folgendem Inhalt aus:
   
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
   
    Anforderungstext
   
        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 

    Um eine vorhandene Firewallregel auf Serverebene zu entfernen, führen Sie die DELETE-Methode mit folgendem Inhalt aus:

        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>Verwalten von Firewallregeln mithilfe der REST-API
* [Create or Update Firewall Rule](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Delete Firewall Rule](https://msdn.microsoft.com/library/azure/mt445502.aspx)
* [Get Firewall Rule](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [List Firewall Rules](https://msdn.microsoft.com/library/azure/mt604478.aspx)

## <a name="next-steps"></a>Nächste Schritte
Einen Artikel mit einer Anleitung zur Verwendung von Transact-SQL zum Erstellen von Firewallregeln auf Serverebene und Datenbankebene finden Sie unter [Konfigurieren von Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank mit T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Artikel mit Anleitungen zum Erstellen von Firewallregeln auf Serverebene mit anderen Methoden finden Sie unter: 

* [Konfigurieren von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md)
* [Konfigurieren von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe von PowerShell](sql-database-configure-firewall-settings-powershell.md)

Ein Tutorial zum Erstellen einer Datenbank finden Sie unter [Erstellen einer SQL-Datenbank in Minuten mit dem Azure-Portal](sql-database-get-started.md).
Hilfe beim Herstellen einer Verbindung mit einer Azure SQL-Datenbank über Open Source-Anwendungen oder Anwendungen von Drittanbietern finden Sie unter [Clientcodebeispiele für die ersten Schritte mit SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Informationen zum Navigieren zu Datenbanken finden Sie unter [Verwalten von Datenbankzugriff und Anmeldesicherheit](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Sichern der Datenbank](sql-database-security-overview.md)
* [Sicherheitscenter für SQL Server-Datenbankmodul und Azure SQL-Datenbank](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->





<!--HONumber=Feb17_HO2-->


