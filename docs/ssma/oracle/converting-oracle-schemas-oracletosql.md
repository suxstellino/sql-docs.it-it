---
title: Conversione di schemi Oracle (OracleToSQL) | Microsoft Docs
description: Informazioni su come convertire gli oggetti di database Oracle in SQL Server oggetti di database con SSMA per Oracle, dopo aver impostato le opzioni e aver eseguito la connessione a Oracle e SQL Server.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Conversion Results
ms.assetid: e021182d-31da-443d-b110-937f5db27272
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 8e2ee101ec916a920d4b72d924456a52850e8bc7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058756"
---
# <a name="converting-oracle-schemas-oracletosql"></a>Conversione di schemi Oracle (OracleToSQL)
Dopo la connessione a Oracle, la connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e l'impostazione delle opzioni di mapping dei dati e del progetto, è possibile convertire gli oggetti di database Oracle in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti di database.  
  
## <a name="the-conversion-process"></a>Processo di conversione  
La conversione di oggetti di database accetta le definizioni degli oggetti da Oracle, le converte in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti simili e quindi carica tali informazioni nei metadati SSMA. Non carica le informazioni nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È quindi possibile visualizzare gli oggetti e le relative proprietà utilizzando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati.  
  
Durante la conversione, SSMA stampa i messaggi di output nel riquadro di output e i messaggi di errore nel riquadro Elenco errori. Utilizzare le informazioni sull'output e sull'errore per determinare se è necessario modificare i database Oracle o il processo di conversione per ottenere i risultati della conversione desiderati.  
  
## <a name="setting-conversion-options"></a>Impostazione delle opzioni di conversione  
Prima di convertire gli oggetti, esaminare le opzioni di conversione del progetto nella finestra di dialogo **Impostazioni progetto** . Utilizzando questa finestra di dialogo è possibile impostare il modo in cui SSMA converte le funzioni e le variabili globali. Per ulteriori informazioni, vedere [Impostazioni progetto &#40;conversione&#41; &#40;&#41;OracleToSQL ](../../ssma/oracle/project-settings-conversion-oracletosql.md).  
  
## <a name="conversion-results"></a>Risultati della conversione  
Nella tabella seguente vengono illustrati gli oggetti Oracle che vengono convertiti e gli oggetti risultanti [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
|Oggetti Oracle|Oggetti SQL Server risultanti|  
|-|-|  
|Funzioni|Se la funzione può essere convertita direttamente in [!INCLUDE[tsql](../../includes/tsql-md.md)] , SSMA crea una funzione.<br /><br />In alcuni casi, la funzione deve essere convertita in un stored procedure. In questo caso, SSMA crea una stored procedure e una funzione che chiama l'stored procedure.|  
|Procedure|Se la procedura può essere convertita direttamente in [!INCLUDE[tsql](../../includes/tsql-md.md)] , SSMA crea una stored procedure.<br /><br />In alcuni casi è necessario chiamare un stored procedure in una transazione autonoma. In questo caso, SSMA crea due stored procedure: una che implementa la routine e un'altra utilizzata per chiamare il stored procedure di implementazione.|  
|Pacchetti|SSMA crea un set di stored procedure e funzioni unificate da nomi di oggetti simili.|  
|Sequenze|SSMA crea oggetti sequenza (SQL Server 2012 o SQL Server 2014) o emula le sequenze Oracle.|  
|Tabelle con oggetti dipendenti, ad esempio indici e trigger|SSMA crea tabelle con oggetti dipendenti.|  
|Visualizzazione con oggetti dipendenti, ad esempio trigger|SSMA crea viste con oggetti dipendenti.|  
|Viste materializzate|**SSMA crea viste indicizzate in SQL Server con alcune eccezioni. La conversione avrà esito negativo se la vista materializzata include uno o più dei costrutti seguenti:**<br /><br />Funzione definita dall'utente<br /><br />Campo/funzione/espressione non deterministica nelle clausole SELECT, WHERE o GROUP BY<br /><br />Utilizzo della colonna float nelle clausole SELECT *, WHERE o GROUP BY (caso speciale del problema precedente)<br /><br />Tipo di dati personalizzato (incluse le tabelle nidificate)<br /><br />CONTEGGIO ( &lt; campo Distinct &gt; )<br /><br />FETCH<br /><br />OUTER join (LEFT, RIGHT o FULL)<br /><br />Sottoquery, altra visualizzazione<br /><br />OVER, RANGO, LEAD, LOG<br /><br />MIN, MAX<br /><br />UNION, MENO, INTERSECT<br /><br />HAVING|  
|Trigger|**SSMA crea trigger basati sulle regole seguenti:**<br /><br />PRIMA che i trigger vengano convertiti in trigger INSTEAD OF.<br /><br />I trigger AFTER vengono convertiti in trigger AFTER.<br /><br />I trigger INSTEAD OF vengono convertiti in trigger INSTEAD OF. Più trigger INSTEAD OF definiti nella stessa operazione vengono combinati in un unico trigger.<br /><br />I trigger a livello di riga vengono emulati utilizzando cursori.<br /><br />I trigger di propagazione vengono convertiti in più trigger singoli.|  
|Sinonimi|**I sinonimi vengono creati per i tipi di oggetti seguenti:**<br /><br />Tabelle e tabelle di oggetti<br /><br />Viste e visualizzazioni di oggetti<br /><br />Stored procedure<br /><br />Funzioni<br /><br />**I sinonimi per gli oggetti seguenti vengono risolti e sostituiti dai riferimenti a oggetti diretti:**<br /><br />Sequenze<br /><br />Pacchetti<br /><br />Oggetti dello schema di classe Java<br /><br />Tipi di oggetti definiti dall'utente<br /><br />Non è possibile eseguire la migrazione di sinonimi per un altro sinonimo e verranno contrassegnati come errori.<br /><br />I sinonimi non vengono creati per le viste materializzate.|  
|Tipi definiti dall'utente|**SSMA non fornisce supporto per la conversione di tipi definiti dall'utente. I tipi definiti dall'utente, incluso l'utilizzo nei programmi PL/SQL, vengono contrassegnati con errori di conversione speciali, guidati dalle regole seguenti:**<br /><br />La colonna di tabella di un tipo definito dall'utente viene convertita in VARCHAR (8000).<br /><br />L'argomento del tipo definito dall'utente in una funzione o stored procedure viene convertito in VARCHAR (8000).<br /><br />La variabile del tipo definito dall'utente nel blocco PL/SQL è convertita in VARCHAR (8000).<br /><br />La tabella oggetti viene convertita in una tabella standard.<br /><br />La visualizzazione oggetti viene convertita in una vista standard.|  
  
## <a name="converting-oracle-database-objects"></a>Conversione di oggetti Oracle Database  
Per convertire gli oggetti di database Oracle, è necessario innanzitutto selezionare gli oggetti che si desidera convertire, quindi fare in modo che SSMA esegua la conversione. Per visualizzare i messaggi di output durante la conversione, scegliere **output** dal menu **Visualizza** .  
  
**Per convertire gli oggetti Oracle in SQL Server sintassi**  
  
1.  In Oracle Metadata Explorer espandere il server Oracle, quindi espandere **schemi**.  
  
2.  Selezionare gli oggetti da convertire:  
  
    -   Per convertire tutti gli schemi, selezionare la casella di controllo accanto agli **schemi**.  
  
    -   Per convertire o omettere un database, selezionare la casella di controllo accanto al nome dello schema.  
  
    -   Per convertire o omettere una categoria di oggetti, espandere uno schema, quindi selezionare o deselezionare la casella di controllo accanto alla categoria.  
  
    -   Per convertire o omettere singoli oggetti, espandere la cartella Category, quindi selezionare o deselezionare la casella di controllo accanto all'oggetto.  
  
3.  Per convertire tutti gli oggetti selezionati, fare clic con il pulsante destro del mouse su **schemi** e scegliere **Converti schema**.  
  
    È anche possibile convertire singoli oggetti o categorie di oggetti facendo clic con il pulsante destro del mouse sull'oggetto o la relativa cartella padre, quindi selezionando **Converti schema**.  
  
## <a name="viewing-conversion-problems"></a>Visualizzazione dei problemi di conversione  
Alcuni oggetti Oracle potrebbero non essere convertiti. È possibile determinare le percentuali di riuscita della conversione visualizzando il report di conversione di riepilogo.  
  
**Per visualizzare un report di riepilogo**  
  
1.  In Esplora metadati Oracle selezionare **schemi**.  
  
2.  Nel riquadro destro selezionare la scheda **report** .  
  
    Questo report Mostra il report di valutazione riepilogo per tutti gli oggetti di database che sono stati valutati o convertiti. È inoltre possibile visualizzare un report di riepilogo per i singoli oggetti:  
  
    -   Per visualizzare il report per un singolo schema, selezionare lo schema in Oracle Metadata Explorer.  
  
    -   Per visualizzare il report per un singolo oggetto, selezionare l'oggetto in Oracle Metadata Explorer. Gli oggetti che presentano problemi di conversione presentano un'icona di errore rossa.  
  
Per gli oggetti che non hanno superato la conversione, è possibile visualizzare la sintassi che ha generato l'errore di conversione.  
  
**Per visualizzare i singoli problemi di conversione**  
  
1.  In Esplora metadati Oracle espandere **schemi**.  
  
2.  Espandere lo schema che mostra un'icona di errore rossa.  
  
3.  In schema espandere una cartella con un'icona di errore rossa.  
  
4.  Selezionare l'oggetto con un'icona di errore rossa.  
  
5.  Nel riquadro di destra fare clic sulla scheda **report** .  
  
6.  Nella parte superiore della scheda del **report** è presente un elenco a discesa. Se nell'elenco sono visualizzate le **statistiche**, modificare la selezione in **origine**.  
  
    SSMA visualizzerà il codice sorgente e diversi pulsanti immediatamente sopra il codice.  
  
7.  Fare clic sul pulsante **problema successivo** . Si tratta di un'icona di errore rossa con una freccia che punta a destra.  
  
    SSMA evidenzierà il primo codice sorgente problematico che trova nell'oggetto corrente.  
  
Per ogni elemento che non è stato possibile convertire, è necessario determinare cosa si vuole fare con l'oggetto:  
  
-   È possibile modificare il codice sorgente per le procedure nella scheda **SQL** .  
  
-   È possibile modificare l'oggetto nel database Oracle per rimuovere o rivedere il codice problematico. Per caricare il codice aggiornato in SSMA, sarà necessario aggiornare i metadati. Per ulteriori informazioni, vedere [connessione a Oracle Database &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-oracle-database-oracletosql.md).  
  
-   È possibile escludere l'oggetto dalla migrazione. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati e in Esplora metadati Oracle deselezionare la casella di controllo accanto all'elemento prima di caricare gli oggetti in ed eseguire la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] migrazione dei dati da Oracle.  
  
## <a name="next-step"></a>passaggio successivo  
Il passaggio successivo del processo di migrazione consiste nel [caricare gli oggetti convertiti in SQL Server](loading-converted-database-objects-into-sql-server-oracletosql.md).  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di database Oracle a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
