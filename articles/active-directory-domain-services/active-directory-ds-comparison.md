---
title: "Azure AD Domain Services: Vergleich zwischen Azure AD Domain Services und DIY-Domänencontrollern | Microsoft Docs"
description: "Vergleich zwischen Azure Active Directory Domain Services und DIY-Domänencontrollern"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 165249d5-e0e7-4ed1-aa26-91a05a87bdc9
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 1c4045bd9b705ab3e909a06035f27b85635fdf36
ms.openlocfilehash: 3d83a919d8e7bc59bd51e226c56ff2bb42c87955


---
# <a name="how-to-decide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>So entscheiden Sie, ob die Azure AD Domain Services für Ihren Anwendungsfall geeignet sind
Die Azure AD Domain Services ermöglichen es Ihnen, Ihre Workloads in Azure-Infrastrukturdiensten bereitzustellen, ohne sich über die Wartung Ihrer Identitätsinfrastruktur Gedanken machen zu müssen. Dieser verwaltete Dienst unterscheidet sich von einer typischen Windows Server Active Directory-Bereitstellung, die Sie selbst bereitstellen und verwalten. Der Dienst ist für eine einfache Bereitstellung, eine automatisierte Integritätsüberwachung und -bereinigung sowie eine einfache Identitätsstruktur für die Cloud konzipiert. Wir verbessern den Dienst fortlaufend, um Unterstützung für gängige Bereitstellungsszenarios hinzuzufügen.

Um zu entscheiden, ob Sie die Azure AD Domain Services verwenden oder in Azure eine eigene AD-Infrastruktur (Do-It-Yourself) einrichten und verwalten sollten, können Sie folgende Ressourcen heranziehen:

* Sehen Sie sich die Liste der [Azure AD Domain Services-Features](active-directory-ds-features.md)an.
* Informieren Sie sich über die [Bereitstellungsszenarios für die Azure AD Domain Services](active-directory-ds-scenarios.md).
* Und schließlich sollten Sie sich den [Vergleich zwischen Azure AD Domain Services und einer DIY-AD-Option](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure)ansehen.

## <a name="compare-azure-ad-domain-services-to-diy-ad-domain-in-azure"></a>Vergleich zwischen Azure AD Domain Services und einer DYI-AD-Domäne in Azure
Die folgende Tabelle dient als Entscheidungshilfe bei der Auswahl zwischen der Verwendung der Azure AD Domain Services und der Verwaltung einer eigenen AD-Infrastruktur in Azure.

| **Feature** | **Azure AD Domain Services** | **„Do-It-Yourself“ AD in Azure-VMs** |
| --- |:---:|:---:|
| [**Verwalteter Dienst**](active-directory-ds-comparison.md#managed-service) |**&#x2713;** |**&#x2715;** |
| [**Sichere Bereitstellungen**](active-directory-ds-comparison.md#secure-deployments) |**&#x2713;** |Der Administrator muss die Bereitstellung absichern. |
| [**DNS-Server**](active-directory-ds-comparison.md#dns-server) |**&#x2713;** (verwalteter Dienst) |**&#x2713;** |
| [**Domain or Enterprise administrator privileges**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges) |**&#x2715;** |**&#x2713;** |
| [**Domänenbeitritt**](active-directory-ds-comparison.md#domain-join) |**&#x2713;** |**&#x2713;** |
| [**Domänenauthentifizierung mithilfe von NTLM und Kerberos**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos) |**&#x2713;** |**&#x2713;** |
| [**Eingeschränkte Kerberos-Delegierung**](active-directory-ds-comparison.md#kerberos-constrained-delegation)|Ressourcenbasiert|Ressourcen- und kontobasiert|
| [**Benutzerdefinierte OE-Struktur**](active-directory-ds-comparison.md#custom-ou-structure) |**&#x2713;** |**&#x2713;** |
| [**Schemaerweiterungen**](active-directory-ds-comparison.md#schema-extensions) |**&#x2715;** |**&#x2713;** |
| [**AD-Domänen-/Gesamtstrukturvertrauensstellungen**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts) |**&#x2715;** |**&#x2713;** |
| [**LDAP read**](active-directory-ds-comparison.md#ldap-read) |**&#x2713;** |**&#x2713;** |
| [**Sicheres LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap) |**&#x2713;** |**&#x2713;** |
| [**LDAP write**](active-directory-ds-comparison.md#ldap-write) |**&#x2715;** |**&#x2713;** |
| [**Group Policy**](active-directory-ds-comparison.md#group-policy) |Einfach |Vollständig |
| [**Geografisch verteilte Bereitstellungen**](active-directory-ds-comparison.md#geo-dispersed-deployments) |**&#x2715;** |**&#x2713;** |

#### <a name="managed-service"></a>Verwalteter Dienst
Azure AD Domain Services-Domänen werden von Microsoft verwaltet. Sie müssen sich weder um Patches, Updates, Überwachung oder Sicherung noch um die Gewährleistung der Verfügbarkeit Ihrer Domäne kümmern. Diese Verwaltungsaufgaben werden als Dienst von Microsoft Azure für Ihre verwalteten Domänen angeboten.

#### <a name="secure-deployments"></a>Sichere Bereitstellungen
Die verwaltete Domäne wird basierend auf den Microsoft Best Practices für AD-Bereitstellungen zuverlässig geschützt. Diese Best Practices basieren auf der jahrzehntelangen Erfahrung des AD-Produktteams mit der Entwicklung und Unterstützung von AD-Bereitstellungen. Für DIY-Bereitstellungen (Do-It-Yourself) müssen bestimmte Bereitstellungsschritte ausgeführt werden, um Ihre Bereitstellung einzuschränken/zu schützen.

#### <a name="dns-server"></a>DNS-Server
Eine verwaltete Azure AD Domain Services-Domäne umfasst verwaltete DNS-Dienste. Mitglieder der Gruppe „AAD DC Adminstrators“ können das DNS-System in der verwalteten Domäne verwalten. Mitglieder dieser Gruppe erhalten vollständige DNS-Administratorrechte für die verwaltete Domäne. Die DNS-Verwaltung kann mithilfe der im RSAT-Paket (Remoteserver-Verwaltungstools) enthaltenen DNS-Verwaltungskonsole durchgeführt werden.
[Weitere Informationen](active-directory-ds-admin-guide-administer-dns.md)

#### <a name="domain-or-enterprise-administrator-privileges"></a>Berechtigungen als Domänen- oder Unternehmensadministrator
Diese erhöhten Rechte werden in einer verwalteten AAD DS-Domäne nicht bereitgestellt. Anwendungen, die diese erhöhten Rechte für eine Installation/Ausführung benötigen, können in verwalteten Domänen nicht ausgeführt werden. Ein kleinerer Teilsatz der Administratorrechte steht Mitgliedern der Gruppe der delegierten Administratoren namens „AAD DC Administrators“ zur Verfügung. Mitglieder dieser Gruppe erhalten die Berechtigung zum Konfigurieren von DNS, können Gruppenrichtlinien konfigurieren, erhalten Administratorrechte auf in die Domäne eingebundenen Computern usw.

#### <a name="domain-join"></a>Domänenbeitritt
Sie können der verwalteten Domäne in ähnlicher Weise virtuelle Computer hinzufügen wie Sie einer AD-Domäne Computer hinzufügen.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>Domänenauthentifizierung mithilfe von NTLM und Kerberos
Mit den Azure AD Domain Services können Sie Ihre Unternehmensanmeldeinformationen zur Authentifizierung bei der verwalteten Domäne verwenden. Die Anmeldeinformationen werden mit Ihrem Azure AD-Mandanten synchronisiert. Für synchronisierte Mandanten stellt Azure AD Connect sicher, dass Änderungen, die lokal an den Anmeldeinformationen vorgenommen werden, mit Azure AD synchronisiert werden. Bei einem DIY-Domänensetup müssen Sie möglicherweise eine Domänenvertrauensstellung mit einer lokalen Kontogesamtstruktur einrichten, damit Benutzer sich mit ihren Unternehmensanmeldeinformationen authentifizieren können. Alternativ ist möglicherweise die Einrichtung einer AD-Replikation erforderlich, um sicherzustellen, dass Benutzerkennwörter mit Ihren Azure-Domänencontroller-VMs synchronisiert werden.

#### <a name="kerberos-constrained-delegation"></a>Eingeschränkte Kerberos-Delegierung
Sie besitzen in einer durch Active Directory Domain Services verwalteten Domäne keine Domänenadministratorberechtigungen. Deswegen können Sie keine herkömmliche kontobasierte eingeschränkte Kerberos-Delegierung konfigurieren. Die sicherere ressourcenbasierte eingeschränkte Delegierung können Sie dagegen konfigurieren.
[Weitere Informationen](active-directory-ds-enable-kcd.md)

#### <a name="custom-ou-structure"></a>Benutzerdefinierte OE-Struktur
Mitglieder der Gruppe „AAD DC Adminstrators“ können benutzerdefinierte OEs in der verwalteten Domäne erstellen. Benutzer, die benutzerdefinierte OEs erstellen, erhalten über die von ihnen erstellten OEs volle Administratorrechte. 
[Weitere Informationen](active-directory-ds-admin-guide-create-ou.md)

#### <a name="schema-extensions"></a>Schemaerweiterungen
Sie können das Basisschema einer verwalteten Azure AD Domain Services-Domäne nicht erweitern. Daher können Anwendungen, die von Erweiterungen des AD-Schemas abhängen (beispielsweise neue Attribute unter dem Benutzerobjekt) nicht per „Lift and Shift“ in AAD DS-Domänen verschoben werden.

#### <a name="ad-domain-or-forest-trusts"></a>AD-Domänen-/Gesamtstrukturvertrauensstellungen
Verwaltete Domänen können nicht zum Einrichten von Vertrauensstellungen (eingehend/ausgehend) mit anderen Domänen konfiguriert werden. Aus diesem Grund können die Azure AD Domain Services in Szenarios mit Ressourcengesamtstruktur-Bereitstellungen oder in Fällen, in denen Kennwörter nicht mit Azure AD synchronisiert werden sollen, nicht verwendet werden.

#### <a name="ldap-read"></a>LDAP-Lesevorgänge
Die verwaltete Domäne unterstützt LDAP-Leseworkloads. Daher können Sie Anwendungen bereitstellen, die LDAP-Lesevorgänge in der verwalteten Domäne ausführen.

#### <a name="secure-ldap"></a>Sicheres LDAP (LDAPS)
Sie können die Azure AD Domain Services so konfigurieren, dass ein Zugriff über sicheres LDAP auf Ihre verwaltete Domäne möglich ist – auch über das Internet.
[Weitere Informationen](active-directory-ds-admin-guide-configure-secure-ldap.md)

#### <a name="ldap-write"></a>LDAP-Schreibvorgänge
Die verwaltete Domäne ist für Benutzerobjekte schreibgeschützt. Deshalb können Anwendungen, die LDAP-Schreibvorgänge für Attribute des Benutzerobjekts ausführen, nicht in einer verwalteten Domäne verwendet werden. Darüber hinaus können Benutzerkennwörter nicht innerhalb der verwalteten Domäne geändert werden. Das Ändern von Gruppenmitgliedschaften oder Gruppenattributen innerhalb der verwalteten Domäne ist ebenfalls unzulässig. Änderungen hingegen, die (über PowerShell/Azure-Portal) in Azure AD an Benutzerattributen oder -kennwörtern oder im lokalen AD-System durchgeführt werden, werden mit der verwalteten AAD DS-Domäne synchronisiert.

#### <a name="group-policy"></a>Gruppenrichtlinie
Es ist jeweils ein integriertes GPO für die Container „AADDC Computers“ und „AADDC Users“ vorhanden, die zum Konfigurieren von Gruppenrichtlinien angepasst werden können. Mitglieder der Gruppe „AAD DC Administrators“ können auch benutzerdefinierte GPOs erstellen und diese mit bestehenden OEs (auch benutzerdefinierten) verknüpfen.
[Weitere Informationen](active-directory-ds-admin-guide-administer-group-policy.md)

#### <a name="geo-dispersed-deployments"></a>Geografisch verteilte Bereitstellungen
Verwaltete Azure AD Domain Services-Domänen stehen in einem einzigen virtuellen Netzwerk in Azure zur Verfügung. Für Szenarios, die es erforderlich machen, dass Domänencontroller in mehreren Azure-Regionen weltweit zur Verfügung stehen, stellt das Einrichten von Domänencontrollern in Azure-IaaS-VMs möglicherweise die bessere Alternative dar.

## <a name="do-it-yourself-diy-ad-deployment-options"></a>DIY-AD-Bereitstellungsoptionen
Es gibt Anwendungsfälle, bei denen möglicherweise einige der Funktionen einer Windows Server AD-Installation benötigt werden. Erwägen Sie in diesen Fällen eine der folgenden DIY-Optionen (Do-It-Yourself):

* **Eigenständige Clouddomäne:** Sie können eine eigenständige Clouddomäne mithilfe von virtuellen Azure-Computern einrichten, die als Domänencontroller konfiguriert wurden. Diese Infrastruktur kann nicht in Ihre lokale AD-Umgebung integriert werden. Diese Option erfordert einen anderen Satz mit Anmeldeinformationen, um sich bei den VMs in der Cloud anzumelden und diese zu verwalten.
* **Ressourcengesamtstruktur-Bereitstellung:** Sie können unter Verwendung von virtuellen Azure-Computern, die als Domänencontroller konfiguriert sind, eine Domäne in der Ressourcengesamtstruktur-Topologie einrichten. Als Nächstes können Sie eine AD-Vertrauensstellung mit Ihrer lokalen AD-Umgebung konfigurieren. Dann können Sie in die Domäne eingebundene Computer (Azure-VMs) zu dieser Ressourcengesamtstruktur in der Cloud hinzufügen. Die Benutzerauthentifizierung erfolgt über eine VPN- oder ExpressRoute-Verbindung mit Ihrem lokalen Verzeichnis.
* **Ausweitung Ihrer lokalen Domäne auf Azure:** Sie können ein virtuelles Azure-Netzwerk über eine VPN-/ExpressRoute-Verbindung mit Ihrem lokalen Netzwerk verbinden, sodass Sie Azure-VMs in Ihre lokale AD-Umgebung einbinden können. Eine weitere Alternative besteht darin, Replikatdomänencontroller Ihrer lokalen Domäne zu einer VM in Azure heraufzustufen. Anschließend können Sie eine Replikation über eine VPN- oder ExpressRoute-Verbindung mit Ihrem lokalen Verzeichnis einrichten. Durch diese Bereitstellung wird Ihre lokale Domäne effizient auf Azure ausgeweitet.

> [!NOTE]
> Sie stellen möglicherweise fest, dass für Ihre Anwendungsfälle eine DIY-Option besser geeignet ist. Wir würden uns über [Ihr Feedback](active-directory-ds-contact-us.md) freuen, damit wir besser verstehen, welche Features Sie benötigen, um sich in Zukunft für die Azure AD Domain Services zu entscheiden. Dieses Feedback hilft uns dabei, den Dienst weiter zu verbessern und an die Anforderungen Ihrer Bereitstellungen und Anwendungsfälle anzupassen.
> 
> 

Wir haben [Richtlinien für die Bereitstellung von Windows Server Active Directory auf virtuellen Azure-Computern](https://msdn.microsoft.com/library/azure/jj156090.aspx) veröffentlicht, um DIY-Installationen zu vereinfachen.

## <a name="related-content"></a>Verwandte Inhalte
* [Features: Azure AD Domain Services](active-directory-ds-features.md)
* [Bereitstellungsszenarios: Azure AD Domain Services](active-directory-ds-scenarios.md)
* [Richtlinien für die Bereitstellung von Windows Server Active Directory auf virtuellen Azure-Computern](https://msdn.microsoft.com/library/azure/jj156090.aspx)




<!--HONumber=Feb17_HO2-->


