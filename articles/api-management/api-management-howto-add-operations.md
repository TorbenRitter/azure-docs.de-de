---
title: "Hinzufügen von Operationen zu einer API in Azure API Management | Microsoft Docs"
description: "Erfahren Sie, wie Sie Operationen zu einer API in Azure API Management hinzufügen."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 1158a023-1913-4e9c-93de-9164b672f9b3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
translationtype: Human Translation
ms.sourcegitcommit: d8b2f91a4d47513572fc5a56c60524d8338c8df4
ms.openlocfilehash: 4527e27f760a7a0685f92a7108c4e8ff9759d33c


---
# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>Hinzufügen von Operationen zu einer API in Azure API Management
Bevor eine API in API Management verwendet werden kann, müssen Operationen hinzugefügt werden. Diese Anleitung beschreibt, wie Sie unterschiedliche Typen von Operationen zu einer API in API Management hinzufügen können.

## <a name="add-operation"> </a>Hinzufügen einer Operation
Operationen werden einer API im Herausgeberportal hinzugefügt und in diesem konfiguriert. Um auf das Herausgeberportal zuzugreifen, klicken Sie im Azure-Portal für Ihren API Management-Dienst auf **Herausgeberportal**.

![Herausgeberportal][api-management-management-console]

> Falls Sie noch keine API Management-Dienstinstanz erstellt haben, finden Sie weitere Informationen im Abschnitt [Erstellen einer API Management-Dienstinstanz][Create an API Management service instance] im Tutorial [Erste Schritte mit Azure API Management][Get started with Azure API Management].
> 
> 

Wählen Sie die gewünschte API im Herausgeberportal aus, und öffnen Sie die Registerkarte **Vorgänge** . 

![Vorgänge][api-management-operations]

Klicken Sie auf **Operation hinzufügen** , um eine neue Operation zu erstellen. **Neue Operation** wird angezeigt, und die Registerkarte **Signatur** ist standardmäßig ausgewählt.

![Operation hinzufügen][api-management-add-operation]

Wählen Sie das gewünschte **HTTP-Verb** aus der Dropdownliste aus.

![HTTP-Methode][api-management-http-method]

<a name="url-template"></a>

Definieren Sie die URL-Vorlage, indem Sie ein URL-Fragment bestehend aus einem oder mehreren URL-Pfadsegmenten und null oder mehreren Abfrageparametern ein. Die URL-Vorlage wird an die Basis-URL der API angehängt und identifiziert eine einzige HTTP-Operation. Die Vorlage enthält einen oder mehrere variable Bestandteile, die durch geschweifte Klammern gekennzeichnet sind. Diese variablen Teile nennt man Vorlageparameter. Sie enthalten dynamische Werte aus der URL der Anforderung, wenn die Anforderung von der API-Verwaltungsplattform verarbeitet wird.

> Die URL-Vorlage kann Platzhaltermuster enthalten. Wenn Sie z.B. `/*` angeben, werden alle Anforderungen für diese HTTP-Methode an den Enddienst zurückgegeben.

![URL-Vorlage][api-management-url-template]

<a name="rewrite-url-template"></a>

Geben Sie ggf. die Vorlage **URL umschreiben** an. Mit dieser Vorlage können Sie die Standard-URL-Vorlage für die Verarbeitung eingehender Anforderungen im Front-End verwenden und gleichzeitig das Back-End über eine konvertierte URL anhand der Umschreibevorlage aufrufen. Die Vorlageparameter aus der URL-Vorlage sollten in der Umschreibevorlage verwendet werden. Im folgenden Beispiel wird der als Pfadsegment im Webdienst aus dem vorherigen Beispiel codierte Inhaltstyp mithilfe der URL-Vorlagen als Abfrageparameter an eine API übergeben werden kann, die über die API Management-Plattform veröffentlicht wurde.

![URL-Umschreibevorlage][api-management-url-template-rewrite]

Aufrufe an die Operation verwenden das Format `/customers?customerid=ALFKI` und werden `/Customers('ALFKI')` zugeordnet, wenn das Back-End aufgerufen wird.

**Anzeigename** und **Beschreibung** enthalten eine Beschreibung der Operation und dienen als Dokumentation für Entwickler, die diese API im Entwicklerportal verwenden.

![Beschreibung][api-management-description]

Die Operationsbeschreibung kann als Nur-Text oder HTML im Textfeld **Beschreibung** eingegeben werden.

## <a name="operation-caching"> </a>Zwischenspeichern von Operationen
Das Zwischenspeichern von Antworten verbessert die Latenz der API-Consumer und senkt Bandbreitenverbrauch sowie Prozessorlast auf dem HTTP-Webdienst, der die API implementiert. 

Wählen Sie die Registerkarte **Zwischenspeichern**, und markieren Sie das Kontrollkästchen **Aktivieren**, um die Zwischenspeicherung für die Operation schnell und einfach zu aktivieren.

![Caching][api-management-caching-tab]

**Dauer** gibt den Zeitraum an, für den eine Operationsantwort im Cache verbleibt. Der Standardwert beträgt 3600 Sekunden oder 1 Stunde.

Cacheschlüssel werden zur Unterscheidung von Antworten verwendet, und die Antwort für jeden einzelnen Cacheschlüssel erhält einen eigenen Wert im Cache. Geben Sie optional Abfrageparameter und/oder HTTP-Header in die Textfelder **Nach Abfrageparametern variieren** bzw. **Nach Headern variieren** ein, die bei der Berechnung der Cacheschlüsselwerte verwendet werden sollen. Wenn Sie keine Parameter angeben, werden die komplette Anforderungs-URL und die folgenden HTTP-Headerwerte beim Generieren der Cacheschlüssel verwendet: **Accept** und **Accept-Charset**.

> Weitere Informationen zum Zwischenspeichern und zu Richtlinien für das Zwischenspeichern finden Sie unter [Zwischenspeichern von Operationsergebnissen in Azure API Management][How to cache operation results in Azure API Management].
> 
> 

## <a name="request-parameters"> </a>Anforderungsparameter
Operationsparameter werden auf der Registerkarte "Parameter" verwaltet. Die in der **URL-Vorlage** auf der Registerkarte **Signatur** angegebenen Parameter werden automatisch hinzugefügt und können nur durch Bearbeiten der URL-Vorlage geändert werden. Zusätzliche Parameter können manuell eingegeben werden.

Klicken Sie auf **Abfrageparameter hinzufügen** und geben Sie die folgenden Informationen ein, um einen neuen Abfrageparameter zu hinzuzufügen:

* **Name** – Parametername.
* **Beschreibung** – eine kurze Beschreibung des Parameters (optional).
* **Typ** – Parametertyp aus einer Dropdownliste ausgewählt.
* **Werte** – Werte, die dieser Parameter annehmen kann. Einer der Werte kann als Standard markiert werden (optional).
* **Erforderlich** – Markieren Sie dieses Kontrollkästchen, um diesen Parameter obligatorisch zu machen. 

![Anforderungsparameter][api-management-request-parameters]

## <a name="request-body"> </a>Anforderungstext
Wenn die Operation einen Anforderungstext erlaubt und benötigt (z. B. PUT, POST), dann können Sie ein Beispiel in allen unterstützten Darstellungsformaten angeben (z. B. JSON, XML). 

> Der Anforderungstext wird nur zu Dokumentationszwecken verwendet und wird nicht geprüft.
> 
> 

Wechseln Sie zur Registerkarte **Text** , um einen Anforderungstext einzugeben.

Klicken Sie auf **Darstellung hinzufügen**, geben Sie den gewünschten Inhaltstyp ein (z.B. „application/json“), und wählen Sie den Typ in der Dropdownliste aus. Fügen Sie dann den gewünschten Beispielanforderungstext für das jeweilige Format in das Textfeld ein. 

![Anforderungstext][api-management-request-body]

Neben den Darstellungen können Sie auch eine optionale **Beschreibung** in Textform in das Textfeld eingeben.

## <a name="responses"> </a>Antworten
Sie sollten nach Möglichkeit Beispielantworten für alle Statuscodes angeben, die die Operation zurückgeben kann. Jeder Statuscode kann mehr als einen Antworttext existieren, nämlich einen pro unterstütztem Inhaltstyp. 

Klicken Sie zum Hinzufügen einer Antwort auf **Hinzufügen**, und beginnen Sie mit der Eingabe des gewünschten Statuscodes. Dieses Beispiel verwendet den Statuscode **200 OK**. Sobald der Code in der Dropdownliste angezeigt wird, können Sie ihn auswählen. Daraufhin wird der Antwortcode angelegt und zu Ihrer Operation hinzugefügt.

![Antwortcode][api-management-response-code]

Klicken Sie auf **Darstellung hinzufügen**, beginnen Sie mit der Eingabe des gewünschten Inhaltstyps (z.B. „application/json“), und wählen Sie den Typ in der Dropdownliste aus.

![Inhaltstyp des Texts][api-management-response-body-content-type]

Fügen Sie den Beispiel-Antworttext für das entsprechende Format in das Textfeld ein. 

![Antworttext][api-management-response-body]

Fügen Sie bei Bedarf eine optionale Beschreibung in das Textfeld **Beschreibung** ein.

Wenn Sie die Operation konfiguriert haben, klicken Sie auf **Speichern**.

## <a name="next-steps"> </a>Nächste Schritte
Nachdem Sie die Operationen zur API hinzugefügt haben, können Sie die API zu einem Produkt zuordnen und veröffentlichen, sodass Entwickler die Operationen aufrufen können.

* [Erstellen und Veröffentlichen von Produkten][How to create and publish a product]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to cache operation results in Azure API Management]: api-management-howto-cache.md



<!--HONumber=Dec16_HO2-->


