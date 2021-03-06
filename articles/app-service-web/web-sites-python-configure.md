---
title: Konfigurieren von Python in Azure App Service-Web-Apps
description: "Dieses Lernprogramm beschreibt die Optionen für das Erstellen und Konfigurieren einer grundlegenden WSGI-kompatiblen (Web Server Gateway Interface) Python-Anwendung für Azure App Service-Web-Apps."
services: app-service
documentationcenter: python
tags: python
author: huguesv
manager: wpickett
editor: 
ms.assetid: fd00dc91-9935-4331-b955-4bd71e66d518
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/26/2016
ms.author: huvalo
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: c4fe19e608061c4b69f3856313ece87e9870bbf9


---
# <a name="configuring-python-with-azure-app-service-web-apps"></a>Konfigurieren von Python in Azure App Service-Web-Apps
Dieses Lernprogramm beschreibt die Optionen für das Authoring und Konfigurieren einer grundlegenden Web Server Gateway Interface (WSGI)-kompatiblen Python-Anwendung für [Azure App Service-Web-Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Außerdem beschrieben werden zusätzliche Funktionen der Git-Bereitstellung, z. B. virtuelle Umgebung und Paketinstallation mithilfe von "requirements.txt".

## <a name="bottle-django-or-flask"></a>Bottle, Django oder Flask?
Der Azure Marketplace enthält Vorlagen für die Frameworks Bottle, Django und Flask. Wenn Sie Ihre erste Web-App in Azure App Service entwickeln oder nicht mit Git vertraut sind, empfehlen wir, sich mit den folgenden Lernprogrammen vertraut zu machen, die schrittweise Anleitungen zum Entwickeln einer funktionierenden Anwendung aus dem Katalog mithilfe der Git-Bereitstellung unter Windows oder Mac enthalten:

* [Erstellen von Web-Apps mit Bottle](web-sites-python-create-deploy-bottle-app.md)
* [Erstellen von Web-Apps mit Django](web-sites-python-create-deploy-django-app.md)
* [Erstellen von Web-Apps mit Flask](web-sites-python-create-deploy-flask-app.md)

## <a name="web-app-creation-on-azure-portal"></a>Web-App-Erstellung im Azure-Portal
Für dieses Tutorial wird davon ausgegangen, dass Sie über ein Azure-Abonnement verfügen und Zugriff auf das Azure-Portal haben.

Wenn Sie noch nicht über eine Web-App verfügen, können Sie eine über das [Azure-Portal](https://portal.azure.com)erstellen.  Klicken Sie in der linken oberen Ecke auf „NEU“ und anschließend auf **Web und mobil** > **Web-App**.

## <a name="git-publishing"></a>Git-Veröffentlichung
Konfigurieren Sie die Git-Veröffentlichung für Ihre neu erstellte Web-App anhand der Anleitung unter [Lokale Git-Bereitstellung in Azure App Service](app-service-deploy-local-git.md). In diesem Lernprogramm wird die Python-Web-App mit Git erstellt, verwaltet und für Azure App Service veröffentlicht.

Sobald die Git-Veröffentlichung eingerichtet ist, wird ein Git-Repository erstellt und Ihrer Web-App zugewiesen. Die URL des Repository wird angezeigt und kann ab sofort verwendet werden, um Daten aus der lokalen Entwicklungsumgebung mittels Push in die Cloud zu übertragen. Stellen Sie beim Veröffentlichen von Anwendungen mittels Git sicher, dass auch ein Git-Client installiert ist, und verwenden Sie die bereitgestellten Anweisungen, um den Inhalt Ihrer Web-App mittels Push an Azure App Service zu übermitteln.

## <a name="application-overview"></a>Anwendungsübersicht
In den nächsten Abschnitten werden die folgenden Dateien erstellt. Sie müssen im Stammverzeichnis des Git-Repositorys gespeichert werden.

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a>WSGI-Handler
WSGI ist ein Python-Standard, der in der Spezifikation [PEP 3333](http://www.python.org/dev/peps/pep-3333/) beschrieben wird und der eine Schnittstelle zwischen dem Webserver und Python definiert. Es bietet eine standardisierte Schnittstelle zum Schreiben verschiedener Webanwendungen und Frameworks mit Python. Beliebte Python-Webframeworks nutzen heute WSGI. Azure App Service-Web-Apps bieten Unterstützung für solche Frameworks. Darüber hinaus können fortgeschrittene Benutzer auch ihre eigenen Frameworks erstellen, sofern der benutzerdefinierte Handler die Richtlinien der WSGI-Spezifikationen erfüllt.

Hier ist ein Beispiel für `app.py`, in dem ein benutzerdefinierter Handler definiert wird:

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

Sie können diese Anwendung lokal mit `python app.py` ausführen und dann in Ihrem Webbrowser zu `http://localhost:5555` navigieren.

## <a name="virtual-environment"></a>Virtuelle Umgebung
Obwohl die obige Beispiel-App keine externen Pakete erfordert, ist es wahrscheinlich, dass Ihre Anwendung einige benötigt.

Zur besseren Verwaltung externer Paketabhängigkeiten unterstützt die Azure Git-Bereitstellung die Erstellung virtueller Umgebungen.

Wenn Azure im Stammverzeichnis des Repositorys eine Datei namens "requirements.txt" erkennt, wird automatisch eine virtuelle Umgebung mit dem Namen `env`erstellt. Dies erfolgt nur bei der ersten Bereitstellung oder bei jeder Bereitstellung, nachdem die ausgewählte Python-Laufzeit geändert wurde.

Sie werden wahrscheinlich eine virtuelle Umgebung für die lokale Entwicklung erstellen. Fügen Sie diese jedoch nicht Ihrem Git-Repository hinzu.

## <a name="package-management"></a>Verwalten von Paketen
In "requirements.txt" aufgeführte Paket werden mit Pip in der virtuellen Umgebung automatisch installiert. Dies erfolgt bei jeder Bereitstellung, doch Pip überspringt die Installation, wenn ein Paket bereits installiert ist.

Beispiel für `requirements.txt`:

    azure==0.8.4


## <a name="python-version"></a>Python-Version
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

Beispiel für `runtime.txt`:

    python-2.7


## <a name="webconfig"></a>Web.config
Sie müssen eine "web.config"-Datei erstellen, um anzugeben, wie der Server Anforderungen verarbeiten soll.

Beachten Sie, dass wenn Sie eine "web.x.y.config"-Datei im Repository vorhanden ist, wobei "x.y" der ausgewählten Python-Laufzeit entspricht, dann kopiert Azure automatisch die entsprechende Datei als "web.config".

Die folgenden "web.config"-Beispiele basieren auf einem Proxyskript für eine virtuelle Umgebung, das im nächsten Abschnitt beschrieben wird.  Sie funktionieren mit dem WSGI-Handler aus dem obigen Beispiel für `app.py` .

`web.config` (Beispiel) für Python 2.7:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


`web.config` (Beispiel) für Python 3.4:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Statische Dateien werden für eine bessere Leistung direkt vom Webserver ohne Umweg über Python-Code verarbeitet.

In den obigen Beispielen muss der Speicherort der statischen Dateien auf dem Datenträger mit dem Speicherort in der URL übereinstimmen. Das bedeutet, dass eine Anforderung für `http://pythonapp.azurewebsites.net/static/site.css` der Datei auf dem Datenträger in `\static\site.css` gilt.

`WSGI_ALT_VIRTUALENV_HANDLER` dient zum Angeben des WSGI-Handlers. In den obigen Beispielen, handelt es sich um `app.wsgi_app`, da der Handler eine Funktion namens `wsgi_app` in `app.py` (im Stammordner) ist.

`PYTHONPATH` kann angepasst werden. Wenn Sie jedoch alle Ihre Abhängigkeiten in der virtuellen Umgebung installieren, indem Sie sie in "requirements.txt" angeben, sollten keine Änderungen erforderlich sein.

## <a name="virtual-environment-proxy"></a>Proxy für die virtuelle Umgebung
Das folgende Skript dient zum Abrufen des WSGI-Handlers, Aktivieren der virtuellen Umgebung und Protokollieren von Fehlern. Es ist generisch ausgelegt und soll ohne Änderungen verwendet werden.

Inhalt von `ptvs_virtualenv_proxy.py`:

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject to terms and conditions of the Apache License, Version 2.0. A 
     # copy of the license can be found in the License.html file at the root of this distribution. If 
     # you cannot locate the Apache License, Version 2.0, please send an email to 
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing to be bound 
     # by the terms of the Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors to a log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n')

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')

        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)

        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()

        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))

        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []

        site.main()

        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a>Anpassen der Git-Bereitstellung
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]

## <a name="troubleshooting---package-installation"></a>Problembehandlung - Paketinstallation
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a>Problembehandlung – virtuelle Umgebung
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie im [Python Developer Center](/develop/python/).

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](https://azure.microsoft.com/try/app-service/)sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
> 
> 

## <a name="whats-changed"></a>Änderungen
* Hinweise zu den Änderungen von Websites zum App Service finden Sie unter: [Azure App Service und vorhandene Azure-Dienste](http://go.microsoft.com/fwlink/?LinkId=529714)




<!--HONumber=Feb17_HO3-->


