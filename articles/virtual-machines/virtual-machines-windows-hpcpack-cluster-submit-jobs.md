---
title: "Übermitteln von Aufträgen an einen HPC Pack-Cluster in Azure | Microsoft Docs"
description: "Enthält Informationen zum Einrichten eines lokalen Computers für die Übermittlung von Aufträgen an einen HPC Pack-Cluster in Azure."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 78f6833c-4aa6-4b3e-be71-97201abb4721
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 10/14/2016
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: ee34a7ebd48879448e126c1c9c46c751e477c406
ms.openlocfilehash: 71c896673706fad3eb215f12893b65af7b697843


---
# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a>Übermitteln von HPC-Aufträgen von einem lokalen Computer an einen in Azure bereitgestellten HPC Pack-Cluster
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Konfigurieren Sie einen lokalen Clientcomputer, um Aufträge an einen [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029)-Cluster in Azure zu übermitteln. In diesem Artikel wird beschrieben, wie Sie einen lokalen Computer mit Clienttools einrichten, um Aufträge per HTTPS an den Cluster in Azure zu übermitteln. Auf diese Weise können mehrere Clusterbenutzer Aufträge an einen cloudbasierten HPC Pack-Cluster übermitteln, ohne eine direkte Verbindung mit der Hauptknoten-VM herstellen oder auf ein Azure-Abonnement zugreifen zu müssen.

![Übermitteln eines Auftrags an einen Cluster in Azure][jobsubmit]

## <a name="prerequisites"></a>Voraussetzungen
* **Auf einer Azure-VM bereitgestellter HPC Pack-Hauptknoten:** Es wird empfohlen, automatisierte Tools wie eine [Azure-Schnellstartvorlage](https://azure.microsoft.com/documentation/templates/) oder ein [Azure PowerShell-Skript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) zu verwenden, um den Hauptknoten und den Cluster bereitzustellen. Sie benötigen den DNS-Namen des Hauptknotens und die Anmeldeinformationen eines Clusteradministrators, um die Schritte in diesem Artikel auszuführen.
* **Clientcomputer:** Sie benötigen einen Windows- oder Windows Server-Clientcomputer, auf dem HPC Pack-Clienthilfsprogramme ausgeführt werden können (siehe [Systemanforderungen](https://technet.microsoft.com/library/dn535781.aspx)). Wenn Sie nur das HPC Pack-Webportal oder die REST-API zum Übermitteln von Aufträgen verwenden möchten, können Sie einen Clientcomputer Ihrer Wahl nutzen.
* **HPC Pack-Installationsmedien:** Für die Installation der HPC Pack-Clienthilfsprogramme ist das kostenlose Installationspaket für die neueste Version von HPC Pack (HPC Pack 2012 R2) im [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024)verfügbar. Stellen Sie sicher, dass Sie die gleiche Version von HPC Pack herunterladen, die auf der Hauptknoten-VM installiert ist.

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a>Schritt 1: Installieren und Konfigurieren der Webkomponenten auf dem Hauptknoten
Um für eine REST-Schnittstelle die Übermittlung von Aufträgen an den Cluster per HTTPS zu ermöglichen, müssen Sie sicherstellen, dass die HPC Pack-Webkomponenten auf dem HPC Pack-Hauptknoten konfiguriert sind. Falls noch nicht geschehen, installieren Sie zuerst die Webkomponenten, indem Sie die Installationsdatei „HpcWebComponents.msi“ ausführen. Anschließend konfigurieren Sie die Komponenten, indem Sie das HPC PowerShell-Skript **Set-HPCWebComponents.ps1**ausführen.

Ausführliche Verfahren finden Sie unter [Installieren der Microsoft HPC Pack-Webkomponenten](http://technet.microsoft.com/library/hh314627.aspx).

> [!TIP]
> Bestimmte Azure-Schnellstartvorlagen für HPC Pack installieren und konfigurieren die Webkomponenten automatisch. Wenn Sie zum Erstellen des Clusters das [HPC Pack-IaaS-Bereitstellungsskript](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) verwenden, können Sie die Webkomponenten optional als Teil der Bereitstellung installieren und konfigurieren.
> 
> 

**So installieren Sie die Webkomponenten**

1. Stellen Sie eine Verbindung mit dem virtuellen Hauptknotencomputer her, indem Sie die Anmeldeinformationen eines Clusteradministrators verwenden.
2. Führen Sie aus dem Setupordner von HPC Pack die Datei „HpcWebComponents.msi“ auf dem Hauptknoten aus.
3. Führen Sie die Schritte im Assistenten aus, um die Webkomponenten zu installieren.

**So konfigurieren Sie die Webkomponenten**

1. Starten Sie HPC PowerShell auf dem Hauptknoten als Administrator.
2. Geben Sie den folgenden Befehl ein, um das Verzeichnis in den Speicherort des Konfigurationsskripts zu ändern:
   
    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. Geben Sie den folgenden Befehl ein, um die REST-Schnittstelle zu konfigurieren und den HPC-Webdienst zu starten:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```
4. Wenn Sie zum Auswählen eines Zertifikats aufgefordert werden, wählen Sie das Zertifikat aus, das dem öffentlichen DNS-Namen des Hauptknotens entspricht. Beispiel: Wenn Sie die Hauptknoten-VM mit dem klassischen Bereitstellungsmodell bereitstellen, hat der Zertifikatname das Format „CN=&lt;*HeadNodeDnsName*&gt;.cloudapp.net“. Bei Verwendung des Resource Manager-Bereitstellungsmodells hat der Zertifikatname das Format „CN=&lt;*HeadNodeDnsName*&gt;.&lt;*Region*&gt;.cloudapp.azure.com“.
   
   > [!NOTE]
   > Sie wählen dieses Zertifikat später aus, wenn Sie Aufträge von einem lokalen Computer an den Hauptknoten übermitteln. Wählen bzw. konfigurieren Sie kein Zertifikat, das dem Computernamen des Hauptknotens in der Active Directory-Domäne entspricht (z.B. CN=*MyHPCHeadNode.HpcAzure.local*).
   > 
   > 
5. Geben Sie den folgenden Befehl ein, um das Webportal für die Auftragsübermittlung zu konfigurieren:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. Nach Abschluss des Skripts beenden und starten Sie den HPC-Auftragsplanerdienst neu, indem Sie die folgenden Befehle eingeben:
   
    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a>Schritt 2: Installieren der HPC Pack-Clienthilfsprogramme auf einem lokalen Computer
Wenn Sie die HPC Pack-Clienthilfsprogramme auf Ihrem Computer installieren möchten, laden Sie die HPC Pack-Setupdateien (vollständige Installation) aus dem [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=328024) herunter. Wenn Sie die Installation starten, wählen Sie die Setupoption für die **HPC Pack-Clienthilfsprogramme** aus.

Um die HPC Pack-Clienttools zum Übermitteln von Aufträgen an den Hauptknoten verwenden zu können, müssen Sie außerdem ein Zertifikat vom Hauptknoten exportieren und auf dem Clientcomputer installieren. Das Zertifikat muss im CER-Format vorliegen.

**So exportieren Sie das Zertifikat aus dem Hauptknoten**

1. Fügen Sie auf dem Hauptknoten das Zertifikate-Snap-In einer Microsoft Management Console für das Konto „Lokaler Computer“ hinzu. Informationen zu den Schritten zum Hinzufügen des Snap-Ins finden Sie unter [Hinzufügen des Zertifikate-Snap-Ins zu einer MMC](https://technet.microsoft.com/library/cc754431.aspx).
2. Erweitern Sie in der Konsolenstruktur **Zertifikate – Lokaler Computer** > **Persönlich**, und klicken Sie dann auf **Zertifikate**.
3. Suchen Sie nach dem Zertifikat, das Sie für die HPC Pack-Webkomponenten in [Schritt 1: Installieren und Konfigurieren der Webkomponenten auf dem Hauptknoten](#step-1:-install-and-configure-the-web-components-on-the-head-node) konfiguriert haben (z.B. „CN=&lt;*HeadNodeDnsName*&gt;.cloudapp.net“).
4. Klicken Sie mit der rechten Maustaste auf das Zertifikat, und klicken Sie dann auf **Alle Aufgaben** > **Exportieren**.
5. Klicken Sie im Zertifikatexport-Assistenten auf **Weiter**, und stellen sicher, dass **Nein, privaten Schlüssel nicht exportieren** ausgewählt ist.
6. Führen Sie im Assistenten die restlichen Schritte zum Exportieren des Zertifikats im Format „DER encoded binary X.509 (.CER)“ aus.

**So importieren Sie das Zertifikat auf dem Clientcomputer**

1. Kopieren Sie das Zertifikat, das Sie vom Hauptknoten exportiert haben, in einen Ordner auf dem Clientcomputer.
2. Führen Sie auf dem Clientcomputer „certmgr.msc“ aus.
3. Erweitern Sie im Zertifikat-Manager **Zertifikate – Aktueller Benutzer** > **Vertrauenswürdige Stammzertifizierungsstellen**, klicken Sie mit der rechten Maustaste auf **Zertifikate**, und klicken Sie dann auf **Alle Aufgaben** > **Importieren**.
4. Klicken Sie im Zertifikatimport-Assistenten auf **Weiter** , und führen Sie die Schritte zum Importieren des Zertifikats aus, das Sie vom Hauptknoten in den Speicher für vertrauenswürdige Stammzertifizierungsstellen exportiert haben.

> [!TIP]
> Unter Umständen wird eine Sicherheitswarnung angezeigt, da die Zertifizierungsstelle auf dem Hauptknoten vom Clientcomputer nicht erkannt wird. Zu Testzwecken können Sie diese Warnung ignorieren und den Zertifikatimport abschließen.
> 
> 

## <a name="step-3-run-test-jobs-on-the-cluster"></a>Schritt 3: Ausführen von Testaufträgen im Cluster
Zum Überprüfen der Konfiguration können Sie versuchen, Aufträge im Cluster in Azure über den lokalen Computer auszuführen. Beispielsweise können Sie zum Senden von Aufträgen an den Cluster HPC Pack-GUI-Tools oder Befehlszeilenbefehle verwenden. Außerdem können Sie zum Übermitteln von Aufträgen auch ein webbasiertes Portal verwenden.

**So führen Sie Befehle zur Auftragsübermittlung auf dem Clientcomputer aus**

1. Starten Sie auf einem Clientcomputer, auf dem die HPC Pack-Clienthilfsprogramme installiert sind, eine Eingabeaufforderung.
2. Geben Sie einen Beispielbefehl ein. Orientieren Sie sich zum Auflisten aller Aufträge für den Cluster abhängig vom vollständigen DNS-Namen des Hauptknotens an den folgenden Befehlen:
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
   
    oder
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```
   
   > [!TIP]
   > Verwenden Sie in der Scheduler-URL den vollständigen DNS-Namen des Hauptknotens,nicht die IP-Adresse. Wenn Sie die IP-Adresse angeben, wird ein Fehler der Art „Das Serverzertifikat muss entweder über eine gültige Vertrauenskette verfügen oder im vertrauenswürdigen Stammspeicher angeordnet sein“ angezeigt.
   > 
   > 
3. Geben Sie bei entsprechender Aufforderung den Benutzernamen (in der Form „&lt;Domänenname&gt;\\&lt;Benutzername&gt;“) und das Kennwort des HPC-Clusteradministrators oder eines anderen Clusterbenutzers ein, den Sie konfiguriert haben. Sie können die Anmeldeinformationen für weitere Auftragsvorgänge auch lokal speichern.
   
    Eine Liste mit Aufträgen wird angezeigt.

**So verwenden Sie den HPC-Auftrags-Manager auf dem Clientcomputer**

1. Falls Sie beim Übermitteln eines Auftrags noch keine Domänenanmeldeinformationen für einen Clusterbenutzer gespeichert haben, können Sie die Anmeldeinformationen in der Anmeldeinformationsverwaltung hinzufügen.
   
    a. Starten Sie auf dem Clientcomputer in der Systemsteuerung die Anmeldeinformationsverwaltung.
   
    b. Klicken Sie auf **Windows-Anmeldeinformationen** > **Generische Anmeldeinformationen hinzufügen**.
   
    c. Geben Sie die Internetadresse (z.B. „https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler“ oder „https://&lt;HeadNodeDnsName&gt;.&lt;Region&gt;.cloudapp.azure.com/HpcScheduler“) und den Benutzernamen (&lt;DomainName&gt;\\&lt;UserName&gt;) und das Kennwort des Clusteradministrators oder eines anderen Clusterbenutzers an, den Sie konfiguriert haben.
2. Starten Sie auf dem Clientcomputer den HPC-Auftrags-Manager.
3. Geben Sie im Dialogfeld **Hauptknoten auswählen** die URL zum Hauptknoten in Azure ein (z.B. „https://&lt;HeadNodeDnsName&gt;.cloudapp.net“ oder „https://&lt;HeadNodeDnsName&gt;.&lt;Region&gt;.cloudapp.azure.com“).
   
    Der HPC-Auftrags-Manager wird geöffnet und enthält eine Liste der Aufträge auf dem Hauptknoten.

**So verwenden Sie das auf dem Hauptknoten ausgeführte Webportal**

1. Starten Sie einen Webbrowser auf dem Clientcomputer, und geben Sie abhängig vom vollständigen DNS-Namen des Hauptknotens eine der folgenden Adressen ein:
   
    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
   
    oder
   
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. Geben Sie im angezeigten Sicherheitsdialogfeld die Domänenanmeldeinformationen des HPC-Clusteradministrators ein. (Sie können auch andere Clusterbenutzer mit unterschiedlichen Rollen hinzufügen. Informationen finden Sie unter [Verwalten von Clusterbenutzern](https://technet.microsoft.com/library/ff919335.aspx).)
   
    Das Webportal wird geöffnet, und die Auftragsliste wird angezeigt.
3. Klicken Sie im Navigationsbereich auf der linken Seite auf **Neuer Auftrag** , um einen Beispielauftrag zu übermitteln, mit dem die Zeichenfolge „Hello World“ aus dem Cluster zurückgegeben wird.
4. Klicken Sie auf der Seite **Neuer Auftrag** unter **Von Übermittlungsseiten** auf **HelloWorld**. Die Seite für die Auftragsübermittlung wird angezeigt.
5. Klicken Sie auf **Senden**. Geben Sie bei Aufforderung die Domänenanmeldeinformationen des HPC-Clusteradministrators ein. Der Auftrag wird übermittelt, und die Auftrags-ID wird auf der Seite **Eigene Aufträge** angezeigt.
6. Klicken Sie zum Anzeigen der Ergebnisse des von Ihnen übermittelten Auftrags auf die Auftrags-ID, und klicken Sie anschließend auf **Aufgaben anzeigen**, um die Befehlsausgabe anzuzeigen (unter **Ausgabe**).

## <a name="next-steps"></a>Nächste Schritte
* Sie können auch mit der [HPC Pack-REST-API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx)Aufträge an den Azure-Cluster übermitteln.
* Wenn Sie Clusteraufträge von einem Linux-Client übermitteln möchten, können Sie sich hierzu das Python-Beispiel im [HPC Pack 2012 R2 SDK und Beispielcode](https://www.microsoft.com/download/details.aspx?id=41633)ansehen.

<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png



<!--HONumber=Nov16_HO3-->


