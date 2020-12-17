---
description: Chiavi di crittografia SSRS - Eseguire il backup e il ripristino delle chiavi di crittografia
title: Eseguire il backup e il ripristino delle chiavi di crittografia di Reporting Services | Microsoft Docs
ms.date: 05/31/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- backing up encryption keys [Reporting Services]
- restoring encryption keys [Reporting Services]
- encryption keys [Reporting Services]
- symmetric keys [Reporting Services]
ms.assetid: 6773d5df-03ef-4781-beb7-9f6825bac979
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: eb35ae0143b2146d5d5d38ebf0f2ab6d51a94049
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439326"
---
# <a name="ssrs-encryption-keys---back-up-and-restore-encryption-keys"></a>Chiavi di crittografia SSRS - Eseguire il backup e il ripristino delle chiavi di crittografia
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Un aspetto importante della configurazione del server di report riguarda la creazione di una copia di backup della chiave simmetrica utilizzata per crittografare le informazioni riservate. La copia di backup della chiave è obbligatoria per molte operazioni di routine e consente di riutilizzare un database del server di report esistente in una nuova installazione.  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  Modalità nativa di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] | Modalità SharePoint di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]  

> [!NOTE]
> L'integrazione di Reporting Services con SharePoint non è più disponibile nelle versioni successive a SQL Server 2016.
  
 È necessario ripristinare la copia di backup della chiave di crittografia se si verifica uno degli eventi seguenti:  
  
-   Modifica del nome account del servizio Windows ReportServer o reimpostazione della password. Quando si usa Gestione configurazione del server di report, il backup della chiave fa parte dell'operazione di modifica del nome dell'account del servizio.  
  
    > [!NOTE]
    > La reimpostazione della password non equivale all'operazione di modifica della password. Per reimpostare la password è necessario disporre dell'autorizzazione per sovrascrivere le informazioni relative all'account nel controller di dominio. Le reimpostazioni delle password vengono eseguite da un amministratore di sistema quando l'utente dimentica o non conosce una password specifica. Il ripristino della chiave simmetrica è richiesto solo si rende necessario reimpostare la password. Il ripristino della chiave simmetrica non è necessario quando la password dell'account viene modificata periodicamente.  
  
-   Ridenominazione del computer o dell'istanza che ospita il server di report (un'istanza del server di report è basata su un nome di istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ).  
  
-   Migrazione dell'installazione del server di report o configurazione di un server di report per utilizzare un diverso database del server di report.  
  
-   Recupero dell'installazione del server di report in seguito a un errore hardware.  
  
 È sufficiente eseguire il backup di una sola copia della chiave simmetrica. Tra un database del server di report e una chiave simmetrica esiste una corrispondenza uno-a-uno. Sebbene sia richiesto il backup di una sola copia, potrebbe essere necessario ripristinare la chiave più volte se si eseguono più server di report in un modello di distribuzione con scalabilità orizzontale. Per ogni istanza del server di report è necessario disporre della relativa copia della chiave simmetrica per bloccare e sbloccare i dati nel database del server di report.

 Durante il backup della chiave simmetrica, la chiave viene scritta nel file specificato dall'utente e quindi viene codificata utilizzando la password fornita dall'utente. La chiave simmetrica non può mai essere archiviata non crittografata, pertanto è necessario fornire una password per codificarla al momento di salvarla su disco. Dopo aver creato il file, è necessario archiviarlo in un percorso protetto e **ricordare la password** per poterlo sbloccare. Per eseguire il backup della chiave simmetrica, è possibile utilizzare gli strumenti riportati di seguito.  
  
 **Modalità nativa:** Gestione configurazione del server di report o l'utilità **rskeymgmt**.  

::: moniker range="=sql-server-2016"
  
 **Modalità SharePoint:** pagine di Amministrazione centrale SharePoint o PowerShell.  
  
##  <a name="backup-sharepoint-mode-report-servers"></a><a name="bkmk_backup_sharepoint"></a> Eseguire il backup di server di report in modalità SharePoint  
 Per i server di report in modalità SharePoint è possibile utilizzare i comandi di PowerShell o le pagine di gestione per l'applicazione di servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Per altre informazioni, vedere la sezione "Gestione chiavi" di [Gestire un'applicazione di servizio SharePoint di Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)  

::: moniker-end
  
##  <a name="back-up-encryption-keys--report-server-configuration-manager-native-mode"></a><a name="bkmk_backup_configuration_manager"></a> Eseguire il backup di chiavi di crittografia - Gestione configurazione del server di report (modalità nativa)  
  
1.  Avviare Gestione configurazione del server di report e quindi connettersi all'istanza del server di report che si vuole configurare.  
  
2.  Fare clic su **Chiavi di crittografia** e quindi su **Backup**.  
  
3.  Utilizzare una password complessa.  
  
4.  Specificare un file in cui includere la chiave archiviata. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] aggiunge l'estensione snk al file. È consigliabile archiviare il file su un disco separato dal server di report.  
  
5.  Selezionare **OK**.  
  
###  <a name="back-up-encryption-keys--rskeymgmt-native-mode"></a><a name="bkmk_backup_rskeymgmt"></a> Eseguire il backup di chiavi di crittografia - rskeymgmt (modalità nativa)  
  
1.  Eseguire l'utilità **rskeymgmt.exe** nel computer locale che ospita il server di report. È necessario usare l'argomento di estrazione **-e** per copiare la chiave e specificare un nome file e una password. Nell'esempio seguente vengono illustrati gli argomenti che è necessario specificare:  
  
    ```  
    rskeymgmt -e -f d:\rsdbkey.snk -p<password>  
    ```  
  
## <a name="restore-encryption-keys"></a>Ripristinare le chiavi di crittografia  
 Durante il ripristino della chiave simmetrica, la chiave simmetrica esistente e archiviata nel database del server di report viene sovrascritta. Durante il ripristino di una chiave di crittografia, una chiave inutilizzabile viene sostituita con una copia salvata in precedenza su disco. A seguito del ripristino delle chiavi di crittografia si verificano le azioni seguenti:  
  
-   La chiave simmetrica viene aperta dal file di backup protetto da password.  
  
-   La chiave simmetrica viene crittografata tramite la chiave pubblica del servizio Windows ReportServer.  
  
-   La chiave simmetrica crittografata viene archiviata nel database del server di report.  
  
-   I dati della chiave simmetrica archiviati in precedenza, ad esempio le informazioni sulla chiave già presenti nel database del server di report a seguito di una distribuzione precedente, vengono eliminati.  
  
 Per ripristinare la chiave di crittografia è necessario possederne una copia su file. È inoltre necessario conoscere la password per l'accesso alla copia archiviata. Il possesso della chiave e della password consente di eseguire lo strumento di configurazione di Reporting Services o l'utilità **rskeymgmt** per ripristinare la chiave. La chiave simmetrica deve essere la stessa che blocca e sblocca i dati crittografati attualmente archiviati nel database del server di report. Se viene ripristinata una copia non valida, il server di report non è in grado di accedere ai dati crittografati attualmente archiviati nel database del server di report. In questo caso, potrebbe essere necessario eliminare tutti i valori crittografati se non è possibile ripristinare una chiave valida. Se per qualche ragione non è possibile ripristinare la chiave di crittografia, ad esempio se non si possiede una copia di backup, è necessario eliminare la chiave esistente e il contenuto crittografato. Per altre informazioni, vedere [Eliminare e ricreare chiavi di crittografia &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-delete-and-re-create-encryption-keys.md). Per altre informazioni sulla creazione di chiavi simmetriche, vedere [Inizializzare un server di report &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-initialize-a-report-server.md).  
  
###  <a name="restore-encryption-keys--report-server-configuration-manager-native-mode"></a><a name="bkmk_restore_configuration_manager"></a> Ripristinare le chiavi di crittografia - Gestione configurazione del server di report (modalità nativa)  
  
1.  Avviare Gestione configurazione del server di report e quindi connettersi all'istanza del server di report che si vuole configurare.  
  
2.  Nella pagina Chiavi di crittografia fare clic su **Ripristina**.  
  
3.  Selezionare il file con estensione snk che contiene la copia di backup.  
  
4.  Digitare la password che sblocca il file.  
  
5.  Selezionare **OK**. 
  
###  <a name="restore-encryption-keys---rskeymgmt-native-mode"></a><a name="bkmk_restore_rskeymgmt"></a> Ripristinare le chiavi di crittografia - rskeymgmt (modalità nativa)  
  
1.  Eseguire l'utilità **rskeymgmt.exe** nel computer locale che ospita il server di report. Usare l'argomento **-a** per ripristinare le chiavi. È necessario fornire un nome file completo e specificare una password. Nell'esempio seguente vengono illustrati gli argomenti che è necessario specificare:  
  
    ```  
    rskeymgmt -a -f d:\rsdbkey.snk -p<password>  
    ```  
  
## <a name="see-also"></a>Vedere anche  
 [Configurare e gestire chiavi di crittografia &#40;Gestione configurazione del server di report&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)  
  
  
