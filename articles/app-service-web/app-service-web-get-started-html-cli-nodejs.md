---
title: "Bereitstellen Ihrer ersten HTML-Web-App für Azure in fünf Minuten | Microsoft-Dokumentation"
description: "Erfahren Sie, wie einfach die Ausführung von Web-Apps in App Service ist, indem Sie eine Beispiel-App bereitstellen. Sie können in kürzester Zeit mit der Entwicklung beginnen und sofort Ergebnisse erzielen."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 27c50be7-421a-47c9-8279-506519e404a4
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/04/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 5e532c130910e292d8b742353dfe7e24f1bf865e
ms.openlocfilehash: 37c364ab2a3948acd20b016639887a46ee18c470


---
# <a name="deploy-your-first-html-web-app-to-azure-in-five-minutes"></a>Bereitstellen Ihrer ersten HTML-Web-App für Azure in fünf Minuten

> [!div class="op_single_selector"]
> * [Erste HTML-Website](app-service-web-get-started-html-cli-nodejs.md)
> * [Erste .NET-App](app-service-web-get-started-dotnet-cli-nodejs.md)
> * [Erste PHP-App](app-service-web-get-started-php-cli-nodejs.md)
> * [Erste Node.js-App](app-service-web-get-started-nodejs-cli-nodejs.md)
> * [Erste Python-App](app-service-web-get-started-python-cli-nodejs.md)
> * [Erste Java-App](app-service-web-get-started-java.md)
> 
> 

In diesem Tutorial erfahren Sie, wie Sie eine einfache Web-App mit HTML und CSS für [Azure App Service](../app-service/app-service-value-prop-what-is.md)bereitstellen.
Mit App Service können Sie Web-Apps, [Mobile App-Back-Ends](/documentation/learning-paths/appservice-mobileapps/) und [API-Apps](../app-service-api/app-service-api-apps-why-best-platform.md) erstellen.

In diesem Tutorial führen Sie folgende Schritte aus: 

* Erstellen einer Web-App in Azure App Service
* Bereitstellen von HTML und CSS in der App
* Anzeigen der live in der Produktion ausgeführten Seiten
* Aktualisieren des Inhalts wie beim [Ausführen von Git-Commits mittels Push](https://git-scm.com/docs/git-push)

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure CLI 1.0:](app-service-web-get-started-html-cli-nodejs.md) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
- [Azure CLI 2.0 (Vorschau):](app-service-web-get-started-html.md) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell

## <a name="prerequisites"></a>Voraussetzungen
* [Git](http://www.git-scm.com/downloads).
* [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md).
* Ein Microsoft Azure-Konto. Falls Sie noch kein Konto haben, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) oder [Ihre Visual Studio-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Zum [Testen von App Service](https://azure.microsoft.com/try/app-service/) benötigen Sie kein Azure-Konto. Sie können eine Starter-App erstellen und bis zu einer Stunde damit experimentieren – ohne Kreditkarte und ohne jegliche Verpflichtungen.
> 
> 

## <a name="deploy-a-simple-html-site"></a>Bereitstellen einer einfachen HTML-Website
1. Öffnen Sie eine neue Windows-Eingabeaufforderung, ein PowerShell-Fenster, eine Linux-Shell oder ein OS X-Terminal. Vergewissern Sie sich durch Ausführen von `git --version` und `azure --version`, dass Git und die Azure-Befehlszeilenschnittstelle auf Ihrem Computer installiert sind.
   
    ![Testen der Installation der CLI-Tools für Ihre erste Web-App in Azure](./media/app-service-web-get-started/1-test-tools.png)
   
    Entsprechende Downloadlinks finden Sie bei Bedarf unter [Voraussetzungen](#Prerequisites) .
2. Melden Sie sich wie folgt bei Azure an:
   
        azure login
   
    Folgen Sie der Hilfemeldung, um den Anmeldeprozess fortzusetzen.
   
    ![Anmelden bei Azure zur Erstellung Ihrer ersten Web-App](./media/app-service-web-get-started/3-azure-login.png)

3. Wechseln Sie Azure-CLI in den ASM-Modus, und legen Sie dann den Bereitstellungsbenutzer für App Service fest. Sie werden später unter Verwendung der Anmeldeinformationen Code bereitstellen.
   
        azure config mode asm
        azure site deployment user set --username <username> --pass <password>

4. Wechseln Sie in ein Arbeitsverzeichnis (`CD`), und klonen Sie die HTML-Beispielwebsite wie folgt:
   
        git clone https://github.com/Azure-Samples/app-service-web-html-get-started.git

5. Wechseln Sie in das Repository Ihrer Beispiel-App. 
   
        cd app-service-web-html-get-started

6. Erstellen Sie die App Service-App-Ressource in Azure mit einem eindeutigen App-Namen und dem zuvor konfigurierten Bereitstellungsbenutzer. Geben Sie die Zahl der gewünschten Region ein, wenn Sie dazu aufgefordert werden.
   
        azure site create <app_name> --git --gitusername <username>
   
    ![Erstellen der Azure-Ressource für Ihre erste Web-App in Azure](./media/app-service-web-get-started/4-create-site.png)
   
    Ihre App wird jetzt in Azure erstellt. Ihr aktuelles Verzeichnis wird außerdem für Git initialisiert und mit der neuen App Service-App als Git-Remoteelement verbunden.
    Sie können zwar auch zur URL der App (http://&lt;app_name>.azurewebsites.net) navigieren, um die HTML-Standardseite anzuzeigen, wir möchten hier aber eigentlich Ihren eigenen Code verwenden.
7. Gehen Sie wie bei anderen Pushübertragungen von Code mit Git vor, um Ihren Beispielcode für die Azure-App bereitzustellen. Wenn Sie aufgefordert werden, verwenden Sie das Kennwort, das Sie zuvor konfiguriert haben.
   
        git push azure master
   
    ![Übertragen von Code an Ihre ersten Web-App in Azure mittels Push](./media/app-service-web-get-started/5-push-code.png)
   
    Bei Verwendung eines der Sprachen-Frameworks wird eine andere Ausgabe angezeigt. Das hat folgenden Grund: Mit `git push` wird nicht nur Code in Azure eingefügt, sondern es werden auch Bereitstellungsaufgaben im Bereitstellungsmodul ausgelöst. Falls Ihr Projektstamm (bzw. Repositorystamm) die Datei „package.json“ (Node.js) oder „requirements.txt“ (Python) enthält oder falls Ihr ASP.NET-Projekt die Datei „packages.config“ enthält, stellt das Bereitstellungsskript die erforderlichen Pakete wieder für Sie her. Sie können auch die [Composer-Erweiterung aktivieren](web-sites-php-mysql-deploy-use-git.md#composer) , um Dateien vom Typ „composer.json“ in Ihrer PHP-App automatisch zu verarbeiten.

Herzlichen Glückwunsch! Sie haben Ihre App in Azure App Service bereitgestellt.

## <a name="see-your-app-running-live"></a>Verfolgen der Liveausführung der App
Führen Sie den folgenden Befehl aus einem beliebigen Verzeichnis in Ihrem Repository aus, um die Liveausführung der App in Azure zu verfolgen:

    azure site browse

## <a name="make-updates-to-your-app"></a>Durchführen von Updates für die App
Sie können jetzt Git verwenden, um aus Ihrem Projektstamm (Repositorystamm) jederzeit einen Pushvorgang durchzuführen und so ein Update für die Live-Website vorzunehmen. Dazu gehen Sie wie bei der erstmaligen Bereitstellung Ihres Codes vor. Wenn Sie beispielsweise eine neue Änderung übertragen möchten, die Sie lokal getestet haben, führen Sie einfach die folgenden Befehle in Ihrem Projektstamm (Repositorystamm) aus:

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie den Artikel zu den bevorzugten Schritten für die Entwicklung und Bereitstellung für Ihr Sprachen-Framework:

* [.NET](web-sites-dotnet-get-started.md)
* [PHP](app-service-web-php-get-started-cli-nodejs.md)
* [Node.js](app-service-web-nodejs-get-started-cli-nodejs.md)
* [Python](web-sites-python-ptvs-django-mysql.md)
* [Java](web-sites-java-get-started.md)

Sie können auch weiter mit Ihrer ersten Web-App arbeiten. Beispiel:

* Testen Sie [weitere Methoden zum Bereitstellen Ihres Codes in Azure](web-sites-deploy.md). Wenn Sie eines Ihrer GitHub-Repositorys zur Bereitstellung verwenden möchten, wählen Sie beispielsweise unter **Bereitstellungsoptionen** einfach **GitHub** anstelle von **Lokales Git-Repository** aus.
* Entwickeln Sie Ihre Azure-App weiter. Authentifizieren Sie Ihre Benutzer. Skalieren Sie die App je nach Bedarf. Richten Sie einige Leistungswarnungen ein. Es sind jeweils nur wenige Klicks erforderlich. Weitere Informationen finden Sie unter [Hinzufügen von Funktionen zu Ihrer ersten Web-App](app-service-web-get-started-2.md).




<!--HONumber=Jan17_HO4-->


