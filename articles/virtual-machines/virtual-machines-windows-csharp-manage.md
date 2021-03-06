---
title: Verwalten von virtuellen Computern mithilfe von Azure Resource Manager und C# | Microsoft Docs
description: Verwalten Sie virtuelle Computer mit Azure Resource Manager und C#.
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 93898bad-b861-4359-9a4b-348e1d491822
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: 0782000e87bed0d881be5238c1b91f89a970682c
ms.openlocfilehash: 81706c6c95199b25a07589780d2a27fc0f8dffa7


---
# <a name="manage-azure-virtual-machines-using-azure-resource-manager-and-c"></a>Verwalten von virtuellen Azure-Computern mit Azure Resource Manager und C# #
Die Aufgaben in diesem Artikel zeigen, wie virtuelle Computer verwaltet werden. Dazu gehört z.B. das Starten, das Beenden und das Aktualisieren. Sie müssen über einen virtuellen Computer in einer Ressourcengruppe verfügen, um die Aufgaben in diesem Artikel ausführen zu können.

Zum Ausführen der Anweisungen in diesem Artikel benötigen Sie Folgendes:

* [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
* [Einen Authentifizierungstoken](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="create-a-visual-studio-project-and-install-packages"></a>Erstellen eines Visual Studio-Projekts und Installieren von Paketen
NuGet-Pakete sind die einfachste Möglichkeit, um die Bibliotheken zu installieren, die Sie zum Fertigstellen der Aufgaben in diesem Artikel benötigen. Für diesen Artikel müssen Sie die Azure Active Directory-Authentifizierungsbibliothek und die Computeressourcenanbieter-Bibliothek installieren. Führen Sie folgende Schritte aus, um diese Bibliotheken in Visual Studio abzurufen:

1. Klicken Sie auf **Datei** > **Neu** > **Projekt**.
2. Wählen Sie unter **Vorlagen** > **Visual C#** die Option **Konsolenanwendung** aus, geben Sie Name und Speicherort des Projekts ein, und klicken Sie dann auf **OK**.
3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen und anschließend auf **NuGet-Pakete verwalten**.
4. Geben Sie *Active Directory* in das Suchfeld ein, klicken Sie für das Active Directory-Authentifizierungsbibliothek-Paket auf **Installieren** , und befolgen Sie dann die Anweisungen zum Installieren des Pakets.
5. Wählen Sie am oberen Rand der Seite **Vorabversion einschließen**aus. Geben Sie *Microsoft.Azure.Management.Compute* in das Suchfeld ein, klicken Sie für die .NET-Computebibliotheken auf **Installieren** , und befolgen Sie dann die Anweisungen zum Installieren des Pakets.

Sie können nun die Bibliotheken verwenden, um Ihre virtuellen Computer zu verwalten.

## <a name="set-up-the-project"></a>Einrichten des Projekts
Nachdem nun die Anwendung erstellt und die Bibliotheken installiert wurden, erstellen Sie als Nächstes mithilfe der Anwendungsinformationen ein Token. Das Token wird zum Authentifizieren von Anforderungen an Azure Resource verwendet.

1. Öffnen Sie die Datei „Program.cs“ für das von Ihnen erstellte Projekt, und fügen Sie dann die folgenden using-Anweisungen am Anfang der Datei hinzu:
   
        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.Compute;
        using Microsoft.Azure.Management.Compute.Models;
        using Microsoft.Rest;
2. Fügen Sie der Main-Methode der Program-Klasse Variablen hinzu, um die Namen der Ressourcengruppe, den Namen des virtuellen Computers und Ihre Abonnement-ID anzugeben:
   
        var groupName = "resource group name";
        var vmName = "virtual machine name";  
        var subscriptionId = "subsciption id";
   
    Sie erhalten die Abonnement-ID durch Ausführen von Get-AzureRmSubscription.
3. Um das Token abzurufen, das zum Erstellen der Anmeldeinformationen benötigt wird, fügen Sie der Program-Klasse die folgende Methode hinzu:
   
        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token");
          }
          return token;
        }
   
    Ersetzen Sie {client-id} durch den Bezeichner der Azure Active Directory-Anwendung, {-client-secret} durch den Zugriffsschlüssel der AD-Anwendung und {tenant-id} durch die Mandanten-ID für Ihr Abonnement. Sie erhalten die Mandanten-ID durch Ausführen von Get-AzureRmSubscription. Den Zugriffsschlüssel finden Sie mithilfe des Azure-Portals.
4. Fügen Sie der Main-Methode in „Program.cs“ den folgenden Code hinzu, um die Anmeldeinformationen zu erstellen:
   
        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);
5. Speichern Sie die Datei "Program.cs".

## <a name="display-information-about-a-virtual-machine"></a>Anzeigen von Informationen zu einem virtuellen Computer
1. Fügen Sie der Program-Klasse im Projekt, das Sie zuvor erstellt haben, diese Methode hinzu:
   
        public static async void GetVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Getting information about the virtual machine...");
   
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(
            groupName, 
            vmName, 
            InstanceViewTypes.InstanceView);
   
          Console.WriteLine("hardwareProfile");
          Console.WriteLine("   vmSize: " + vmResult.HardwareProfile.VmSize);
   
          Console.WriteLine("\nstorageProfile");
          Console.WriteLine("  imageReference");
          Console.WriteLine("    publisher: " + vmResult.StorageProfile.ImageReference.Publisher);
          Console.WriteLine("    offer: " + vmResult.StorageProfile.ImageReference.Offer);
          Console.WriteLine("    sku: " + vmResult.StorageProfile.ImageReference.Sku);
          Console.WriteLine("    version: " + vmResult.StorageProfile.ImageReference.Version);
          Console.WriteLine("  osDisk");
          Console.WriteLine("    osType: " + vmResult.StorageProfile.OsDisk.OsType);
          Console.WriteLine("    name: " + vmResult.StorageProfile.OsDisk.Name);
          Console.WriteLine("    createOption: " + vmResult.StorageProfile.OsDisk.CreateOption);
          Console.WriteLine("    uri: " + vmResult.StorageProfile.OsDisk.Vhd.Uri);
          Console.WriteLine("    caching: " + vmResult.StorageProfile.OsDisk.Caching);
   
          Console.WriteLine("\nosProfile");
          Console.WriteLine("  computerName: " + vmResult.OsProfile.ComputerName);
          Console.WriteLine("  adminUsername: " + vmResult.OsProfile.AdminUsername);
          Console.WriteLine("  provisionVMAgent: " + vmResult.OsProfile.WindowsConfiguration.ProvisionVMAgent.Value);
          Console.WriteLine("  enableAutomaticUpdates: " + vmResult.OsProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);
   
          Console.WriteLine("\nnetworkProfile");
          foreach (NetworkInterfaceReference nic in vmResult.NetworkProfile.NetworkInterfaces)
          {
            Console.WriteLine("  networkInterface id: " + nic.Id);
          }
   
          Console.WriteLine("\nvmAgent");
          Console.WriteLine("  vmAgentVersion" + vmResult.InstanceView.VmAgent.VmAgentVersion);
          Console.WriteLine("    statuses");
          foreach (InstanceViewStatus stat in vmResult.InstanceView.VmAgent.Statuses)
          {
            Console.WriteLine("    code: " + stat.Code);
            Console.WriteLine("    level: " + stat.Level);
            Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
            Console.WriteLine("    message: " + stat.Message);
            Console.WriteLine("    time: " + stat.Time);
          }
   
          Console.WriteLine("\ndisks");
          foreach (DiskInstanceView idisk in vmResult.InstanceView.Disks)
          {
            Console.WriteLine("  name: " + idisk.Name);
            Console.WriteLine("  statuses");
            foreach (InstanceViewStatus istat in idisk.Statuses)
            {
              Console.WriteLine("    code: " + istat.Code);
              Console.WriteLine("    level: " + istat.Level);
              Console.WriteLine("    displayStatus: " + istat.DisplayStatus);
              Console.WriteLine("    time: " + istat.Time);
            }
          }
   
          Console.WriteLine("\nVM general status");
          Console.WriteLine("  provisioningStatus: " + vmResult.ProvisioningState);
          Console.WriteLine("  id: " + vmResult.Id);
          Console.WriteLine("  name: " + vmResult.Name);
          Console.WriteLine("  type: " + vmResult.Type);
          Console.WriteLine("  location: " + vmResult.Location);
          Console.WriteLine("\nVM instance status");
          foreach (InstanceViewStatus istat in vmResult.InstanceView.Statuses)
          {
            Console.WriteLine("\n  code: " + istat.Code);
            Console.WriteLine("  level: " + istat.Level);
            Console.WriteLine("  displayStatus: " + istat.DisplayStatus);
          }
   
        }
2. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:
   
        GetVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
3. Speichern Sie die Datei "Program.cs".
4. Klicken Sie in Visual Studio auf **Starten** , und melden Sie sich dann bei Azure AD mit demselben Benutzernamen und Kennwort an, die Sie für Ihr Abonnement verwenden.
   
    Beim Ausführen dieser Methode sollte die Ausgabe ähnlich wie dieses Beispiel aussehen:
   
        Getting information about the virtual machine...
        hardwareProfile
          vmSize: Standard_A0
   
        storageProfile
          imageReference
            publisher: MicrosoftWindowsServer
            offer: WindowsServer
            sku: 2012-R2-Datacenter
            version: latest
          osDisk
            osType: Windows
            name: myosdisk
            createOption: FromImage
            uri: http://store1.blob.core.windows.net/vhds/myosdisk.vhd
            caching: ReadWrite
   
          osProfile
            computerName: vm1
            adminUsername: account1
            provisionVMAgent: True
            enableAutomaticUpdates: True
   
          networkProfile
            networkInterface 
              id: /subscriptions/{subscription-id}
                /resourceGroups/rg1/providers/Microsoft.Network/networkInterfaces/nc1
   
          vmAgent
            vmAgentVersion2.7.1198.766
            statuses
            code: ProvisioningState/succeeded
            level: Info
            displayStatus: Ready
            message: GuestAgent is running and accepting new configurations.
            time: 4/13/2016 8:35:32 PM
   
          disks
            name: myosdisk
            statuses
              code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
              time: 4/13/2016 8:04:36 PM
   
          VM general status
            provisioningStatus: Succeeded
            id: /subscriptions/{subscription-id}
              /resourceGroups/rg1/providers/Microsoft.Compute/virtualMachines/vm1
            name: vm1
            type: Microsoft.Compute/virtualMachines
            location: centralus
   
          VM instance status
            code: ProvisioningState/succeeded
              level: Info
              displayStatus: Provisioning succeeded
            code: PowerState/running
              level: Info
              displayStatus: VM running

## <a name="stop-a-virtual-machine"></a>Beenden eines virtuellen Computers
Sie können einen virtuellen Computer auf zwei Arten beenden. Sie können einen virtuellen Computer beenden und sämtliche Einstellungen beibehalten – dafür werden allerdings weiterhin Gebühren berechnet –, oder Sie können einen virtuellen Computer beenden und die Zuordnung aufheben. Wenn die Zuordnung eines virtuellen Computers aufgehoben wird, wird auch die Zuordnung aller mit diesem Computer verknüpften Ressourcen aufgehoben, und die Abrechnung wird beendet.

1. Kommentieren Sie Code aus, den Sie zuvor der Main-Methode hinzugefügt haben, mit Ausnahme des Codes, der zum Abrufen der Anmeldeinformationen erforderlich ist.
2. Fügen Sie der Program-Klasse die folgende Methode hinzu:
   
        public static async void StopVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Stopping the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.PowerOffAsync(groupName, vmName);
        }
   
    Wenn Sie die Zuordnung des virtuellen Computers aufheben möchten, ändern Sie den Aufruf zum Herunterfahren in folgenden Code:
   
        computeManagementClient.VirtualMachines.Deallocate(groupName, vmName);
3. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:
   
        StopVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
4. Speichern Sie die Datei "Program.cs".
5. Klicken Sie in Visual Studio auf **Starten** , und melden Sie sich dann bei Azure AD mit demselben Benutzernamen und Kennwort an, die Sie für Ihr Abonnement verwenden.
   
    Daraufhin sollte der Status des virtuellen Computers in „Beendet“ geändert werden. Wenn Sie die Methode ausgeführt haben, die „Freigegeben“ aufruft, lautet der Status „Beendet (Zuordnung aufgehoben)“.

## <a name="start-a-virtual-machine"></a>Starten eines virtuellen Computers
1. Kommentieren Sie Code aus, den Sie zuvor der Main-Methode hinzugefügt haben, mit Ausnahme des Codes, der zum Abrufen der Anmeldeinformationen erforderlich ist.
2. Fügen Sie der Program-Klasse die folgende Methode hinzu:
   
        public static async void StartVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Starting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.StartAsync(groupName, vmName);
        }
3. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:
   
        StartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
4. Speichern Sie die Datei "Program.cs".
5. Klicken Sie in Visual Studio auf **Starten** , und melden Sie sich dann bei Azure AD mit demselben Benutzernamen und Kennwort an, die Sie für Ihr Abonnement verwenden.
   
    Daraufhin sollte der Status des virtuellen Computers in „Wird ausgeführt“ geändert werden.

## <a name="restart-a-running-virtual-machine"></a>Neustarten eines ausgeführten virtuellen Computers
1. Kommentieren Sie Code aus, den Sie zuvor der Main-Methode hinzugefügt haben, mit Ausnahme des Codes, der zum Abrufen der Anmeldeinformationen erforderlich ist.
2. Fügen Sie der Program-Klasse die folgende Methode hinzu:
   
        public static async void RestartVirtualMachineAsync(
          TokenCredentials credential,
          string groupName,
          string vmName,
          string subscriptionId)
        {
          Console.WriteLine("Restarting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.RestartAsync(groupName, vmName);
        }
3. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:
   
        RestartVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
4. Speichern Sie die Datei "Program.cs".
5. Klicken Sie in Visual Studio auf **Starten** , und melden Sie sich dann bei Azure AD mit demselben Benutzernamen und Kennwort an, die Sie für Ihr Abonnement verwenden.

## <a name="resize-a-virtual-machine"></a>Ändern der Größe eines virtuellen Computers
Dieses Beispiel zeigt, wie Sie die Größe eines ausgeführten virtuellen Computers ändern können.

1. Kommentieren Sie Code aus, den Sie zuvor der Main-Methode hinzugefügt haben, mit Ausnahme des Codes, der zum Abrufen der Anmeldeinformationen erforderlich ist.
2. Fügen Sie der Program-Klasse die folgende Methode hinzu:
   
        public static async void UpdateVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Updating the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.HardwareProfile.VmSize = "Standard_A1";
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }
3. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:
   
        UpdateVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
4. Speichern Sie die Datei "Program.cs".
5. Klicken Sie in Visual Studio auf **Starten** , und melden Sie sich dann bei Azure AD mit demselben Benutzernamen und Kennwort an, die Sie für Ihr Abonnement verwenden.
   
    Daraufhin sollte die Größe des virtuellen Computers in „Standard_A1“ geändert werden.

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Hinzufügen eines Datenträgers zu einem virtuellen Computer
Dieses Beispiel zeigt, wie Sie einem ausgeführten virtuellen Computer einen Datenträger hinzufügen.

1. Kommentieren Sie Code aus, den Sie zuvor der Main-Methode hinzugefügt haben, mit Ausnahme des Codes, der zum Abrufen der Anmeldeinformationen erforderlich ist.
2. Fügen Sie der Program-Klasse die folgende Methode hinzu:
   
        public static async void AddDataDiskAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Adding the disk to the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          var vmResult = await computeManagementClient.VirtualMachines.GetAsync(groupName, vmName);
          vmResult.StorageProfile.DataDisks.Add(
            new DataDisk
              {
                Lun = 0,
                Name = "mydatadisk1",
                Vhd = new VirtualHardDisk
                  {
                    Uri = "https://mystorage1.blob.core.windows.net/vhds/mydatadisk1.vhd"
                  },
                CreateOption = DiskCreateOptionTypes.Empty,
                DiskSizeGB = 2,
                Caching = CachingTypes.ReadWrite
              });
          await computeManagementClient.VirtualMachines.CreateOrUpdateAsync(groupName, vmName, vmResult);
        }
3. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:
   
        AddDataDiskAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
4. Speichern Sie die Datei "Program.cs".
5. Klicken Sie in Visual Studio auf **Starten** , und melden Sie sich dann bei Azure AD mit demselben Benutzernamen und Kennwort an, die Sie für Ihr Abonnement verwenden.

## <a name="delete-a-virtual-machine"></a>Löschen eines virtuellen Computers
1. Kommentieren Sie Code aus, den Sie zuvor der Main-Methode hinzugefügt haben, mit Ausnahme des Codes, der zum Abrufen der Anmeldeinformationen erforderlich ist.
2. Fügen Sie der Program-Klasse die folgende Methode hinzu:
   
        public static async void DeleteVirtualMachineAsync(
          TokenCredentials credential, 
          string groupName, 
          string vmName, 
          string subscriptionId)
        {
          Console.WriteLine("Deleting the virtual machine...");
          var computeManagementClient = new ComputeManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await computeManagementClient.VirtualMachines.DeleteAsync(groupName, vmName);
        }
3. Fügen Sie der Main-Methode den folgenden Code hinzu, um die gerade hinzugefügte Methode aufzurufen:
   
        DeleteVirtualMachineAsync(
          credential,
          groupName,
          vmName,
          subscriptionId);
        Console.WriteLine("\nPress enter to continue...");
        Console.ReadLine();
4. Speichern Sie die Datei "Program.cs".
5. Klicken Sie in Visual Studio auf **Starten** , und melden Sie sich dann bei Azure AD mit demselben Benutzernamen und Kennwort an, die Sie für Ihr Abonnement verwenden.

## <a name="next-steps"></a>Nächste Schritte
Falls bei der Bereitstellung Probleme aufgetreten sind, können Sie den Artikel [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md) lesen.




<!--HONumber=Jan17_HO2-->


