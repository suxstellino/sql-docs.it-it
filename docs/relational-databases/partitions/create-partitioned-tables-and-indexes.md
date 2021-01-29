---
description: Creare tabelle e indici partizionati
title: Creare tabelle e indici partizionati | Microsoft Docs
ms.custom: ''
ms.date: 1/5/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.createpartition.progress.f1
- sql13.swb.createpartition.partitioncolumn.f1
- sql13.swb.createpartition.createjob.f1
- sql13.swb.createpartition.finish.f1
- sql13.swb.createpartition.selectoutput.f1
- sql13.swb.createpartition.partitionfunction.f1
- sql13.swb.createpartition.partitionscheme.f1
- sql13.swb.createpartition.getstart.f1
- sql13.swb.createpartition.mappartition.f1
- sql13.swb.createpartition.summary.f1
helpviewer_keywords:
- partitioned indexes [SQL Server], creating
- partition schemes [SQL Server], creating
- partition functions [SQL Server], creating
- partitioned tables [SQL Server], creating
- partition functions [SQL Server]
- partition schemes [SQL Server]
ms.assetid: 7641df10-1921-42a7-ba6e-4cb03b3ba9c8
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 273e6f625b0a86be179da529e8b52e0d7ce991cc
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049154"
---
# <a name="create-partitioned-tables-and-indexes"></a>Creare tabelle e indici partizionati
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Utilizzando [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] è possibile creare una tabella o un indice partizionato in [!INCLUDE[tsql](../../includes/tsql-md.md)]. I dati delle tabelle e degli indici partizionati vengono suddivisi orizzontalmente in unità che possono essere distribuite in più filegroup di un database. Il partizionamento semplifica la gestione delle tabelle e degli indici di grandi dimensioni e li rende più scalabili.  
  
 La creazione di una tabella o di un indice partizionato richiede generalmente quattro operazioni:  
  
1.  Creazione di uno o più filegroup e dei file corrispondenti in cui saranno incluse le partizioni specificate dal relativo schema.  
  
2.  Creazione di una funzione di partizione tramite cui viene eseguito il mapping delle righe di una tabella o di un indice alle partizioni in base ai valori della colonna specificata.  
  
3.  Creazione di uno schema di partizione tramite cui viene eseguito il mapping delle partizioni di una tabella o un indice partizionato ai nuovi filegroup.  
  
4.  Creazione o modifica di una tabella o un indice e specificazione dello schema di partizione come percorso di archiviazione.  
 
> [!NOTE]
> In Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] sono supportati solo i filegroup primari.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per creare una tabella o un indice partizionato tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   L'ambito di una funzione e di uno schema di partizione è limitato al database in cui sono stati creati. All'interno del database le funzioni di partizione si trovano in uno spazio dei nomi separato rispetto ad altre funzioni.  
  
-   Se in qualsiasi riga all'interno di una funzione di partizione sono disponibili colonne di partizionamento con valori Null, queste righe vengono allocate alla partizione più a sinistra. Tuttavia, se è specificato NULL come valore limite ed è indicato RIGHT, la partizione più a sinistra rimane vuota e i valori NULL vengono collocati nella seconda partizione.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 La creazione di una tabella partizionata richiede l'autorizzazione CREATE TABLE per il database e l'autorizzazione ALTER per lo schema in cui viene creata la tabella. La creazione di un indice partizionato richiede l'autorizzazione ALTER per la tabella o la vista in cui viene creato l'indice. Per la creazione di una tabella o un indice partizionato è richiesta una delle seguenti autorizzazioni aggiuntive:  
  
-   Autorizzazione ALTER ANY DATASPACE. Questa autorizzazione viene concessa per impostazione predefinita al ruolo predefinito del server **sysadmin** e ai ruoli predefiniti del database **db_owner** e **db_ddladmin** .  
  
-   Autorizzazione CONTROL o ALTER per il database in cui vengono creati la funzione e lo schema di partizione.  
  
-   Autorizzazione CONTROL SERVER o ALTER ANY DATABASE per il server del database in cui vengono creati la funzione e lo schema di partizione.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
 Per creare uno o più filegroup, i file corrispondenti e una tabella attenersi ai passaggi della procedura riportata di seguito. Gli oggetti della procedura descritta di seguito verranno utilizzati come riferimento quando si crea la tabella partizionata.  
  
#### <a name="to-create-new-filegroups-for-a-partitioned-table"></a>Per creare nuovi filegroup per una tabella partizionata  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse sul database in cui si vuole creare una tabella partizionata e scegliere **Proprietà**.  
  
2.  Nella finestra di dialogo **Proprietà database -** *nome_database* selezionare **Filegroup** in **Selezione pagina**.  
  
3.  In **Righe** fare clic su **Aggiungi**. Nella nuova riga immettere il nome del filegroup.  
  
    > [!WARNING]  
    >  Quando si specificano più filegroup, è necessario disporre sempre di un filegroup aggiuntivo oltre al numero di filegroup specificati per i valori limite durante la creazione delle partizioni.  
  
4.  Continuare ad aggiungere righe finché non vengono creati tutti i filegroup per la tabella partizionata.  
  
5.  Fare clic su **OK**.  
  
6.  In **Selezione pagina** selezionare **File**.  
  
7.  In **Righe** fare clic su **Aggiungi**. Nella nuova riga immettere un nome di file e selezionare un filegroup.  
  
8.  Continuare ad aggiungere righe finché non viene creato almeno un file per ogni filegroup.  
  
9. Espandere la cartella **Tabelle** e creare una tabella normalmente. Per altre informazioni, vedere [Creare tabelle &#40;Motore di database&#41;](../../relational-databases/tables/create-tables-database-engine.md). In alternativa, è possibile specificare una tabella esistente nella procedura descritta di seguito.  
  
#### <a name="to-create-a-partitioned-table"></a>Per creare una tabella partizionata  
  
1.  Fare clic con il pulsante destro del mouse sulla tabella che si vuole partizionare e scegliere **Archiviazione** e quindi selezionare **Create Partition (Crea partizione)**.  
  
2.  Nella pagina **Creazione guidata partizione** della **relativa procedura guidata** fare clic su **Avanti**.  
  
3.  Nella griglia **Colonne di partizionamento disponibili** della pagina **Seleziona una colonna di partizionamento** selezionare la colonna in cui partizionare la tabella. Nella griglia **Colonne di partizionamento disponibili** verranno visualizzate solo le colonne con i tipi di dati che possono essere utilizzati per partizionare dati. Se come colonna di partizionamento se ne sceglie una calcolata, la colonna deve essere definita come persistente.  
  
     Le scelte disponibili per la colonna di partizionamento e per l'intervallo di valori sono determinate innanzitutto dalla possibilità di raggruppare i dati in modo logico. È possibile, ad esempio, scegliere di dividere i dati in raggruppamenti logici in base ai mesi o ai trimestri di un anno. Le query che verranno eseguite sui dati consentiranno di determinare se questo raggruppamento logico sia appropriato per la gestione delle partizioni delle tabelle. Come colonne di partizionamento possono essere usati tutti i tipi di dati tranne **text**, **ntext**, **image**, **xml**, **timestamp**, **varchar(max)**, **nvarchar(max)**, **varbinary(max)**, tipi di dati alias o tipi di dati CLR definiti dall'utente.  
  
     In questa pagina sono disponibili le seguenti opzioni aggiuntive:  
  
     **Colloca tabella nella tabella partizionata selezionata**  
     Consente di selezionare una tabella partizionata contenente dati correlati e di creare un join con la tabella nella colonna di partizionamento. Le query sulle tabelle con partizioni unite nelle colonne di partizionamento vengono in genere eseguite in modo più efficiente.  
  
     **Allinea nello spazio di archiviazione indici univoci e non univoci con una colonna di partizione indicizzata**  
     Consente di allineare tutti gli indici della tabella partizionati con lo stesso schema di partizione. Quando una tabella e i relativi indici sono allineati, è possibile spostare in modo più efficace le partizioni all'interno e all'esterno delle tabelle partizionate, in quanto i dati vengono partizionati con lo stesso algoritmo.  
  
     Dopo aver selezionato la colonna di partizionamento e qualsiasi altra opzione, fare clic su **Avanti**.  
  
4.  In **Seleziona funzione di partizione** della pagina **Seleziona una funzione di partizione** fare clic su **Nuova funzione di partizione** o **Funzione di partizione esistente**. Se si sceglie **Nuova funzione di partizione**, immettere il nome della funzione. Se si sceglie **Funzione di partizione esistente**, selezionare nell'elenco il nome della funzione che si desidera utilizzare. L'opzione **Funzione di partizione esistente** non sarà disponibile se non sono presenti altre funzioni di partizione nel database.  
  
     Dopo aver completato questa pagina, fare clic su **Avanti**.  
  
5.  In **Seleziona schema di partizione** della pagina **Seleziona uno schema di partizione** fare clic su **Nuovo schema di partizione** o **Schema di partizione esistente**. Se si sceglie **Nuovo schema di partizione**, immettere il nome dello schema. Se si sceglie **Schema di partizione esistente**, selezionare nell'elenco il nome dello schema che si desidera utilizzare. L'opzione **Schema di partizione esistente** non sarà disponibile se non sono presenti altri schemi di partizione nel database.  
  
     Dopo aver completato questa pagina, fare clic su **Avanti**.  
  
6.  In **Intervallo** della pagina **Esegui mapping partizioni** selezionare **Limite sinistro** o **Limite destro** per specificare se includere il valore di delimitazione superiore o inferiore all'interno di ogni filegroup creato. Quando si specificano più filegroup, è necessario immettere sempre un filegroup aggiuntivo oltre al numero di filegroup specificati per i valori limite durante la creazione delle partizioni.  
  
     In **Filegroup** nella griglia **Selezionare i filegroup e specificare i valori limite** selezionare il filegroup in cui si desidera partizionare i dati. In **Limite** immettere il valore limite per ogni filegroup. Se il valore limite viene lasciato vuoto, la funzione di partizione consente di eseguire il mapping dell'intera tabella o dell'intero indice a un'unica partizione tramite il nome della funzione di partizione.  
  
     In questa pagina sono disponibili le seguenti opzioni aggiuntive:  
  
     **Imposta limiti...**  
     Consente di aprire la finestra di dialogo **Imposta valori limite** per selezionare i valori limite e gli intervalli di date desiderati per le partizioni. Questa opzione è disponibile solo se è stata selezionata una colonna di partizionamento contenente uno dei tipi di dati seguenti: **date**, **datetime**, **smalldatetime**, **datetime2** o **datetimeoffset**.  
  
     **Valuta spazio di archiviazione**  
     Consente di valutare il numero di righe, lo spazio necessario e quello disponibile per l'archiviazione di ciascun filegroup specificato per le partizioni. Questi valori vengono visualizzati nella griglia come valori di sola lettura.  
  
     Nella finestra di dialogo **Imposta valori limite** sono inoltre disponibili le seguenti opzioni aggiuntive:  
  
     **Data inizio**  
     Consente di selezionare la data di inizio per i valori di intervallo delle partizioni.  
  
     **Data fine**  
     Consente di selezionare la data di fine per i valori di intervallo delle partizioni. Se è stata selezionata l'opzione **Limite sinistro** nella pagina **Esegui mapping partizioni** , questa data costituirà l'ultimo valore per ogni filegroup/partizione. Se è stata selezionata l'opzione **Limite destro** nella pagina **Esegui mapping partizioni** , questa data costituirà il primo valore nel penultimo filegroup.  
  
     **Intervallo di date**  
     Consente di selezionare la granularità della data o l'incremento dei valori di intervallo desiderato per ogni partizione.  
  
     Dopo aver completato questa pagina, fare clic su **Avanti**.  
  
7.  Nella pagina **Seleziona un'opzione di output** specificare il modo in cui si desidera completare la tabella partizionata. Selezionare **Crea script** per creare uno script SQL in base alle pagine precedenti della procedura guidata. Selezionare **Esegui immediatamente** per creare la nuova tabella partizionata dopo aver completato tutte le pagine rimanenti della procedura guidata. Selezionare **Pianifica** per creare la nuova tabella partizionata in un momento predeterminato nel futuro.  
  
     Se si seleziona **Crea script**, in **Opzioni di scripting** sono disponibili le opzioni seguenti:  
  
     **Genera script nel file**  
     Genera lo script come file con estensione sql. Immettere un nome di file e il percorso nella casella **Nome file** o fare clic su **Sfoglia** per aprire la finestra di dialogo **Percorso file script** . In **Salva con nome** selezionare **Testo Unicode** o **Testo ANSI**.  
  
     **Genera script negli Appunti**  
     Salva lo script negli Appunti.  
  
     **Genera script in nuova finestra Query**  
     Genera lo script in una nuova finestra dell'editor di query. Si tratta della selezione predefinita.  
  
     Se si seleziona **Pianifica**, fare clic su **Cambia pianificazione**.  
  
    1.  Nella casella **Nome** della finestra di dialogo **Nuova pianificazione processo** immettere il nome della pianificazione del processo.  
  
    2.  Nell'elenco **Tipo pianificazione** selezionare il tipo di pianificazione:  
  
        -   **Avvia automaticamente all'avvio di SQL Server Agent**  
  
        -   **Avvia quando la CPU risulta inattiva**  
  
        -   **Periodica**. Selezionare questa opzione se la nuova tabella partizionata viene aggiornata regolarmente con nuove informazioni.  
  
        -   **Singola occorrenza**. Si tratta della selezione predefinita.  
  
    3.  Selezionare o deselezionare la casella di controllo **Abilitata** per abilitare o disabilitare la pianificazione.  
  
    4.  Se si seleziona **Periodica**:  
  
        1.  In **Frequenza** nell'elenco **Ricorrenza** specificare la frequenza di occorrenza:  
  
            -   Se si seleziona **Giornaliera**, nella casella **Ogni** immettere la frequenza in base alla quale si ripete la pianificazione del processo nei giorni.  
  
            -   Se si seleziona **Settimanale**, nella casella **Ogni** immettere la frequenza in base alla quale si ripete la pianificazione del processo nelle settimane. Selezionare i giorni della settimana durante i quali viene eseguita la pianificazione del processo.  
  
            -   Se si seleziona **Mensile**, selezionare **Giorno** oppure **Ogni**.  
  
                -   Se si seleziona **Giorno**, immettere sia la data del mese in cui si desidera sia eseguita la pianificazione del processo sia la frequenza in base alla quale si ripete questa pianificazione nei mesi. Ad esempio, se si vuole che la pianificazione del processo sia eseguita il giorno 15 del mese a mesi alterni, selezionare **Giorno** e immettere "15" nella prima casella e "2" nella seconda casella. Si noti che il numero più grande consentito nella seconda casella è "99".  
  
                -   Se si sceglie **Ogni**, selezionare il giorno specifico della settimana del mese in cui si desidera sia eseguita la pianificazione del processo e la frequenza in base alla quale si ripete questa pianificazione nei mesi. Ad esempio, se si vuole che la pianificazione del processo sia eseguita l'ultimo giorno feriale del mese a mesi alterni, selezionare **Giorno**, selezionare **ultimo** nel primo elenco e **giorno feriale** nel secondo elenco, quindi immettere "2" nell'ultima casella. Nei primi due elenchi è anche possibile selezionare **primo**, **secondo**, **terzo** o **quarto**, nonché i giorni della settimana specifici, ad esempio: domenica o mercoledì. Si noti che il numero più grande consentito nell'ultima casella è "99".  
  
        2.  In **Frequenza giornaliera** specificare la frequenza in base alla quale si ripete la pianificazione del processo in quel determinato giorno:  
  
            -   Se si seleziona **Una sola volta alle**, immettere l'ora specifica del giorno in cui deve essere eseguita la pianificazione del processo nella casella **Una sola volta alle** . Immettere l'ora, il minuto e il secondo del giorno, nonché AM o PM.  
  
            -   Se si seleziona **Ogni** specificare la frequenza in base alla quale la pianificazione del processo viene eseguita durante il giorno scelto in **Frequenza**. Ad esempio, se si vuole che la pianificazione del processo sia ripetuta ogni 2 ore durante il giorno scelto per questa pianificazione, selezionare **Ogni**, immettere "2" nella prima casella e quindi selezionare **ora/e** nell'elenco. In questo elenco è anche possibile selezionare **minuto/i** e **secondo/i**. Si noti che il numero più grande consentito nella prima casella è "100".  
  
                 Nella casella **A partire dalle** immettere l'ora in cui dovrebbe iniziare l'esecuzione della pianificazione del processo. Nella casella **Fino alle** immettere l'ora in cui dovrebbe terminare la ripetizione della pianificazione del processo. Immettere l'ora, il minuto e il secondo del giorno, nonché AM o PM.  
  
        3.  In **Durata** di **Data inizio** immettere la data in cui si desidera sia avviata l'esecuzione della pianificazione del processo. Selezionare **Data fine** o **Nessuna data di fine** per indicare quando dovrebbe terminare l'esecuzione della pianificazione del processo. Se si seleziona **Data fine** immettere la data in cui si desidera venga terminata l'esecuzione della pianificazione del processo.  
  
    5.  Se si seleziona **Singola occorrenza**, in **Singola occorrenza**, nella casella **Data** immettere la data in cui verrà eseguita la pianificazione del processo. Nella casella **Ora** immettere l'ora in cui verrà eseguita la pianificazione del processo. Immettere l'ora, il minuto e il secondo del giorno, nonché AM o PM.  
  
    6.  In **Descrizione** in **Riepilogo** verificare che tutte le impostazioni della pianificazione del processo siano corrette.  
  
    7.  Fare clic su **OK**.  
  
     Dopo aver completato questa pagina, fare clic su **Avanti**.  
  
8.  In **Controlla selezioni** della pagina **Controlla riepilogo** espandere tutte le opzioni disponibili per verificare che tutte le impostazioni della partizione siano corrette. Se tutte le impostazioni sono corrette, fare clic su **Fine**.  
  
9. Nella pagina **Stato Creazione guidata partizione** monitorare le informazioni sullo stato delle azioni della Creazione guidata partizione. A seconda delle opzioni selezionate nella procedura guidata, la pagina di stato può contenere una o più azioni. Nella casella superiore viene visualizzato lo stato complessivo della procedura guidata e viene indicato il numero di messaggi di stato, di errore e di avviso restituiti durante l'esecuzione della procedura guidata.  
  
     Le opzioni seguenti sono disponibili nella pagina **Stato Creazione guidata partizione** :  
  
     **Dettagli**  
     Consente di visualizzare i messaggi di azione, di stato e di altro tipo restituiti dall'azione eseguita nella procedura guidata.  
  
     **Azione**  
     Specifica il tipo e il nome di ciascuna azione.  
  
     **Status**  
     Indica se l'intera azione della procedura guidata ha restituito il valore **Esito positivo** o **Esito negativo**.  
  
     **Messaggio**  
     Fornisce tutti i messaggi di errore o di avviso restituiti dal processo.  
  
     **Report**  
     Crea un report contenente i risultati della Creazione guidata partizione. Le opzioni sono **Visualizza report**, **Salva report su file**, **Copia report negli Appunti** e **Invia report per posta elettronica**.  
  
     **Visualizza report**  
     Apre la finestra di dialogo **Visualizza report** in cui è contenuto un report di testo dello stato della Creazione guidata partizione.  
  
     **Salva report su file**  
     Apre la finestra di dialogo **Salva report con nome** .  
  
     **Copia report negli Appunti**  
     Copia i risultati del report dello stato della procedura guidata negli Appunti.  
  
     **Invia report per posta elettronica**  
     Copia i risultati del report dello stato della procedura guidata in un messaggio di posta elettronica.  
  
     Al termine, fare clic su **Chiudi**.  
  
 Creazione guidata partizione crea la funzione e lo schema di partizione, quindi applica il partizionamento alla tabella specificata. Per verificare il partizionamento della tabella, in Esplora oggetti fare clic con il pulsante destro del mouse sulla tabella e scegliere **Proprietà**. Fare clic sulla pagina **Archiviazione** . Nella pagina vengono visualizzate informazioni come il nome della funzione e dello schema di partizione e il numero di partizioni.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-create-a-partitioned-table"></a>Per creare una tabella partizionata  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. Nell'esempio vengono creati i nuovi filegroup, una funzione di partizione e un schema di partizione. Una nuova tabella viene creata con lo schema di partizione specificato come percorso di archiviazione.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Adds four new filegroups to the AdventureWorks2012 database  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test1fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test2fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test3fg;  
    GO  
    ALTER DATABASE AdventureWorks2012  
    ADD FILEGROUP test4fg;   
  
    -- Adds one file for each filegroup.  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = test1dat1,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t1dat1.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP test1fg;  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = test2dat2,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t2dat2.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP test2fg;  
    GO  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = test3dat3,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t3dat3.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP test3fg;  
    GO  
    ALTER DATABASE AdventureWorks2012   
    ADD FILE   
    (  
        NAME = test4dat4,  
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\t4dat4.ndf',  
        SIZE = 5MB,  
        MAXSIZE = 100MB,  
        FILEGROWTH = 5MB  
    )  
    TO FILEGROUP test4fg;  
    GO  
    -- Creates a partition function called myRangePF1 that will partition a table into four partitions  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
        AS RANGE LEFT FOR VALUES (1, 100, 1000) ;  
    GO  
    -- Creates a partition scheme called myRangePS1 that applies myRangePF1 to the four filegroups created above  
    CREATE PARTITION SCHEME myRangePS1  
        AS PARTITION myRangePF1  
        TO (test1fg, test2fg, test3fg, test4fg) ;  
    GO  
    -- Creates a partitioned table called PartitionTable that uses myRangePS1 to partition col1  
    CREATE TABLE PartitionTable (col1 int PRIMARY KEY, col2 char(10))  
        ON myRangePS1 (col1) ;  
    GO  
    ```  


#### <a name="to-create-a-partitioned-table-in-azure-sqldbesa"></a>Per creare una tabella partizionata in Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]

In Azure [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] l'aggiunta di file e gruppi di file non è supportata, ma il partizionamento delle tabelle è supportato tramite il partizionamento solo del filegroup PRIMARY.
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
1.  Sulla barra Standard fare clic su **Nuova query**.  
  
1.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. In questo esempio vengono creati una funzione di partizione e un schema di partizione. Una nuova tabella viene creata con lo schema di partizione specificato come percorso di archiviazione. 

    ```
    -- Creates a partition function called myRangePF1 that will partition a table into four partitions  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
        AS RANGE LEFT FOR VALUES (1, 100, 1000) ;  
    GO  
    -- Creates a partition scheme called myRangePS1 that applies myRangePF1 to the PRIMARY filegroup 
    CREATE PARTITION SCHEME myRangePS1  
        AS PARTITION myRangePF1  
        ALL TO ('PRIMARY') ;  
    GO  
    -- Creates a partitioned table called PartitionTable that uses myRangePS1 to partition col1  
    CREATE TABLE PartitionTable (col1 int PRIMARY KEY, col2 char(10))  
        ON myRangePS1 (col1) ;  
    GO
    ```  

  
#### <a name="to-determine-if-a-table-is-partitioned"></a>Per determinare se una tabella è partizionata  
  
1.  Tramite la query seguente vengono restituite una o più righe se la tabella `PartitionTable` è partizionata. Se la tabella non è partizionata, non viene restituita alcuna query.  
  
    ```  
    SELECT *   
    FROM sys.tables AS t   
    JOIN sys.indexes AS i   
        ON t.[object_id] = i.[object_id]   
        AND i.[type] IN (0,1)   
    JOIN sys.partition_schemes ps   
        ON i.data_space_id = ps.data_space_id   
    WHERE t.name = 'PartitionTable';   
    GO  
    ```  
  
#### <a name="to-determine-the-boundary-values-for-a-partitioned-table"></a>Per determinare i valori limite per una tabella partizionata  
  
1.  Tramite la query seguente vengono restituiti i valori limite per ogni partizione nella tabella `PartitionTable` .  
  
    ```  
    SELECT t.name AS TableName, i.name AS IndexName, p.partition_number, p.partition_id, i.data_space_id, f.function_id, f.type_desc, r.boundary_id, r.value AS BoundaryValue   
    FROM sys.tables AS t  
    JOIN sys.indexes AS i  
        ON t.object_id = i.object_id  
    JOIN sys.partitions AS p  
        ON i.object_id = p.object_id AND i.index_id = p.index_id   
    JOIN  sys.partition_schemes AS s   
        ON i.data_space_id = s.data_space_id  
    JOIN sys.partition_functions AS f   
        ON s.function_id = f.function_id  
    LEFT JOIN sys.partition_range_values AS r   
        ON f.function_id = r.function_id and r.boundary_id = p.partition_number  
    WHERE t.name = 'PartitionTable' AND i.type <= 1  
    ORDER BY p.partition_number;  
    ```  
  
#### <a name="to-determine-the-partition-column-for-a-partitioned-table"></a>Per determinare la colonna di partizione per una tabella partizionata  
  
1.  Tramite la query seguente viene restituito il nome della colonna di partizionamento per la tabella. `PartitionTable`.  
  
    ```  
    SELECT   
        t.[object_id] AS ObjectID   
        , t.name AS TableName   
        , ic.column_id AS PartitioningColumnID   
        , c.name AS PartitioningColumnName   
    FROM sys.tables AS t   
    JOIN sys.indexes AS i   
        ON t.[object_id] = i.[object_id]   
        AND i.[type] <= 1 -- clustered index or a heap   
    JOIN sys.partition_schemes AS ps   
        ON ps.data_space_id = i.data_space_id   
    JOIN sys.index_columns AS ic   
        ON ic.[object_id] = i.[object_id]   
        AND ic.index_id = i.index_id   
        AND ic.partition_ordinal >= 1 -- because 0 = non-partitioning column   
    JOIN sys.columns AS c   
        ON t.[object_id] = c.[object_id]   
        AND ic.column_id = c.column_id   
    WHERE t.name = 'PartitionTable' ;   
    GO  
    ```  
  
 Per altre informazioni, vedere:  
  
-   [Opzioni per file e filegroup ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)  
  
-   [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)  
  
-   [CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)  
  
-   [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
  
  
