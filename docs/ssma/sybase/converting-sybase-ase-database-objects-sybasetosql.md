---
description: Conversione di oggetti di database di ASE Sybase (SybaseToSQL)
title: Conversione di oggetti di database Sybase ASE (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Converting Database Objects
ms.assetid: 509cb65d-2f54-427a-83d7-37919cc4e3e3
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 07560faaa3fb3316864216859f79cd5e1b7ff950
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056836"
---
# <a name="converting-sap-ase-database-objects-sybasetosql"></a>Conversione di oggetti di database SAP ASE (SybaseToSQL)
Dopo essersi connessi a SAP Adaptive Server Enterprise (ASE), connesso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL e sono state impostate le opzioni di mapping di progetti e dati, è possibile convertire oggetti di database di SAP Adaptive Server Enterprise (ASE) in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o oggetti di database SQL di Azure.  
  
## <a name="the-conversion-process"></a>Processo di conversione  
La conversione di oggetti di database accetta le definizioni degli oggetti dall'ambiente del servizio app, le converte in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti simili o SQL Azure e quindi carica tali informazioni nei metadati SSMA. Non carica le informazioni nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o di Azure SQL. È quindi possibile visualizzare gli oggetti e le relative proprietà usando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati di Azure SQL.
  
Durante la conversione, SSMA stampa i messaggi di output nel riquadro di output e i messaggi di errore nel riquadro **Elenco errori** . Usare le informazioni sull'output e sull'errore per determinare se è necessario modificare i database dell'ambiente del servizio app o il processo di conversione per ottenere i risultati della conversione desiderati.  
  
## <a name="setting-conversion-options"></a>Impostazione delle opzioni di conversione  
Prima di convertire gli oggetti, esaminare le opzioni di conversione del progetto nella finestra di dialogo **Impostazioni progetto** . Utilizzando questa finestra di dialogo è possibile impostare il modo in cui SSMA converte le funzioni e le variabili globali. Per ulteriori informazioni, vedere [Impostazioni progetto &#40;conversione&#41; &#40;&#41;SybaseToSQL ](../../ssma/sybase/project-settings-conversion-sybasetosql.md).  
  
## <a name="converting-ase-database-objects"></a>Conversione di oggetti di database ASE  
Per convertire gli oggetti di database dell'ambiente del servizio app, selezionare prima gli oggetti da convertire e quindi fare in modo che SSMA esegua la conversione. Per visualizzare i messaggi di output durante la conversione, scegliere **output** dal menu **Visualizza** .  
  
**Per convertire gli oggetti dell'ambiente del servizio app in SQL Server o SQL Azure sintassi**  
  
1.  In Sybase Metadata Explorer espandere il server ASE, quindi espandere **database**.  
  
2.  Selezionare gli oggetti da convertire:  
  
    -   Per convertire tutti i database, selezionare la casella di controllo accanto a **database**.  
  
    -   Per convertire o omettere un database, selezionare o deselezionare la casella di controllo accanto al nome del database.  
  
    -   Per convertire o omettere singoli schemi, espandere il database, espandere **schemi**, quindi selezionare o deselezionare la casella di controllo accanto allo schema.  
  
    -   Per convertire o omettere una categoria di oggetti, espandere lo schema, quindi selezionare o deselezionare la casella di controllo accanto alla categoria.  
  
    -   Per convertire o omettere singoli oggetti, espandere la cartella Category, quindi selezionare o deselezionare la casella di controllo accanto all'oggetto.  
  
3.  Per convertire tutti gli oggetti selezionati, fare clic con il pulsante destro del mouse su **database**, quindi scegliere **Converti schema**.  
  
    È anche possibile convertire singoli oggetti o categorie di oggetti facendo clic con il pulsante destro del mouse sull'oggetto o sulla cartella che lo contiene, quindi selezionando **Converti schema**.  
  
> [!NOTE]  
> Alcune funzioni di sistema di SAP ASE non corrispondono esattamente alle funzioni di sistema SQL Server equivalenti nel comportamento. Per emulare il comportamento dell'ambiente del servizio app SAP, SSMA genera funzioni definite dall'utente nel database SQL Server convertito sotto uno schema denominato 2SS ' s. A seconda delle impostazioni del progetto, alcune delle SQL Server funzioni di sistema vengono sostituite con queste funzioni emulate. SSMA crea le seguenti funzioni definite dall'utente:  

:::row:::
    :::column:::
        **char_length_nvarchar**  
        **char_length_varchar**  
        **charindex_nvarchar**  
        **charindex_varchar**  
        **hextoint**  
        **index_colorder**  
    :::column-end:::
    :::column:::
        **inttohex**  
        **ssma_current_time**  
        **ssma_datediff**  
        **ssma_datepart**  
        **substring_nvarchar**  
        **substring_varbinary**  
    :::column-end:::
    :::column:::
        **substring_varchar**  
        **to_unichar**  
        **uhighsurr**  
        **ulowsurr**  
    :::column-end:::
:::row-end:::

## <a name="objects-not-supported-in-azure-sql"></a>Oggetti non supportati in SQL di Azure  
Le parole chiave T-SQL seguenti vengono usate da SSMA per SAP ASE durante la conversione a SQL Server in locale, ma queste parole chiave non sono supportate da SQL Azure sintassi T-SQL:  

:::row:::
    :::column:::
        CHECKPOINT  
        CREATE/ALTER/DROP DEFAULT  
        CREATE/DROP RULE  
        DBCC TRACEOFF  
        DBCC TRACEON  
    :::column-end:::
    :::column:::
        GRANT/REVOKE/DENY ALL  
        KILL  
        READTEXT  
        SELECT INTO  
        SET OFFSETS  
    :::column-end:::
    :::column:::
        SETUSER  
        SHUTDOWN  
        WRITETEXT  
    :::column-end:::
:::row-end:::

## <a name="viewing-conversion-problems"></a>Visualizzazione dei problemi di conversione  
Alcuni oggetti di SAP ASE potrebbero non essere convertiti. È possibile determinare le percentuali di riuscita della conversione visualizzando il report di conversione di riepilogo.  
  
**Per visualizzare un report di riepilogo**  
  
1.  In Sybase Metadata Explorer selezionare **database**.  
  
2.  Nel riquadro destro selezionare la scheda **report** .  
  
    Questo report Mostra il report di valutazione riepilogo per tutti gli oggetti di database che sono stati valutati o convertiti. È inoltre possibile visualizzare un report di riepilogo per i singoli oggetti:  
  
    -   Per visualizzare il report per un singolo database, selezionare il database in Sybase Metadata Explorer.  
  
    -   Per visualizzare il report per un singolo oggetto di database, selezionare l'oggetto in Sybase Metadata Explorer. Gli oggetti che presentano problemi di conversione presentano un'icona di errore rossa.  
  
Per gli oggetti che non hanno superato la conversione, è possibile visualizzare la sintassi che ha generato l'errore di conversione.  
  
**Per visualizzare i singoli problemi di conversione**  
  
1.  In Sybase Metadata Explorer espandere **database**.  
  
2.  Espandere il database che mostra un'icona di errore rossa.  
  
3.  Espandere la cartella **schemi** , quindi espandere lo schema che mostra un'icona di errore rossa.  
  
4.  In schema espandere una cartella con un'icona di errore rossa.  
  
5.  Selezionare l'oggetto con un'icona di errore rossa.  
  
6.  Nel riquadro destro selezionare la scheda **report** .  
  
7.  Nella parte superiore della scheda del **report** è presente un elenco a discesa. Se nell'elenco sono visualizzate le **statistiche**, modificare la selezione in **origine**.  
  
    SSMA visualizzerà il codice sorgente e diversi pulsanti immediatamente sopra il codice.  
  
8.  Selezionare il **problema successivo**, un'icona di errore rossa con una freccia rivolta verso destra.  
  
    SSMA per SAP ASE evidenzierà il primo codice sorgente problematico che trova nell'oggetto corrente.  
  
Per ogni elemento che non è stato possibile convertire, è necessario determinare cosa si vuole fare con l'oggetto:  
  
-   È possibile modificare il codice sorgente per le procedure e i trigger nella scheda **SQL** .  
  
-   È possibile modificare l'oggetto SAP ASE per rimuovere o rivedere il codice problematico. Per caricare il codice aggiornato in SSMA, è necessario aggiornare i metadati. Per altre informazioni, vedere [connessione a SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md).  
  
-   È possibile escludere l'oggetto dalla migrazione. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o in Esplora metadati di SQL Azure e in Esplora metadati Sybase deselezionare la casella di controllo accanto all'elemento prima di caricare gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL ed eseguire la migrazione dei dati da SAP ASE.  
  
## <a name="next-steps"></a>Passaggi successivi  
Il passaggio successivo del processo di migrazione consiste nel [caricare gli oggetti di database convertiti in SQL Server/SQL Azure (SybaseToSQL)](./loading-converted-database-objects-into-sql-server-sybasetosql.md).  
  
## <a name="see-also"></a>Vedi anche  
[Migrazione dei database SAP ASE a SQL Server-database SQL di Azure &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
