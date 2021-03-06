---
title: 'Tutorial: Azure Active Directory-Integration mit Zoom | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Zoom mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 0ebdab6c-83a8-4737-a86a-974f37269c31
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/15/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 80668119e273aaaa7d36d2c093fba344ea50e3d8


---
# <a name="tutorial-azure-active-directory-integration-with-zoom"></a>Tutorial: Azure Active Directory-Integration mit Zoom
In diesem Tutorial wird die Integration von Azure und Zoom erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Zoom-Mandanten

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Zoom zugewiesen haben, mittels einmaligen Anmeldens auf der Zoom-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Zoom
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-zoom-tutorial/IC784693.png "Scenario")

## <a name="enabling-the-application-integration-for-zoom"></a>Aktivieren der Anwendungsintegration für Zoom
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Zoom aktivieren.

### <a name="to-enable-the-application-integration-for-zoom-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Zoom:
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-zoom-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-zoom-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-zoom-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-zoom-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** als Suchbegriff **Zoom** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-zoom-tutorial/IC784694.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **Zoom** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Zoom](./media/active-directory-saas-zoom-tutorial/IC784695.png "Zoom")

## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Zoom zu authentifizieren.  
Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei erstellen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Zoom** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zoom-tutorial/IC784696.png "Configure single sign-on")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Zoom anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zoom-tutorial/IC784697.png "Configure single sign-on")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Zoom-Anmelde-URL** die URL im Format *http://unternehmen.zoom.us* ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-zoom-tutorial/IC784698.png "Configure App URL")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Zoom** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zoom-tutorial/IC784699.png "Configure single sign-on")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Zoom-Unternehmenswebsite als Administrator an.
6. Klicken Sie auf die Registerkarte **Einmaliges Anmelden** .
   
   ![Einmaliges Anmelden](./media/active-directory-saas-zoom-tutorial/IC784700.png "Single sign-on")
7. Klicken Sie auf die Registerkarte **Sicherheitskontrollen**, und navigieren Sie dann zu den Einstellungen für **Einmaliges Anmelden**.
8. Führen Sie im Abschnitt „Einmaliges Anmelden“ die folgenden Schritte aus:
   
   ![Einmaliges Anmelden](./media/active-directory-saas-zoom-tutorial/IC784701.png "Single sign-on")
   
   1. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Zoom** den Wert für **Dienst-URL für einmaliges Anmelden**, und fügen Sie ihn in das Textfeld für die **URL der Anmeldeseite** ein.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Zoom** den Wert für **Dienst-URL für einmaliges Abmelden**, und fügen Sie ihn in das Textfeld für die **URL der Abmeldeseite** ein.
   3. Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat.  
      
      > [!TIP]
      > Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)
      > 
      > 
   4. Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Zertifikat des Identitätsanbieters** ein.
   5. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Zoom** den Wert für **Aussteller-URL**, und fügen Sie ihn in das Textfeld **Aussteller** ein.
   6. Klicken Sie auf **Speichern**.
9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Configure single sign-on](./media/active-directory-saas-zoom-tutorial/IC784702.png "Configure single sign-on")

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
Damit sich Azure AD-Benutzer bei Zoom anmelden können, müssen sie in Zoom bereitgestellt werden.  
Im Fall von Zoom ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:
1. Melden Sie sich bei der **Zoom** -Unternehmenswebsite als Administrator an.
2. Klicken Sie auf die Registerkarte **Kontenverwaltung** und anschließend auf **Benutzerverwaltung**.
3. Klicken Sie im Abschnitt „Benutzerverwaltung“ auf **Benutzer hinzufügen**.
   
   ![Benutzerverwaltung](./media/active-directory-saas-zoom-tutorial/IC784703.png "User management")
4. Führen Sie auf der Seite **Benutzer hinzufügen** die folgenden Schritte aus:
   
   ![Hinzufügen von Benutzern](./media/active-directory-saas-zoom-tutorial/IC784704.png "Add users")
   
   1. Wählen Sie als **Benutzertyp** die Option **Basic** aus.
   2. Geben Sie im Textfeld **E-Mails** die E-Mail-Adresse eines gültigen AAD-Benutzerkontos ein, das Sie bereitstellen möchten.
   3. Klicken Sie auf **Hinzufügen**.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Zoom-Benutzerkonten oder mithilfe der von Zoom bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-zoom-perform-the-following-steps"></a>So weisen Sie Zoom Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Zoom** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-zoom-tutorial/IC784705.png "Assign users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-zoom-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


