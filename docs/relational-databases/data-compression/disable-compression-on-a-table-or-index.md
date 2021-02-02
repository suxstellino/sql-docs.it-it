---
title: Disabilitare la compressione in una tabella o un indice | Microsoft Docs
description: Informazioni su come disabilitare la compressione per una tabella o un indice usando SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- data compression [SQL Server], disabling
ms.assetid: bda1e452-397b-4757-82a4-181217361589
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016'
ms.openlocfilehash: 994c22db58c0d06b5a763769688d911cd739b367
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250714"
---
# <a name="disable-compression-on-a-table-or-index"></a>Disabilitare la compressione in una tabella o un indice

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In questo argomento viene descritto come disabilitare la compressione in una tabella o un indice in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per disabilitare la compressione in una tabella o un indice utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Se la tabella è un heap, l'operazione di ricompilazione per la modalità ONLINE sarà a thread singolo. Utilizzare la modalità OFFLINE per un'operazione di ricompilazione di heap multithread. Per altre informazioni sulla compressione dei dati, vedere [Compressione dei dati](../../relational-databases/data-compression/data-compression.md).  
  
-   Per valutare il modo in cui la modifica dello stato di compressione influirà su una tabella, un indice o una partizione, usare la stored procedure [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) .  
  
-   Non è possibile modificare l'impostazione di compressione di una singola partizione se la tabella include indici non allineati.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'autorizzazione ALTER per la tabella o l'indice.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-disable-compression-on-a-table"></a>Per disabilitare la compressione in una tabella  
  
1.  In Esplora oggetti espandere il database contenente la tabella in cui si desidera disabilitare la compressione, quindi espandere la cartella **Tabelle** .  
  
2.  Fare clic con il pulsante destro del mouse sulla tabella o sull'indice in cui si vuole disabilitare la compressione, scegliere **Archiviazione**, quindi selezionare **Gestione partizione...** .  
  
3.  Per disabilitare la compressione in un indice, espandere la tabella contenente l'indice che si desidera comprimere, quindi espandere la cartella **Indici** .  
  
4.  In Compressione guidata dati nella pagina **Compressione guidata dati** fare clic su **Avanti**.  
  
5.  Nella pagina **Seleziona tipo di compressione** selezionare **Nessuno** come tipo di compressione da applicare a ogni partizione nella tabella o nell'indice in cui si desidera disabilitare la compressione. Al termine, fare clic su **Avanti**.  
  
     Le opzioni seguenti sono disponibili nella pagina **Seleziona tipo di compressione** :  
  
     Casella di controllo **Usa lo stesso tipo di compressione per tutte le partizioni**  
     Selezionare questa opzione per configurare la stessa impostazione di compressione per tutte le partizioni. La casella di selezione viene abilitata e la colonna **Tipo di compressione** nella griglia viene disabilitata. Se viene selezionata, le opzioni nell'elenco adiacente sono **Nessuno**, **Riga** e **Pagina**.  
  
     **Numero partizioni**  
     Elenca tutte le partizioni nella tabella o nell'indice. Questa colonna è di sola lettura.  
  
     **Tipo di compressione**  
     Selezionare l'opzione di compressione per ciascuna partizione. Questa opzione non è disponibile se **Usa lo stesso tipo di compressione per tutte le partizioni** è selezionata. Le opzioni nell'elenco sono **Nessuno**, **Riga** e **Pagina**.  
  
     **Limite**  
     Visualizza il limite della partizione. Questa colonna è di sola lettura.  
  
     **Conteggio righe**  
     Visualizza il numero di righe nella partizione. Questa colonna è di sola lettura.  
  
     **Spazio corrente**  
     Visualizza lo spazio corrente occupato dalla partizione, espresso in megabyte (MB). Questa colonna è di sola lettura.  
  
     **Spazio compresso richiesto**  
     Quando si fa clic su **Calcola**, in questa colonna vengono visualizzate le dimensioni stimate di ciascuna partizione dopo la compressione eseguita mediante l'uso dell'impostazione specificata nella colonna **Tipo di compressione** . Questa colonna è di sola lettura.  
  
     **Calcola**  
     Fare clic per eseguire la stima delle dimensioni di ciascuna partizione dopo la compressione eseguita mediante l'uso dell'impostazione specificata nella colonna **Tipo di compressione** .  
  
6.  Nella pagina **Seleziona un'opzione di output** specificare il modo in cui si desidera completare l'attività. Selezionare **Crea script** per creare uno script SQL in base alle pagine precedenti della procedura guidata. Selezionare **Esegui immediatamente** per creare la nuova tabella partizionata dopo aver completato tutte le pagine rimanenti della procedura guidata. Selezionare **Pianifica** per creare la nuova tabella partizionata in un momento predeterminato nel futuro.  
  
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
  
7.  In **Verificare le selezioni** nella pagina **Controlla riepilogo** espandere tutte le opzioni disponibili per verificare che tutte le impostazioni siano corrette. Se tutte le impostazioni sono corrette, fare clic su **Fine**.  
  
8.  Nella pagina **Stato Compressione guidata** monitorare le informazioni sullo stato delle azioni della Creazione guidata partizione. A seconda delle opzioni selezionate nella procedura guidata, la pagina di stato può contenere una o più azioni. Nella casella superiore viene visualizzato lo stato complessivo della procedura guidata e viene indicato il numero di messaggi di stato, di errore e di avviso restituiti durante l'esecuzione della procedura guidata.  
  
     Nella pagina **Stato Compressione guidata** sono disponibili le opzioni seguenti:  
  
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
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-disable-compression-on-a-table"></a>Per disabilitare la compressione in una tabella  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    ALTER TABLE Person.Person REBUILD PARTITION = ALL  
    WITH (DATA_COMPRESSION = NONE);  
    GO  
    ```  
  
#### <a name="to-disable-compression-on-an-index"></a>Per disabilitare la compressione in un indice  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    ALTER INDEX AK_Person_rowguid ON Person.Person REBUILD PARTITION = ALL WITH (DATA_COMPRESSION = NONE);  
    GO  
    ```  
  
 Per altre informazioni, vedere [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) e [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
  
