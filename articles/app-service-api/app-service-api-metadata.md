---
title: "Metadaten in App Service-API-Apps für die API-Ermittlung und Codegenerierung | Microsoft-Dokumentation"
description: Erfahren Sie, wie API-Apps in Azure App Service mithilfe von Swagger-Metadaten die API-Ermittlung und Codegenerierung erleichtern.
services: app-service\api
documentationcenter: .net
author: tdykstra
manager: wpickett
editor: 
ms.assetid: c7f8e33a-61cc-486f-89df-4a97dc3c71d4
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2016
ms.author: rachelap
translationtype: Human Translation
ms.sourcegitcommit: a0580f8d303c7ce33a65f0ce6faecf2492f851b0
ms.openlocfilehash: 3b41340f30239dc0102808b5e492ee7300dcd12b


---
# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>Metadaten in App Service-API-Apps für die API-Ermittlung und Codegenerierung
In App Service-API-Apps ist eine Unterstützung für [Swagger 2.0](http://swagger.io/)-API-Metadaten integriert. Sie müssen Swagger nicht nutzen. Wenn Sie dieses Framework für APIs jedoch verwenden, können von API-Apps-Features profitieren, die die Ermittlung und die Nutzung erleichtern.   

## <a name="swagger-endpoint"></a>Swagger-Endpunkt
Sie können einen Endpunkt angeben, der Swagger 2.0-JSON-Metadaten für eine API-App in einer Eigenschaft der API-App bereitstellt. Der Endpunkt kann sich auf den Basis-URL der API-App oder auf eine absolute URL beziehen. Absolute URLs können nach außerhalb der API-App verweisen. 

Für viele nachgelagerte Clients (z. B. Visual Studio-Codegenerierung und PowerApps-Schritt „API hinzufügen“) muss die URL öffentlich zugänglich sein (ohne Schutz durch Benutzer- oder Dienstauthentifizierung). Dies bedeutet, dass wenn Sie die App Service-Authentifizierung verwenden und die API-Definition innerhalb der App selbst verfügbar machen möchten, Sie eine Authentifizierungsmethode wählen müssen, die anonymen Datenverkehr zu lässt, um Ihre API zu erreichen. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung für App Service-API-Apps](app-service-api-authentication.md).

### <a name="portal-blade"></a>Blatt „Portal“
Die Endpunkt-URL wird im [Azure-Portal](https://portal.azure.com/) auf dem Blatt **API-Definition** angezeigt und kann dort geändert werden.

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>„Azure-Ressourcen-Manager“-Eigenschaft
Sie können die API-Definitions-URL für eine API-App auch konfigurieren, indem Sie den [Ressourcen-Explorer](https://resources.azure.com/) oder [Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md) in Befehlszeilentools verwenden, beispielsweise in [Azure PowerShell](/powershell/azureps-cmdlets-docs) und der [Azure-CLI](../xplat-cli-install.md). 

Beispiel: Wechseln Sie im **Ressourcen-Explorer** zu **Abonnements > {Ihr Abonnement} > Ressourcengruppen > {Ihre Ressourcengruppe} > Anbieter > Microsoft.Web > Websites > {Ihre Website} > Konfigurieren > Web**. Hier wird die `apiDefinition`-Eigenschaft angezeigt:

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

Öffnen Sie die [Datei „azuredeploy.json“ in der Aufgabenlisten-Beispielanwendung](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json), um ein Beispiel für eine Azure Resource Manager-Vorlage zum Festlegen der`apiDefinition`-Eigenschaft anzuzeigen. Suchen Sie nach dem Abschnitt der Vorlage, die wie im oben gezeigten JSON-Beispiel aussieht..

### <a name="default-value"></a>Standardwert
Wenn Sie eine API-App mit Visual Studio erstellen, wird der API-Definitionsendpunkt automatisch auf die Basis-URL der API-App und `/swagger/docs/v1`festgelegt. Dies ist die Standard-URL, die vom [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) -NuGet-Paket verwendet wird, um ein ASP.NET-Web-API-Projekt mit dynamisch generierten Swagger-Metadaten zu versorgen. 

## <a name="code-generation"></a>Codegenerierung
Einer der Vorteile der Integration von Swagger in Azure API-Apps ist die automatische Codegenerierung. Generierte Clientklassen erleichtern es, Code zu schreiben, der eine API-App aufruft.

Sie können Clientcode für eine API-App mit Visual Studio oder über die Befehlszeile generieren. Informationen zum Generieren von Clientklassen für ein ASP.NET-Web-API-Projekt mit Visual Studio finden Sie unter [Erste Schritte mit API-Apps und ASP.NET](app-service-api-dotnet-get-started.md#codegen). Informationen zur Vorgehensweise mit der Befehlszeile für alle unterstützten Sprachen finden Sie auf GitHub.com im Repository [Azure/autorest](https://github.com/azure/autorest) in der Readme-Datei.

## <a name="next-steps"></a>Nächste Schritte
Ein Schritt-für-Schritt-Tutorial mit Anleitungen zum Erstellen, Bereitstellen und Nutzen einer API-App finden Sie unter [Erste Schritte mit API-Apps in Azure App Service](app-service-api-dotnet-get-started.md).

Wenn Sie Azure API Management mit API-Apps verwenden, können Sie Ihre API mithilfe von Swagger-Metadaten in API Management importieren. Weitere Informationen finden Sie unter [Importieren einer API-Definition mit Operationen in Azure API Management](../api-management/api-management-howto-import-api.md). 




<!--HONumber=Dec16_HO3-->


