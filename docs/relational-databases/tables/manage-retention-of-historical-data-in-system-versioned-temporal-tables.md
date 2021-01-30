---
description: Gestire la conservazione dei dati cronologici nelle tabelle temporali con controllo delle versioni di sistema
title: Gestire i dati cronologici nelle tabelle temporali con controllo delle versioni di sistema
ms.custom: seo-lt-2019
ms.date: 05/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 7925ebef-cdb1-4cfe-b660-a8604b9d2153
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 54ffa0af033377f3647d57eead126a802e442d68
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159470"
---
# <a name="manage-retention-of-historical-data-in-system-versioned-temporal-tables"></a>Gestire la conservazione dei dati cronologici nelle tabelle temporali con controllo delle versioni di sistema


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Le tabelle temporali con controllo delle versioni di sistema consentono alla tabella di cronologia di aumentare le dimensioni del database in modo superiore rispetto alle tabelle normali, in particolare se si verificano le condizioni seguenti:

- I dati cronologici vengono conservati per un lungo periodo di tempo
- Si esegue un aggiornamento o un'eliminazione con un modello di modifica dati con impatto elevato

Una tabella di cronologia di grandi dimensioni e in continua crescita può costituire un problema, a causa dei semplici costi di archiviazione e dell'impatto sulle prestazioni delle query temporali. Di conseguenza, lo sviluppo di criteri di conservazione dei dati per la gestione dei dati nella tabella della cronologia è un aspetto importante della pianificazione e della gestione del ciclo di vita di ogni tabella temporale.

## <a name="data-retention-management-for-history-table"></a>Gestione della conservazione dei dati per la tabella di cronologia

Per gestire la conservazione dei dati della tabella temporale, è prima di tutto necessario determinare il periodo di conservazione obbligatorio per ogni tabella temporale. Nella maggior parte dei casi, i criteri di conservazione devono essere considerati come parte della logica di business dell'applicazione che usa le tabelle temporali. Ad esempio le applicazioni negli scenari di controllo dei dati e di spostamento cronologico prevedono requisiti rigorosi a livello di durata della disponibilità dei dati cronologici per le query online.

Dopo avere determinato il periodo di conservazione dei dati, è necessario sviluppare un piano per la gestione dei dati cronologici, per la modalità e la posizione di archiviazione dei dati cronologici e per l'eliminazione dei dati cronologici precedenti ai requisiti di conservazione. Per la gestione dei dati cronologici nella tabella di cronologia temporale sono disponibili i quattro approcci seguenti:

- [Stretch Database](#using-stretch-database-approach)
- [Partizionamento delle tabelle](#using-table-partitioning-approach)
- [Script di pulizia personalizzato](#using-custom-cleanup-script-approach)
- [Criteri di conservazione](#using-temporal-history-retention-policy-approach)

 In ogni approccio la logica per la migrazione o la pulizia dei dati cronologici è basata sulla colonna che corrisponde alla fine del periodo nella tabella corrente. Il valore relativo alla fine del periodo per ogni riga determina il momento in cui la versione della riga diventa "chiusa", ovvero quando viene inserita nella tabella di cronologia. Ad esempio, la condizione `SysEndTime < DATEADD (DAYS, -30, SYSUTCDATETIME ())` specifica che i dati cronologici più vecchi di un mese devono essere rimossi o spostati dalla tabella di cronologia.

> [!NOTE]
> Gli esempi in questo argomento usano questo [esempio di tabella temporale](creating-a-system-versioned-temporal-table.md).

## <a name="using-stretch-database-approach"></a>Uso dell'approccio con Stretch Database

> [!NOTE]
> L'approccio con Stretch Database si applica solo a [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] e non a [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

[Stretch Database](../../sql-server/stretch-database/stretch-database.md) in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] esegue la migrazione dei dati cronologici in modo trasparente in Azure. Per una maggiore sicurezza, è possibile crittografare i dati in transito usando la funzionalità [Crittografia sempre attiva](../security/encryption/always-encrypted-database-engine.md) di SQL Server. È anche possibile usare la [sicurezza a livello di riga](../../relational-databases/security/row-level-security.md) e altre funzionalità avanzate per la sicurezza di SQL Server con Estensione database e un database temporale per proteggere i dati.

L'approccio con Stretch Database consente di estendere alcune o tutte le tabelle di cronologia temporali in Azure e SQL Server sposterà automaticamente i dati cronologici in Azure. L'abilitazione di una tabella di cronologia per l'estensione non modifica la modalità di interazione con la tabella temporale a livello di modifica dei dati e di query temporali.

- **Estendere l'intera tabella di cronologia:** configurare Stretch Database per l'intera tabella di cronologia se lo scenario principale è costituito dal controllo dei dati in un ambiente con modifiche frequenti dei dati e query relativamente rare sui dati cronologici. In altri termini, usare questo approccio se le prestazioni delle query temporali non sono essenziali. In questo caso, Azure potrebbe risultare economicamente conveniente. Quando si estende l'intera tabella di cronologia, è possibile usare la procedura guidata per l'estensione o Transact-SQL. Ecco un esempio di entrambi.

- **Estendere una parte della tabella di cronologia:** configurare Stretch Database solo per una parte della tabella di cronologia per migliorare le prestazioni se lo scenario principale prevede principalmente l'esecuzione di query sui dati cronologici recenti, ma si vuole mantenere l'opzione per l'esecuzione di query su dati cronologici precedenti se necessario, archiviando al tempo stesso questi dati in modalità remota a un costo inferiore. Transact-SQL consente di ottenere questo risultato specificando una funzione di predicato per selezionare le righe di cui verrà eseguita la migrazione dalla tabella di cronologia, invece di eseguire la migrazione di tutte le righe. Quando si utilizzano le tabelle temporali, è in genere consigliabile spostare i dati in base alla condizione temporale, ovvero in base all'età della versione della riga nella tabella di cronologia.

  L'uso di una funzione di predicato deterministica consente di mantenere una parte della cronologia nello stesso database con i dati correnti, mentre viene eseguita la migrazione del resto della cronologia in Azure. Per esempi e limitazioni, vedere [Selezionare le righe di cui eseguire la migrazione tramite una funzione di filtro (Estensione database)](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md). Poiché le funzioni non deterministiche non sono valide, per trasferire i dati cronologici usando una finestra temporale scorrevole è necessario modificare in modo regolare la definizione della funzione di predicato inline, in modo che la finestra di righe mantenuta in locale sia costante a livello di età. La finestra temporale scorrevole consente di spostare in modo costante i dati temporali più vecchi di un mese in Azure. Ecco un esempio di questo approccio.

> [!NOTE]
> Stretch Database esegue la migrazione dei dati in Azure. È quindi necessario avere un account Azure e una sottoscrizione per la fatturazione. Per ottenere un account di valutazione gratuito di Azure, fare clic sulla [versione di valutazione gratuita di un mese](https://azure.microsoft.com/pricing/free-trial/).

È possibile configurare una tabella di cronologia temporale per l'estensione usando la procedura guidata per l'estensione o Transact-SQL ed è possibile abilitare una tabella di cronologia temporale per l'estensione se il controllo delle versioni di sistema è **attivato**. L'estensione della tabella corrente non è consentita perché si tratta di un'operazione superflua.

### <a name="using-the-stretch-wizard-to-stretch-the-entire-history-table"></a>Uso della procedura guidata per l'estensione per estendere l'intera tabella di cronologia

Il metodo più semplice per i principianti consiste nell'usare la procedura guidata per l'estensione per abilitare l'estensione per l'intero database e quindi selezionare la tabella di cronologia temporale all'interno della procedura guidata per l'estensione. Questo esempio presuppone che la tabella Department sia stata configurata come tabella temporale con controllo delle versioni di sistema in un database altrimenti vuoto. In [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]non è possibile fare clic con il pulsante destro del mouse sulla tabella di cronologia temporale stessa e scegliere Estendi.

1. Fare clic con il pulsante destro del mouse sul database e scegliere **Attività**, quindi **Estendi** e infine **Abilitare** per avviare la procedura guidata.
2. Nella finestra **Selezionare le tabelle** selezionare la casella di controllo della tabella di cronologia temporale e quindi fare clic su Avanti.

    ![Selezione della tabella di cronologia nella pagina Selezionare le tabelle](../../relational-databases/tables/media/stretch-wizard-2-for-temporal.png "Selezione della tabella di cronologia nella pagina Selezionare le tabelle")
3. Nella finestra **Configura Azure** specificare le proprie credenziali di accesso. Accedere a Microsoft Azure o iscriversi per ottenere un account. Selezionare la sottoscrizione da usare e l'area di Azure. Creare quindi un nuovo server o selezionare un server esistente. Fare clic su **Avanti**.

    ![Creazione di un nuovo server di Azure - Procedura guidata Stretch Database](../../relational-databases/tables/media/stretch-wizard-4.png "Creazione di un nuovo server di Azure - Procedura guidata Stretch Database")
4. Nella finestra **Credenziali protette** specificare una password per la chiave master del database per proteggere le credenziali del database SQL Server di origine e quindi fare clic su Avanti.

    ![Pagina Credenziali protette della procedura guidata Stretch Database](../../relational-databases/tables/media/stretch-wizard-6.png "Pagina Credenziali protette della procedura guidata Stretch Database")
5. Nella finestra **Selezionare l'indirizzo IP** specificare l'intervallo di indirizzi IP per SQL Server per consentire al server di Azure di comunicare con SQL Server. Se si seleziona un server esistente per cui esiste già una regola del firewall, è sufficiente fare clic su Avanti per usare tale regola. Fare clic su **Avanti**, quindi su **Fine** per abilitare Stretch Database ed estendere la tabella di cronologia temporale.

    ![Pagina Selezionare l'indirizzo IP della procedura guidata Stretch Database](../../relational-databases/tables/media/stretch-wizard-7.png "Pagina Selezionare l'indirizzo IP della procedura guidata Stretch Database")
6. Al termine della procedura guidata, verificare che il database sia stato abilitato per l'estensione. Si noti che le icone in Esplora oggetti indicano che il database è stato esteso.

> [!NOTE]
> Se l'abilitazione del database per l'estensione ha esito negativo,controllare il log degli errori. Un errore comune consiste nella configurazione non corretta della regola del firewall.

Vedere anche la pagina relativa alla

- [Abilitare Stretch Database per un database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)
- [Introduzione all'esecuzione della procedura guidata Abilitare il database per l'estensione](../../sql-server/stretch-database/get-started-by-running-the-enable-database-for-stretch-wizard.md)
- [Abilitare Stretch Database per una tabella](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)

### <a name="using-transact-sql-to-stretch-the-entire-history-table"></a>Uso di Transact-SQL per estendere l'intera tabella di cronologia

È anche possibile usare Transact-SQL per abilitare l'estensione sul server locale e [abilitare Estensione database per un database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md). È quindi possibile [usare Transact-SQL per abilitare Stretch Database in una tabella](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md). Con un database già abilitato per l'estensione, eseguire lo script di Transact-SQL seguente per estendere una tabella di cronologia temporale con controllo delle versioni di sistema esistente:

```sql
ALTER TABLE <history table name>
SET (REMOTE_DATA_ARCHIVE = ON (MIGRATION_STATE = OUTBOUND));
```

### <a name="using-transact-sql-to-stretch-a-portion-of-the-history-table"></a>Uso di Transact-SQL per estendere una parte della tabella di cronologia

Per estendere solo una parte della tabella di cronologia, creare prima di tutto una [funzione di predicato inline](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md). Per questo esempio si presupponga di aver configurato la funzione di predicato inline per la prima volta il 1° dicembre 2015 e di voler estendere in Azure tutta la cronologia precedente al 1° novembre 2015. Per ottenere questo risultato, creare prima di tutto la funzione seguente:

```sql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20151101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2015-11-01T00:00:00', 101) ;
```

Usare quindi lo script seguente per aggiungere il predicato di filtro alla tabella di cronologia e impostare lo stato della migrazione su OUTBOUND per abilitare la migrazione dei dati basata sui predicati per la tabella di cronologia.

```sql
ALTER TABLE <history table name>
SET (
      REMOTE_DATA_ARCHIVE = ON
        (
          FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20151101 (SysEndTime)
            , MIGRATION_STATE = OUTBOUND
        )
    )
;
```

Per mantenere una finestra temporale scorrevole, è necessario che la funzione di predicato sia precisa ogni giorno, vale a dire che la condizione della riga di filtro venga modificata di un giorno tutti i giorni. Lo script seguente è lo script da eseguire il 2 dicembre 2015:

```sql
BEGIN TRAN
/*(1) Create new predicate function definition */
  CREATE FUNCTION dbo.fn_StretchBySystemEndTime20151102(@systemEndTime datetime2)
   RETURNS TABLE
    WITH SCHEMABINDING
      AS
        RETURN SELECT 1 AS is_eligible
          WHERE @systemEndTime < CONVERT(datetime2,'2015-11-02T00:00:00', 101)
  GO
 
/*(2) Set the new function as filter predicate */
  ALTER TABLE <history table name>
    SET
      (
        REMOTE_DATA_ARCHIVE = ON
          (
            FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20151102(SysEndTime),
              MIGRATION_STATE = OUTBOUND
          )
      )
COMMIT ;
```

Usare SQL Server Agent o un altro meccanismo di pianificazione per assicurare una definizione valida per la funzione di predicato in ogni momento.

## <a name="using-table-partitioning-approach"></a>Uso dell'approccio con partizionamento delle tabelle

Il[partizionamento delle tabelle](../partitions/create-partitioned-tables-and-indexes.md) può semplificare la gestione e il ridimensionamento di tabelle di grandi dimensioni. L'approccio con partizionamento delle tabelle consente di usare le partizioni delle tabelle di cronologia per implementare la pulizia dei dati personalizzata o l'archiviazione offline in base a una condizione temporale. Il partizionamento delle tabelle offrirà anche vantaggi a livello di prestazioni in caso di query su tabelle temporali relative a un subset di cronologia dei dati tramite l'eliminazione delle partizioni.

Il partizionamento delle tabelle consente di implementare un approccio con finestra temporale scorrevole per spostare le parti più vecchie dalla tabella di cronologia e mantenere costanti le dimensioni della parte conservata in termini di età, mantenendo i dati della tabella di cronologia uguali al periodo di conservazione necessario. L'operazione di disattivazione dei dati dalla tabella di cronologia è supportata se SYSTEM_VERSIONING è ON, ovvero è possibile pulire una parte dei dati di cronologia senza introdurre una finestra di manutenzione o bloccare i carichi di lavoro normali.

> [!NOTE]
> Per eseguire la disattivazione delle partizioni, è necessario che l'indice cluster sulla tabella di cronologia sia allineato allo schema di partizionamento, ovvero che contenga SysEndTime. La tabella di cronologia predefinita creata dal sistema contiene un indice cluster che include le colonne SysEndTime e SysStartTime, ottimali per il partizionamento, l'inserimento di nuovi dati di cronologia e le query temporali tipiche. Per altre informazioni, vedere [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

In un approccio con finestra temporale scorrevole è necessario eseguire due set di attività:

- Attività di configurazione del partizionamento
- Attività di manutenzione ricorrenti della partizione

Si supponga, ad esempio, che si vogliano conservare i dati cronologici per 6 mesi e inserire i dati di ogni mese in una partizione separata. Si presupponga anche di avere attivato il controllo delle versioni di sistema nel mese di settembre 2015.

Un'attività di configurazione del partizionamento crea la configurazione iniziale del partizionamento per la tabella di cronologia. Per questo esempio viene creato un numero di partizioni corrispondente alle dimensioni della finestra temporale scorrevole, in mesi, oltre a una partizione aggiuntiva vuota già preparata, come illustrato di seguito. Questa configurazione assicura che il sistema archivierà correttamente nuovi dati quando viene avviata per la prima volta l'attività ricorrente di manutenzione della partizione e assicura che le partizioni non verranno mai suddivise con i dati, per evitare spostamenti costosi dei dati. È consigliabile eseguire questa attività usando Transact-SQL con lo script di esempio seguente.

La figura seguente illustra la configurazione iniziale del partizionamento per la conservazione di 6 mesi di dati.

![Diagramma che mostra la configurazione iniziale del partizionamento per conservare sei mesi di dati.](../../relational-databases/tables/media/partitioning.png "Partizionamento")

> [!NOTE]
> Vedere Considerazioni sulle prestazioni con il partizionamento delle tabelle più avanti per informazioni sulle conseguenze dell'uso di RANGE LEFT invece di RANGE RIGHT sulle prestazioni durante la configurazione del partizionamento.

La prima e l'ultima partizione sono "aperte" rispettivamente sul limite inferiore e superiore, per garantire che ogni nuova riga abbia una partizione di destinazione, indipendentemente dal valore della colonna di partizionamento. Con il passare del tempo, le nuove righe della tabella di cronologia verranno inserite in partizioni superiori. Quando la sesta partizione viene riempita, si raggiunge il limite del periodo di conservazione specificato. Questo è il momento in cui avviare per la prima volta l'attività ricorrente di manutenzione della partizione. Questa attività deve essere pianificata per l'esecuzione periodica, una volta al mese in questo esempio.

La figura seguente illustra le attività ricorrenti di manutenzione della partizione. Vedere la procedura dettagliata più avanti.

![Diagramma che mostra le attività ricorrenti di manutenzione della partizione.](../../relational-databases/tables/media/partitioning2.png "Partizionamento 2")

Ecco la procedura dettagliata per le attività ricorrenti di manutenzione della partizione:

1. SWITCH OUT: creare una tabella di staging e quindi cambiare una partizione tra la tabella di cronologia e la tabella di staging usando l'istruzione [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) con l'argomento SWITCH PARTITION. Vedere l'esempio C relativo al cambio di partizioni tra tabelle.

    ```sql
    ALTER TABLE <history table> SWITCH PARTITION 1 TO <staging table>
    ```

    Dopo il cambio di partizione, è possibile archiviare facoltativamente i dati dalla tabella di staging e quindi eliminare o troncare la tabella di staging in modo da essere pronti per quando sarà necessario eseguire di nuovo questa attività ricorrente di manutenzione della partizione.
2. MERGE RANGE: unire la partizione 1 vuota con la partizione 2 usando l'istruzione [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md) con MERGE RANGE. Vedere l'esempio B. Rimuovendo il limite inferiore mediante questa funzione, si unisce effettivamente la partizione 1 vuota con la partizione 2 precedente per formare una nuova partizione 1. Vengono modificati anche i numeri ordinali relativi alle altre partizioni.
3. SPLIT RANGE: creare una nuova partizione 7 vuota usando l'istruzione [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md) con SPLIT RANGE. Vedere l'esempio A. Aggiungendo un nuovo limite superiore mediante questa funzione, si crea effettivamente una partizione separata per il mese successivo.

### <a name="use-transact-sql-to-create-partitions-on-history-table"></a>Usare Transact-SQL per creare partizioni nella tabella di cronologia

Usare lo script di Transact-SQL nella finestra di codice seguente per creare la funzione di partizione e ricreare l'indice cluster in modo che sia allineato a livello di partizioni con lo schema delle partizioni e le partizioni. Per questo esempio verrà creato un approccio con finestra temporale scorrevole di sei mesi con partizioni mensili a partire dal mese di settembre 2015.

```sql
BEGIN TRANSACTION
/*Create partition function*/
    CREATE PARTITION FUNCTION [fn_Partition_DepartmentHistory_By_SysEndTime] (datetime2(7))
        AS RANGE LEFT FOR VALUES
          (
            N'2015-09-30T23:59:59.999'
          , N'2015-10-31T23:59:59.999'
          , N'2015-11-30T23:59:59.999'
          , N'2015-12-31T23:59:59.999'
          , N'2016-01-31T23:59:59.999'
          , N'2016-02-29T23:59:59.999'
          )
/*Create partition scheme*/
    CREATE PARTITION SCHEME [sch_Partition_DepartmentHistory_By_SysEndTime]
        AS PARTITION [fn_Partition_DepartmentHistory_By_SysEndTime]
            TO ([PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY], [PRIMARY])
/*Re-create index to be partition-aligned with the partitioning schema*/
    CREATE CLUSTERED INDEX [ix_DepartmentHistory] ON [dbo].[DepartmentHistory]
        (
            [SysEndTime] ASC
          , [SysStartTime] ASC
        )  
    WITH
        (
            PAD_INDEX = OFF
          , STATISTICS_NORECOMPUTE = OFF
          , SORT_IN_TEMPDB = OFF
          , DROP_EXISTING = ON
          , ONLINE = OFF
          , ALLOW_ROW_LOCKS = ON
          , ALLOW_PAGE_LOCKS = ON
          , DATA_COMPRESSION = PAGE
        )
    ON [sch_Partition_DepartmentHistory_By_SysEndTime] ([SysEndTime])

COMMIT TRANSACTION;
```

### <a name="using-transact-sql-to-maintain-partitions-in-sliding-window-scenario"></a>Uso di Transact-SQL per la manutenzione di partizioni in uno scenario con finestra temporale scorrevole

Usare lo script di Transact-SQL nella finestra di codice seguente per la manutenzione delle partizioni nello scenario con finestra temporale scorrevole. Per questo esempio verrà disattivata la partizione per il mese di settembre 2015 mediante MERGE RANGE e quindi verrà aggiunta una nuova partizione per il mese di marzo 2016 mediante SPLIT RANGE.

```sql
BEGIN TRANSACTION
/*(1) Create staging table */
    CREATE TABLE [dbo].[staging_DepartmentHistory_September_2015]
        (
            [DeptID] [int] NOT NULL
          , [DeptName] [varchar](50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
          , [ManagerID] [int] NULL
          , [ParentDeptID] [int] NULL
          , [SysStartTime] [datetime2](7) NOT NULL
          , [SysEndTime] [datetime2](7) NOT NULL
        ) ON [PRIMARY]
    WITH
        (
            DATA_COMPRESSION = PAGE
        )
/*(2) Create index on the same filegroups as the partition that will be switched out*/
    CREATE CLUSTERED INDEX [ix_staging_DepartmentHistory_September_2015]
        ON [dbo].[staging_DepartmentHistory_September_2015]
        (
            [SysEndTime] ASC
          , [SysStartTime] ASC
        )
    WITH
        (
            PAD_INDEX = OFF
          , SORT_IN_TEMPDB = OFF
          , DROP_EXISTING = OFF
          , ONLINE = OFF
          , ALLOW_ROW_LOCKS = ON
          , ALLOW_PAGE_LOCKS = ON
        )
    ON [PRIMARY]
/*(3) Create constraints matching the partition that will be switched out*/
    ALTER TABLE [dbo].[staging_DepartmentHistory_September_2015] WITH CHECK
        ADD CONSTRAINT [chk_staging_DepartmentHistory_September_2015_partition_1]
            CHECK ([SysEndTime]<=N'2015-09-30T23:59:59.999')
    ALTER TABLE [dbo].[staging_DepartmentHistory_September_2015]
        CHECK CONSTRAINT [chk_staging_DepartmentHistory_September_2015_partition_1]
/*(4) Switch partition to staging table*/
    ALTER TABLE [dbo].[DepartmentHistory]
        SWITCH PARTITION 1 TO [dbo].[staging_DepartmentHistory_September_2015]
        WITH (WAIT_AT_LOW_PRIORITY (MAX_DURATION = 0 MINUTES, ABORT_AFTER_WAIT = NONE))
/*(5) [Commented out] Optionally archive the data and drop staging table
      INSERT INTO [ArchiveDB].[dbo].[DepartmentHistory]
      SELECT * FROM [dbo].[staging_DepartmentHistory_September_2015];
      DROP TABLE [dbo].[staging_DepartmentHIstory_September_2015];
*/
/*(6) merge range to move lower boundary one month ahead*/
    ALTER PARTITION FUNCTION [fn_Partition_DepartmentHistory_By_SysEndTime]()
        MERGE RANGE(N'2015-09-30T23:59:59.999')
/*(7) Create new empty partition for "April and after" by creating new boundary point and specifying NEXT USED file group*/
    ALTER PARTITION SCHEME [sch_Partition_DepartmentHistory_By_SysEndTime] NEXT USED [PRIMARY]
        ALTER PARTITION FUNCTION [fn_Partition_DepartmentHistory_By_SysEndTime]() SPLIT RANGE(N'2016-03-31T23:59:59.999')
COMMIT TRANSACTION
```

È possibile modificare leggermente lo script precedente e usarlo nel processo di manutenzione mensile regolare:

1. Nel passaggio (1) creare una nuova tabella di staging per il mese da rimuovere. Il mese successivo nell'esempio è ottobre.
2. Nel passaggio (3) creare e controllare il vincolo corrispondente al mese di dati da rimuovere: `[SysEndTime]<=N'2015-10-31T23:59:59.999'` per la partizione di ottobre.
3. Nel passaggio (4) eseguire l'istruzione SWITCH per la partizione 1 nella tabella di staging appena creata.
4. Nel passaggio (6) modificare la funzione di partizione unendo il limite inferiore: `MERGE RANGE(N'2015-10-31T23:59:59.999'` dopo la rimozione dei dati per ottobre.
5. Nel passaggio (7) suddividere la funzione di partizione creando un nuovo limite superiore: `SPLIT RANGE (N'2016-04-30T23:59:59.999'` dopo la rimozione dei dati per ottobre.

La soluzione ottimale consiste tuttavia nell'eseguire uno script di Transact-SQL generico, in grado di eseguire l'azione appropriata ogni mese, senza modifiche allo script. È possibile generalizzare lo script precedente in modo che reagisca ai parametri specificati, ovvero un limite inferiore da unire e un nuovo limite che verrà creato con la suddivisione della partizione. Per evitare di creare ogni mese una tabella di staging, è possibile crearne una prima e riutilizzarla cambiando il vincolo di verifica in modo che corrisponda alla partizione che verrà disattivata. Per informazioni su [come automatizzare completamente la finestra temporale scorrevole](/previous-versions/sql/sql-server-2005/administrator/aa964122(v=sql.90)) usando uno script di Transact-SQL, vedere le pagine seguenti.

### <a name="performance-considerations-with-table-partitioning"></a>Considerazioni sulle prestazioni con il partizionamento delle tabelle

È importante eseguire le operazioni MERGE e SPLIT RANGE per evitare qualsiasi spostamento di dati, perché lo spostamento di dati può provocare un overhead significativo delle prestazioni. Per altre informazioni, vedere [Modificare una funzione di partizione](../../relational-databases/partitions/modify-a-partition-function.md). Per ottenere questo risultato, usare RANGE LEFT invece di RANGE RIGHT quando si esegue l'istruzione [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md).

Di seguito una spiegazione visiva del significato delle opzioni RANGE LEFT e RANGE RIGHT:

![Diagramma che mostra le opzioni RANGE LEFT e RANGE RIGHT.](../../relational-databases/tables/media/partitioning3.png "Partizionamento 3")

Quando si definisce una funzione di partizione come RANGE LEFT, i valori specificati corrispondono ai limiti superiori delle partizioni. Quando si usa RANGE RIGHT, i valori specificati sono i limiti inferiori delle partizioni. Quando si usa l'operazione MERGE RANGE per rimuovere un limite dalla definizione della funzione di partizione, l'implementazione sottostante rimuove anche la partizione che contiene il limite. Se tale partizione non è vuota, i dati verranno spostati nella partizione che è il risultato dell'operazione MERGE RANGE.

In uno scenario con finestra temporale scorrevole, si rimuove sempre il limite inferiore della partizione.

- Caso RANGE LEFT: con RANGE LEFT il limite inferiore della partizione appartiene alla partizione 1, che è vuota, dopo il cambio di partizioni, pertanto MERGE RANGE non provocherà alcuno spostamento di dati.
- Caso RANGE RIGHT: nel caso di RANGE RIGHT, il limite inferiore della partizione appartiene alla partizione 2, che non è vuota perché è stato presupposto che la partizione 1 sia stata vuotata dalla disattivazione. In questo caso MERGE RANGE provocherà lo spostamento di dati, perché i dati dalla partizione 2 verranno spostati nella partizione 1. Per evitare questo problema, è necessario che RANGE RIGHT nello scenario con finestra temporale scorrevole abbia la partizione 1, che è sempre vuota. Se si usa RANGE RIGHT, è quindi necessario creare e mantenere una partizione aggiuntiva rispetto al caso di RANGE LEFT.

**Conclusione**: l'uso di RANGE LEFT nella partizione con finestra temporale scorrevole è molto più semplice per la gestione delle partizioni e consente di evitare lo spostamento dei dati. La definizione dei limiti delle partizioni con RANGE RIGHT risulta tuttavia leggermente più semplice, perché non è necessario gestire i problemi di data/ora di tipo "time tick".

## <a name="using-custom-cleanup-script-approach"></a>Uso dell'approccio con script di pulizia personalizzato

Nei casi in cui gli approcci con Stretch Database e con partizionamento delle tabelle non sono opzioni valide, il terzo approccio consiste nell'eliminare i dati dalla tabella di cronologia usando uno script di pulizia personalizzato. L'eliminazione dei dati dalla tabella di cronologia è possibile solo se **SYSTEM_VERSIONING = OFF**. Per evitare l'incoerenza dei dati, eseguire una pulizia durante la finestra di manutenzione, quando i carichi di lavoro che modificano i dati non sono attivi, oppure entro una transazione, bloccando effettivamente altri carichi di lavoro. Questa operazione richiede l'autorizzazione **CONTROL** sulla tabella corrente e sulla tabella di cronologia.

Per ridurre al minimo il blocco delle applicazioni normali e delle query utente, eliminare i dati in blocchi ridotti, con un ritardo durante l'esecuzione dello script di pulizia all'interno di una transazione. Anche se non esistono dimensioni ottimali per ogni blocco di dati da eliminare per tutti gli scenari, l'eliminazione di più di 10.000 righe in una singola transazione potrebbe avere un impatto significativo.

La logica di pulizia è uguale per ogni tabella temporale, quindi è possibile automatizzare la pulizia in modo relativamente semplice tramite una stored procedure generica pianificata per l'esecuzione periodica per ogni tabella temporale di cui si vuole limitare la cronologia dei dati.

Il diagramma seguente illustra come organizzare la logica di pulizia per una singola tabella, in modo da ridurre l'impatto sui carichi di lavoro in esecuzione.

![Diagramma che mostra come organizzare la logica di pulizia per una singola tabella, in modo da ridurre l'impatto sui carichi di lavoro in esecuzione.](../../relational-databases/tables/media/customcleanupscriptdiagram.png "Diagramma script pulizia personalizzata")

Ecco alcune indicazioni generali per l'implementazione del processo. Pianificare la logica di pulizia in modo che venga eseguita ogni giorno ed eseguire l'iterazione su tutte le tabelle temporali che necessitano della pulizia dei dati. Usare SQL Server Agent o uno strumento diverso per pianificare questo processo:

- Eliminare i dati cronologici in ogni tabella temporale a partire dalle righe più vecchie fino alle righe più recenti in diverse iterazioni in piccoli blocchi ed evitare di eliminare tutte le righe in una singola transazione, come illustrato nella figura precedente.
- Implementare ogni iterazione come una chiamata della stored procedure generica che rimuove una parte di dati dalla tabella di cronologia. Per informazioni su questa procedura, vedere l'esempio di codice seguente.
- Calcolare il numero di righe da eliminare per una singola tabella temporale ogni volta che si chiama il processo. In base a tale valore e al numero di iterazioni desiderato, determinare i punti di suddivisione dinamici per ogni chiamata di procedura.
- Pianificare un periodo di ritardo tra le iterazioni per una singola tabella, in modo da ridurre l'impatto sulle applicazioni che accedono alla tabella temporale.

Una stored procedure che elimina i dati per una singola tabella temporale potrebbe avere un aspetto analogo a quello del frammento di codice seguente. Verificare attentamente il codice e modificarlo prima di applicarlo all'ambiente specifico:

```sql
DROP PROCEDURE IF EXISTS sp_CleanupHistoryData;
GO

CREATE PROCEDURE sp_CleanupHistoryData
        @temporalTableSchema sysname
      , @temporalTableName sysname
      , @cleanupOlderThanDate datetime2
AS
    DECLARE @disableVersioningScript nvarchar(max) = '';
    DECLARE @deleteHistoryDataScript nvarchar(max) = '';
    DECLARE @enableVersioningScript nvarchar(max) = '';

DECLARE @historyTableName sysname
DECLARE @historyTableSchema sysname
DECLARE @periodColumnName sysname

/*Generate script to discover history table name and end of period column for given temporal table name*/
EXECUTE sp_executesql
    N'SELECT @hst_tbl_nm = t2.name, @hst_sch_nm = s.name, @period_col_nm = c.name
        FROM sys.tables t1
            JOIN sys.tables t2 on t1.history_table_id = t2.object_id
        JOIN sys.schemas s on t2.schema_id = s.schema_id
            JOIN sys.periods p on p.object_id = t1.object_id
           JOIN sys.columns c on p.end_column_id = c.column_id and c.object_id = t1.object_id
                  WHERE
                 t1.name = @tblName and s.name = @schName'
                , N'@tblName sysname
                , @schName sysname
                , @hst_tbl_nm sysname OUTPUT
                , @hst_sch_nm sysname OUTPUT
                , @period_col_nm sysname OUTPUT'
                , @tblName = @temporalTableName
                , @schName = @temporalTableSchema
                , @hst_tbl_nm = @historyTableName OUTPUT
                , @hst_sch_nm = @historyTableSchema OUTPUT
                , @period_col_nm = @periodColumnName OUTPUT
  
IF @historyTableName IS NULL OR @historyTableSchema IS NULL OR @periodColumnName IS NULL
    THROW 50010, 'History table cannot be found. Either specified table is not system-versioned temporal or you have provided incorrect argument values.', 1

/*Generate 3 statements that will run inside a transaction:
  (1) SET SYSTEM_VERSIONING = OFF,
  (2) DELETE FROM history_table,
  (3) SET SYSTEM_VERSIONING = ON
  On SQL Server 2016, it is critical that (1) and (2) run in separate EXEC statements, or SQL Server will generate the following error:
  Msg 13560, Level 16, State 1, Line XXX
  Cannot delete rows from a temporal history table '<database_name>.<history_table_schema_name>.<history_table_name>'.
*/
SET @disableVersioningScript = @disableVersioningScript + 'ALTER TABLE [' + @temporalTableSchema + '].[' + @temporalTableName + '] SET (SYSTEM_VERSIONING = OFF)'
SET @deleteHistoryDataScript = @deleteHistoryDataScript + ' DELETE FROM [' + @historyTableSchema + '].[' + @historyTableName + ']
    WHERE ['+ @periodColumnName + '] < ' + '''' + convert(varchar(128), @cleanupOlderThanDate, 126) + ''''
SET @enableVersioningScript = @enableVersioningScript + ' ALTER TABLE [' + @temporalTableSchema + '].[' + @temporalTableName + ']
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = [' + @historyTableSchema + '].[' + @historyTableName + '], DATA_CONSISTENCY_CHECK = OFF )); '

BEGIN TRAN
    EXEC (@disableVersioningScript);
    EXEC (@deleteHistoryDataScript);
    EXEC (@enableVersioningScript);
COMMIT;
```

## <a name="using-temporal-history-retention-policy-approach"></a>Uso dell'approccio con criteri di conservazione della cronologia temporale

> [!NOTE]
> l'uso dell'approccio con criteri di conservazione della cronologia temporale si applica a [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] e a SQL Server 2017 a partire da CTP 1.3.

La conservazione della cronologia temporale può essere configurata a livello di singola tabella. Ciò consente agli utenti di creare criteri di aging flessibili. L'applicazione della conservazione della cronologia temporale è semplice e richiede l'impostazione di un solo parametro durante la creazione della tabella o la modifica dello schema.

Dopo che sono stati definiti i criteri di conservazione, il database SQL di Azure inizia a verificare periodicamente la presenza di righe di cronologia con i requisiti per la pulizia automatica dei dati. L'identificazione di righe corrispondenti e la loro rimozione dalla tabella di cronologia si verificano in modo trasparente nell'attività in background pianificata ed eseguita dal sistema. La condizione cronologica delle righe della tabella viene verificata in base alla colonna che rappresenta la fine del periodo SYSTEM_TIME. Se ad esempio il periodo di conservazione impostato è pari a sei mesi, le righe di tabella idonee per la rimozione soddisfano la condizione seguente:

```sql
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
```
Nell'esempio precedente si presuppone che la colonna ValidTo corrisponda alla fine del periodo SYSTEM_TIME.

### <a name="how-to-configure-retention-policy"></a>Come si configurano i criteri di conservazione?

Prima di configurare i criteri di conservazione per una tabella temporale, verificare se la conservazione cronologica temporale è abilitata a livello di database:

```sql
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
```

Il flag del database **is_temporal_history_retention_enabled** è ON per impostazione predefinita, ma gli utenti possono modificarlo con l'istruzione ALTER DATABASE. Il flag viene anche impostato automaticamente su OFF dopo l'operazione di ripristino temporizzato. Per impostare la pulizia della cronologia temporale per il database eseguire l'istruzione seguente:

```sql
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION ON
```

I criteri di conservazione vengono configurati durante la creazione della tabella specificando un valore per il parametro HISTORY_RETENTION_PERIOD:

```sql
CREATE TABLE dbo.WebsiteUserInfo
(
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )
 WITH
 (
    SYSTEM_VERSIONING = ON
    (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
    )
 );
```

È possibile specificare il periodo di conservazione con unità di tempo diverse: DAYS, WEEKS, MONTHS e YEARS. Se HISTORY_RETENTION_PERIOD viene omesso, viene usata la conservazione INFINITE. È anche possibile usare esplicitamente la parola chiave INFINITE.
In alcuni scenari risulta utile configurare la conservazione dopo la creazione della tabella o per modificare un valore configurato in precedenza. In questi casi usare l'istruzione ALTER TABLE:

```sql
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
```

Per esaminare lo stato corrente del criterio di conservazione usare la seguente query, che unisce il flag di abilitazione della conservazione temporale a livello di database con i periodi di conservazione per le singole tabelle:

```sql
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName, SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
```

### <a name="how-sql-database-deletes-aged-rows"></a>In che modo vengono eliminate le righe obsolete dal database SQL?

Il processo di pulizia dipende dal layout dell'indice della tabella di cronologia. È importante notare che *solo nelle tabelle di cronologia con un indice cluster (albero B o columnstore) è possibile configurare criteri di conservazione finiti*. Viene creata un'attività in background per eseguire la pulizia dei dati obsoleti per tutte le tabelle temporali con periodo di conservazione finito. La logica di pulizia per l'indice rowstore cluster (albero B) elimina le righe obsolete in gruppi più piccoli (fino a 10000 unità), riducendo il carico di lavoro del log di database e del sottosistema I/O. Anche se la logica di pulizia usa l'indice albero B richiesto, l'ordine di eliminazione delle righe con durata superiore al periodo di conservazione non può essere garantito con certezza. Di conseguenza *evitare qualsiasi dipendenza dall'ordine di pulizia nelle applicazioni*.

L'attività di pulizia per il columnstore cluster rimuove interi gruppi (ognuno in genere costituito da un milione di righe) in una sola operazione. Questo approccio è molto efficiente, soprattutto quando vengono generati dati cronologici a ritmi elevati.

![Conservazione columnstore cluster](../../relational-databases/tables/media/cciretention.png "Conservazione columnstore cluster")

Un'ottima compressione dei dati e una pulizia efficiente dei dati conservati fanno dell'indice columnstore cluster la soluzione ottimale per gli scenari in cui il carico di lavoro genera rapidamente volumi elevati di dati cronologici. Questo scenario è tipico di carichi di lavoro di elaborazione transazioni intensiva, che usano le tabelle temporali per il rilevamento e il controllo delle modifiche, l'analisi dei trend o l'inserimento di dati IoT.

Per altre informazioni, vedere [Gestire i dati cronologici nelle tabelle temporali con criteri di conservazione](/azure/sql-database/sql-database-temporal-tables-retention-policy).

## <a name="next-steps"></a>Passaggi successivi

- [Tabelle temporali](../../relational-databases/tables/temporal-tables.md)
- [Introduzione alle tabelle temporali con controllo delle versioni di sistema](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [Verifiche di coerenza del sistema della tabella temporale](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Partizionamento con le tabelle temporali](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [Considerazioni e limitazioni delle tabelle temporali](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [Sicurezza di una tabella temporale](../../relational-databases/tables/temporal-table-security.md)
- [Tabelle temporali con controllo delle versioni di sistema con tabelle con ottimizzazione per la memoria](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [Funzioni e viste per i metadati delle tabelle temporali](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)