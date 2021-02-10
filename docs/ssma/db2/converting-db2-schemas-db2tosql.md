---
description: Conversione di schemi DB2 (DB2ToSQL)
title: Conversione di schemi DB2 (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 7947efc3-ca86-4ec5-87ce-7603059c75a0
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: af91274d4e48ce99894316c730a58d68fd633242
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062886"
---
# <a name="converting-db2-schemas-db2tosql"></a>Conversione di schemi DB2 (DB2ToSQL)
Dopo la connessione a DB2, la connessione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e l'impostazione delle opzioni di mapping dei dati e del progetto, è possibile convertire gli oggetti di database DB2 in oggetti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] database.  
  
## <a name="the-conversion-process"></a>Processo di conversione  
La conversione di oggetti di database accetta le definizioni degli oggetti da DB2, le converte in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oggetti simili e quindi carica tali informazioni nei metadati SSMA. Non carica le informazioni nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . È quindi possibile visualizzare gli oggetti e le relative proprietà utilizzando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Esplora metadati.  
  
Durante la conversione, SSMA stampa i messaggi di output nel riquadro di output e i messaggi di errore nel riquadro Elenco errori. Usare le informazioni sull'output e sull'errore per determinare se è necessario modificare i database DB2 o il processo di conversione per ottenere i risultati della conversione desiderati.  
  
## <a name="setting-conversion-options"></a>Impostazione delle opzioni di conversione  
Prima di convertire gli oggetti, esaminare le opzioni di conversione del progetto nella finestra di dialogo **Impostazioni progetto** . Utilizzando questa finestra di dialogo è possibile impostare il modo in cui SSMA converte le funzioni e le variabili globali. Per ulteriori informazioni, vedere [Impostazioni progetto &#40;conversione&#41; &#40;&#41;DB2ToSQL ](../../ssma/db2/project-settings-conversion-db2tosql.md).  
  
## <a name="conversion-results"></a>Risultati della conversione  
Nella tabella seguente vengono indicati gli oggetti DB2 convertiti e gli oggetti risultanti [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
|Oggetti DB2|Oggetti SQL Server risultanti|  
|-----------|----------------------------|  
|Tipi di dati|**SSMA esegue il mapping di ogni tipo, ad eccezione di quanto indicato di seguito:**<br /><br />CLOB: alcune funzioni native per l'utilizzo di questo tipo non sono supportate (ad esempio CLOB_EMPTY ())<br /><br />BLOB: alcune funzioni native per l'utilizzo di questo tipo non sono supportate (ad esempio BLOB_EMPTY ())<br /><br />DBLOB: alcune funzioni native per l'utilizzo di questo tipo non sono supportate (ad esempio DBLOB_EMPTY ())|  
|Tipi definiti dall'utente|**SSMA esegue il mapping dei seguenti elementi definiti dall'utente:**<br /><br />Tipo distinct<br /><br />Tipo strutturato<br /><br />Tipi di dati di SQL PL-Nota: il tipo di cursore debole non è supportato.|  
|Registri speciali|**SSMA esegue il mapping solo di registri elencati di seguito:**<br /><br />TIMESTAMP CORRENTE<br /><br />DATA CORRENTE<br /><br />ORA CORRENTE<br /><br />FUSO ORARIO CORRENTE<br /><br />UTENTE CORRENTE<br /><br />SESSION_USER e utente<br /><br />SYSTEM_USER<br /><br />CLIENT_APPLNAME CORRENTE<br /><br />CLIENT_WRKSTNNAME CORRENTE<br /><br />TIMEOUT BLOCCO CORRENTE<br /><br />SCHEMA CORRENTE<br /><br />SERVER CORRENTE<br /><br />ISOLAMENTO CORRENTE<br /><br />Non è stato eseguito il mapping di altri registri speciali alla semantica di SQL Server.|  
|CREA TABELLA|**SSMA esegue il mapping di CREATE TABLE con le eccezioni seguenti:**<br /><br />Tabelle di clustering multidimensionali (MDC)<br /><br />Tabelle con intervallo cluster (RCT)<br /><br />Tabelle partizionate<br /><br />Tabella scollegata<br /><br />Clausola di acquisizione dati<br /><br />Opzione IMPLICITamente nascosta<br /><br />Opzione VOLATILE|  
|CREATE VIEW|SSMA Maps crea la vista con l'opzione ' WITH LOCAL CHECK OPTION ', ma non è stato eseguito il mapping di altre opzioni alla semantica di SQL Server|  
|CREATE INDEX|**SSMA Maps CREATE INDEX con le eccezioni seguenti:**<br /><br />Indice XML<br /><br />Opzione BUSINESS_TIME senza sovrapposizioni<br /><br />Clausola PARTITIONed<br /><br />Opzione solo specifica<br /><br />Estendi USING (opzione)<br /><br />MINPCTUSED-opzione<br /><br />Opzione di suddivisione pagina|  
|Trigger|**SSMA esegue il mapping della semantica del trigger seguente:**<br /><br />Trigger AFTER/per ogni riga<br /><br />DOPO/FOR ogni istruzione viene attivata<br /><br />BEFORe/FOR ogni riga e anziché/per ogni trigger di riga|  
|Sequenze|Viene mappato.|  
|Istruzione SELECT|**SSMA Maps SELECT con le eccezioni seguenti:**<br /><br />Clausola data-Change-Table-Reference: parzialmente mappato, ma le tabelle finali non sono supportate<br /><br />Clausola di riferimento a tabella: parzialmente mappato, ma solo-tabella-riferimento, esterno-tabella-riferimento, analyze_table-espressione, raccolta-derivata-tabella, XMLTable-Expression non è mappata alla semantica di SQL Server<br /><br />Clausola period-Specification-non mappato.<br /><br />Clausola di gestione continua-non mappato.<br /><br />Clausola di correlazione tipizzata-non mappata.<br /><br />Clausola di accesso simultaneo-non mappato.|  
|VALUEs-istruzione|Viene mappato.|  
|Istruzione INSERT|Viene mappato.|  
|Istruzione UPDATE|S **SMA Maps Update con le eccezioni seguenti:**<br /><br />Tabella-riferimento clausola-only-table-reference non è mappato alla semantica di SQL Server<br /><br />Clausola period: non è mappato.|  
|Istruzione MERGE|**SSMA Maps MERGE con le eccezioni seguenti:**<br /><br />Singole occorrenze di e multiple di ogni clausola: viene eseguito il mapping alla semantica di SQL Server per le occorrenze limitate di ogni clausola<br /><br />Clausola SIGNAL: non esegue il mapping alla semantica SQL Server<br /><br />Clausole di aggiornamento ed eliminazione miste: non esegue il mapping alla semantica di SQL Server<br /><br />Clausola period-non viene mappata alla semantica SQL Server|  
|Istruzione DELETE|**SSMA Maps DELETE con le eccezioni seguenti:**<br /><br />Tabella-riferimento clausola-only-table-reference non è mappato alla semantica di SQL Server<br /><br />Clausola period: non esegue il mapping alla semantica SQL Server|  
|Livello di isolamento e tipo di blocco|Viene mappato.|  
|Procedure (SQL)|Viene mappato.|  
|Procedure (esterne)|Richiedi aggiornamento manuale.|  
|Procedure (originate)|Non eseguire il mapping alla semantica SQL Server.|  
|Istruzione di assegnazione|Viene mappato.|  
|Istruzione CALL per una routine|Viene mappato.|  
|Istruzione CASE|Viene mappato.|  
|Istruzione FOR|Viene mappato.|  
|GOTO - istruzione|Viene mappato.|  
|Istruzione IF|Viene mappato.|  
|ITERation (istruzione)|Viene mappato.|  
|Istruzione LEAVE|Viene mappato.|  
|Istruzione LOOP|Viene mappato.|  
|Istruzione REPEAT|Viene mappato.|  
|Resignal (istruzione)|Le condizioni non sono supportate. I messaggi possono essere facoltativi.|  
|RETURN (istruzione)|Viene mappato.|  
|SIGNAL (istruzione)|Le condizioni non sono supportate. I messaggi possono essere facoltativi.|  
|Istruzione WHILE|Viene mappato.|  
|OTTENERE l'istruzione di diagnostica|**SSMA Maps GET Diagnostics con le eccezioni seguenti:**<br /><br />Viene eseguito il mapping di ROW_COUNT.<br /><br />Viene eseguito il mapping di DB2_RETURN_STATUS.<br /><br />Viene eseguito il mapping di MESSAGE_TEXT.<br /><br />DB2_SQL_NESTING_LEVEL: non esegue il mapping alla semantica di SQL Server<br /><br />DB2_TOKEN_STRING: non esegue il mapping alla semantica di SQL Server|  
|Cursori|**SSMA esegue il mapping dei CURSORi con le eccezioni seguenti:**<br /><br />Istruzione ALLOCAte CURSOR-non viene mappata alla semantica SQL Server<br /><br />Istruzione ASSOCIATE LOCATORs: non esegue il mapping alla semantica di SQL Server<br /><br />Istruzione DECLARE CURSOR-la clausola restituzione non è mappata alla semantica di SQL Server<br /><br />Istruzione FETCH-mapping parziale. Le variabili come destinazione sono supportate solo. Il descrittore SQLDA non è mappato alla semantica di SQL Server|  
|Variabili|Viene mappato.|  
|Eccezioni, gestori e condizioni|**SSMA esegue il mapping di "gestione delle eccezioni" con le eccezioni seguenti:**<br /><br />Gestori di uscita-mappati.<br /><br />Gestori di annullamento-mappati.<br /><br />Gestori continua-non è stato eseguito il mapping.<br /><br />Condizioni: non viene mappata alla semantica di SQL Server.|  
|SQL dinamica|Non mappato.|  
|Alias|Viene mappato.|  
|I nomi alternativi|Mapping parziale. L'elaborazione manuale è necessaria per l'oggetto sottostante|  
|Sinonimi|Viene mappato.|  
|Funzioni standard in DB2|SSMA esegue il mapping delle funzioni di DB2 standard quando una funzione equivalente è disponibile nel SQL Server:|  
|Autorizzazione|Non mappato.|  
|Predicati|Viene mappato.|  
|SELECT INTO - istruzione|Non mappato.|  
|VALUEs INTO-istruzione|Non mappato.|  
|Controllo transazione|Non mappato.|  
  
## <a name="converting-db2-database-objects"></a>Conversione di oggetti di database DB2  
Per convertire gli oggetti di database DB2, è necessario innanzitutto selezionare gli oggetti che si desidera convertire, quindi fare in modo che SSMA esegua la conversione. Per visualizzare i messaggi di output durante la conversione, scegliere **output** dal menu **Visualizza** .  
  
**Per convertire gli oggetti DB2 in SQL Server sintassi**  
  
1.  In DB2 Metadata Explorer espandere il server DB2, quindi espandere **schemi**.  
  
2.  Selezionare gli oggetti da convertire:  
  
    -   Per convertire tutti gli schemi, selezionare la casella di controllo accanto agli **schemi**.  
  
    -   Per convertire o omettere un database, selezionare la casella di controllo accanto al nome dello schema.  
  
    -   Per convertire o omettere una categoria di oggetti, espandere uno schema, quindi selezionare o deselezionare la casella di controllo accanto alla categoria.  
  
    -   Per convertire o omettere singoli oggetti, espandere la cartella Category, quindi selezionare o deselezionare la casella di controllo accanto all'oggetto.  
  
3.  Per convertire tutti gli oggetti selezionati, fare clic con il pulsante destro del mouse su **schemi** e scegliere **Converti schema**.  
  
    È anche possibile convertire singoli oggetti o categorie di oggetti facendo clic con il pulsante destro del mouse sull'oggetto o la relativa cartella padre, quindi selezionando **Converti schema**.  
  
## <a name="viewing-conversion-problems"></a>Visualizzazione dei problemi di conversione  
Alcuni oggetti DB2 potrebbero non essere convertiti. È possibile determinare le percentuali di riuscita della conversione visualizzando il report di conversione di riepilogo.  
  
**Per visualizzare un report di riepilogo**  
  
1.  In DB2 Metadata Explorer selezionare **schemas**.  
  
2.  Nel riquadro destro selezionare la scheda **report** .  
  
    Questo report Mostra il report di valutazione riepilogo per tutti gli oggetti di database che sono stati valutati o convertiti. È inoltre possibile visualizzare un report di riepilogo per i singoli oggetti:  
  
    -   Per visualizzare il report per un singolo schema, selezionare lo schema in DB2 Metadata Explorer.  
  
    -   Per visualizzare il report per un singolo oggetto, selezionare l'oggetto in DB2 Metadata Explorer. Gli oggetti che presentano problemi di conversione presentano un'icona di errore rossa.  
  
Per gli oggetti che non hanno superato la conversione, è possibile visualizzare la sintassi che ha generato l'errore di conversione.  
  
**Per visualizzare i singoli problemi di conversione**  
  
1.  In DB2 Metadata Explorer espandere **schemi**.  
  
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
  
-   È possibile modificare l'oggetto nel database DB2 per rimuovere o modificare il codice problematico. Per caricare il codice aggiornato in SSMA, sarà necessario aggiornare i metadati. Per ulteriori informazioni, vedere [connessione al database DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-db2-database-db2tosql.md).  
  
-   È possibile escludere l'oggetto dalla migrazione. In Esplora [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadati e in Esplora metadati DB2 deselezionare la casella di controllo accanto all'elemento prima di caricare gli oggetti in ed eseguire la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] migrazione dei dati da DB2.  
  
## <a name="next-step"></a>passaggio successivo  
Il passaggio successivo del processo di migrazione consiste nel [caricare gli oggetti convertiti in SQL Server](./loading-converted-database-objects-into-sql-server-db2tosql.md).  
  
## <a name="see-also"></a>Vedere anche  
[Migrazione di dati DB2 in SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-data-into-sql-server-db2tosql.md)  
