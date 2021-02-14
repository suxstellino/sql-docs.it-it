---
title: Visualizzare e leggere i file di log del programma di installazione di SQL Server | Microsoft Docs
description: Questo articolo descrive i file di log creati dal programma di installazione di SQL Server. I file di log si trovano in una cartella contrassegnata da data e ora.
ms.custom: ''
ms.date: 09/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- viewing logs
- displaying log files
- Setup [SQL Server], logs
- installation log files [SQL Server]
- installing SQL Server, logs
- errors [SQL Server], Setup
- logs [SQL Server], Setup
ms.assetid: 9d77af64-9084-4375-908a-d90f99535062
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 86e9ebb91c2be1c39652d939ebea3a7574ec0c6c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353804"
---
# <a name="view-and-read-sql-server-setup-log-files"></a>Visualizzare e leggere i file di log del programma di installazione di SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Il programma di installazione di SQL Server crea i file di log in una cartella con data di validità e timestamp entro **\%programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log** per impostazione predefinita, in cui *nnn* sono numeri che corrispondono alla versione di SQL che viene installata. Il formato del nome della cartella dei log con indicazione di data e ora è AAAAMMGG_hhmmss. Se il programma di installazione viene eseguito in modalità automatica i file di log vengono creati nel percorso % temp%\sqlsetup*.log. Tutti i file inclusi nella cartella di log vengono archiviati nel file Log\*.cab nella rispettiva cartella di log.  

   | File           | Path |
   | :------        | :----------------------------- |
   | **Summary.txt**    | \%programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log |
   | **Summary_\<MachineName>\_Date.txt**  | \%programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log\AAAAMMGG_hhmmss |
   | **Detail.txt** | \%programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log\AAAAMMGG_hhmmss|
   | **Datastore** | \%programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log\AAAAMMDD_hhmmss\Datastore
   | **File di log MSI** | \%programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log\YYYYMMDD_hhmmss\\\<Name>.log|
   | **ConfigurationFile.ini** | \%programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log\AAAAMMGG_hhmmss |
   | **SystemConfigurationCheck_Report.htm** | \%programfiles%\Microsoft SQL Server\\*nnn*\Setup Bootstrap\Log\AAAAMMGG_hhmmss |
   | **Per le installazioni automatiche** | %temp%\sqlsetup*.log |


 ![Screenshot che mostra dove trovare il file ConfigurationFiles.ini nella cartella Setup Bootstrap.](media/view-and-read-sql-server-setup-log-files/setup-bootstrap-example.png)

 >[!NOTE]
 > I numeri *nnn* nel percorso corrispondono alla versione di SQL di cui è in corso l'installazione. Nell'immagine precedente è stato installato SQL 2017, pertanto il numero della cartella è 140. Per SQL 2016 la cartella sarebbe 130, mentre per SQL 2014 sarebbe 120.
  
 Il programma di installazione di SQL Server completa tre fasi principali: 
  
1.  Verifica delle regole globali: convalida i requisiti di sistema di base
2.  Aggiornamento di componenti: controlla se sono disponibili aggiornamenti per il supporto in fase di installazione
3.  Azione richiesta dall'utente: consente all'utente di selezionare e personalizzare le funzionalità
  

Questo flusso di lavoro genera un log di riepilogo singolo e un log dettagliato singolo per un'installazione di SQL Server base o due log dettagliati per quando viene installato l'aggiornamento, ad esempio un service pack, con l'installazione base. 
  
Inoltre, ci sono file dell'archivio dati che contengono uno snapshot dello stato di tutti gli oggetti di configurazione registrati dal processo di installazione e contribuiscono a risolvere i problemi relativi agli errori di configurazione. Vengono creati file dump XML per ogni fase dell'esecuzione e tali file vengono salvati nella sottocartella dei log Datastore della cartella dei log con indicazione di data e ora. 

Nelle sezioni seguenti vengono illustrati i file di log del programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="summarytxt-file"></a>File Summary.txt 
  
### <a name="overview"></a>Panoramica  
 Nel file sono indicati i componenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rilevati durante l'installazione, l'ambiente del sistema operativo, i valori dei parametri della riga di comando eventualmente specificati e lo stato globale di ogni MSI/MSP eseguito.
  
 Il log è organizzato nelle sezioni seguenti:
  
-   Riepilogo globale dell'esecuzione.  
-   Proprietà e configurazione del computer in cui è stato eseguito il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] funzionalità del prodotto installate in precedenza nel computer  
-   Descrizione delle proprietà relative alla versione dell'installazione e al pacchetto di installazione.
-   Impostazioni di input al runtime fornite durante l'installazione.  
-   Percorso del file di configurazione.  
-   Dettagli dei risultati di esecuzione.  
-   Regole globali.  
-   Regole specifiche per lo scenario di installazione.  
-   Regole non riuscite.  
-   Percorso del file di report delle regole.


  >[!NOTE]
  > Quando si applicano patch possono essere presenti più sottocartelle (una per ogni istanza di destinazione delle patch e una per le funzionalità condivise) che contengono set di file simili (ad esempio %programfiles%\MicrosoftSQL Server\130\Setup Bootstrap\Log\<YYYYMMDD_HHMM>\MSSQLSERVER). 
  
### <a name="location"></a>Location  
 Il file summary.txt si trova nel percorso %programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\.
  
 Per individuare gli errori nel file del testo di riepilogo, eseguire una ricerca nel file utilizzando le parole chiave "error" o "failed".
  
## <a name="summary_machinename_yyyymmdd_hhmmsstxt-file"></a>File Summary_\<MachineName>_YYYYMMDD_HHMMss.txt
  
### <a name="overview"></a>Panoramica  
 Il file di base summary_engine è simile al file di riepilogo e viene generato durante il flusso di lavoro principale.
  
### <a name="location"></a>Location  
 Il file Summary_\<MachineName>_YYYYMMDD_HHMMss.txt file si trova nel percorso %programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<YYYYMMDD_HHMM>\\.
  
  
## <a name="detailtxt-file"></a>File Detail.txt
  
### <a name="overview"></a>Panoramica
 Il file Detail.txt viene generato per il flusso di lavoro principale, ad esempio installazione o aggiornamento, e fornisce i dettagli dell'esecuzione. I log nel file vengono generati in base all'ora in cui è stata chiamata ogni azione per l'installazione. Il file di testo indica l'ordine in cui sono state eseguite le azioni, nonché le dipendenze delle azioni.  
  
### <a name="location"></a>Location  
 Il file detail.txt si trova in %programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<YYYYMMDD_HHMM>\Detail.txt.  
  
 Se si verifica un errore durante il processo di installazione, l'eccezione o l'errore viene registrato alla fine di questo file. Per trovare gli errori in questo file, esaminare prima la parte finale del file, quindi eseguire una ricerca nel file delle parole chiave "error" o "exception".
    
## <a name="msi-log-files"></a>File di log MSI
  
### <a name="overview"></a>Panoramica  
 I file di log MSI forniscono dettagli sul processo del pacchetto di installazione. Vengono generati da MSIEXEC durante l'installazione del pacchetto specificato.  
  
 Tipi di file di log MSI:
  
-   \<Feature>_\<Architecture>\_\<Interaction>.log   
-   \<Feature>_\<Architecture>\_\<Language>\_\<Interaction>.log   
-   \<Feature>_\<Architecture>\_\<Interaction>\_\<workflow>.log  
  
### <a name="location"></a>Location  
 I file di log MSI si trovano nel percorso %Programmi%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<YYYYMMDD_HHMM>\\<Name\>.log.  
  
 La parte finale del file include un riepilogo dell'esecuzione, in cui sono indicati lo stato riuscito o non riuscito e le proprietà. Per trovare l'errore nel file MSI, cercare "value 3" ed esaminare il testo prima e dopo tale stringa.  
  
## <a name="configurationfileini-file"></a>File ConfigurationFile.ini
  
### <a name="overview"></a>Panoramica  
 Il file di configurazione contiene le impostazioni di input fornite durante l'installazione. Può essere utile riavviare l'installazione senza dover immettere manualmente le impostazioni. Nel file di configurazione non vengono tuttavia salvati le password degli account, i PID e alcuni parametri. Le impostazioni possono essere aggiunte al file o specificate tramite una riga di comando o l'interfaccia utente del programma di installazione. Per altre informazioni, vedere [Installare SQL Server 2016 tramite un file di configurazione](./install-sql-server-using-a-configuration-file.md).  
  
### <a name="location"></a>Location  
 Il file ConfigurationFile.ini si trova nel percorso %programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<YYYYMMDD_HHMM>\\.  
  
## <a name="systemconfigurationcheck_reporthtm-file"></a>File SystemConfigurationCheck_Report.htm
  
### <a name="overview"></a>Panoramica  
 Il report di controllo della configurazione di sistema contiene una breve descrizione di ogni regola eseguita, nonché lo stato di esecuzione.
  
### <a name="location"></a>Location  
Il file SystemConfigurationCheck_Report.htm si trova nel percorso %programfiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<YYYYMMDD_HHMM>\\.

[!INCLUDE[get-help-options](../../includes/paragraph-content/get-help-options.md)]
  
## <a name="see-also"></a>Vedere anche  
 [Installare SQL Server 2019](../../database-engine/install-windows/install-sql-server.md)