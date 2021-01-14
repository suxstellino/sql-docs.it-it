---
title: Impostazioni di sottoscrizione e account di condivisione file (Gestione configurazione) | Microsoft Docs
description: Usare la pagina Impostazioni sottoscrizione di Gestione configurazione del server di report per configurare un account di condivisione file per i server di report in modalità nativa e le sottoscrizioni di condivisione file.
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
f1_keywords:
- SQL13.rsconfigtool.subscriptionsettings.F1
ms.assetid: fefa7bdb-b5f2-4db7-b91c-b58869279f3c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 90e585e887e9415a8cf1f9d0104595547f2d1c87
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171983"
---
# <a name="subscription-settings-and-a-file-share-account-report-server-configuration-manager"></a>Impostazioni di sottoscrizione e un account di condivisione file (Gestione configurazione del server di report)
  Usare la pagina **Impostazioni sottoscrizione** di Gestione configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] per configurare un account di condivisione file per i server di report in modalità nativa e le sottoscrizioni con recapito tramite condivisione file. L'account di condivisione file consente di usare un singolo set di credenziali in più sottoscrizioni che recapitano i report a una condivisione file. Quando è necessario modificare le credenziali, è possibile configurare la modifica per l'account di condivisione file e non è necessario aggiornare ogni sottoscrizione.  
  
 Sono disponibili due flussi di lavoro con le sottoscrizioni con recapito tramite condivisione dei file [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
-   A partire dalla versione [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] , l'amministratore di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] può configurare un singolo account di condivisione file, utilizzato per una o più sottoscrizioni. È necessario configurare l'opzione **Specificare un account di condivisione file** e quindi gli utenti selezionano **Usa l'account di condivisione file** nelle singole pagine di configurazione della sottoscrizione.  
  
-   È possibile configurare singole sottoscrizioni con credenziali specifiche per la condivisione file di destinazione.  
  
-   È anche possibile combinare i due approcci e fare in modo che alcune sottoscrizioni con condivisioni file usino l'account di condivisione file centrale, mentre altre sottoscrizioni usano credenziali specifiche.  
  
 [!INCLUDE[applies](../../includes/applies-md.md)] Modalità nativa di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
## <a name="specify-a-file-share-account"></a>Specificare un account di condivisione file  
 Se questa opzione è selezionata, sarà possibile fornire un account da usare per accedere alle condivisioni di file dal server di report. Se si configura l'account di condivisione file, tutti gli utenti possono selezionare l'account per tutte le sottoscrizioni configurate per il recapito dei report a una condivisione file. Se questa opzione non è selezionata, l'account di condivisione file **non** è disponibile in alcuna sottoscrizione.  
  
 Si noti che è necessario verificare che l'account configurato come account di condivisione file abbia le autorizzazioni di lettura e scrittura per qualsiasi condivisione file che verrà usata dagli utenti per il recapito tramite condivisione file.  
  
 L'immagine seguente illustra le opzioni visualizzate agli utenti per le sottoscrizioni configurate per il recapito tramite condivisione file. L'opzione **Usa l'account di condivisione file** è disabilitata se non è stato configurato un account di condivisione file.  
  
 ![Account di condivisione file di Configuration Manager](../../reporting-services/install-windows/media/ssrs-fileshare-account.png "Account di condivisione file di Configuration Manager")  
  
## <a name="prevent-privilege-escalation-or-elevated-privileges"></a>Evitare l'escalation dei privilegi o privilegi elevati  
  
> [!IMPORTANT]
> L'account del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] controlla il recapito delle sottoscrizioni e interagisce con l'account usato per le sottoscrizioni con recapito tramite condivisione file. Le funzionalità di sicurezza Windows non consentono di usare in combinazione 1) l'account del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e 2) l'account usato per gli account di condivisione file. Ad esempio, se un account di sistema operativo predefinito viene usato per l'account di condivisione file, l'account del servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] deve essere un altro account di servizio con autorizzazioni di rappresentazione. Se sono configurati un account di condivisione file e una password espliciti, l'account di condivisione file deve avere il diritto di accesso al computer che esegue il servizio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Se l'account di condivisione file non ha le autorizzazioni necessarie, le sottoscrizioni che usano l'account di condivisione file avranno esito negativo con un messaggio di errore simile al seguente:  
>   
>  `"Failure writing file {file} : An impersonation error occurred using the security context of the current user."`  
  
## <a name="powershell-sample-to-audit-use-of-the-file-share-account"></a>Esempio di PowerShell per controllare l'uso dell'account di condivisione file  
 Eseguire lo script di Windows PowerShell seguente per elencare tutte le sottoscrizioni di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] configurate per l'uso dell' **account di condivisione file**. Sostituire `SERVERNAME` con un valore appropriato per il server di report in uso.  
  
```  
# get all file share subscriptions using the default file share account  
$extensionNameMatch = "Report Server FileShare"  
$extensionSettingMatch = "DEFAULTCREDENTIALS"  
$valueMatch = "True"  
  
# filter for subscriptions that have a given extension setting  
filter script:extensionSettingFilter  
{  
    # subscription must match the extension name  
    if($_.DeliverySettings.Extension -eq $extensionNameMatch)  
    {  
        # locate the extension parameter of interest  
        ForEach($extensionParameter in $_.DeliverySettings.ParameterValues)  
        {  
            # if the setting has the desired value, return the subscription  
            if($extensionParameter.Name -eq $extensionSettingMatch -and $extensionParameter.Value -eq $valueMatch)  
            {  
                $_  
                break  
            }  
        }  
    }  
}  
  
$rs2010 = New-WebServiceProxy -Uri "https:// SERVERNAME/ReportServer/ReportService2010.asmx" -Namespace SSRS.ReportingService2010 -UseDefaultCredential;  
$subscriptions = $rs2010.ListSubscriptions("/");  
  
Write-Host "----- File share subscriptions using the default file share account ----";  
Write-Host "-------------------------------------------------------------------------- ";  
$subscriptions | extensionSettingFilter | select report, owner, status, lastexecuted, description, subscriptionid | format-table -auto  
  
```  
  
 L'output dello script sarà simile al seguente:  
  
 `----- File share subscriptions using the default file share account ----`  
  
 `-----------------------------------------------------------------------------------------------------`  
  
 `Report                    Owner          Status   LastExecuted         SubscriptionID`  
  
 `------------------------  -------------- -------- -------------------- ------------------------------------`  
  
 `Aworks_sales_by_territory DOMAIN\UserName Disabled 10/5/2014 1:04:04 PM e843bc2b-023e-45a3-ba23-22f9dc9a0934`  
  
## <a name="see-also"></a>Vedere anche  
 [Recapito tramite condivisione file in Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md)   
 [Creare e gestire sottoscrizioni per server di report in modalità nativa](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md)
  
  
