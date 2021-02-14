---
title: T-SQL non supportato da OLTP in memoria
description: Informazioni sulle funzionalità di Transact-SQL non supportate per le tabelle ottimizzate per la memoria, le stored procedure compilate in modo nativo e le funzioni definite dall'utente.
ms.custom: seo-dt-2019
ms.date: 11/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: e3f8009c-319d-4d7b-8993-828e55ccde11
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a41ac84e7e34e0df190c725d9dbadf84f15cac47
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "100353629"
---
# <a name="transact-sql-constructs-not-supported-by-in-memory-oltp"></a>Costrutti Transact-SQL non supportati da OLTP in memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Le tabelle con ottimizzazione per la memoria, le stored procedure compilate in modo nativo e le funzioni definite dall'utente non supportano la superficie di attacco totale di [!INCLUDE[tsql](../../includes/tsql-md.md)] supportata dalle tabelle basate su disco, dalle stored procedure [!INCLUDE[tsql](../../includes/tsql-md.md)] interpretate e dalle funzioni definite dall'utente. Quando si tenta di usare una delle funzionalità non supportate, il server restituisce un errore.  
  
 Nel testo del messaggio di errore viene indicato il tipo di istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] (ad esempio funzionalità, operazione, opzione) e il nome della funzionalità o della parola chiave [!INCLUDE[tsql](../../includes/tsql-md.md)] . La maggior parte delle funzionalità non supportate restituirà l'errore 10794, con il testo del messaggio di errore che indica la funzionalità non supportata. Nelle tabelle seguenti vengono elencate le parole chiave e le funzionalità di [!INCLUDE[tsql](../../includes/tsql-md.md)] che possono essere incluse nel testo del messaggio di errore e l'azione correttiva per risolvere l'errore.  
  
 Per altre informazioni sulle funzionalità supportate con tabelle ottimizzate per la memoria e stored procedure compilate in modo nativo, vedere:  
  
-   [Problemi di migrazione relativi alle stored procedure compilate in modo nativo](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
-   [Supporto di Transact-SQL per OLTP in memoria](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)  
  
-   [Funzionalità di SQL Server non supportate per OLTP in memoria](../../relational-databases/in-memory-oltp/unsupported-sql-server-features-for-in-memory-oltp.md)  
  
-   [Stored procedure compilate in modo nativo](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
## <a name="databases-that-use-in-memory-oltp"></a>Database che utilizzano OLTP in memoria  
 La tabella seguente elenca le funzionalità [!INCLUDE[tsql](../../includes/tsql-md.md)] non supportate e le parole chiave che possono essere incluse nel testo del messaggio di un errore relativo a un database OLTP in memoria. La tabella include anche una risoluzione dell'errore.  
  
|Type|Nome|Risoluzione|  
|----------|----------|----------------|  
|Opzione|AUTO_CLOSE|L'opzione di database AUTO_CLOSE=ON non è supportata con i database che contengono un filegroup MEMORY_OPTIMIZED_DATA.|  
|Opzione|ATTACH_REBUILD_LOG|L'opzione di database CREATE ATTACH_REBUILD_LOG non è supportata con i database contenenti un filegroup MEMORY_OPTIMIZED_DATA.|  
|Funzionalità|DATABASE SNAPSHOT|La creazione di snapshot del database non è supportata con i database che contengono un filegroup MEMORY_OPTIMIZED_DATA.|  
|Funzionalità|Replica che utilizza 'database snapshot' o 'database snapshot character' come sync_method|La replica che utilizza 'database snapshot' or 'database snapshot character' come sync_method non è supportata con i database che contengono un filegroup MEMORY_OPTIMIZED_DATA.|  
|Funzionalità|DBCC CHECKDB<br /><br /> DBCC CHECKTABLE|DBCC CHECKDB ignora le tabelle ottimizzate per la memoria nel database.<br /><br /> DBCC CHECKTABLE avrà esito negativo per le tabelle ottimizzate per la memoria.|  
  
## <a name="memory-optimized-tables"></a>Tabelle con ottimizzazione per la memoria  
 La tabella seguente elenca le funzionalità [!INCLUDE[tsql](../../includes/tsql-md.md)] non supportate e le parole chiave che possono essere incluse nel testo del messaggio di un errore relativo a una tabella ottimizzata per la memoria. La tabella include anche una risoluzione dell'errore.  
  
|Type|Nome|Risoluzione|  
|----------|----------|----------------|  
|Funzionalità|ON|Le tabelle con ottimizzazione per la memoria non possono essere posizionate in uno schema di partizione o filegroup. Rimuovere la clausola ON dall'istruzione **CREATE TABLE** .<br /><br /> Tutte le tabelle ottimizzate per la memoria vengono mappate al filegroup ottimizzato per la memoria.|  
|Tipo di dati|*Nome del tipo di dati*|Il tipo di dati indicato non è supportato. Sostituirlo con un tipo di dati supportato. Per altre informazioni, vedere [Tipi di dati supportati](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md).|  
|Funzionalità|Colonne calcolate|**Si applica a**: [!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)] e [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Le colonne calcolate non sono supportate dalle tabelle ottimizzate per la memoria. Rimuovere le colonne calcolate dall'istruzione **CREATE TABLE** .<br/><br/>[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] e SQL Server a partire da [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] supportano le colonne calcolate nelle tabelle e negli indici ottimizzati per la memoria.|  
|Funzionalità|Replica|La replica non è supportata con le tabelle ottimizzate per la memoria.|  
|Funzionalità|FILESTREAM|L'archiviazione FILESTREAM non è supportata dalle colonne delle tabelle ottimizzate per la memoria. Rimuovere la parola chiave **FILESTREAM** dalla definizione della colonna.|  
|Funzionalità|SPARSE|Le colonne delle tabelle ottimizzate per la memoria non possono essere definite come SPARSE. Rimuovere la parola chiave **SPARSE** dalla definizione della colonna.|  
|Funzionalità|ROWGUIDCOL|L'opzione ROWGUIDCOL non è supportata dalle colonne delle tabelle ottimizzate per la memoria. Rimuovere la parola chiave **ROWGUIDCOL** dalla definizione della colonna.|  
|Funzionalità|FOREIGN KEY|**Si applica a**: [!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] e SQL Server a partire da [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Per le tabelle ottimizzate per la memoria, i vincoli FOREIGN KEY sono supportati solo per chiavi esterne che fanno riferimento a chiavi primarie di altre tabelle ottimizzate per la memoria. Rimuovere il vincolo dalla definizione della tabella se la chiave esterna fa riferimento a un vincolo univoco.<br/><br/>In [!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)] i vincoli FOREIGN KEY non sono supportati con le tabelle ottimizzate per la memoria.|  
|Funzionalità|indice cluster|Specificare un indice non cluster. Nel caso di un indice di chiave primaria assicurarsi di specificare **PRIMARY KEY NONCLUSTERED**.|  
|Funzionalità|DDL all'interno delle transazioni|Le tabelle con ottimizzazione per la memoria e le stored procedure compilate in modo nativo non possono essere create o eliminate nel contesto di una transazione utente. Non avviare una transazione e assicurarsi che l'impostazione della sessione IMPLICIT_TRANSACTIONS sia OFF prima di eseguire l'istruzione CREATE o DROP.|  
|Funzionalità|trigger DDL|Le tabelle con ottimizzazione per la memoria e le stored procedure compilate in modo nativo non possono essere create o eliminate se esiste un trigger di database o di server per l'operazione DDL. Rimuovere i trigger di database e di server da CREATE/DROP TABLE e CREATE/DROP PROCEDURE.|  
|Funzionalità|EVENT NOTIFICATION|Le tabelle con ottimizzazione per la memoria e le stored procedure compilate in modo nativo non possono essere create o eliminate se esiste una notifica degli eventi di database o di server per l'operazione DDL. Rimuovere le notifiche degli eventi di database e di server in CREATE TABLE o DROP TABLE e CREATE PROCEDURE o DROP PROCEDURE.|  
|Funzionalità|FileTable|Le tabelle con ottimizzazione per la memoria non possono essere create come tabelle di file. Rimuovere l'argomento **AS FileTable** dall'istruzione **CREATE TABLE** .|  
|Operazione|Aggiornamento di colonne chiave primaria|Le colonne chiave primaria delle tabelle ottimizzate per la memoria e dei tipi di tabella non possono essere aggiornate. Se la chiave primaria deve essere aggiornata, eliminare la vecchia riga e inserire una nuova riga con la chiave primaria aggiornata.|  
|Operazione|CREATE INDEX|Gli indici delle tabelle ottimizzate per la memoria devono essere specificati inline con l'istruzione **CREATE TABLE** o **ALTER TABLE**.|  
|Operazione|CREATE FULLTEXT INDEX|Gli indici full-text non sono supportati dalle tabelle ottimizzate per la memoria.|  
|Operazione|modifica schema|Le tabelle con ottimizzazione per la memoria e le stored procedure compilate in modo nativo non supportano determinate modifiche dello schema:<br/> [!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] e SQL Server a partire da [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]: sono supportate le operazioni ALTER TABLE, ALTER PROCEDURE e sp_rename. Altre modifiche dello schema, ad esempio l'aggiunta di proprietà estese, non sono supportate.<br/><br/>[!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]: sono supportate le operazioni ALTER TABLE e ALTER PROCEDURE. Altre modifiche dello schema, tra cui sp_rename, non sono supportate.<br/><br/>[!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)]: le modifiche dello schema non sono supportate. Per modificare la definizione di una tabella ottimizzata per la memoria o di una stored procedure compilata in modo nativo, rilasciare prima l'oggetto e quindi ricrearlo con la definizione desiderata.| 
|Operazione|TRUNCATE TABLE|L'operazione TRUNCATE non è supportata dalle tabelle ottimizzate per la memoria. Per rimuovere tutte le righe da una tabella, eliminare tutte le righe usando **DELETE FROM**_table_ oppure eliminare e ricreare la tabella.|  
|Operazione|ALTER AUTHORIZATION|La modifica del proprietario di una tabella ottimizzata per la memoria o di una stored procedure compilata in modo nativo non è supportata. Eliminare e ricreare la tabella o la stored procedure per modificare la proprietà.|  
|Operazione|ALTER SCHEMA|Il trasferimento di una tabella o di una stored procedure compilata in modo nativo esistente in un altro schema non è supportato. Rilasciare e ricreare l'oggetto per trasferirlo da uno schema a un altro.|  
|Operazione|DBCC CHECKTABLE|L'operazione DBCC CHECKTABLE non è supportata con le tabelle ottimizzate per la memoria. Per verificare l'integrità dei file di checkpoint su disco, eseguire un backup del filegroup MEMORY_OPTIMIZED_DATA.|  
|Funzionalità|ANSI_PADDING OFF|L'opzione di sessione **ANSI_PADDING** deve essere impostata su ON durante la creazione delle tabelle ottimizzate per la memoria e delle stored procedure compilate in modo nativo. Eseguire **SET ANSI_PADDING ON** prima di eseguire l'istruzione CREATE.|  
|Opzione|DATA_COMPRESSION|La compressione dati non è supportata dalle tabelle ottimizzate per la memoria. Rimuovere l'opzione dalla definizione della tabella.|  
|Funzionalità|DTC|Non è possibile accedere alle tabelle con ottimizzazione per la memoria e alle stored procedure compilate in modo nativo da transazioni distribuite. Utilizzare le transazioni SQL.|  
|Operazione|Tabelle con ottimizzazione per la memoria come destinazione di MERGE|Le tabelle con ottimizzazione per la memoria non possono essere la destinazione di un'operazione **MERGE** . Usare le istruzioni **INSERT**, **UPDATE** e **DELETE**.|  
  
## <a name="indexes-on-memory-optimized-tables"></a>Indici in tabelle con ottimizzazione per la memoria  
 Nella tabella seguente vengono elencate le parole chiave e le funzionalità di [!INCLUDE[tsql](../../includes/tsql-md.md)] che possono essere inclusi nel testo del messaggio di un errore che interessa l'indice di una tabella ottimizzata per la memoria e l'azione correttiva per risolvere l'errore.  
  
|Type|Nome|Risoluzione|  
|----------|----------|----------------|  
|Funzionalità|Indice filtrato|Gli indici filtrati non sono supportati con le tabelle ottimizzate per la memoria. Omettere la clausola **WHERE** dalla specifica dell'indice.|  
|Funzionalità|included_columns|Non è necessario specificare le colonne incluse per le tabelle ottimizzate per la memoria. Tutte le colonne della tabella ottimizzata per la memoria vengono incluse in modo implicito in ogni indice ottimizzato per la memoria.|  
|Operazione|DROP INDEX|L'eliminazione degli indici nelle tabelle ottimizzate per la memoria non è supportata. È possibile eliminare gli indici mediante ALTER TABLE.<br /><br /> Per altre informazioni, vedere [Modifica di tabelle con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md).|  
|Opzione dell'indice|*Opzione di indice*|È supportata solo un'opzione di indice: BUCKET_COUNT per gli indici HASH.|  
  
## <a name="nonclustered-hash-indexes"></a>Indici hash non cluster  
 Nella tabella seguente vengono elencate le parole chiave e le funzionalità di [!INCLUDE[tsql](../../includes/tsql-md.md)] che possono essere incluse nel testo del messaggio di un errore che interessa un indice hash non cluster e l'azione correttiva per risolvere l'errore.  
  
|Type|Nome|Risoluzione|  
|----------|----------|----------------|  
|Opzione|ASC/DESC|Gli indici hash non cluster non sono ordinati. Rimuovere le parole chiave **ASC** e **DESC** dalla specifica della chiave di indice.|  
  
## <a name="natively-compiled-stored-procedures-and-user-defined-functions"></a>Funzioni definite dall'utente e stored procedure compilate in modo nativo  
 La tabella seguente elenca le parole chiave e le funzionalità di [!INCLUDE[tsql](../../includes/tsql-md.md)] che possono essere incluse nel testo del messaggio di un errore che interessa le stored procedure compilate in modo nativo, le funzioni definite dall'utente e l'azione correttiva per risolvere l'errore.  
  
|Type|Funzionalità|Risoluzione|  
|----------|-------------|----------------|  
|Funzionalità|Variabili di tabella inline|I tipi di tabella non possono essere dichiarati inline con le dichiarazioni di variabili. I tipi di tabella devono essere dichiarati in modo esplicito utilizzando un'istruzione **CREATE TYPE** .|  
|Funzionalità|Cursori|I cursori non sono supportati nelle stored procedure compilate in modo nativo.<br /><br /> Quando si esegue la procedura dal client, usare RPC anziché l'API cursore. Con ODBC, evitare l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] . **EXECUTE** e specificare direttamente il nome della procedura.<br /><br /> Quando si esegue la procedura da un batch [!INCLUDE[tsql](../../includes/tsql-md.md)] o da un'altra stored procedure, evitare di usare un cursore con la stored procedure compilata in modo nativo.<br /><br /> Quando si crea una stored procedure compilata in modo nativo, usare al posto del cursore la logica basata su set o un ciclo **WHILE** .|  
|Funzionalità|Valori predefiniti del parametro non costanti|Quando si utilizzano i valori predefiniti con i parametri nelle stored procedure compilate in modo nativo, i valori devono essere costanti. Rimuovere tutti i caratteri jolly dalle dichiarazioni di parametro.|  
|Funzionalità|EXTERNAL|Le stored procedure CLR non possono essere compilate in modo nativo. Rimuovere la clausola AS EXTERNAL o l'opzione NATIVE_COMPILATION dall'istruzione CREATE PROCEDURE.|  
|Funzionalità|Stored procedure numerate|Le stored procedure compilate in modo nativo non possono essere numerate. Rimuovere **;** _number_ dall'istruzione **CREATE PROCEDURE** .|  
|Funzionalità|INSERT di più righe... Istruzioni VALUES|Non è possibile inserire più righe utilizzando la stessa istruzione **INSERT** in una stored procedure compilata in modo nativo. Creare istruzioni **INSERT** per ogni riga.|  
|Funzionalità|Espressioni di tabella comuni|Le espressioni di tabella comuni (CTE) non sono supportate nelle stored procedure compilate in modo nativo. Riformulare la query.|  
|Funzionalità|COMPUTE|La clausola **COMPUTE** non è supportata. Rimuoverla dalla query.|  
|Funzionalità|SELECT INTO|La clausola **INTO** non è supportata con l'istruzione **SELECT** . Riscrivere la query come **INSERT INTO** _Table_ **SELECT**.|  
|Funzionalità|elenco delle colonne di inserimento incompleto|In genere, nelle istruzioni INSERT i valori devono essere specificati per tutte le colonne della tabella.<br /><br /> Tuttavia, nelle tabelle con ottimizzazione per la memoria Microsoft supporta i vincoli DEFAULT e le colonne IDENTITY(1,1). Tali colonne possono essere, e nel caso delle colonne IDENTITY devono essere, omesse dall'elenco di colonne INSERT.|  
|Funzionalità|*Funzione*|Alcune funzioni predefinite non sono supportate nelle stored procedure compilate in modo nativo. Rimuovere la funzione rifiutata dalla stored procedure. Per altre informazioni sulle funzioni predefinite supportate, vedere<br />[Funzionalità supportate per i moduli T-SQL compilati in modo nativo](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)o<br />[Stored procedure compilate in modo nativo](./a-guide-to-query-processing-for-memory-optimized-tables.md).|  
|Funzionalità|CASE|**Si applica a:** [!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)] e SQL Server a partire da [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Le espressioni **CASE** non sono supportate nelle query all'interno di stored procedure compilate in modo nativo. Creare query per ogni istruzione CASE. Per altre informazioni, vedere [Implementazione di un'istruzione CASE](../../relational-databases/in-memory-oltp/implementing-a-case-expression-in-a-natively-compiled-stored-procedure.md).<br/><br/>[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] e SQL Server a partire da [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] supportano le espressioni CASE.|  
|Funzionalità|INSERT EXECUTE|Rimuovere il riferimento.|  
|Funzionalità|EXECUTE|Funzionalità supportata solo per eseguire le stored procedure compilate in modo nativo e le funzioni definite dall'utente.|  
|Funzionalità|aggregazioni definite dall'utente|Le funzioni di aggregazione definite dall'utente non possono essere utilizzate nelle stored procedure compilate in modo nativo. Rimuovere il riferimento alla funzione dalla procedura.|  
|Funzionalità|metadati in modalità browse|Nelle stored procedure compilate in modo nativo non è ancora previsto il supporto per i metadati in modalità browse. Accertarsi che l'opzione di sessione **NO_BROWSETABLE** sia impostata su OFF.|  
|Funzionalità|DELETE con clausola FROM|La clausola **FROM** non è supportata dalle istruzioni **DELETE** con un'origine della tabella nelle stored procedure compilate in modo nativo.<br /><br /> **DELETE** con la clausola **FROM** è supportata quando viene utilizzata per indicare la tabella da cui eseguire l'eliminazione.|  
|Funzionalità|UPDATE con clausola FROM|La clausola **FROM** non è supportata dalle istruzioni **UPDATE** nelle stored procedure compilate in modo nativo.| 
|Funzionalità|stored procedure temporanee|Le stored procedure temporanee non possono essere compilate in modo nativo. Creare una stored procedure compilata in modo nativo permanente o una stored procedure [!INCLUDE[tsql](../../includes/tsql-md.md)] temporaneamente interpretata.|  
|Livello di isolamento|READ UNCOMMITTED|Il livello di isolamento READ UNCOMMITTED non è supportato dalle stored procedure compilate in modo nativo. Utilizzare un livello di isolamento supportato, ad esempio SNAPSHOT.|  
|Livello di isolamento|READ COMMITTED|Il livello di isolamento READ COMMITTED non è supportato dalle stored procedure compilate in modo nativo. Utilizzare un livello di isolamento supportato, ad esempio SNAPSHOT.|  
|Funzionalità|tabelle temporanee|Le tabelle del tempdb non possono essere utilizzate nelle stored procedure compilate in modo nativo. In alternativa, usare una variabile di tabella o una tabella ottimizzata per la memoria con DURABILITY=SCHEMA_ONLY.|  
|Funzionalità|DTC|Non è possibile accedere alle tabelle con ottimizzazione per la memoria e alle stored procedure compilate in modo nativo da transazioni distribuite. Utilizzare le transazioni SQL.|  
|Funzionalità|EXECUTE WITH RECOMPILE|L'opzione **WITH RECOMPILE** non è supportata dalle stored procedure compilate in modo nativo.|  
|Funzionalità|Esecuzione dalla connessione amministrativa dedicata.|Le stored procedure compilate in modo nativo non possono essere eseguite dalla connessione amministrativa dedicata. Utilizzare una connessione normale.|  
|Operazione|punto di salvataggio|Le stored procedure compilate in modo nativo non possono essere richiamate dalle transazioni che hanno un punto di salvataggio attivo. Rimuovere il punto di salvataggio dalla transazione.|  
|Operazione|ALTER AUTHORIZATION|La modifica del proprietario di una tabella ottimizzata per la memoria o di una stored procedure compilata in modo nativo non è supportata. Eliminare e ricreare la tabella o la stored procedure per modificare la proprietà.|  
|Operatore|OPENROWSET|Questo operatore non è supportato. Rimuovere **OPENROWSET** dalla stored procedure compilata in modo nativo.|  
|Operatore|OPENQUERY|Questo operatore non è supportato. Rimuovere **OPENQUERY** dalla stored procedure compilata in modo nativo.|  
|Operatore|OPENDATASOURCE|Questo operatore non è supportato. Rimuovere **OPENDATASOURCE** dalla stored procedure compilata in modo nativo.|  
|Operatore|OPENXML|Questo operatore non è supportato. Rimuovere **OPENXML** dalla stored procedure compilata in modo nativo.|  
|Operatore|CONTAINSTABLE|Questo operatore non è supportato. Rimuovere **CONTAINSTABLE** dalla stored procedure compilata in modo nativo.|  
|Operatore|FREETEXTTABLE|Questo operatore non è supportato. Rimuovere **FREETEXTTABLE** dalla stored procedure compilata in modo nativo.|  
|Funzionalità|funzioni con valori di tabella|Non è possibile fare riferimento alle funzioni con valori di tabella nelle stored procedure compilate in modo nativo. Una soluzione alternativa possibile per questa restrizione consiste nell'aggiungere la logica nelle funzioni con valori di tabella al corpo della procedura.|  
|Operatore|CHANGETABLE|Questo operatore non è supportato. Rimuovere **CHANGETABLE** dalla stored procedure compilata in modo nativo.|  
|Operatore|GOTO|Questo operatore non è supportato. Utilizzare altri costrutti procedurali come WHILE.|  
|Operatore|OFFSET|Questo operatore non è supportato. Rimuovere **OFFSET** dalla stored procedure compilata in modo nativo.|  
|Operatore|INTERSECT|Questo operatore non è supportato. Rimuovere **INTERSECT** dalla stored procedure compilata in modo nativo. In alcuni casi è possibile usare INNER JOIN per ottenere lo stesso risultato.|  
|Operatore|EXCEPT|Questo operatore non è supportato. Rimuovere **EXCEPT** dalla stored procedure compilata in modo nativo.|  
|Operatore|APPLY|**Si applica a:** [!INCLUDE[ssSQL14-md](../../includes/sssql14-md.md)] e SQL Server a partire da [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Questo operatore non è supportato. Rimuovere **APPLY** dalla stored procedure compilata in modo nativo.<br/><br/>[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] e SQL Server a partire da [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] supportano l'operatore APPLY nei moduli compilati in modo nativo.|  
|Operatore|PIVOT|Questo operatore non è supportato. Rimuovere **PIVOT** dalla stored procedure compilata in modo nativo.|  
|Operatore|UNPIVOT|Questo operatore non è supportato. Rimuovere **UNPIVOT** dalla stored procedure compilata in modo nativo.|  
|Operatore|CONTAINS|Questo operatore non è supportato. Rimuovere **CONTAINS** dalla stored procedure compilata in modo nativo.|  
|Operatore|FREETEXT|Questo operatore non è supportato. Rimuovere **FREETEXT** dalla stored procedure compilata in modo nativo.|  
|Operatore|TSEQUAL|Questo operatore non è supportato. Rimuovere **TSEQUAL** dalla stored procedure compilata in modo nativo.|  
|Operatore|LIKE|Questo operatore non è supportato. Rimuovere **LIKE** dalla stored procedure compilata in modo nativo.|  
|Operatore|NEXT VALUE FOR|Non è possibile fare riferimento alle sequenze all'interno di stored procedure compilate in modo nativo. Ottenere il valore utilizzando [!INCLUDE[tsql](../../includes/tsql-md.md)]interpretato, quindi passarlo alla stored procedure compilata in modo nativo. Per altre informazioni, vedere [Implementazione di IDENTITY in una tabella con ottimizzazione per la memoria](../../relational-databases/in-memory-oltp/implementing-identity-in-a-memory-optimized-table.md).|  
|Opzione SET|*opzione*|Le opzioni SET non possono essere modificate all'interno di stored procedure compilate in modo nativo. Alcune opzioni possono essere impostate tramite l'istruzione BEGIN ATOMIC. Per altre informazioni, vedere la sezione relativa ai blocchi atonici in [Natively Compiled Stored Procedures](./a-guide-to-query-processing-for-memory-optimized-tables.md).|  
|Operando|TABLESAMPLE|Questo operatore non è supportato. Rimuovere **TABLESAMPLE** dalla stored procedure compilata in modo nativo.|  
|Opzione|RECOMPILE|Le stored procedure compilate in modo nativo vengono compilate al momento della creazione. Rimuovere **RECOMPILE** dalla definizione della procedura.<br /><br /> È possibile eseguire sp_recompile in una stored procedure compilata in modo nativo per ricompilarla alla successiva esecuzione.|  
|Opzione|ENCRYPTION|Questa opzione non è supportata. Rimuovere **ENCRYPTION** dalla definizione della procedura.|  
|Opzione|FOR REPLICATION|Le stored procedure compilate in modo nativo non possono essere create per la replica. Rimuovere **FOR REPLICATION** dalla definizione della procedura.|  
|Opzione|FOR XML|Questa opzione non è supportata. Rimuovere **FOR XML** dalla stored procedure compilata in modo nativo.|  
|Opzione|FOR BROWSE|Questa opzione non è supportata. Rimuovere **FOR BROWSE** dalla stored procedure compilata in modo nativo.|  
|Hint per il join|HASH, MERGE|Nelle stored procedure compilate in modo nativo sono supportati solo i join a cicli annidati. I join merge e hash non sono supportati. Rimuovere l'hint per il join.|  
|Hint per la query|*Hint per la query*|Questo hint per la query non è all'interno di stored procedure compilate in modo nativo. Per gli hint per la query supportati, vedere [Hint per la query &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).|  
|Opzione|PERCENT|Questa opzione non è supportata con le clausole **TOP** . Rimuovere **PERCENT** dalla query nella stored procedure compilata in modo nativo.|  
|Opzione|WITH TIES|**Si applica a**: [!INCLUDE[ssSDS14_md](../../includes/sssql14-md.md)] e [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Questa opzione non è supportata con le clausole **TOP** . Rimuovere **WITH TIES** dalla query nella stored procedure compilata in modo nativo.<br/><br/>[!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] e SQL Server a partire da [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] supportano **TOP WITH TIES**.|  
|Funzione di aggregazione|*Funzione di aggregazione*|Non tutte le funzioni di aggregazione sono supportate. Per altre informazioni sulle funzioni di aggregazione supportate nei moduli T-SQL compilati in modo nativo, vedere [Funzionalità supportate per i moduli T-SQL compilati in modo nativo](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).|  
|Funzione di rango|*Funzione di rango*|Le funzioni di rango non sono supportate nelle stored procedure compilate in modo nativo. Rimuoverle dalla definizione della procedura.|  
|Funzione|*Funzione*|Questa funzione non è supportata. Per altre informazioni sulle funzioni supportate nei moduli T-SQL compilati in modo nativo, vedere [Funzionalità supportate per i moduli T-SQL compilati in modo nativo](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).|  
|.|*Istruzione*|Questa istruzione non è supportata. Per altre informazioni sulle funzioni supportate nei moduli T-SQL compilati in modo nativo, vedere [Funzionalità supportate per i moduli T-SQL compilati in modo nativo](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md).|  
|Funzionalità|MIN e MAX utilizzati con stringhe binarie e di caratteri|Le funzioni di aggregazione **MIN** e **MAX** non possono essere utilizzate per i valori delle stringhe binarie e di caratteri all'interno di stored procedure compilate in modo nativo.|  
|Funzionalità|GROUP BY ALL|ALL non può essere utilizzato con le clausole GROUP BY nelle stored procedure compilate in modo nativo. Rimuovere ALL dalla clausola GROUP BY.|  
|Funzionalità|GROUP BY ()|il raggruppamento da un elenco vuoto non è supportato. Rimuovere la clausola GROUP BY o includere colonne nell'elenco di raggruppamento.|  
|Funzionalità|ROLLUP|**ROLLUP** non può essere utilizzato con le clausole **GROUP BY** nelle stored procedure compilate in modo nativo. Rimuovere **ROLLUP** dalla definizione della procedura.|  
|Funzionalità|CUBE|**CUBE** non può essere utilizzato con le clausole **GROUP BY** nelle stored procedure compilate in modo nativo. Rimuovere **CUBE** dalla definizione della procedura.|  
|Funzionalità|GROUPING SETS|**GROUPING SETS** non può essere utilizzato con le clausole **GROUP BY** nelle stored procedure compilate in modo nativo. Rimuovere **GROUPING SETS** dalla definizione della procedura.|  
|Funzionalità|BEGIN TRANSACTION, COMMIT TRANSACTION e ROLLBACK TRANSACTION|Utilizzare i blocchi ATOMIC per controllare le transazioni e la gestione degli errori. Per altre informazioni, vedere [Atomic Blocks](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md).|  
|Funzionalità|Dichiarazioni di variabili di tabelle inline.|Le variabili di tabella devono fare riferimento ai tipi di tabella ottimizzata per la memoria definiti in modo esplicito. È consigliabile creare un tipo di tabella ottimizzata per la memoria e usare tale tipo per la dichiarazione di variabili, anziché specificare il tipo inline.|  
|Funzionalità|Tabelle basate su disco|Non è possibile accedere alle tabelle basate su dico dalle stored procedure compilate in modo nativo. Rimuovere i riferimenti alle tabelle basate su disco dalle stored procedure compilate in modo nativo. In alternativa, eseguire la migrazione delle tabelle basate su disco alle tabelle con ottimizzazione per la memoria.|  
|Funzionalità|Viste|Non è possibile accedere alle viste dalle stored procedure compilate in modo nativo. Anziché alle viste, fare riferimento alle tabelle di base sottostanti.|  
|Funzionalità|Funzioni con valori di tabella|**Si applica a:** [!INCLUDE[ssSDSFull_md](../../includes/ssSDSFull-md.md)] e SQL Server a partire da [!INCLUDE[ssSQL15-md](../../includes/sssql16-md.md)]<br/>Non è possibile accedere alle funzioni con valori di tabella con istruzioni multiple dai moduli T-SQL compilati in modo nativo. Le funzioni con valori di tabella inline sono supportate, ma devono essere create con la clausola WITH NATIVE_COMPILATION.<br/><br/>**Si applica a**: [!INCLUDE[ssSQL14-md](../../includes/ssSQL14-md.md)]<br/>Non è possibile fare riferimento alle funzioni con valori di tabella da moduli T-SQL compilati in modo nativo.|  
|Opzione|PRINT|Rimuovere il riferimento.|  
|Funzionalità|DDL|All'interno di moduli T-SQL compilati in modo nativo non sono supportate DLL.|  
|Opzione|STATISTICS XML|Non supportato. Quando si esegue una query, con l'opzione STATISTICS XML abilitata, viene restituito il contenuto XML senza la parte per la stored procedure compilata in modo nativo.|  
  
## <a name="transactions-that-access-memory-optimized-tables"></a>Transazioni che accedono alle tabelle con ottimizzazione per la memoria  
 Nella tabella seguente vengono elencate le parole chiave e le funzionalità di [!INCLUDE[tsql](../../includes/tsql-md.md)] che possono essere incluse nel testo del messaggio di un errore che interessa le transazioni che accedono alle tabelle ottimizzate per la memoria e l'azione correttiva per risolvere l'errore.  
  
|Type|Nome|Risoluzione|  
|----------|----------|----------------|  
|Funzionalità|punto di salvataggio|La creazione di punti di salvataggio espliciti nelle transazioni che accedono alle tabelle ottimizzate per la memoria non è supportata.|  
|Funzionalità|transazione associata|Le sessioni associate non possono partecipare alle transazioni che accedono alle tabelle ottimizzate per la memoria. Non associare la sessione prima di eseguire la procedura.|  
|Funzionalità|DTC|Le transazioni che accedono alle tabelle ottimizzate per la memoria non possono essere transazioni distribuite.|  
  
## <a name="see-also"></a>Vedere anche  
 [Migrazione a OLTP in memoria](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  
