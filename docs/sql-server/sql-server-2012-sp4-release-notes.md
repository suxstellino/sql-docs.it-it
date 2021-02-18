---
title: Note sulla versione di SQL Server 2012 Service Pack | Microsoft Docs
description: Questo articolo contiene le note sulla versione aggregate dei quattro Service Pack per SQL Server 2012. Ogni Service Pack è cumulativo rispetto ai Service Pack precedenti.
ms.prod: sql
ms.technology: release-landing
ms.custom: ''
ms.date: 07/22/2020
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 67cb8b3e-3d82-47f4-840d-0f12a3bff565
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: 83a7828c93d6863e99d88d3cce45654e8f73abbd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341118"
---
# <a name="sql-server-2012-service-pack-release-notes"></a>Note sulla versione di SQL Server 2012 Service Pack
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]
Questo argomento contiene le note sulla versione aggregate dei quattro Service Pack per SQL Server 2012. Ogni Service Pack è cumulativo rispetto ai Service Pack precedenti.

I Service Pack sono disponibili solo online, non sui supporti di installazione, e possono essere scaricati come indicato di seguito:
- [SQL Server 2012 SP4](https://go.microsoft.com/fwlink/?linkid=846937)
- [SQL Server 2012 SP3](https://support.microsoft.com/help/3072779/sql-server-2012-service-pack-3-release-information)
- [SQL Server 2012 SP2](https://support.microsoft.com/KB/2958429)
- [SQL Server 2012 SP1](https://go.microsoft.com/fwlink/p/?LinkID=268158)

## <a name="service-pack-4-release-notes"></a>Note sulla versione di Service Pack 4

### <a name="download-pages"></a>Pagine di download

- [SQL Server 2012 SP4 Feature Pack](https://www.microsoft.com/download/details.aspx?id=56041)
- [Installazione della patch di SQL Server 2012 SP4](https://www.microsoft.com/download/details.aspx?id=56040)
- [SQL Server 2012 SP4 Express](https://www.microsoft.com/download/details.aspx?id=56042)


### <a name="performance-and-scale-improvements"></a>Miglioramenti in termini di prestazioni e scalabilità
- **Procedura di pulizia dell'agente di distribuzione migliorata**: un database di distribuzione di dimensioni eccessive è la causa di una situazione di blocco e di deadlock. Grazie a una procedura di pulizia migliorata è possibile eliminare scenari di blocco e di deadlock di questo genere. 
- **Scalabilità dinamica degli oggetti in memoria**: partizione dinamica di oggetti in memoria in base al numero di nodi e di core per ridimensionare l'hardware moderno. Obiettivo della promozione dinamica è impedire potenziali colli di bottiglia ed eseguire automaticamente la partizione di un oggetto in memoria thread-safe. È possibile promuovere dinamicamente gli oggetti in memoria non partizionati perché siano partizionati in base al nodo. Il numero delle partizioni corrisponde al numero di nodi NUMA. Gli oggetti in memoria partizionati in base al nodo possono essere ulteriormente promossi perché siano partizionati in base alla CPU, nel caso in cui il numero delle partizioni sia corrispondente al numero delle CPU.
- **Abilitazione di 8 TB per pool di buffer**: abilitazione dello spazio degli indirizzi virtuali a 128 TB per pool di buffer
- **Pulizia di Rilevamento modifiche**: miglioramento delle prestazioni di pulizia di Rilevamento modifiche ed efficienza delle tabelle lato Rilevamento modifiche. 

### <a name="supportability-and-diagnostics-improvements"></a>Miglioramenti al supporto e alla diagnostica
- **Supporto per dump completi per gli agenti di replica**: oggi, se gli agenti di replica incontrano un'eccezione non gestita, il comportamento predefinito prevede la creazione di un breve dump dei sintomi dell'eccezione. Tale comportamento richiede passaggi complessi per la risoluzione dei problemi correlati alle eccezioni non gestite. Il Supporto Package 4 introduce una nuova chiave del Registro di sistema che supporta la creazione di un dump completo per gli agenti di replica.
- **Diagnostica avanzata nel file XML dello showplan**: il file XML dello showplan è stato migliorato per contenere informazioni su flag di traccia abilitati, frazioni di memoria per l'ottimizzazione del join annidato dei cicli, tempo di CPU e tempo trascorso. 
- **Migliore correlazione tra la diagnostica di XEvent e delle DMV**: i campi query_hash e query_plan_hash vengono usati per identificare una query in modo univoco. Nelle DMV sono definiti come varbinary (8), mentre in XEvent come UINT64. Poiché in SQL Server non esiste un "bigint senza segno", il cast non sempre funziona. Questo miglioramento introduce nuove colonne azione/filtro di XEvent equivalenti ai campi query_hash e query_plan_hash ad eccezione del fatto che tali valori sono definiti come INT64. In questo modo è possibile correlare le query tra XEvent e le viste a gestione dinamica. 
- **Migliore diagnostica di concessione/utilizzo memoria**: nuovo evento XEvent query_memory_grant_usage (backport da Server 2016 SP1)
- **Aggiunta della traccia di protocollo ai passaggi di negoziazione SSL**: aggiungere informazioni di traccia bit su negoziazione riuscita/non riuscita, ad esempio il protocollo, può essere utile per risolvere problemi di connettività,ad esempio durante la distribuzione di TLS 1.2
- **Impostazione del livello di compatibilità corretto per il database di distribuzione**: dopo l'installazione del Service Pack il livello di compatibilità del database di distribuzione viene impostato su 90. La modifica del livello era dovuta a un problema nella stored procedure sp_vupgrade_replication. Il Service Pack è stato modificato e ora imposta il livello di compatibilità corretto per il database di distribuzione. 
- **Nuovo comando DBCC per la clonazione di un database**: è stato aggiunto un nuovo comando DBCC per la clonazione del database che consente agli utenti esperti, ad esempio CSS, di risolvere problemi di database di produzione esistenti clonando lo schema e i metadati e non i dati. La chiamata viene eseguita con DBCC clonedatabase ('nome_database_origine', 'nome_database_clone'). È consigliabile non usare i database clonati in ambienti di produzione. Per vedere se un database è stato generato da una chiamata per la clonazione del database, è possibile usare il comando seguente, selezionare DATABASEPROPERTYEX('clonedb', 'isClone'). Il valore restituito pari a 1 è true e il valore 0 è false. 
- **Informazioni sul file TempDB e sulle dimensioni nel log degli errori di SQL**: se le dimensioni e l'aumento automatico sono diversi per i file di dati TempDB durante l'avvio, viene stampato il numero di file e attivato un avviso.
- **Messaggi di supporto per l'inizializzazione immediata dei file nel log degli errori di SQL Server**: nel log degli errori viene indicato se l'inizializzazione immediata dei file di database è abilitata/disabilitata
- **Nuova DMF in sostituzione a DBCC INPUTBUFFER**: in sostituzione a DBCC INPUTBUFFER è stata introdotta sys.dm_input_buffer, una nuova funzione a gestione dinamica che usa session_id come parametro
- **Miglioramento di eventi XEvent in caso di errore di routing di lettura per un gruppo di disponibilità**: attualmente l'evento XEvent read_only_rout_fail viene generato solo in presenza di un elenco di routing. Nessuno dei server contenuto in questo elenco è tuttavia disponibile per le connessioni. Questo miglioramento aggiunge informazioni in caso di risoluzione dei problemi e interessa anche gli elementi di codice in cui è attivo XEvent. 
- **Gestione migliorata di Service Broker in caso di failover del gruppo di disponibilità**: attualmente quando Service Broker è abilitato in un database del gruppo di disponibilità, durante un failover del gruppo di disponibilità tutte le connessioni a Service Broker che hanno avuto origine nella replica primaria vengono lasciate aperte. Questo miglioramento chiude tutte le connessioni aperte durante un failover del gruppo di disponibilità.
- **Partizionamento soft-NUMA automatico**: con SQL 2014 SP2 viene introdotto il partizionamento [soft-NUMA](../database-engine/configure-windows/soft-numa-sql-server.md) automatico quando il flag di traccia 8079 è abilitato a livello di server. Quando il flag di traccia 8079 viene abilitato durante l'avvio, SQL Server 2014 SP2 interroga il layout di hardware e configura automaticamente soft-NUMA nei sistemi che segnalano 8 o più CPU per nodo NUMA. Il comportamento soft-NUMA automatico supporta l'Hyper-Threading (HT/processore logico). Il partizionamento e la creazione di nodi aggiuntivi ridimensionano l'elaborazione in background aumentando il numero di listener tramite scalabilità e le funzionalità di crittografia e di rete. È consigliabile testare le prestazioni del carico di lavoro con la configurazione soft-NUMA automatica prima di attivarlo nell'ambiente di produzione.

## <a name="service-pack-3-release-notes"></a>Note sulla versione di Service Pack 3

### <a name="download-pages"></a>Pagine di download
- [SQL Server 2012 SP3 Feature Pack](https://go.microsoft.com/fwlink/?linkid=615935)
- [SQL Server 2012 SP3 Express](https://go.microsoft.com/fwlink/?linkid=692144)

Per informazioni più dettagliate per identificare il percorso e il nome del file da scaricare in base alla versione attualmente installata, vedere la sezione "Scegliere il file corretto da scaricare" in [Note sulla versione di SQL Server 2012 Service Pack 3](https://support.microsoft.com/help/3072779/sql-server-2012-service-pack-3-release-information).

## <a name="service-pack-2-release-notes"></a>Note sulla versione di Service Pack 2
  
### <a name="download-pages"></a>Pagine di download 
- [SQL Server 2012 SP2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=43340)
- [SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)

Usare la tabella seguente per identificare la posizione e il nome del file da scaricare in base alla versione attualmente installata. Le pagine per il download includono informazioni sui requisiti di sistema e istruzioni di base per l'installazione.  

|Versione attualmente installata|Operazione da eseguire|File da scaricare e installare|  
|---|---|---|   
|Installazioni a 32 bit:|||  
|Versione a 32 bit di qualsiasi edizione di SQL Server 2012|Eseguire l'aggiornamento alla versione a 32 bit di SQL Server 2012 SP2|**SQLServer2012SP2-KB2958429-** <arch> **-** <lang id> **.exe** nella [pagina di download di SQL Server 2012 SP2](https://go.microsoft.com/fwlink/?LinkID=401006)|  
|Versione a 32 bit di SQL Server 2012 RTM Express|Eseguire l'aggiornamento alla versione a 32 bit di SQL Server 2012 Express SP2|**SQLEXPR_** <arch> **_** <lang> **.msi** nella pagina di download di [SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Versione a 32 bit solo degli strumenti client e di gestibilità per SQL Server 2012 (incluso SQL Server 2012 Management Studio)|Eseguire l'aggiornamento degli strumenti client e di gestibilità alla versione a 32 bit di SQL Server 2012 SP2|**SQLEXPRWT_** <arch> **_** <lang> **.msi** nella pagina di download di [SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Versione a 32 bit di SQL Server 2012 Management Studio Express|Eseguire l'aggiornamento alla versione a 32 bit di SQL Server 2012 SP2 Management Studio Express|**SQLManagementStudio_** <arch> **_** <lang> **.msi** nella pagina di download di [SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Versione a 32 bit di qualsiasi edizione di SQL Server 2012 e versione a 32 bit degli strumenti client e di gestibilità (incluso SQL Server 2012 RTM Management Studio)|Eseguire l'aggiornamento di tutti i prodotti alla versione a 32 bit di SQL Server 2012 SP2|**SQLEXPRADV_** <arch> **_** <lang> **.msi** nella pagina di download di [SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Versione a 32 bit di uno o più strumenti di [Microsoft SQL Server 2012 RTM Feature Pack](https://www.microsoft.com/download/details.aspx?id=56041) o di [Microsoft SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575)|Eseguire l'aggiornamento degli strumenti alla versione a 32 bit di Microsoft SQL Server 2012 SP2 Feature Pack|Uno o più strumenti dalla [pagina per il download di Microsoft SQL Server 2012 SP2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=43340)|  
|Installazioni a 64 bit:|||  
|Versione a 64 bit di qualsiasi edizione di SQL Server 2012|Eseguire l'aggiornamento alla versione a 64 bit di SQL Server 2012 SP2|SQLServer2012SP2-KB2958429-<arch>-<langid>.exe nella pagina di download di [SQL Server 2012 SP2](https://go.microsoft.com/fwlink/?LinkID=401006)|  
|Versione a 64 bit di SQL Server 2012 RTM Express|Eseguire l'aggiornamento alla versione a 64 bit di SQL Server 2012 SP2|**SQLEXPR_** <arch> **_** <lang> **.msi** nella pagina di download di [SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Versione a 64 bit solo degli strumenti client e di gestibilità per SQL Server 2012 (incluso SQL Server 2012 Management Studio)|Eseguire l'aggiornamento degli strumenti client e di gestibilità alla versione a 64 bit di SQL Server 2012 SP2|**SQLEXPRWT_** <arch> **_** <lang> **.msi** nella pagina di download di [SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Versione a 64 bit di SQL Server 2012 Management Studio Express|Eseguire l'aggiornamento alla versione a 64 bit di SQL Server 2012 SP2 Management Studio Express|**SQLManagementStudio_** <arch> **_** <lang> **.msi** nella pagina di download di [SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Versione a 64 bit di uno o più strumenti di [Microsoft SQL Server 2012 RTM Feature Pack](https://www.microsoft.com/download/details.aspx?id=56041) o di [Microsoft SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575)|Eseguire l'aggiornamento degli strumenti alla versione a 64 bit di Microsoft SQL Server 2012 SP2 Feature Pack|Uno o più strumenti dalla [pagina per il download di Microsoft SQL Server 2012 SP2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=43340)|   


## <a name="service-pack-1-release-notes"></a>Note sulla versione di Service Pack 1

### <a name="download-pages"></a>Pagine di download  
- [SQL Server 2012 SP1 Feature Pack](https://go.microsoft.com/fwlink/p/?LinkID=268158)
- [SQL Server 2012 SP1 Express](https://www.microsoft.com/download/details.aspx?id=35579)


Fare riferimento alla tabella seguente per determinare il file da scaricare e installare. Verificare di disporre dei requisiti di sistema corretti prima di installare il Service Pack. I requisiti di sistema sono indicati nelle pagine di download di cui viene fornito il collegamento all'interno della tabella.  

|Versione attualmente installata|Operazione da eseguire|File da scaricare e installare|  
|---|---|---|  
|**Installazioni a 32-bit:**|||  
|Versione a 32 bit di qualsiasi edizione di SQL Server 2012|Eseguire l'aggiornamento alla versione a 32 bit di SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x86-ENU.exe in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Versione a 32 bit di SQL Server 2012 RTM Express|Eseguire l'aggiornamento alla versione a 32 bit di SQL Server 2012 Express SP1|SQLServer2012SP1-KB2674319-x86-ENU.exe in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Versione a 32 bit solo degli strumenti client e di gestibilità per SQL Server 2012 (incluso SQL Server 2012 Management Studio)|Eseguire l'aggiornamento degli strumenti client e di gestibilità alla versione a 32 bit di SQL Server 2012 SP1|SQLManagementStudio_x86_ENU.exe in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Versione a 32 bit di SQL Server 2012 Management Studio Express|Eseguire l'aggiornamento alla versione a 32 bit di SQL Server 2012 SP1 Management Studio Express|SQLManagementStudio_x86_ENU.exe in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Versione a 32 bit di qualsiasi edizione di SQL Server 2012 **e** versione a 32 bit degli strumenti client e di gestibilità (incluso SQL Server 2012 RTM Management Studio)|Eseguire l'aggiornamento di tutti i prodotti alla versione a 32 bit di SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x86-ENU.exe in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Versione a 32 bit di uno o più strumenti di [Microsoft SQL Server 2012 RTM Feature Pack](https://www.microsoft.com/download/details.aspx?id=44272)|Eseguire l'aggiornamento degli strumenti alla versione a 32 bit di Microsoft SQL Server 2012 SP1 Feature Pack|Uno o più file di [Microsoft SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575)|  
|Nessuna installazione a 32 bit di SQL Server 2012|Installare SQL Server 2012 a 32 bit con SP1 (nuova istanza con SP1 preinstallato)|SQLServer2012SP1-FullSlipstream-x86-ENU.exe **e** SQLServer2012SP1-FullSlipstream-x86-ENU.box in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Nessuna installazione a 32 bit di SQL Server 2012 Management Studio|Installare SQL Server 2012 Management Studio a 32 bit con SP1|SQLManagementStudio_x86_ENU.exe in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkId=267905)|  
|Nessuna versione a 32 bit di SQL Server 2012 RTM Express|Installare SQL Server 2012 Express a 32 bit con SP1|SQLEXPR32_x86_ENU.exe in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkId=267905)|  
|Installazione a 32 bit di **SQL Server 2008** o **SQL Server 2008 R2**|**Aggiornamento sul posto** a SQL Server 2012 a 32 bit con SP1|SQLServer2012SP1-FullSlipstream-x86-ENU.exe **e** SQLServer2012SP1-FullSlipstream-x86-ENU.box in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|**Installazioni a 64-bit:**|||  
|Versione a 64 bit di qualsiasi edizione di SQL Server 2012|Eseguire l'aggiornamento alla versione a 64 bit di SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x64-ENU.exe in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Versione a 64 bit di SQL Server 2012 RTM Express|Eseguire l'aggiornamento alla versione a 64 bit di SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x64-ENU.exe in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Versione a 64 bit solo degli strumenti client e di gestibilità per SQL Server 2012 (incluso SQL Server 2012 Management Studio)|Eseguire l'aggiornamento degli strumenti client e di gestibilità alla versione a 64 bit di SQL Server 2012 SP1|SQLManagementStudio_x64_ENU.exe in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Versione a 64 bit di SQL Server 2012 Management Studio Express|Eseguire l'aggiornamento alla versione a 64 bit di SQL Server 2012 SP1 Management Studio Express|SQLManagementStudio_x64_ENU.exe in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Versione a 64 bit di qualsiasi edizione di SQL Server 2012 **e** versione a 64 bit degli strumenti client e di gestibilità (incluso SQL Server 2012 RTM Management Studio)|Eseguire l'aggiornamento di tutti i prodotti alla versione a 64 bit di SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x64-ENU.exe in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Versione a 64 bit di uno o più strumenti di [Microsoft SQL Server 2012 RTM Feature Pack](https://www.microsoft.com/download/details.aspx?id=44272)|Eseguire l'aggiornamento degli strumenti alla versione a 64 bit di Microsoft SQL Server 2012 SP1 Feature Pack|Uno o più file di [Microsoft SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575)|  
|Nessuna installazione a 64 bit di SQL Server 2012|Installare SQL Server 2012 a 64 bit con SP1 (nuova istanza con SP1 preinstallato)|SQLServer2012SP1-FullSlipstream-x64-ENU.exe **e** SQLServer2012SP1-FullSlipstream-x64-ENU.box in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Nessuna installazione a 64 bit di SQL Server 2012 Management Studio|Installare SQL Server 2012 Management Studio a 64 bit con SP1|SQLManagementStudio_x64_ENU.exe in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Nessuna versione a 64 bit di SQL Server 2012 RTM Express|Installare SQL Server 2012 Express a 64 bit con SP1|SQLEXPR_x64_ENU.exe in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Installazione a 64 bit di **SQL Server 2008** o **SQL Server 2008 R2**|**Aggiornamento sul posto** a SQL Server 2012 a 64 bit con SP1|SQLServer2012SP1-FullSlipstream-x64-ENU.exe **e** SQLServer2012SP1-FullSlipstream-x64-ENU.box in [questa pagina](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  

### <a name="known-issues-fixed-in-this-service-pack"></a>Problemi noti risolti in questo Service Pack  
Per un elenco completo dei bug e dei problemi noti risolti in questo Service Pack, vedere [questo articolo della Knowledge Base](https://support.microsoft.com/kb/2674319).   

### <a name="reinstalling--instances-of-sql-server-failover-cluster-fails-if-you-use-the-same-ip-address"></a>La reinstallazione di istanze del cluster di failover di SQL Server non riesce se si usa lo stesso indirizzo IP  
**Problema:** se si specifica un indirizzo IP non corretto durante l'installazione di un'istanza del cluster di failover di SQL Server, l'installazione non riesce. Dopo la disinstallazione dell'istanza con errori, e in caso di tentativo di reinstallazione dell'istanza del cluster di failover di SQL Server con lo stesso nome istanza, e indirizzo IP corretto, l'installazione non viene completata. L'errore è dovuto al gruppo di risorse duplicate lasciato dall'installazione precedente.  
  
**Soluzione alternativa:** per risolvere il problema, usare un nome istanza diverso durante la reinstallazione oppure eliminare manualmente il gruppo di risorse prima della reinstallazione. Per altre informazioni, vedere la pagina relativa all' [aggiunta o alla rimozione di nodi in un cluster di failover di SQL Server](failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md). 
  
### <a name="analysis-services-and-powerpivot"></a>Analysis Services e PowerPivot  
  
##### <a name="powerpivot-configuration-tool-does-not-create-the-powerpivot-gallery"></a>Tramite lo strumento di configurazione PowerPivot non viene creata la raccolta PowerPivot  
**Problema:** lo strumento di configurazione PowerPivot esegue il provisioning di un sito del team, pertanto non viene creata alcuna raccolta PowerPivot.  
  
**Soluzione alternativa:** creare una nuova app (libreria).  
  
1.  Verificare che la funzionalità per le raccolte siti **Integrazione della funzionalità di PowerPivot per le raccolte siti** sia attiva.  
  
2.  Nella pagina **Contenuto sito** di un sito esistente fare clic su **Aggiungi applicazione**.  
  
3.  Fare clic su **Raccolta PowerPivot**.  
  
#### <a name="to-use-powerpivot-for-excel-with-excel-2013-you-must-use-the-add-in-that-is-installed-with-excel"></a>Per usare PowerPivot per Excel con Excel 2013, è necessario usare il componente aggiuntivo installato con Excel  
**Problema:** con Office 2010, PowerPivot per Excel è un componente aggiuntivo autonomo che può essere scaricato da [https://www.microsoft.com/bi/powerpivot.aspx](https://www.microsoft.com/bi/powerpivot.aspx). In alternativa, può essere scaricato anche dall' [Area download Microsoft](https://www.microsoft.com/download/details.aspx?id=29074). Si noti che sono disponibili due versioni del componente aggiuntivo PowerPivot: una fornita con SQL Server 2008 R2 e un'altra con SQL Server 2012. Tuttavia, per Office 2013, PowerPivot per Excel viene fornito con Office e viene installato insieme a Excel. Anche se le versioni SQL Server 2008 R2 e SQL Server 2012 di PowerPivot per Excel 2010 non sono compatibili con Excel 2013, è comunque possibile installare PowerPivot per Excel 2010 nel computer client se si desidera un'esecuzione side-by-side di Excel 2010 ed Excel 2013. In altre parole, le due versioni di Excel possono coesistere, così come i componenti aggiuntivi PowerPivot corrispondenti.  
  
**Soluzione alternativa:** per usare PowerPivot per Excel 2013 è necessario abilitare il componente aggiuntivo COM. In Excel 2013 selezionare **File** | **Opzioni** | **Componenti aggiuntivi**. Nella casella di riepilogo a discesa **Gestisci** selezionare **Componenti aggiuntivi COM** e fare clic su **Vai**. In **Componenti aggiuntivi COM** selezionare **Microsoft Office PowerPivot for Excel 2013** e fare clic su **OK**.  
  
### <a name="reporting-services"></a>Reporting Services  
  
#### <a name="install-and-configure-sharepoint-server-2013-prior-to-installing-reporting-services"></a>Installare e configurare SharePoint Server 2013 prima di installare Reporting Services  
**Problema:** completare le operazioni seguenti **prima** di installare SQL Server Reporting Services (SSRS).  
  
1.  Eseguire l'Utilità preparazione prodotti SharePoint 2013.  
  
2.  Installare SharePoint Server 2013.  
  
3.  Eseguire la Configurazione guidata del prodotto SharePoint 2013 o completare un set equivalente di passaggi di configurazione per configurare la farm di SharePoint.  
  
**Soluzione alternativa:**  se è stata installata la modalità SharePoint di [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] prima che la farm di SharePoint venisse configurata, la soluzione alternativa richiesta dipenderà dagli altri componenti installati.  
  
#### <a name="power-view-in-sharepoint-server-2013-requires-microsoftanalysisservicesspclientdll"></a>Per Power View in SharePoint Server 2013 è richiesto il file Microsoft.AnalysisServices.SPClient.dll  
**Problema:** [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] non installa un componente obbligatorio, ovvero **Microsoft.AnalysisServices.SPClient.dll**. Se si installano SharePoint Server 2013 Preview e [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)][!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] in modalità SharePoint ma non si scarica né si installa il pacchetto di installazione di PowerPivot per SharePoint 2013, **spPowerPivot.msi** , Power View non funzionerà e verranno mostrati i sintomi riportati di seguito.  
  
**Sintomi:** quando si prova a creare un report Power View, viene visualizzato un messaggio di errore simile al seguente:  
  
-   "Impossibile creare una connessione all'origine dei dati...''  
  
Nei dettagli interni dell'errore sarà presente un messaggio simile al seguente:  
  
-   "Il valore 'SharePoint Principal' non è supportato per la proprietà della stringa di connessione 'User Identity'".  
  
**Soluzione alternativa:** installare il pacchetto di installazione di PowerPivot per SharePoint 2013 (**spPowerPivot.msi**) in SharePoint Server 2013. Il pacchetto di installazione è disponibile come parte del Feature Pack di [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)] . Il Feature Pack può essere scaricato dall'Area Download [!INCLUDE[msCoName](../includes/msconame-md.md)] in corrispondenza di [SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575)  
  
#### <a name="power-view-sheets-in-a-powerpivot-workbook-are-deleted-after-a-scheduled-data-refresh"></a>I fogli di Power View in una cartella di lavoro di PowerPivot vengono eliminati dopo un aggiornamento dati pianificato  
**Problema**: nel componente aggiuntivo PowerPivot per SharePoint l'uso dell'**aggiornamento dati pianificato** in una cartella di lavoro con Power View comporterà l'eliminazione di tutti i fogli di Power View.  
  
**Soluzione alternativa**: per usare l'**aggiornamento dati pianificato** con le cartelle di lavoro di Power View, creare una cartella di lavoro di PowerPivot che funge semplicemente da modello di dati. Creare una cartella di lavoro separata con i fogli di Excel e di Power View tramite cui è possibile collegarsi alla cartella di lavoro di PowerPivot con il modello di dati. È consigliabile pianificare per l'aggiornamento dati solo la cartella di lavoro di PowerPivot con il modello di dati.  
  
### <a name="data-quality-services"></a>Data Quality Services  
  
#### <a name="dqs-available-in-the-incorrect-edition-of-sql-server-2012"></a>DQS disponibile nell'edizione errata di SQL Server 2012  
**Problema:** nella versione [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] RTM la funzionalità Data Quality Services (DQS) è disponibile nelle edizioni di SQL Server diverse da Enterprise, Business Intelligence e Developer. Dopo aver installato SQL Server 2012 SP1, DQS non sarà disponibile in alcuna edizione, ad eccezione di Enterprise, Business Intelligence e Developer.  
  
**Soluzione alternativa**: se si usa DQS in un'edizione non supportata, eseguire l'aggiornamento a un'edizione supportata o rimuovere dalle applicazioni la dipendenza da questa funzionalità.  
  
### <a name="sql-server-express"></a>SQL Server Express  
  
#### <a name="full-version-of-sql-server-management-studio-available-in-sql-server-2012-express-sp1"></a>Versione completa di SQL Server Management Studio disponibile in SQL Server 2012 Express SP1  
Nella versione SQL Server 2012 Express Service Pack 1 (SP1) è inclusa la versione completa di SQL Server 2012 Management Studio (che in precedenza era disponibile solo nel DVD di SQL Server 2012), anziché SQL Server 2012 Management Studio Express. Per scaricare e installare SQL Server 2012 Express SP1, vedere la pagina relativa a [SQL Server 2012 Express Service Pack 1](https://go.microsoft.com/fwlink/p/?linkid=267905).  
  
### <a name="change-data-capture-service-and-designer-for-oracle-by-attunity"></a>Servizio Change Data Capture e Designer per Oracle di Attunity  
  
#### <a name="upgrading-the-cdc-service-and-designer"></a>Aggiornamento di CDC Service e Designer  
**Problema:** se Change Data Capture Designer per Oracle e Change Data Capture Service per Oracle di Attunity sono presenti nel computer quando si installa SQL Server 2012 SP1, questi componenti non vengono aggiornati dall'installazione di SP1.  
  
**Soluzione alternativa:** per aggiornare i componenti CDC alla versione più recente:  
  
1.  Scaricare i file con estensione msi per Change Data Capture Service per Oracle di Attunity dalla pagina di download di [SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575).  
  
2.  Eseguire il file con estensione msi.  
  
### <a name="sql-server-data-tier-application-framework-dacfx"></a>SQL Server Data-Tier Application Framework (DACFx)  
**Supporto dell'aggiornamento sul posto**  
  
Questa versione di Data-Tier Application Framework (DACFx) supporta l'aggiornamento sul posto delle versioni precedenti, pertanto non è necessario rimuovere le installazioni precedenti di DACFx prima di eseguire l'aggiornamento a questa versione. Le versioni future di DACFx sono disponibili [qui](/previous-versions/sql/sql-server-2016/dn702988(v=sql.130)).  
  
**Supporto per Indice XML selettivo**  
  
In SQL Server 2012 SP1 è incluso il supporto per [Indice XML selettivo (SXI)](../relational-databases/xml/selective-xml-indexes-sxi.md), una nuova funzionalità di SQL Server che fornisce una nuova modalità di indicizzazione della colonna XML con efficienza e prestazioni migliorate.  
  
DACFx supporta ora gli indici SXI in tutti gli strumenti client e scenari di applicazione livello dati. SXI è supportato solo nell'ultima versione di SSDT, non è supportato nelle versioni SSDT RTM e di settembre 2012.  
  
**Supporto per il formato dati BCP nativo**  
  
JSON è stato il formato dati utilizzato in precedenza per archiviare i dati della tabella in pacchetti DACPAC e BACPAC. Con questo aggiornamento, BCP nativo è ora il formato di persistenza dei dati. Grazie a questa modifica è possibile migliorare la compatibilità del tipo di dati di SQL Server a DACFx in cui sono inclusi il supporto per i tipi SQL_Variant, nonché prestazioni di distribuzione avanzate per database in larga scala.  
  
**Mantenimento dello stato del vincolo CHECK nella creazione/distribuzione del pacchetto**  
  
In precedenza, tramite DACFx non è stato possibile mantenere lo stato (WITH CHECK/NOCHECK) dei vincoli CHECK definiti nelle tabelle dello schema del database, né archiviare queste informazioni nei file DACPAC. Tramite questo comportamento è possibile la generazione di problemi relativi alla distribuzione del pacchetto qualora vengano violati i vincoli CHECK dai dati della tabella esistente. Con questo aggiornamento, tramite DACFx è ora possibile archiviare lo stato corrente dei vincoli CHECK nel file DACPAC quando estratti da un database e viene ripristinato in modo appropriato questo stato alla distribuzione del pacchetto.  
  
**Aggiornamenti a SqlPackage.exe (strumento della riga di comando di DACFx)**  
  
-   Estrazione DACPAC con dati. Viene creato un file snapshot del database con estensione dacpac da un dabatase SQL Server o SQL di Azure attivo contenente dati di tabelle utente oltre allo schema del database. Questi pacchetti possono essere pubblicati in un database SQL Server o SQL di Azure nuovo o esistente tramite l'azione Pubblica di SqlPackage.exe. I dati contenuti nel pacchetto sostituiscono quelli esistenti nel database di destinazione.  
  
-   Esportazione di BACPAC. Viene creato un file di backup logico con estensione bacpac di un database SQL Server o SQL di Azure attivo contenente lo schema del database e i dati utente utilizzabili per eseguire la migrazione di un database da SQL Server locale al database SQL di Azure. I database compatibili con Azure possono essere esportati e quindi importati in un secondo momento tra le versioni supportate di SQL Server.  
  
-   Importazione BACPAC. Viene importato un file con estensione bacpac per creare un nuovo database SQL Server o SQL di Azure o popolarne uno vuoto.  
  
La documentazione completa di SqlPackage.exe su MSDN è disponibile [qui](../tools/sqlpackage/sqlpackage.md).  
  
**Compatibilità dei pacchetti**  
  
In questa versione sono disponibili diversi scenari di compatibilità di inoltro per pacchetti di applicazione livello dati.  
  
-   I pacchetti di applicazione livello dati creati da questa versione, non contenenti elementi SXI né dati della tabella, possono essere utilizzati dalle versioni precedenti di DACFx (SQL Server 2012 RTM, SQL Server 2012 CU1 e DACFx di settembre 2012).  
  
-   Tutti i pacchetti di applicazione livello dati creati dalle versioni precedenti di DACFx possono essere utilizzati da questa versione.  
  
## <a name="see-also"></a>Vedere anche
- [Installare gli aggiornamenti dei servizi di SQL Server 2012](/previous-versions/sql/sql-server-2012/hh479746(v=sql.110))
- [Identificazione della versione e dell'edizione di SQL Server](https://support.microsoft.com/help/321185)
- [Installare gli aggiornamenti dei servizi di SQL Server 2012](/previous-versions/sql/sql-server-2012/hh479746(v=sql.110))
- [Identificazione della versione e dell'edizione di SQL Server](https://support.microsoft.com/help/321185) 
- [Identificazione della versione e dell'edizione di SQL Server](https://support.microsoft.com/kb/321185)  
- [Funzionalità supportate dalle edizioni di SQL Server 2014](./editions-and-components-of-sql-server-2016.md)  

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
