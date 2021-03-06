


## <a name="attach-an-empty-disk"></a>Anfügen eines leeren Datenträgers
Eine einfachere Methode zum Hinzufügen eines Datenträgers besteht im Anfügen eines leeren Datenträgers, weil Azure die VHD-Datei automatisch erstellt und sie im Speicherkonto ablegt.

1. Klicken Sie auf **Virtuelle Computer**, und wählen Sie dann den betreffenden virtuellen Computer aus.
2. Klicken Sie auf der Befehlsleiste auf **Anfügen**, und klicken Sie dann auf **Leeren Datenträger anfügen**.

    ![Anfügen eines leeren Datenträgers](./media/howto-attach-disk-windows-linux/AttachEmptyDisk.png)

3. Das Dialogfeld **Leeren Datenträger anfügen** wird angezeigt.

    ![Neuen leeren Datenträger anfügen](./media/howto-attach-disk-windows-linux/AttachEmptyDetail.png)

    Führen Sie die folgenden Schritte aus:
    - Übernehmen Sie in **Dateiname**den Standardnamen, oder geben Sie einen anderen Namen für die VHD-Datei ein. Der Datenträger verwendet einen automatisch generierten Namen, selbst wenn Sie einen anderen Namen für die VHD-Datei eingeben.
    - Geben Sie die **Größe (GB)** des Datenträgers ein.
    - Klicken Sie auf das Häkchen, um fortzufahren.

4. Nachdem der Datenträger erstellt und angefügt ist, wird er im Dashboard des virtuellen Computers aufgeführt.
   
   ![Leerer Datenträger erfolgreich angefügt](./media/howto-attach-disk-windows-linux/AttachEmptySuccess.png)

> [!NOTE]
> Nachdem Sie einen Datenträger hinzugefügt haben, müssen Sie sich beim virtuellen Computer anmelden und den Datenträger initialisieren, sodass dieser verwendet werden kann. 

## <a name="how-to-attach-an-existing-disk"></a>Gewusst wie: Anfügen eines vorhandenen Datenträgers
Zum Anfügen eines vorhandenen Datenträgers wird eine VHD-Datei im Speicherkonto benötigt. Verwenden Sie das Cmdlet [Add-AzureVhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) , um die VHD-Datei in das Speicherkonto hochzuladen. Nachdem Sie eine VHD-Datei erstellt und hochgeladen haben, können Sie sie an einen virtuellen Computer anfügen.

1. Klicken Sie auf **Virtuelle Computer**, und wählen Sie dann den betreffenden virtuellen Computer aus.
2. Klicken Sie auf der Befehlsleiste auf **Anfügen**, und wählen Sie anschließend **Datenträger anfügen**.

    ![Datenträger anfügen](./media/howto-attach-disk-windows-linux/AttachExistingDisk.png)

3. Wählen Sie den Datenträger aus, und aktivieren Sie das Kontrollkästchen, um ihn anzufügen.
   
    ![Details zum Datenträger eingeben](./media/howto-attach-disk-windows-linux/AttachExistingDetail.png)
4. Nachdem der Datenträger angefügt ist, wird er im Dashboard des virtuellen Computers aufgeführt.

    ![Datenträger erfolgreich angefügt](./media/howto-attach-disk-windows-linux/AttachExistingSuccess.png)


<!--HONumber=Nov16_HO3-->


