---
title: "Bereitstellen Ihrer ersten Node.js-Web-App für Azure in fünf Minuten (CLI 2.0 Preview) | Microsoft-Dokumentation"
description: "Erfahren Sie, wie einfach die Ausführung von Web-Apps in App Service ist, indem Sie eine Beispiel-Node.js-App bereitstellen. Sie können in kürzester Zeit mit der Entwicklung beginnen und sofort Ergebnisse erzielen."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 412cc786-5bf3-4e1b-b696-6a08cf46501e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: 06a8dfbac31024cb44fd38bcf9c4a4ea79dbc968


---
# <a name="deploy-your-first-nodejs-web-app-to-azure-in-five-minutes-cli-20-preview"></a>Bereitstellen Ihrer ersten Node.js-Web-App für Azure in fünf Minuten (CLI 2.0 Preview)

> [!div class="op_single_selector"]
> * [Erste HTML-Website](app-service-web-get-started-html.md)
> * [Erste .NET-App](app-service-web-get-started-dotnet.md)
> * [Erste PHP-App](app-service-web-get-started-php.md)
> * [Erste Node.js-App](app-service-web-get-started-nodejs.md)
> * [Erste Python-App](app-service-web-get-started-python.md)
> * [Erste Java-App](app-service-web-get-started-java.md)
> 
> 

In diesem Tutorial erfahren Sie, wie Sie Ihre erste Node.js-Web-App für [Azure App Service](../app-service/app-service-value-prop-what-is.md)bereitstellen.
Mit App Service können Sie Web-Apps, [Mobile App-Back-Ends](/documentation/learning-paths/appservice-mobileapps/) und [API-Apps](../app-service-api/app-service-api-apps-why-best-platform.md) erstellen.

In diesem Tutorial führen Sie folgende Schritte aus: 

* Erstellen einer Web-App in Azure App Service
* Bereitstellen von Node.js-Beispielcode
* Anzeigen des live in der Produktion ausgeführten Codes
* Aktualisieren Ihrer Web-App wie beim [Ausführen von Git-Commits mittels Push](https://git-scm.com/docs/git-push)

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure CLI 1.0:](app-service-web-get-started-nodejs-cli-nodejs.md) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
- [Azure CLI 2.0 (Vorschau):](app-service-web-get-started-nodejs.md) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell

## <a name="prerequisites"></a>Voraussetzungen
* [Git](http://www.git-scm.com/downloads).
* [Azure CLI 2.0 Preview](/cli/azure/install-az-cli2).
* Ein Microsoft Azure-Konto. Falls Sie noch kein Konto haben, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) oder [Ihre Visual Studio-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Zum [Testen von App Service](https://azure.microsoft.com/try/app-service/) benötigen Sie kein Azure-Konto. Sie können eine Starter-App erstellen und bis zu einer Stunde damit experimentieren – ohne Kreditkarte und ohne jegliche Verpflichtungen.
> 
> 

## <a name="deploy-a-nodejs-web-app"></a>Bereitstellen einer Node.js-Web-App
1. Öffnen Sie eine neue Windows-Eingabeaufforderung, ein PowerShell-Fenster, eine Linux-Shell oder ein OS X-Terminal. Vergewissern Sie sich durch Ausführen von `git --version` und `azure --version`, dass Git und die Azure-Befehlszeilenschnittstelle auf Ihrem Computer installiert sind.
   
    ![Testen der Installation der CLI-Tools für Ihre erste Web-App in Azure](./media/app-service-web-get-started-languages/1-test-tools-2.0.png)
   
    Entsprechende Downloadlinks finden Sie bei Bedarf unter [Voraussetzungen](#Prerequisites) .
2. Melden Sie sich wie folgt bei Azure an:
   
        az login
   
    Folgen Sie der Hilfemeldung, um den Anmeldeprozess fortzusetzen.
   
    ![Anmelden bei Azure zur Erstellung Ihrer ersten Web-App](./media/app-service-web-get-started-languages/3-azure-login-2.0.png)

3. Legen Sie den Bereitstellungsbenutzer für App Service fest. Später stellen Sie Code mit diesen Anmeldeinformationen bereit.
   
        az appservice web deployment user set --user-name <username> --password <password>

3. Erstellen Sie eine neue [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). In diesem ersten App Service-Tutorial müssen Sie nicht unbedingt wissen, worum es sich dabei genau handelt.

        az group create --location "<location>" --name my-first-app-group

    Welche Werte Sie für `<location>` verwenden können, erfahren Sie mithilfe des CLI-Befehls `az appservice list-locations`.

3. Erstellen Sie einen neuen [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) vom Typ „FREE“. In diesem ersten App Service-Tutorial reicht es, wenn Sie wissen, dass Ihnen für Web-Apps im Rahmen dieses Plans keine Kosten entstehen.

        az appservice plan create --name my-free-appservice-plan --resource-group my-first-app-group --sku FREE

4. Erstellen Sie in `<app_name>` eine neue Web-App mit einem eindeutigen Namen.

        az appservice web create --name <app_name> --resource-group my-first-app-group --plan my-free-appservice-plan

4. Als Nächstes erhalten Sie den Node.js-Beispielcode, den Sie bereitstellen möchten. Wechseln Sie in ein Arbeitsverzeichnis (`CD`), und klonen Sie die Beispiel-App wie folgt:
   
        cd <working_directory>
        git clone https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git

5. Wechseln Sie in das Repository Ihrer Beispiel-App.
   
        cd app-service-web-nodejs-get-started
5. Konfigurieren Sie mit dem folgenden Befehl die lokale Git-Bereitstellung für Ihre App Service-Web-App:

        az appservice web source-control config-local-git --name <app_name> --resource-group my-first-app-group

    Sie erhalten eine JSON-Ausgabe wie die folgende, was bedeutet, dass das Git-Remoterepository konfiguriert ist:

        {
        "url": "https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git"
        }

6. Fügen Sie die URL im JSON-Code als Git-Remotespeicherort für Ihr lokales Repository (der Einfachheit halber `azure` genannt) hinzu.

        git remote add azure https://<deployment_user>@<app_name>.scm.azurewebsites.net/<app_name>.git
   
7. Gehen Sie wie bei anderen Pushübertragungen von Code mit Git vor, um Ihren Beispielcode für die Azure-App bereitzustellen. Wenn Sie aufgefordert werden, verwenden Sie das Kennwort, das Sie zuvor konfiguriert haben.
   
        git push azure master
   
    ![Übertragen von Code an Ihre ersten Web-App in Azure mittels Push](./media/app-service-web-get-started-languages/node-git-push.png)
   
    Mit `git push` wird nicht nur Code in Azure eingefügt, sondern es werden auch Bereitstellungsaufgaben im Bereitstellungsmodul ausgelöst. 
    Falls Ihr Projektstamm (Repositorystamm) die Datei „package.json“ enthält, stellt das Bereitstellungsskript die erforderlichen Pakete wieder für Sie her. 

Herzlichen Glückwunsch! Sie haben Ihre App in Azure App Service bereitgestellt.

## <a name="see-your-app-running-live"></a>Verfolgen der Liveausführung der App
Führen Sie den folgenden Befehl aus einem beliebigen Verzeichnis in Ihrem Repository aus, um die Liveausführung der App in Azure zu verfolgen:

    az appservice web browse -n <app-name> --resource-group my-first-app-group

## <a name="make-updates-to-your-app"></a>Durchführen von Updates für die App
Sie können jetzt Git verwenden, um aus Ihrem Projektstamm (Repositorystamm) jederzeit einen Pushvorgang durchzuführen und so ein Update für die Live-Website vorzunehmen. Dazu gehen Sie wie bei der erstmaligen Bereitstellung Ihres Codes vor. Wenn Sie beispielsweise eine neue Änderung übertragen möchten, die Sie lokal getestet haben, führen Sie einfach die folgenden Befehle in Ihrem Projektstamm (Repositorystamm) aus:

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Nächste Schritte
[Erstellen, Konfigurieren und Bereitstellen einer Node.js-Express-Web-App in Azure](app-service-web-nodejs-get-started.md) In diesem Tutorial eignen Sie sich grundlegende Kenntnisse zum Ausführen einer Node.js-Web-App in Azure an. Sie erhalten u.a. Informationen zu den folgenden Schritten:

* Erstellen und Konfigurieren von Apps in Azure über PowerShell/Bash
* Festlegen der Node.js-Version
* Verwenden einer Startdatei, die sich nicht im Stammverzeichnis der Anwendung befindet
* Automatisieren mit NPM
* Abrufen von Fehler-und Ausgabeprotokollen

Sie können auch weiter mit Ihrer ersten Web-App arbeiten. Beispiel:

* Testen Sie [weitere Methoden zum Bereitstellen Ihres Codes in Azure](web-sites-deploy.md). Wenn Sie eines Ihrer GitHub-Repositorys zur Bereitstellung verwenden möchten, wählen Sie beispielsweise unter **Bereitstellungsoptionen** einfach **GitHub** anstelle von **Lokales Git-Repository** aus.
* Entwickeln Sie Ihre Azure-App weiter. Authentifizieren Sie Ihre Benutzer. Skalieren Sie die App je nach Bedarf. Richten Sie einige Leistungswarnungen ein. Es sind jeweils nur wenige Klicks erforderlich. Weitere Informationen finden Sie unter [Hinzufügen von Funktionen zu Ihrer ersten Web-App](app-service-web-get-started-2.md).




<!--HONumber=Jan17_HO3-->


