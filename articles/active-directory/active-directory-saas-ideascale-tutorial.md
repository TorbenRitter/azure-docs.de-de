---
title: 'Tutorial: Azure Active Directory-Integration mit IdeaScale | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie IdeaScale mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: f5e9f3ae668cc1f70bb6c0b1abc0e6898f4aaa48


---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Tutorial: Azure Active Directory-Integration mit IdeaScale
In diesem Tutorial wird die Integration von Azure und IdeaScale erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein IdeaScale-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie IdeaScale zugewiesen haben, wie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)beschrieben mittels einmaliger Anmeldung bei der Anwendung anmelden.

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für IdeaScale
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-ideascale-tutorial/IC790838.png "Scenario")

## <a name="enabling-the-application-integration-for-ideascale"></a>Aktivieren der Anwendungsintegration für IdeaScale
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für IdeaScale aktivieren.

### <a name="to-enable-the-application-integration-for-ideascale-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für IdeaScale
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-ideascale-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-ideascale-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-ideascale-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-ideascale-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** das Wort **IdeaScale** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-ideascale-tutorial/IC790841.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **IdeaScale** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![IdeaScale](./media/active-directory-saas-ideascale-tutorial/IC790842.png "IdeaScale")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei IdeaScale zu authentifizieren.  
Zum Konfigurieren des einmaligen Anmeldens für IdeaScale müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **IdeaScale** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ideascale-tutorial/IC790843.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei IdeaScale anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ideascale-tutorial/IC790844.png "Configure Single Sign-On")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **IdeaScale-Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei IdeaScale verwendete URL ein (z.B. „*https://company.IdeaScale.com*“), und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-ideascale-tutorial/IC790845.png "Configure App URL")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für IdeaScale** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ideascale-tutorial/IC790846.png "Configure Single Sign-On")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der IdeaScale-Unternehmenswebsite als Administrator an.
6. Navigieren Sie zu **Communityeinstellungen**.
   
   ![Communityeinstellungen](./media/active-directory-saas-ideascale-tutorial/IC790847.png "Community Settings")
7. Navigieren Sie zu **Sicherheit \> Einstellungen für einmaliges Anmelden**.
   
   ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-ideascale-tutorial/IC790848.png "Single Signon Settings")
8. Wählen Sie für **SSO-Typ** die Einstellung **SAML 2.0** aus.
   
   ![SSO-Typ](./media/active-directory-saas-ideascale-tutorial/IC790849.png "Single Signon Type")
9. Führen Sie im Dialogfeld **Einstellungen für einmaliges Anmelden** die folgenden Schritte aus:
   
   ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-ideascale-tutorial/IC790850.png "Single Signon Settings")
   
   1. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für IdeaScale** den Wert der **Entitäts-ID**, und fügen Sie ihn in das Textfeld **SAML IdP Entity ID** ein.
   2. Kopieren Sie den Inhalt der heruntergeladenen Metadatendatei, und fügen Sie ihn in das Textfeld **SAML-IdP-Metadaten** ein.
   3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für IdeaScale** den Wert der **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Logout Success URL** ein.
   4. Klicken Sie auf **Änderungen speichern**.
10. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ideascale-tutorial/IC790851.png "Configure Single Sign-On")
    
    ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei IdeaScale anmelden können, müssen sie in IdeaScale bereitgestellt werden.  
Im Fall von IdeaScale ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich bei der **IdeaScale** -Unternehmenswebsite als Administrator an.
2. Navigieren Sie zu **Communityeinstellungen**.
   
   ![Communityeinstellungen](./media/active-directory-saas-ideascale-tutorial/IC790847.png "Community Settings")
3. Navigieren Sie zu **Grundlegende Einstellungen \> Mitgliederverwaltung**.
4. Klicken Sie auf **Mitglied hinzufügen**.
   
   ![Mitgliederverwaltung](./media/active-directory-saas-ideascale-tutorial/IC790852.png "Member Management")
5. Führen Sie im Abschnitt „Neues Mitglied hinzufügen“ die folgenden Schritte aus:
   
   ![Neues Mitglied hinzufügen](./media/active-directory-saas-ideascale-tutorial/IC790853.png "Add New Member")
   
   1. Geben Sie im Textfeld **E-Mail-Adressen** die E-Mail-Adresse eines gültigen AAD-Benutzerkontos ein, das Sie bereitstellen möchten.
   2. Klicken Sie auf **Änderungen speichern**.
   
   > [!NOTE]
   > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
   > 
   > 

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von IdeaScale-Benutzerkonten oder mithilfe der von IdeaScale bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-ideascale-perform-the-following-steps"></a>So weisen Sie IdeaScale Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **IdeaScale** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-ideascale-tutorial/IC790854.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-ideascale-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


