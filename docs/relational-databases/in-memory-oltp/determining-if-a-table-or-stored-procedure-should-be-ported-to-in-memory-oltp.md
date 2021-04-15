---
title: Determinare se trasferire una tabella o una stored procedure a OLTP in memoria
description: Usare il report di analisi delle prestazioni delle transazioni in SQL Server Management Studio per valutare se OLTP in memoria è in grado di migliorare le prestazioni delle applicazioni del database.
ms.custom: seo-dt-2019
ms.date: 08/02/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
helpviewer_keywords:
- Analyze, Migrate, Report
- AMR
ms.assetid: c1ef96f1-290d-4952-8369-2f49f27afee2
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b8a6c0e0e9db4e04061691cf331323a4df34f432
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492328"
---
# <a name="determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp"></a>Determinare se una tabella o una stored procedure deve essere trasferita a OLTP in memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Il report di analisi delle prestazioni delle transazioni in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] consente di valutare se OLTP in memoria è in grado di ottimizzare le prestazioni dell'applicazione di database. Il report indica anche la quantità di operazioni che è necessario eseguire per abilitare OLTP in memoria nell'applicazione. Una volta identificata la tabella basata su disco da trasferire in OLTP in memoria, è possibile usare [Ottimizzazione guidata per la memoria](../../relational-databases/in-memory-oltp/memory-optimization-advisor.md)per semplificarne la migrazione. Analogamente, l' [Native Compilation Advisor](../../relational-databases/in-memory-oltp/native-compilation-advisor.md) semplifica il trasferimento di una stored procedure a una stored procedure compilata in modo nativo. Per informazioni sulle metodologie di migrazione, vedere [In-Memory OLTP - Common Workload Patterns and Migration Considerations](/previous-versions/dn673538(v=msdn.10)) (OLTP in memoria: considerazioni sulla migrazione e sui modelli di carico di lavoro comuni).  
  
 Il report di analisi delle prestazioni delle transazioni viene eseguito direttamente su un database di produzione o un database di prova con un carico di lavoro attivo simile al carico di lavoro di produzione.  
  
 Il report e gli assistenti alla migrazione consentono di eseguire le attività seguenti:  
  
-   Analizzare il carico di lavoro per determinare le aree sensibili in cui OLTP in memoria può potenzialmente contribuire a migliorare le prestazioni. Il report di analisi delle prestazioni delle transazioni suggerisce tabelle e stored procedure per cui la conversione in OLTP in memoria potrebbe risultare utile.  
  
-   Facilitare la pianificazione e l'esecuzione della migrazione a OLTP in memoria. Il percorso di migrazione da una tabella basata su disco a una tabella ottimizzata per la memoria può richiedere tempi lunghi. L'Ottimizzazione guidata per la memoria consente di identificare le incompatibilità presenti nella tabella che devono essere rimosse prima dello spostamento della tabella in OLTP in memoria. Lo strumento di ottimizzazione per la memoria consente inoltre di comprendere l'impatto che la migrazione di una tabella a una tabella ottimizzata per la memoria avrà sull'applicazione.  
  
     È possibile verificare se OLTP in memoria può costituire un vantaggio per l'applicazione, quando si desidera pianificare la migrazione a OLTP in memoria e ogni volta che si procede alla migrazione di alcune tabelle e stored procedure a OLTP in memoria.  
  
    > [!IMPORTANT]  
    >  Le prestazioni di un sistema di database dipendono da molti fattori, non tutti osservabili e misurabili tramite l'agente di raccolta delle prestazioni delle transazioni. Pertanto, il report di analisi delle prestazioni delle transazioni non è in grado di garantire che i miglioramenti effettivi delle prestazioni corrisponderanno alle eventuali stime eseguite.  
  
 Il report di analisi delle prestazioni delle transazioni e gli assistenti alla migrazione vengono installati come parte di SQL Server Management Studio (SSMS) quando si seleziona **Strumenti di gestione - Di base** o **Strumenti di gestione - Avanzati** quando si installa [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] oppure quando si sceglie di [scaricare SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).    
  
## <a name="transaction-performance-analysis-reports"></a>Report di analisi delle prestazioni delle transazioni  
 Per generare report di analisi delle prestazioni delle transazioni in **Esplora oggetti** fare clic con il pulsante destro del mouse sul database, scegliere **Report**, quindi **Report standard** e infine **Panoramica dell'analisi delle prestazioni delle transazioni**. Per generare un report di analisi significativo, è richiesto un carico di lavoro attivo o di recente esecuzione del database.  
  
### <a name="tables"></a>Tabelle
  
 Il report dettagli per una tabella è costituito da tre sezioni:  
  
-   Sezione relativa alle statistiche sull'analisi  
  
     In questa sezione è inclusa una singola tabella in cui sono indicate le statistiche raccolte sulle analisi nella tabella di database. Le colonne sono:  
  
    -   Percentuale di accessi totali. Percentuale di analisi e ricerche in questa tabella rispetto all'attività dell'intero database. Più alta è questa percentuale, molto più frequente è l'utilizzo della tabella confrontata con altre tabelle del database.  
  
    -   Statistiche di ricerca/Statistiche analisi intervallo. In questa colonna vengono registrati il numero di ricerche di punti e di analisi di intervalli (analisi di indici e scansioni di tabelle) eseguite sulla tabella durante la profilatura. La media per ogni transazione è una stima.  
    
-   Sezione relativa alle statistiche sulle contese  
  
     In questa sezione è inclusa una tabella in cui viene mostrata la contesa nella tabella di database. Per altre informazioni su latch e blocchi del database, vedere la pagina relativa all'architettura di blocco. Le colonne sono le seguenti:  
  
    -   Percentuale di attese totali. Percentuale di attese di blocchi e di latch in questa tabella di database rispetto all'attività del database. Più alta è questa percentuale, molto più frequente è l'utilizzo della tabella confrontata con altre tabelle del database.  
  
    -   Statistiche latch. In queste colonne viene registrato il numero di attese di latch per query che interessano questa tabella. Per altre informazioni, vedere la pagina relativa ai latch. Più elevato è questo numero, maggiore è la contesa di latch nella tabella.  
  
    -   Statistiche blocchi. In questo gruppo di colonne viene registrato il numero di acquisizioni e attese dei blocchi di pagina per query per la tabella. Per altre informazioni sui blocchi, vedere la pagina relativa alle informazioni sul blocco in SQL Server. Più attese vi sono, maggiore è la contesa di blocchi nella tabella.  
  
-   Sezione relativa alle difficoltà nella migrazione  
  
     In questa sezione è inclusa una tabella in cui vengono mostrate le difficoltà di conversione della tabella di database in una tabella ottimizzata per la memoria. Un grado di difficoltà più elevato indica una maggiore difficoltà di conversione della tabella. Per visualizzare i dettagli relativi alla conversione di questa tabella di database, usare l'Ottimizzazione guidata per la memoria.  
  
Le statistiche sulle contese e sulle analisi nel report dettagli della tabella vengono raccolte e aggregate da sys.dm_db_index_operational_stats (Transact-SQL).  

### <a name="stored-procedures"></a>Stored procedure

 Una stored procedure con un alto rapporto di tempo di utilizzo della CPU rispetto al tempo trascorso è una candidata per la migrazione. Il report include tutti i riferimenti alle tabelle perché le stored procedure compilate in modo nativo possono fare riferimento solo alle tabelle ottimizzate per la memoria che possono costituire un'aggiunta al costo di migrazione.  
  
 Il report dettagli per una stored procedure è costituito da due sezioni:  
  
-   Sezione relativa alle statistiche di esecuzione  
  
     In questa sezione è inclusa una tabella in cui sono indicate le statistiche raccolte sulle esecuzioni della stored procedure. Le colonne sono le seguenti:  
  
    -   Ora di memorizzazione nella cache. Ora di memorizzazione nella cache di questo piano di esecuzione. Se la stored procedure viene ritirata dalla cache dei piani e poi reinserita, verranno indicate le ore per ogni memorizzazione nella cache.  
  
    -   Tempo totale CPU. Tempo totale CPU utilizzato dalla stored procedure durante la profilatura. Più alto è questo numero, maggiore è l'utilizzo della CPU da parte della stored procedure.  
  
    -   Tempo di esecuzione totale. Tempo di esecuzione totale utilizzato dalla stored procedure durante la profilatura. Maggiore è la differenza tra questo numero e il tempo CPU, minore è l'utilizzo della CPU in modo efficace da parte della stored procedure.  
  
    -   Totale riscontri cache mancati. Numero di mancati riscontri nella cache (letture dall'archiviazione fisica) causato dalle esecuzioni della stored procedure durante la profilatura.  
  
    -   Conteggio esecuzioni. Numero di esecuzioni della stored procedure durante la profilatura.  
  
-   Sezione relativa ai riferimenti a tabelle  
  
     In questa sezione è inclusa una tabella in cui vengono mostrate le tabelle a cui fa riferimento questa stored procedure. Prima di convertire la stored procedure in una stored procedure compilata a livello nativo, tutte queste tabelle devono essere convertite in tabelle ottimizzate per la memoria e devono rimanere nello stesso server e database.  
  
 Le statistiche relative alle esecuzioni nel report dettagli della stored procedure vengono raccolte e aggregate da sys.dm_exec_procedure_stats (Transact-SQL). I riferimenti sono ottenuti da sys.sql_expression_dependencies (Transact-SQL).  
  
 Per visualizzare i dettagli relativi a come convertire una stored procedure in una stored procedure compilata in modo nativo, usare l'Assistente compilazione nativa.  
  
## <a name="generating-in-memory-oltp-migration-checklists"></a>Generazione guidata di elenchi di controllo per migrazione OLTP in memoria  
 Gli elenchi di controllo per migrazione identificano qualsiasi funzionalità di stored procedure o tabella non supportata con tabelle ottimizzate per la memoria o stored procedure compilate in modo nativo. L'ottimizzazione guidata della memoria e l'assistente compilazione nativa possono generare un elenco di controllo per una singola tabella basata su disco o stored procedure T-SQL interpretata. Gli elenchi di controllo per migrazione possono essere creati per più tabelle e stored procedure in un database.  
  
 È possibile generare un elenco di controllo per la migrazione in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] usando il comando **Generazione guidata elenchi di controllo per migrazione OLTP in memoria** o tramite PowerShell.  
  
**Per generare un elenco di controllo per migrazione tramite il comando dell'interfaccia utente**  
  
1.  In **Esplora oggetti** fare clic con il pulsante destro del mouse su un database diverso dal database di sistema, scegliere **Attività** e quindi fare clic su **Generazione guidata elenchi di controllo per migrazione OLTP in memoria**.  
  
2.  Nella finestra di dialogo Generazione guidata elenchi di controllo per migrazione OLTP in memoria fare clic su Avanti per passare alla pagina **Configura opzioni di generazione elenchi di controllo** . In questa pagina eseguire le operazioni seguenti.  
  
    1.  Immettere un percorso cartella nella casella di controllo **Salva elenco di controllo in** .  
  
    2.  Verificare che l'opzione **Genera elenchi di controllo per tabelle e stored procedure specifiche** sia selezionata.  
  
    3.  Espandere i nodi **Tabella** e **Stored procedure** nella casella di selezione.  
  
    4.  Selezionare alcuni oggetti nella casella di selezione.  
  
3.  Fare clic su **Avanti** e verificare che l'elenco di attività corrisponda alle impostazioni nella pagina **Configura opzioni di generazione elenco di controllo** .  
  
4.  Fare clic su **Fine** e quindi verificare che i report degli elenchi di controllo per migrazione siano stati generati solo per gli oggetti selezionati.  

 È possibile verificare l'accuratezza dei report confrontandoli con i report generati dagli strumenti Ottimizzazione guidata per la memoria e Assistente compilazione nativa. Per ulteriori informazioni, vedere [Memory Optimization Advisor](../../relational-databases/in-memory-oltp/memory-optimization-advisor.md) e [Native Compilation Advisor](../../relational-databases/in-memory-oltp/native-compilation-advisor.md).  
  
**Per generare un elenco di controllo per migrazione tramite SQL Server PowerShell**  
  
1.  In **Esplora oggetti** fare clic su un database e quindi scegliere **Avvia PowerShell**. Verificare che sia visualizzato il prompt seguente.  
  
    ```  
    PS SQLSERVER: \SQL\{Instance Name}\DEFAULT\Databases\{two-part DB Name}>  
    ```  
  
2.  Immettere il comando seguente.  
  
    ```  
    Save-SqlMigrationReport -FolderPath "<folder_path>"  
    ```  
  
3.  Verificare quanto segue.  
  
    -   Il percorso della cartella viene creato, se non esiste già.  
  
    -   Il report dell'elenco di controllo per migrazione viene generato per tutte le tabelle e stored procedure nel database e archiviato nella posizione specificata da folder_path.  
  
**Per generare un elenco di controllo per migrazione tramite Windows PowerShell**  
  
1.  Avviare una sessione di Windows PowerShell elevata.  
  
2.  Immettere i comandi seguenti. L'oggetto può essere una tabella o una stored procedure.  
  
    ```  
    [System.Reflection.Assembly]::LoadWithPartialName('Microsoft.SqlServer.SMO')  
  
    ```  
  
    ```  
    Save-SqlMigrationReport -Server "<instance_name>" -Database "<db_name>" -FolderPath "<folder_path1>"  
  
    ```  
  
    ```  
    Save-SqlMigrationReport -Server "<instance_name>" -Database "<db_name>" -Object <object_name> -FolderPath "<folder_path2>"  
  
    ```  
  
3.  Verificare quanto segue.  
  
    -   Un report dell'elenco di controllo per migrazione viene generato per tutte le tabelle e stored procedure nel database e archiviato nella posizione specificata da folder_path.  
  
    -   Il report dell'elenco di controllo per migrazione per <object_name> è l'unico report nella posizione specificata da folder_path2.  
  
## <a name="see-also"></a>Vedere anche  
 [Migrazione a OLTP in memoria](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  
