---
title: Tutorial zur StorSimple Virtual Array-Sicherung | Microsoft Docs
description: Beschreibt das Sichern von StorSimple Virtual Array-Freigaben und -Volumes.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: a4f55053-a664-4f7c-ba9d-0cb1fb200ff4
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/07/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: c0e2324a2b2e6294df6e502f2e7a0ae36ff94158
ms.openlocfilehash: 9db591677fb529a72029114db8ad8a8b5d8ab260


---
# <a name="back-up-your-storsimple-virtual-array"></a>Sichern des StorSimple Virtual Array
## <a name="overview"></a>Übersicht
Dieses Tutorial bezieht sich auf das Microsoft Azure StorSimple Virtual Array (auch als „lokales virtuelles StorSimple-Gerät“ oder „virtuelles StorSimple-Gerät“ bezeichnet) mit der Version vom März 2016 (allgemeine Verfügbarkeit) oder höher.

Das StorSimple Virtual Array ist ein lokales virtuelles Cloudspeichergerät, das als Dateiserver oder iSCSI-Server konfiguriert werden kann. Es dient zum Erstellen von Sicherungen, zum Wiederherstellen aus Sicherungen und zum Durchführen eines Gerätefailovers, wenn eine Notfallwiederherstellung erforderlich ist. Wenn als Dateiserver konfiguriert ist, ermöglicht es auch die Wiederherstellung auf Elementebene. Dieses Lernprogramm beschreibt das Erstellen geplanter und manueller Sicherungen Ihres StorSimple Virtual Array mit dem klassischen Azure-Portal oder der StorSimple-Web-UI.

## <a name="back-up-shares-and-volumes"></a>Sichern von Freigaben und Volumes
Sicherungen stellen Zeitpunktschutz zur Verfügung und verbessern die Wiederherstellbarkeit bei gleichzeitiger Minimierung der Wiederherstellungszeiten für Freigaben und Volumes. Für das Sichern einer Freigabe oder eines Volumes auf dem StorSimple-Gerät sind zwei Methoden möglich: **geplant** oder **manuell**. Diese Methoden werden in den folgenden Abschnitten erläutert.

> [!NOTE]
> In dieser Version werden geplante Sicherungen durch eine Standardrichtlinie erstellt, die täglich zu einer bestimmten Uhrzeit ausgeführt wird und alle Freigaben oder Volumes auf dem Gerät sichert. Derzeit können für geplante Sicherungen keine benutzerdefinierten Richtlinien erstellt werden.
> 
> 

## <a name="change-the-backup-schedule"></a>Ändern des Sicherungszeitplans
Ihr virtuelles StorSimple-Gerät verfügt über eine Standard-Sicherungsrichtlinie, die zu einer angegebenen Uhrzeit (22:30) beginnt und alle Freigaben oder Volumes auf dem Gerät einmal pro Tag sichert. Sie können die Zeit ändern, zu der die Sicherung gestartet wird, die Häufigkeit und Vermerkdauer (Anzahl der beizubehaltenden Sicherungen) können nicht geändert werden. Während dieser Sicherungen wird das gesamte virtuelle Gerät gesichert; aus diesem Grund wird empfohlen, diese Sicherungen außerhalb der Spitzenzeiten zu planen.

Führen Sie die folgenden Schritte im [klassischen Azure-Portal](https://manage.windowsazure.com/) aus, um die Standardstartzeit für die Sicherung zu ändern.

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a>So ändern Sie die Startzeit für die Standard-Sicherungsrichtlinie
1. Navigieren Sie zur Registerkarte **Konfiguration** für das Gerät.
2. Legen Sie im Abschnitt **Sicherung** die Startzeit für die tägliche Sicherung fest.
3. Klicken Sie auf **Speichern**.

### <a name="take-a-manual-backup"></a>Erstellen einer manuellen Sicherung
Neben geplanten Sicherungen können Sie jederzeit eine manuelle bedarfsgesteuerte) Sicherung vornehmen.

#### <a name="to-create-a-manual-on-demand-backup"></a>So erstellen Sie eine manuelle (bedarfsgesteuerte) Sicherung
1. Navigieren Sie zu einer der Registerkarten **Freigaben** oder **Volumes**.
2. Klicken Sie unten auf der Seite auf **Alle sichern**. Sie werden aufgefordert, zu bestätigen, dass Sie die Sicherung jetzt ausführen möchten. Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-ova-backup/image3.png) , um mit der Sicherung fortzufahren.
   
    ![Bestätigung der Sicherung](./media/storsimple-ova-backup/image4.png)
   
    Sie werden benachrichtigt, dass ein Sicherungsauftrag gestartet wird.
   
    ![Sicherung wird gestartet](./media/storsimple-ova-backup/image5.png)
   
    Sie werden benachrichtigt, dass der Auftrag erfolgreich erstellt wurde.
   
    ![Sicherungsauftrag erstellt](./media/storsimple-ova-backup/image7.png)
3. Klicken Sie auf **Auftrag anzeigen**, um den Fortschritt des Auftrags nachzuverfolgen.
4. Nachdem der Sicherungsauftrag abgeschlossen wurde, navigieren Sie zur Registerkarte **Sicherungskatalog** . Die abgeschlossene Sicherung sollte angezeigt werden.
   
    ![Sicherung abgeschlossen](./media/storsimple-ova-backup/image8.png)
5. Legen Sie die Filterauswahl auf das entsprechende Gerät, die Sicherungsrichtlinie und den Zeitraum fest, und klicken Sie auf das Häkchensymbol  ![Häkchensymbol](./media/storsimple-ova-backup/image3.png).
   
    Die Sicherung sollte in der Liste der Sicherungssätze enthalten sein, die im Katalog angezeigt wird.

## <a name="view-existing-backups"></a>Anzeigen vorhandener Sicherungen
Führen Sie die folgenden Schritte im klassischen Azure-Portal aus, um die vorhandenen Sicherungen anzuzeigen.

#### <a name="to-view-existing-backups"></a>So zeigen Sie vorhandene Sicherungen an
1. Klicken Sie auf der Seite des StorSimple-Manager-Diensts auf die Registerkarte **Sicherungskatalog** .
2. Wählen Sie wie folgt einen Sicherungssatz aus:
   
   1. Wählen Sie das Gerät aus.
   2. Wählen Sie in der Dropdownliste die Freigabe oder das Volume für die gewünschte Sicherung aus.
   3. Geben Sie den Zeitraum an.
   4. Klicken Sie auf das Häkchensymbol ![](./media/storsimple-ova-backup/image3.png) , um diese Abfrage durchzuführen.
      
      Die der ausgewählten Freigabe bzw. dem ausgewählten Volume zugeordneten Sicherungen sollten in der Liste der Sicherungssätze angezeigt werden.

![video_icon](./media/storsimple-ova-backup/video_icon.png) **Video verfügbar**

In diesem Video wird gezeigt, wie Sie Freigaben erstellen, Freigaben sichern und Daten auf einem StorSimple Virtual Array wiederherstellen.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Use-the-StorSimple-Virtual-Array/player]
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie [StorSimple Virtual Array verwalten](storsimple-ova-web-ui-admin.md).




<!--HONumber=Jan17_HO5-->


