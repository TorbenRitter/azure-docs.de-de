---
title: 'Azure AD Connect: einmaliges Anmelden | Microsoft-Dokumentation'
description: In diesem Thema erfahren Sie, wie das einmalige Anmelden aus einem lokalen Active Directory (AD) bei einem cloudbasierten Azure Active Directory (Azure AD) und damit verbundenen Diensten funktioniert.
services: active-directory
keywords: "Was ist Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD, SSO, Single Sign-On, einmaliges Anmelden"
documentationcenter: 
author: billmath
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: a268907eea2862ae2d054f30accfd4d771a7d880
ms.openlocfilehash: ae97e66b8fb0992550c5a4f1f8418c5cb7e496b5

---

# <a name="what-is-single-sign-on-sso-preview"></a>Was ist das einmalige Anmelden (Vorschau)?
Einmaliges Anmelden (Single Sign-On, SSO) ist eine Option, die in Azure Active Directory Connect entweder über die [Kennwortsynchronisierung](active-directory-aadconnectsync-implement-password-synchronization.md) oder die [Passthrough-Authentifizierung](active-directory-aadconnect-pass-through-authentication.md) aktiviert werden kann. Wenn diese Option aktiviert ist, müssen Benutzer kein Kennwort, sondern nur ihren Benutzernamen eingeben, um sich bei Azure Active Directory (Azure AD) oder anderen Clouddiensten anzumelden, sofern sie an unternehmenseigenen Computern arbeiten und mit dem Unternehmensnetzwerk verbunden sind.

![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso1.png)

Mit dem einmaligen Anmelden können Benutzer auf vertraute Weise auf cloudbasierte Dienste zugreifen, und die Organisation profitiert von einem sicheren und einfachen Prozess, der keinerlei zusätzlichen lokalen Komponenten erfordert.

Dieses Feature wird über Azure AD Connect aktiviert und funktioniert mit der Kennwortsynchronisierung oder der Passthrough-Authentifizierung und Ihrer lokalen Active Directory-Instanz. Damit Ihre Endbenutzer einmaliges Anmelden in Ihrer Umgebung verwenden können, müssen Sie Folgendes sicherstellen:

- Die Benutzer müssen in die Domäne eingebundene Computer verwenden.
- Es muss eine direkte Verbindung mit einem Domänencontroller bestehen, beispielsweise über das Unternehmensnetzwerk (Kabel- oder Funknetzwerk) oder per Remotezugriff z.B. über eine VPN-Verbindung.
- Definieren Sie die Kerberos-Endpunkte in der Cloud als Teil der Intranetzone des Browsers.

Wenn eine dieser Anforderungen nicht erfüllt ist (wenn sich beispielsweise der Computer nicht im Unternehmensnetzwerk befindet), wird der Benutzer aufgefordert, sein Kennwort einzugeben, genauso, als wäre einmaliges Anmelden nicht aktiviert.

## <a name="supported-clients"></a>Unterstützte Clients
Einmaliges Anmelden ist auf browserbasierten Clients und Office-Clients möglich, die [moderne Authentifizierungsmechanismen](https://aka.ms/modernauthga) auf Computern unterstützen, die eine Kerberos-Authentifizierung ausführen können, beispielsweise auf Windows-Computern. Die folgende Matrix zeigt Informationen zu den browserbasierten Clients unter verschiedenen Betriebssystemen:

| Betriebssystem/Browser |Internet Explorer|Chrome|Firefox|Edge
| --- | --- |--- | --- | --- |
|Windows 10|Ja|Ja|Ja*|Nein
|Windows 8.1|Ja|Ja|Ja*|–
|Windows 8|Ja|Ja|Ja*|N/V
|Windows 7|Ja|Ja|Ja*|–
|Mac|N/V|N/V|N/V|–

\*Erfordert separate Konfiguration.

>[!NOTE]
>Bei Windows 10-basierten Clients wird empfohlen, [Azure AD Join](../active-directory-azureadjoin-overview.md) zu verwenden, um für eine optimale Funktionsweise mit Azure AD zu sorgen.

## <a name="how-single-sign-on-works"></a>So funktioniert das einmalige Anmelden

Wenn Sie einmaliges Anmelden in Azure AD Connect aktivieren, wird in der lokalen Active Directory-Instanz ein Computerkonto namens AZUREADSSOACCT erstellt, und der Kerberos-Entschlüsselungsschlüssel wird sicher in Azure AD freigegeben. Darüber hinaus werden zwei Kerberos-Dienstprinzipalnamen erstellt, um die Cloud-URLs darzustellen, die während der Authentifizierung zwischen Client und Azure AD verwendet werden.

Sobald das Setup abgeschlossen ist, erfolgt die Authentifizierung auf die gleiche Weise wie bei jeder anderen auf der integrierten Windows-Authentifizierung (IWA) basierenden Anwendung. Wenn Sie mit der Funktionsweise von IWA vertraut sind, wissen Sie bereits, wie einmaliges Anmelden mit Azure AD funktioniert. Wenn Ihnen die Funktionsweise nicht bekannt ist, finden Sie hier Informationen zum Prozess für IWA:

![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso2.png)

Zunächst versucht ein Benutzer, auf eine Ressource zuzugreifen, die von Azure AD ausgestellten Token vertraut, wie z.B. SharePoint Online. SharePoint Online leitet den Benutzer dann zur Authentifizierung an Azure AD weiter. Der Benutzer gibt dann seinen Benutzernamen ein, sodass Azure AD ermitteln kann, ob einmaliges Anmelden für die Organisation des Benutzers aktiviert ist. Wenn einmaliges Anmelden für die Organisation aktiviert ist, geschieht Folgendes:

1.    Azure AD fordert über eine „401 – Nicht autorisiert“-Antwort den Client auf, ein Kerberos-Ticket bereitzustellen.
2.    Der Client fordert bei Active Directory ein Ticket für Azure AD an.
3.    Active Directory sucht das Computerkonto, das von Azure AD Connect erstellt wurde, und gibt ein Kerberos-Ticket an den Client zurück, das mit dem Geheimnis des Computerkontos verschlüsselt ist. Das Ticket enthält die Identität des Benutzers, der zurzeit am Computer angemeldet ist.
4.    Der Client sendet das von Active Directory abgerufene Kerberos-Ticket an Azure AD.
5.    Azure AD entschlüsselt das Kerberos-Ticket mit dem zuvor freigegebenen Schlüssel. Dann gibt Azure AD entweder ein Token an den Benutzer zurück oder fordert den Benutzer auf, zusätzliche Identitätsnachweise z.B. per Multi-Factor Authentication bereitzustellen – je nachdem, was die Ressource erfordert.

Einmaliges Anmelden ist eine opportunistische Funktion, d.h., wenn die Funktion aus irgendeinem Grund nicht verfügbar ist, können Benutzer einfach wie üblich ihr Kennwort auf der Anmeldeseite eingeben.

## <a name="enabling-sso-with-pass-through-authentication-or-password-sync"></a>Aktivieren des einmaligen Anmeldens mit Passthrough-Authentifizierung oder Kennwortsynchronisierung
Azure AD Connect stellt einen einfachen Prozess zum Aktivieren des einmaligen Anmeldens mit Passthrough-Authentifizierung oder Kennwortsynchronisierung bereit. Stellen Sie sicher, dass Sie in jeder zu synchronisierenden Gesamtstruktur in einer Domäne über Domänenadministratorrechte verfügen, um die Konfiguration der Kerberos-Dienstprinzipalnamen im Computerkonto zu ermöglichen. Der Benutzername und das Kennwort werden nicht in Azure AD Connect oder Azure AD gespeichert und nur für diesen Vorgang verwendet.

Wählen Sie beim Installieren von Azure AD Connect eine benutzerdefinierte Installation aus, damit Sie einmaliges Anmelden auf der Benutzeranmeldeseite auswählen können. Weitere Informationen finden Sie unter [Benutzerdefinierte Installation von Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso3.png)

Sobald einmaliges Anmelden aktiviert ist, können Sie mit dem Installations-Assistenten fortfahren, bis Sie zur Seite „Einmaliges Anmelden“ gelangen.

![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso4.png)

Geben Sie für jede aufgeführte Gesamtstruktur die entsprechenden Kontodetails ein, und einmaliges Anmelden wird für Ihr Azure-Verzeichnis aktiviert.

>[!NOTE]
>Azure AD Connect muss über Port 9090 (TCP) mit „\*.msappproxy.net“ kommunizieren können, damit einmaliges Anmelden konfiguriert werden kann. Dieser geöffnete Port ist nur während der Konfiguration erforderlich, nicht während der Authentifizierung durch Endbenutzer.

## <a name="ensuring-clients-sign-in-automatically"></a>Sicherstellen der automatischen Anmeldung von Clients
Standardmäßig versuchen Browser nicht, Anmeldeinformationen an Webserver zu senden, sofern die URL nicht als in der Intranetzone befindlich definiert wurde. Im Allgemeinen kann der Browser die richtige Zone anhand der URL ermitteln. Wenn die URL etwa „http://intranet/“ lautet, sendet der Browser automatisch Anmeldeinformationen, weil sie einer URL in der Intranetzone zugeordnet ist. Wenn die URL jedoch einen Punkt enthält, z.B. „http://intranet.contoso.com/“, sendet der Computer nicht automatisch Anmeldeinformationen und behandelt die URL so, als wäre sie eine beliebige Internetwebsite.

Da die für einmaliges Anmelden in Azure AD verwendeten URLs einen Punkt enthalten, müssen sie der Intranetzone des Computers ausdrücklich hinzugefügt werden. Diese Einstellung bewirkt, dass der Browser die Anmeldeinformationen des derzeit angemeldeten Benutzers in Form eines Kerberos-Ticket automatisch an Azure AD sendet. Die einfachste Möglichkeit, die erforderlichen URLs zur Intranetzone hinzuzufügen, ist das Erstellen einer Gruppenrichtlinie in Active Directory.

1.    Öffnen Sie die Gruppenrichtlinien-Verwaltungstools.
2.    Bearbeiten Sie die Gruppenrichtlinie, die für alle Benutzer angewendet wird, beispielsweise die **Standarddomänenrichtlinie**.
3.    Navigieren Sie zu **Benutzerkonfiguration\Administrative Vorlagen\Windows-Komponenten\Internet Explorer\Internetsystemsteuerung\Sicherheitsseite**, und wählen Sie die Option **Liste der Site zu Zonenzuweisungen**.
![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso6.png)  
4.    Aktivieren Sie die Richtlinie, und geben Sie die folgenden Werte bzw. Daten in das Dialogfeld ein.  

        Wert: https://autologon.microsoftazuread-sso.com  
        Data 1  
        Wert: https://aadg.windows.net.nsatc.net  
        Data 1  
5.    Es sollte in etwa wie folgt aussehen:  
![Einmaliges Anmelden](./media/active-directory-aadconnect-sso/sso7.png)

6.    Klicken Sie zweimal nacheinander auf **OK******.

Ihre Benutzer können einmaliges Anmelden jetzt verwenden.

>[!NOTE]
>Standardmäßig verwendet Chrome den gleichen Satz vertrauenswürdiger Website-URLs wie Internet Explorer. Wenn Sie andere Einstellungen für Chrome konfiguriert haben, müssen Sie diese Websites separat aktualisieren.

## <a name="troubleshooting-single-sign-on-issues"></a>Behandeln von Problemen beim einmaligen Anmelden
Es muss unbedingt sichergestellt werden, dass der Client ordnungsgemäß für einmaliges Anmelden konfiguriert ist. Dazu gehört Folgendes:

1.    Sowohl „https://autologon.microsoftazuread-sso.com“ als auch „https://aadg.windows.net.nsatc.net“ sind innerhalb der Intranetzone definiert.
2.    Stellen Sie sicher, dass die Arbeitsstation in die Domäne eingebunden ist.
3.    Stellen Sie sicher, dass der Benutzer mit einem Domänenkonto angemeldet ist.
4.    Stellen Sie sicher, dass der Computer mit dem Unternehmensnetzwerk verbunden ist.
5.    Stellen Sie sicher, dass die Uhrzeit des Computers mit der Uhrzeit von Active Directory und den Domänencontrollern synchronisiert ist. Eine Abweichung von 5 Minuten ist zulässig.
6.    Löschen Sie die vorhandenen Kerberos-Tickets vom Client, indem Sie beispielsweise über eine Befehlszeile den Befehl **klist purge** ausführen.

Wenn die oben genannten Anforderungen erfüllt sind, überprüfen Sie die Konsolenprotokolle des Browsers, um weitere Informationen zu erhalten. Sie finden die Konsolenprotokolle bei den Entwicklertools. Diese Protokolle unterstützen Sie bei der Ermittlung des potenziellen Problems.

## <a name="event-log-entries"></a>Einträge des Ereignisprotokolls
Sofern die Erfolgsüberwachung aktiviert ist, wird jedes Mal, wenn sich ein Benutzer per einmaligem Anmelden anmeldet, ein Eintrag in das Ereignisprotokoll des Domänencontrollers geschrieben. Diese Ereignisse finden Sie in den Ereignisprotokollen für das Sicherheitsereignis 4769 für das Computerkonto **AzureADSSOAcc$**. Mit folgendem Filter können Sie alle Sicherheitsereignisse anzeigen, die dem Computerkonto zugeordnet sind:

```
    <QueryList>
      <Query Id="0" Path="Security">
    <Select Path="Security">*[EventData[Data[@Name='ServiceName'] and (Data='AZUREADSSOACC$')]]</Select>
      </Query>
    </QueryList>
```



<!--HONumber=Feb17_HO2-->


