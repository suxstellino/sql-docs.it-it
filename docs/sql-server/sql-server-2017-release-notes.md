---
title: Note sulla versione di SQL Server 2017 | Microsoft Docs
description: Questo articolo descrive le limitazioni e i problemi relativi a SQL Server 2017 e fornisce collegamenti a informazioni correlate.
ms.custom: ''
ms.date: 11/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
ms.assetid: 13942af8-5a40-4cef-80f5-918386767a47
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = sql-server-2017
ms.openlocfilehash: cd13f9fea3ade8f0bcb92e77470ab74f40d69ae2
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98167703"
---
# <a name="sql-server-2017-release-notes"></a>Note sulla versione di SQL Server 2017
[!INCLUDE[SQL Server 2017](../includes/applies-to-version/sqlserver2017.md)]
Questo articolo descrive le limitazioni e i problemi relativi a SQL Server 2017. Per informazioni correlate, vedere:
- [Novità di SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md)
- [Note sulla versione di SQL Server in Linux](../linux/sql-server-linux-release-notes.md)
- [Aggiornamenti cumulativi di SQL Server 2017](https://aka.ms/sql2017cu) per informazioni sulla versione di aggiornamento cumulativo più recente

**Prova SQL Server 2016;**
- [![Download da Evaluation Center](../includes/media/download2.png)](https://go.microsoft.com/fwlink/?LinkID=829477) [Scaricare SQL Server 2017](https://go.microsoft.com/fwlink/?LinkID=829477)
- [![Creare una macchina virtuale](../includes/media/azure-vm.png)](https://azure.microsoft.com/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm) [Avviare una macchina virtuale con SQL Server 2017](https://azure.microsoft.com/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm)

> [!NOTE]
> L'anteprima di SQL Server 2019 è ora disponibile. Per altre informazioni, vedere [Novità di SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15&preserve-view=true).

## <a name="sql-server-2017---general-availability-release-october-2017"></a>SQL Server 2017 - versione di disponibilità generale (ottobre 2017)
### <a name="database-engine"></a>Motore di database

- **Problema e impatto per i clienti:** dopo l'aggiornamento è possibile che la condivisione di rete FILESTREAM esistente non sia più disponibile.

- **Soluzione alternativa:** riavviare il computer e verificare se la condivisione di rete FILESTREAM è disponibile. Se la condivisione non è ancora disponibile, eseguire la procedura seguente:

    1. In Gestione configurazione SQL Server fare clic con il pulsante destro del mouse sull'istanza di SQL Server e scegliere **Proprietà**. 
    2. Nella scheda **FILESTREAM** deselezionare **Abilita FILESTREAM per l'accesso tramite il flusso di I/O dei file** e quindi fare clic su **Applica**.
    3. Selezionare di nuovo **Abilita FILESTREAM per l'accesso tramite il flusso di I/O dei file** con il nome di condivisione originale e fare clic su **Applica**.

### <a name="master-data-services-mds"></a>Master Data Services (MDS)
- **Problema e impatto per i clienti:**  nella pagina delle autorizzazioni utente, quando si concede l'autorizzazione a livello radice nella visualizzazione albero entità, viene visualizzato l'errore seguente: `"The model permission cannot be saved. The object guid is not valid"`

- **Soluzione alternativa:** 
  - Concedere l'autorizzazione per i nodi figlio nella visualizzazione albero anziché a livello radice.

### <a name="analysis-services"></a>Analysis Services
- **Problema e impatto per i clienti:** i connettori dati per le origini seguenti non sono ancora disponibili per i modelli tabulari al livello di compatibilità 1400.
  - Amazon Redshift
  - IBM Netezza
  - Impala
- **Soluzione alternativa:** No.   

- **Problema e impatto per i clienti:** i modelli di query diretta al livello di compatibilità 1400 con prospettive possono avere esito negativo nell'esecuzione di query o nell'individuazione dei metadati.
- **Soluzione alternativa:** rimuovere le prospettive ed eseguire di nuovo la distribuzione.

### <a name="tools"></a>Strumenti
- **Problema e impatto per i clienti:** l'esecuzione *DReplay* ha esito negativo con il messaggio seguente: "Errore DReplay Si è verificato un errore imprevisto".
- **Soluzione alternativa:** No.

![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-release-candidate-rc2---august-2017"></a>SQL Server 2017 Release Candidate (RC2, agosto 2017)
Non sono disponibili note sulla versione di SQL Server in Windows per questa versione. Vedere [Note sulla versione di SQL Server in Linux](../linux/sql-server-linux-release-notes.md).


![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-release-candidate-rc1---july-2017"></a>Versione finale candidata di SQL Server 2017 (RC1 - luglio 2017)
### <a name="sql-server-integration-services-ssis-rc1---july-2017"></a>SQL Server Integration Services (SSIS) (RC1 - luglio 2017)
- **Problema e impatto per i clienti:** Il parametro *runincluster* della stored procedure **[catalog].[create_execution]** è stato rinominato in *runinscaleout* per coerenza e leggibilità.
- **Soluzione alternativa:** Se esistono script che eseguono pacchetti in Scale Out, è necessario modificare il nome del parametro da *runincluster* a *runinscaleout* affinché gli script possano essere usati nella versione RC1.

- **Problema e impatto per i clienti:** In SQL Server Management Studio (SSMS) 17.1 e nelle versioni precedenti non è possibile eseguire i pacchetti in Scale Out nella versione RC1. Il messaggio di errore è: " *\@runincluster* non è un parametro per la procedura **create_execution**." Questo problema viene risolto nella versione 17.2 di SSMS. La versione 17.2 e le versioni successive di SSMS supportano il nuovo nome di parametro ed eseguono i pacchetti in Scale Out. 
- **Soluzione alternativa:** fino a quando non sarà disponibile SSMS versione 17.2:
  1. Usare la versione esistente di SSMS per generare lo script di esecuzione del pacchetto.
  2. Modificare il nome del parametro *runincluster* in *runinscaleout* nello script.
  3. Eseguire lo script.

![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-ctp-21-may--2017"></a>SQL Server 2017 CTP 2.1 (maggio 2017)
### <a name="documentation-ctp-21"></a>Documentazione (CTP 2.1)
- **Problema e impatto per i clienti:** la documentazione per [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] è limitata e il contenuto è incluso nel set di documentazione di [!INCLUDE[ssSQL15_md](../includes/sssql16-md.md)].  Il contenuto di articoli specifici di [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] è indicato con **Si applica a**. 
- **Problema e impatto per i clienti:** non è disponibile contenuto offline per [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)].

### <a name="sql-server-reporting-services-ctp-21"></a>SQL Server Reporting Services (CTP 2.1)

- **Problema e impatto per i clienti:** se SQL Server Reporting Services e il server di report di Power BI sono presenti nello stesso computer e si disinstalla uno dei due, non è possibile connettersi al server di report rimanente con Gestione configurazione del server di report.
- **Soluzione alternativa:** per risolvere questo problema, seguire questa procedura dopo aver disinstallato uno dei server.

    1. Avviare un prompt dei comandi in modalità amministratore.
    2. Passare alla directory in cui è installato il server di report rimanente.

        *Percorso predefinito per il Server di report di Power BI: C:\Programmi\Microsoft Power BI Report Server*

        *Percorso predefinito per SQL Server Reporting Services: C:\Programmi\Microsoft SQL Server Reporting Services*

    3. Passare quindi alla cartella successiva, vale a dire *SSRS* o *PBIRS* a seconda del server di report rimanente.
    4. Passare alla cartella WMI.
    5. Eseguire il comando seguente:

        ```console
        regsvr32 /i ReportingServicesWMIProvider.dll
        ```

        È possibile ignorare l'errore seguente.

        ```
        The module "ReportingServicesWMIProvider.dll" was loaded but the entry-point DLLInstall was not found. Make sure that "ReportingServicesWMIProvider.dll" is a valid DLL or OCX file and then try again.
        ```

### <a name="tsqllanguageservicemsi-ctp-21"></a>TSqlLanguageService.msi (CTP 2.1)

- **Problema e impatto per i clienti:** dopo l'installazione in un computer in cui è installata una versione del 2016 di *TSqlLanguageService.msi* (mediante il programma di installazione di SQL Server o come ridistribuibile autonomo), vengono rimosse le versioni 13.* (SQL 2016) di *Microsoft.SqlServer.Management.SqlParser.dll* e *Microsoft.SqlServer.Management.SystemMetadataProvider.dll*. Qualsiasi applicazione con una dipendenza dalle versioni 2016 di questi assembly smette di funzionare e genera un errore simile a: *Errore: Impossibile caricare il file o l'assembly 'Microsoft.SqlServer.Management.SqlParser, Version=13.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91' o una delle relative dipendenze. Impossibile trovare il file specificato.*

   Inoltre, eventuali tentativi di reinstallare una versione 2016 di TSqlLanguageService.msi avranno esito negativo con il messaggio: *Installazione di Servizio del linguaggio T-SQL di Microsoft SQL Server 2016 non riuscita. Nel computer è già presente una versione successiva*.

- **Soluzione alternativa** Per risolvere questo problema e correggere un'applicazione che dipende dalla versione v13 degli assembly, seguire questi passaggi:

   1. Andare a **Installazione applicazioni**
   2. Cercare *Servizio del linguaggio T-SQL di Microsoft SQL Server 2019 CTP2.1*, selezionarlo facendo clic con il pulsante destro del mouse e selezionare **Disinstalla**.
   3. Dopo che il componente è stato rimosso, riparare l'applicazione danneggiata o reinstallare la versione appropriata di *TSqlLanguageService.MSI*.

   Questa soluzione alternativa rimuove la versione 14 di tali assembly, quindi tutte le applicazioni che dipendono dalle versioni 14 non funzioneranno più. Se sono necessari tali assembly, occorre eseguire un'installazione separata senza installazioni side-by-side del 2016.

![horizontal_bar](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-ctp-20-april--2017"></a>SQL Server 2017 CTP 2.0 (aprile 2017)
### <a name="documentation-ctp-20"></a>Documentazione (CTP 2.0)
- **Problema e impatto per i clienti:** la documentazione per [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] è limitata e il contenuto è incluso nel set di documentazione di [!INCLUDE[ssSQL15_md](../includes/sssql16-md.md)].  Il contenuto di articoli specifici di [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] è indicato con **Si applica a**. 
- **Problema e impatto per i clienti:** non è disponibile contenuto offline per [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)].

### <a name="always-on-availability-groups"></a>Gruppi di disponibilità AlwaysOn

- **Problema e impatto per i clienti:** si verifica un arresto anomalo di un'istanza di SQL Server che ospita una replica secondaria del gruppo di disponibilità se la versione principale di SQL Server è di livello inferiore rispetto all'istanza che ospita la replica primaria. Ciò influisce sugli aggiornamenti di tutte le versioni supportate di SQL Server che ospitano i gruppi di disponibilità per SQL Server [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] CTP 2.0. Il problema si verifica nelle condizioni seguenti. 

> 1. L'utente effettua l'aggiornamento dell'istanza di SQL Server che ospita la replica secondaria, in conformità con le [procedure consigliate](../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md).
> 2. Dopo l'aggiornamento, si verifica un failover e la replica secondaria appena aggiornata diventa primaria prima che venga completato l'aggiornamento di tutte le repliche secondarie nel gruppo di disponibilità. La vecchia replica primaria è ora una replica secondaria, ovvero una versione inferiore rispetto a quella primaria.
> 3. Il gruppo di disponibilità è in una configurazione non supportata e tutte le repliche secondarie rimanenti potrebbero essere esposte a un arresto anomalo del sistema. 

- **Soluzione alternativa** Connettersi all'istanza di SQL Server che ospita la nuova replica primaria e rimuovere la replica secondaria errata dalla configurazione.

   ```sql
   ALTER AVAILABILITY GROUP agName REMOVE REPLICA ON NODE instanceName;
   ```

   Viene recuperata l'istanza di SQL Server che ospitava la replica secondaria.

## <a name="more-information"></a>Ulteriori informazioni
- [note sulla versione di SQL Server Reporting Services](../reporting-services/release-notes-reporting-services.md).
- [Problemi noti di Machine Learning Services](../machine-learning/troubleshooting/known-issues-for-sql-server-machine-learning-services.md)
- [SQL Server Update Center (Centro aggiornamenti di SQL Server): collegamenti e informazioni per tutte le versioni supportate](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

![MS_Logo_X-Small](../sql-server/media/ms-logo-x-small.png)