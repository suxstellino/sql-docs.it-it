---
title: Configurare l'account di esecuzione automatica (Gestione configurazione) | Microsoft Docs
description: Reporting Services offre un account speciale da usare per l'elaborazione automatica dei report e per l'invio di richieste di connessione in rete.
ms.date: 12/04/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
helpviewer_keywords:
- no credentials option [Reporting Services]
- security [Reporting Services], unattended report processing
- unattended report processing [Reporting Services]
- credentials [Reporting Services]
- external data sources [Reporting Services]
- accounts [Reporting Services]
- reports [Reporting Services], processing
ms.assetid: 4e50733e-bd8c-4bf6-8379-98b1531bb9ca
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 549a1659bcbcf954a4039a152c5b372a758d0c74
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014831"
---
# <a name="configure-the-unattended-execution-account-report-server-configuration-manager"></a>Configurare l'account di esecuzione automatica (Gestione configurazione del server di report)
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] offre un account speciale da usare per l'elaborazione automatica dei report e per l'invio di richieste di connessione in rete. L'account viene utilizzato nei modi seguenti:  
  
-   Inviare richieste di connessione in rete per i report che utilizzano l'autenticazione del database oppure connettersi a origini dati del report esterne che non richiedono né utilizzano l'autenticazione. Per altre informazioni, vedere [Specificare le credenziali e le informazioni sulla connessione per le origini dati del report](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).

-   Recuperare file di immagine esterni utilizzati nel report. Se si desidera utilizzare un file di immagine che non è accessibile tramite l'accesso anonimo, sarà possibile configurare l'account per l'esecuzione automatica dei report e concedere a tale account l'autorizzazione di accesso al file desiderato.  
  
 L'esecuzione automatica del report si riferisce a qualsiasi processo di esecuzione del report avviato da un evento che può essere sia un evento determinato dalla pianificazione, sia un evento di aggiornamento dei dati, piuttosto che da una richiesta dell'utente. Il server di report utilizza l'account per l'esecuzione automatica del report per accedere al computer che ospita l'origine dei dati esterna. Tale account è necessario perché le credenziali dell'account del servizio del server di report non vengono mai utilizzate per la connessione ad altri computer.  
  
> [!IMPORTANT]  
>  La configurazione di questo account è facoltativa. Se tuttavia si sceglie di non configurarlo, si disporrà di un minor numero di opzioni per la connessione ad alcune origini dei dati e potrebbe risultare impossibile recuperare file di immagine da computer remoti. Se si configura l'account, sarà necessario mantenerlo aggiornato. In particolare, se si consente la scadenza di una password o se si modificano le informazioni sull'account in Active Directory, si verificherà l'errore seguente alla successiva elaborazione di un report: "Accesso non riuscito (rsLogonFailed). Errore di accesso: nome utente sconosciuto o password non valida". La corretta manutenzione dell'account per l'elaborazione automatica dei report è essenziale, anche se non si intende recuperare immagini esterne o inviare richieste di connessione a computer esterni. Se si configura l'account ma successivamente ci si accorge che non viene utilizzato, sarà possibile eliminarlo per evitare di svolgere le attività di manutenzione di routine per gli account.  
  
## <a name="how-to-configure-the-account"></a>Come configurare l'account  
 È necessario utilizzare un account utente di dominio. Affinché possa essere utilizzato per lo scopo previsto, questo account deve essere diverso da quello utilizzato per l'esecuzione del servizio del server di report. Utilizzare un account con autorizzazioni minime (è sufficiente l'accesso in sola lettura con autorizzazioni di connessione di rete) e accesso limitato ai soli computer in cui risiedono le origini dati e le risorse utilizzate dal server di report.  
  
 Per specificare l'account, è possibile usare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o l'utilità **rsconfig** . Il modo più semplice per configurare l'account di esecuzione automatica consiste nell'utilizzare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e specificare le credenziali nella pagina Account di esecuzione.  
  
1.  Avviare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e connettersi all'istanza del server di report da configurare. Per le istruzioni, vedere [Gestione configurazione del server di report &#40;modalità nativa&#41;](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md).  
  
2.  Nella pagina Account di esecuzione selezionare **Specifica account di esecuzione**.  
  
3.  Digitare account e password, digitare nuovamente la password e quindi fare clic su **Applica**.  
  
### <a name="using-rsconfig-utility"></a>Utilizzo dell'utilità RSCONFIG  
 Un altro modo per impostare l'account consiste nell'usare l'utilità **rsconfig** . Per specificare l'account, usare l'argomento **-e** di **rsconfig**. Se si specifica l'argomento **-e** per **rsconfig** , si indica all'utilità di scrivere le informazioni relative all'account nel file di configurazione. Non è necessario specificare un percorso per RSreportserver.config. Per configurare l'account, effettuare le seguenti operazioni:  
  
1.  Creare o selezionare un account di dominio che abbia accesso ai computer e ai server che forniscono dati o servizi a un server di report. È consigliabile utilizzare un account che disponga di autorizzazioni limitate, ad esempio autorizzazioni di sola lettura.  
  
2.  Aprire il prompt dei comandi: nel menu **Start** scegliere **Esegui**, digitare **cmd** e quindi fare clic su **OK**.  
  
3.  Digitare il comando seguente per configurare l'account su un'istanza del server di report locale:  
  
     **rsconfig -e -u\<domain/username> -p\<password>**  
  
 **rsconfig -e** supporta argomenti aggiuntivi. Per altre informazioni sulla sintassi ed esempi di comandi, vedere [Utilità rsconfig &#40;SSRS&#41;](../../reporting-services/tools/rsconfig-utility-ssrs.md).
 
### <a name="how-account-information-is-stored"></a>Modalità di archiviazione delle informazioni sull'account  
 Quando si imposta l'account, le impostazioni seguenti vengono specificate come valori crittografati nel file RSreportserver.config in un'istanza locale o remota del server di report:  
  
```  
<UnattendedExecutionAccount>  
     <UserName></UserName>  
     <Password></Password>  
     <Domain></Domain>  
</UnattendedExecutionAccount>  
```  
  
 Dopo aver impostato i valori, non è possibile decrittografarli per visualizzare i valori in testo normale. Se si digitano i valori in modo errato o si dimenticano i valori specificati, è necessario usare lo strumento Configurazione di Reporting Services oppure eseguire **rsconfig -e** per ricominciare.  
  
## <a name="how-to-use-the-unattended-report-processing-account"></a>Come utilizzare l'account per l'elaborazione automatica dei report  
 Per recuperare file di immagine, il server di report utilizza automaticamente l'account e non è necessario alcun intervento specifico dell'utente. Per usare l'account per connettersi a origini dati esterne che forniscono dati ai report, è necessario specificare l'opzione **Tipo di credenziali** nella pagina delle proprietà dell'origine dati del report o dell'origine dati condivisa:  
  
-   Nel [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] o in un sito di SharePoint, selezionare l'opzione **Credenziali non richieste** .  

> [!NOTE]
> L'integrazione di Reporting Services con SharePoint non è più disponibile nelle versioni successive a SQL Server 2016.
  
 L'account per l'elaborazione automatica dei report viene utilizzato principalmente per la connessione a server esterni e non come account di accesso a server di database. Se si desidera utilizzare le credenziali dell'account per accedere a un database, è necessario specificarle nella stringa di connessione. È possibile specificare **Integrated Security=SSPI** se il server di database supporta la sicurezza integrata di Windows e l'account usato per l'elaborazione automatica dei report è autorizzato a leggere il database. In caso contrario, è necessario immettere il nome e la password dell'utente nella stringa di connessione, in cui verranno visualizzati come testo non crittografato da tutti gli utenti che dispongono dell'autorizzazione necessaria per modificare le proprietà di connessione dell'origine dati.  
  
 È possibile ma non consigliabile utilizzare l'account per l'elaborazione automatica dei report per recuperare dati dopo aver stabilito la connessione. L'account è progettato per funzioni molto specifiche. Se lo si utilizza per il recupero dei dati, non potrà più essere utilizzato per lo scopo a cui è destinato.  
  
## <a name="how-to-maintain-the-unattended-report-processing-account"></a>Come gestire l'account per l'elaborazione automatica dei report  
 Dopo aver definito l'account è necessario garantire che l'account e la relativa password siano sempre aggiornati. Per aggiornare le impostazioni di configurazione in cui sono archiviate le informazioni sull'account, è possibile usare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
1.  Avviare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e connettersi all'istanza del server di report da configurare.  
  
2.  Nella pagina Account di esecuzione verificare che sia selezionata l'opzione **Specifica account di esecuzione** .  
  
3.  Immettere il nuovo account o la nuova password, digitare nuovamente la password e quindi fare clic su **Applica**.  
  
## <a name="how-to-delete-the-unattended-report-processing-account"></a>Come eliminare l'account per l'elaborazione automatica dei report  
 Se l'account non viene utilizzato, sarà possibile eliminarlo per evitare di svolgere le attività di manutenzione di routine per gli account.  
  
1.  Avviare lo strumento di configurazione di [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] e connettersi all'istanza del server di report da configurare.  
  
2.  Nella pagina Account di esecuzione deselezionare **Specifica account di esecuzione**.  
  
3.  Fare clic su **Applica**.  
  
 Le informazioni relative all'account vengono rimosse dal file RSReportServer.config.  
  
## <a name="see-also"></a>Vedere anche  
 [Gestione configurazione del server di report (modalità nativa)](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  
  
  
