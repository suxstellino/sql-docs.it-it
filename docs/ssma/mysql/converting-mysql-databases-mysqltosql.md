---
title: Conversione di database MySQL (MySQLToSQL) | Microsoft Docs
description: Informazioni su come convertire oggetti di database MySQL in SQL Server o oggetti di database SQL di Azure con SSMA, dopo la connessione e l'impostazione delle opzioni di mapping dei dati e del progetto.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: ac21850b-fb32-4704-9985-5759b7c688c7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 61298bb7e2c65e85959b52ab3750dc49a5105504
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069002"
---
# <a name="converting-mysql-databases-mysqltosql"></a>Conversione di database MySQL (MySQLToSQL)
Dopo la connessione a MySQL, la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o la SQL Azure e l'impostazione delle opzioni di mapping di progetti e dati, è possibile convertire oggetti di database MySQL in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti di database SQL di Azure o.  
  
## <a name="the-conversion-process"></a>Processo di conversione  
La conversione di oggetti di database accetta le definizioni degli oggetti da MySQL, le converte in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti simili o SQL Azure e quindi carica tali informazioni nei metadati SSMA. Non carica le informazioni nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È quindi possibile visualizzare gli oggetti e le relative proprietà utilizzando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure Esplora metadati.  
  
Durante la conversione, SSMA stampa i messaggi di output nel riquadro di output e i messaggi di errore nel riquadro Elenco errori. Usare le informazioni sull'output e sull'errore per determinare se è necessario modificare i database MySQL o il processo di conversione per ottenere i risultati della conversione desiderati.  
  
## <a name="setting-conversion-options"></a>Impostazione delle opzioni di conversione  
Prima di convertire gli oggetti, esaminare le opzioni di conversione del progetto nella finestra di dialogo **Impostazioni progetto** . Utilizzando questa finestra di dialogo è possibile impostare il modo in cui SSMA converte le tabelle e gli indici. Per ulteriori informazioni, vedere [Impostazioni progetto &#40;conversione&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-conversion-mysqltosql.md)  
  
## <a name="conversion-results"></a>Risultati della conversione  
La tabella seguente Mostra gli oggetti MySQL che vengono convertiti e gli [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti risultanti:  
  
|Oggetti MySQL|Oggetti SQL Server risultanti|  
|-|-|  
|Tabelle con oggetti dipendenti, ad esempio indici|SSMA crea tabelle con oggetti dipendenti. La tabella viene convertita con tutti gli indici e i vincoli. Gli indici vengono convertiti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti separati.<br /><br />Il **mapping del tipo di dati spaziali** può essere eseguito solo a livello di nodo della tabella.<br /><br />Per ulteriori informazioni sulle impostazioni di conversione delle tabelle, vedere [impostazioni di conversione](conversion-settings-mysqltosql.md)|  
|Funzioni|Se la funzione può essere convertita direttamente in Transact-SQL, SSMA crea una funzione. In alcuni casi, la funzione deve essere convertita in un stored procedure. Questa operazione può essere eseguita usando la **conversione di funzioni** nelle impostazioni del progetto. In questo caso, SSMA crea una stored procedure e una funzione che chiama l'stored procedure.<br /><br />**Scelte fornite:**<br /><br />Converti in base alle impostazioni del progetto<br /><br />Converti in funzione<br /><br />Converti in stored procedure<br /><br />Per ulteriori informazioni sulle impostazioni di conversione delle funzioni, vedere [impostazioni di conversione](conversion-settings-mysqltosql.md)|  
|Procedure|Se la procedura può essere convertita direttamente in Transact-SQL, SSMA crea una stored procedure. In alcuni casi è necessario chiamare un stored procedure in una transazione autonoma. In questo caso, SSMA crea due stored procedure: una che implementa la routine e un'altra utilizzata per chiamare il stored procedure di implementazione.|  
|Conversione del database|I database come oggetti MySQL non vengono convertiti direttamente da SSMA per MySQL. I database MySQL vengono trattati in modo più simile ai nomi degli schemi e tutti i parametri fisici vengono persi durante la conversione. SSMA per MySQL usa il [mapping dei database MySQL a SQL Server schemi &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md) per eseguire il mapping degli oggetti dal database MySQL alla coppia di database/schema SQL Server appropriata.|  
|Conversione del trigger|**SSMA crea trigger basati sulle regole seguenti:**<br /><br />PRIMA che i trigger vengano convertiti in trigger INSTEAD OF T-SQL<br /><br />I trigger AFTER vengono convertiti in dopo i trigger T-SQL con o senza iterazioni per righe.|  
|Visualizza conversione|SSMA crea viste con oggetti dipendenti|  
|Conversione di istruzioni|-Ogni oggetto istruzione SQL può contenere una singola istruzione MySQL, ad esempio DDL, DML e altri tipi di istruzioni, o BEGIN... Blocco finale.<br />-   **Conversione a più Stati: Begin...** L'istruzione SQL per la conversione end Block può inoltre contenere un'istruzione BEGIN... Blocco END come uno nella definizione di routine, funzione o trigger. Questi blocchi devono essere convertiti nello stesso modo in cui vengono convertiti per i singoli oggetti istruzione MySQL.|  
  
## <a name="converting-mysql-database-objects"></a>Conversione di oggetti di database MySQL  
Per convertire gli oggetti di database MySQL, è necessario innanzitutto selezionare gli oggetti che si desidera convertire, quindi fare in modo che SSMA esegua la conversione. Per visualizzare i messaggi di output durante la conversione, scegliere **output** dal menu **Visualizza** .  
  
**Per convertire gli oggetti MySQL in SQL Server o SQL Azure sintassi**  
  
1.  In MySQL Metadata Explorer espandere il server MySQL, quindi espandere **database**.  
  
2.  Selezionare gli oggetti da convertire:  
  
    -   Per convertire tutti gli schemi, selezionare la casella di controllo accanto a **database**.  
  
    -   Per convertire o omettere un database, selezionare la casella di controllo accanto al nome del database.  
  
    -   Per convertire o omettere una categoria di oggetti, espandere uno schema, quindi selezionare o deselezionare la casella di controllo accanto alla categoria.  
  
    -   Per convertire o omettere singoli oggetti, espandere la cartella Category, quindi selezionare o deselezionare la casella di controllo accanto all'oggetto.  
  
3.  Per convertire tutti gli oggetti selezionati, fare clic con il pulsante destro del mouse su **database** e scegliere **Converti schema**.  
  
    È anche possibile convertire singoli oggetti o categorie di oggetti facendo clic con il pulsante destro del mouse sull'oggetto o la relativa cartella padre, quindi selezionando **Converti schema**.  
  
## <a name="viewing-conversion-problems"></a>Visualizzazione dei problemi di conversione  
Alcuni oggetti MySQL potrebbero non essere convertiti. È possibile determinare le percentuali di riuscita della conversione visualizzando il report di conversione di riepilogo.  
  
**Per visualizzare un report di riepilogo**  
  
1.  In MySQL Metadata Explorer selezionare **database**.  
  
2.  Nel riquadro destro selezionare la scheda **report** .  
  
    Questo report Mostra il report di valutazione riepilogo per tutti gli oggetti di database che sono stati valutati o convertiti. È inoltre possibile visualizzare un report di riepilogo per i singoli oggetti:  
  
    -   Per visualizzare il report per un singolo schema, selezionare il database in MySQL Metadata Explorer.  
  
    -   Per visualizzare il report per un singolo oggetto, selezionare l'oggetto in MySQL Metadata Explorer. Gli oggetti che presentano problemi di conversione presentano un'icona di errore rossa.  
  
Per gli oggetti che non hanno superato la conversione, è possibile visualizzare la sintassi che ha generato l'errore di conversione.  
  
**Per visualizzare i singoli problemi di conversione**  
  
1.  In MySQL Metadata Explorer espandere **database**.  
  
2.  Espandere il database che mostra un'icona di errore rossa.  
  
3.  Nel database espandere una cartella con un'icona di errore rossa.  
  
4.  Selezionare l'oggetto con un'icona di errore rossa.  
  
5.  Nel riquadro di destra fare clic sulla scheda **report** .  
  
6.  Nella parte superiore della scheda del **report** è presente un elenco a discesa. Se nell'elenco sono visualizzate le **statistiche**, modificare la selezione in **origine**.  
  
    SSMA visualizzerà il codice sorgente e diversi pulsanti immediatamente sopra il codice.  
  
7.  Fare clic sul pulsante **problema successivo** . Si tratta di un'icona di errore rossa con una freccia che punta a destra.  
  
    SSMA evidenzierà il primo codice sorgente problematico che trova nell'oggetto corrente.  
  
Per ogni elemento che non è stato possibile convertire, è necessario determinare cosa si vuole fare con l'oggetto:  
  
-   È possibile modificare l'oggetto nel database MySQL per rimuovere o rivedere il codice problematico. Per caricare il codice aggiornato in SSMA, sarà necessario aggiornare i metadati. Per altre informazioni, vedere [connessione a MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
  
-   È possibile escludere l'oggetto dalla migrazione. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure Metadata Explorer e MySQL Metadata Explorer deselezionare la casella di controllo accanto all'elemento prima di caricare gli oggetti in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure ed eseguire la migrazione dei dati da MySQL.  
  
## <a name="next-step"></a>passaggio successivo  
Il passaggio successivo del processo di migrazione consiste nel [caricare gli oggetti di database convertiti in SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/loading-converted-database-objects-into-sql-server-mysqltosql.md)  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di database MySQL a SQL Server-database SQL di Azure &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
