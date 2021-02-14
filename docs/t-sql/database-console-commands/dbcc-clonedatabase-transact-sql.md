---
description: DBCC CLONEDATABASE (Transact-SQL)
title: DBCC CLONEDATABASE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/23/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CLONEDATABASE
- CLONE DATABASE
- DBCC_CLONEDATABASE_TSQL
- DBCC CLONEDATABASE
- DBCC CLONE DATABASE
- CLONEDATABASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database cloning [SQL Server]
- cloning databases
- clone databases
- cloning database
- clone database
- copying databases
- copy databases
- copying database
- copy database
- NO_STATISTICS option
- NO_QUERYSTORE option
- VERIFY_CLONEDB option
- BACKUP_CLONEDB option
- database copying [SQL Server]
- database cloning [SQL Server]
- DBCC CLONEDATABASE statement
ms.assetid: ''
author: bluefooted
ms.author: pamela
manager: amitban
ms.openlocfilehash: e0ddfa333535c48d3338eb26a078a67bbf7079d8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340648"
---
# <a name="dbcc-clonedatabase-transact-sql"></a>DBCC CLONEDATABASE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Genera un clone solo schema di un database tramite DBCC CLONEDATABASE per esaminare i problemi di prestazioni relativi a Query Optimizer.

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
DBCC CLONEDATABASE   
(  
    source_database_name
    ,  target_database_name
)
    [ WITH { [ NO_STATISTICS ] [ , NO_QUERYSTORE ] [ , VERIFY_CLONEDB | SERVICEBROKER ] [ , BACKUP_CLONEDB ] } ]     
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*nome_database_di_origine*  
Nome del database da copiare. 
  
*target_database_name*  
Nome del database in cui verrà copiato il database di origine. Questo database verrà creato da DBCC CLONEDATABASE e non deve esistere già. 
  
NO_STATISTICS  
Specifica se le statistiche di tabella/indice devono essere escluse dal clone. Se questa opzione non è specificata, le statistiche di tabella/indice vengono incluse automaticamente. Questa opzione è disponibile a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3 e [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1.

NO_QUERYSTORE<br>
Specifica se i dati di archivio query devono essere esclusi dal clone. Se questa opzione non è specificata, i dati dell'archivio query verranno copiati nel clone se l'archivio query è abilitato nel database di origine. Questa opzione è disponibile a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1.

VERIFY_CLONEDB  
Controlla la coerenza del database.  Questa opzione è obbligatoria se il database clonato è destinato all'utilizzo per la produzione.  L'abilitazione di VERIFY_CLONEDB disabilita anche la raccolta di archivio query e statistiche pertanto, è equivalente all'esecuzione con VERIFY_CLONEDB, NO_STATISTICS, NO_QUERYSTORE.  Questa opzione è disponibile a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU8.

> [!NOTE]  
> Per confermare che il database clonato è pronto per la produzione è possibile utilizzare il comando seguente: <br/>`SELECT DATABASEPROPERTYEX('clone_database_name', 'IsVerifiedClone')`


SERVICEBROKER<br>
Specifica se i cataloghi del sistema correlato Service Broker devono essere inclusi nel clone.  L'opzione SERVICEBROKER non può essere usata in combinazione con VERIFY_CLONEDB.  Questa opzione è disponibile a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU8.

BACKUP_CLONEDB  
Crea e verifica una copia di backup del database clone.  Se utilizzato in combinazione con VERIFY_CLONEDB, il database clone viene verificato prima della creazione della copia di backup.  Questa opzione è disponibile a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 e [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU8.
  
## <a name="remarks"></a>Osservazioni
Le convalide seguenti vengono eseguite da DBCC CLONEDATABASE. Il comando non riesce se una delle convalide ha esito negativo.
- Il database di origine deve essere un database utente. La clonazione dei database di sistema (master, modello, msdb, tempdb, database di distribuzione e così via) non è consentita.
- Il database di origine deve essere online e leggibile.
- Non deve esistere già un database che utilizza lo stesso nome del database clone.
- Il comando non si trova in una transazione utente.

Se tutte le convalide hanno esito positivo, la clonazione del database di origine viene eseguita con le operazioni seguenti:
- Crea un nuovo database di destinazione che utilizza lo stesso layout di file del database di origine ma con le dimensioni di file predefinite del database modello.
- Crea uno snapshot interno del database di origine.
- Copia i metadati di sistema dal database di origine al database di destinazione.
- Copia gli schemi di tutti gli oggetti dal database di origine al database di destinazione.
- Copia le statistiche per tutti gli indici dal database di origine al database di destinazione.

> [!NOTE]  
> Il nuovo database generato da DBCC CLONEDATABASE è progettato principalmente per scopi di diagnosi e per la risoluzione dei problemi.  Affinché il database clonato sia supportato per l'utilizzo come database di produzione, l'opzione VERIFY_CLONEDB deve essere utilizzata.

Tutti i file nel database di destinazione erediteranno le impostazioni di dimensione e aumento dal database modello. I nomi dei file per il database di destinazione seguiranno la convenzione di numero source_file_name _underscore_random. Se il nome del file generato esiste già nella cartella di destinazione, DBCC CLONEDATABASE avrà esito negativo.

DBCC CLONEDATABASE non supporta la creazione di un clone se sono presenti oggetti utente (tabelle, indici, schemi, ruoli e così via) che sono stati creati nel database modello. Se nel database modello sono presenti oggetti utente, il clone database ha esito negativo e viene visualizzato il messaggio di errore seguente:

```
Msg 2601, Level 14, State 1, Line 1
Cannot insert duplicate key row in object <system table> with unique index 'index name'. The duplicate key value is <key value>   
```

> [!IMPORTANT]
> Se si dispone di indici columnstore, vedere [Considerazioni sull'ottimizzazione delle query con gli indici columnstore nei database clone](https://techcommunity.microsoft.com/t5/SQL-Server/Considerations-when-tuning-your-queries-with-columnstore-indexes/ba-p/385294) per aggiornare le statistiche degli indici columnstore prima di eseguire il comando **DBCC CLONEDATABASE**.  A partire da SQL Server 2019 i passaggi manuali descritti nell'articolo precedente non saranno più necessari in quanto il comando **DBCC CLONEDATABASE** raccoglie queste informazioni automaticamente.

<a name="ctp23"></a>

## <a name="stats-blob-for-columnstore-indexes"></a>BLOB di statistiche per gli indici columnstore

In [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]`DBCC CLONEDATABASE` acquisisce automaticamente i BLOB di statistiche per gli indici columnstore. Di conseguenza non sono necessari passaggi manuali. `DBCC CLONEDATABASE` crea una copia di un database con il solo schema, che include tutti gli elementi necessari per la risoluzione dei problemi di prestazioni delle query senza copiare i dati. Nelle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] il comando non copiava le statistiche necessarie per rilevare con precisione i problemi delle query dell'indice columnstore e per acquisire queste informazioni erano necessari passaggi manuali.

Per informazioni correlate alla sicurezza dei dati nei database clonati, vedere [Understanding data security in cloned databases](https://techcommunity.microsoft.com/t5/SQL-Server/Understanding-data-security-in-cloned-databases-created-using/ba-p/385287) (Informazioni sulla sicurezza dei dati nei database clonati).

## <a name="internal-database-snapshot"></a>Snapshot di database interno
DBCC CLONEDATABASE utilizza uno snapshot di database interno del database di origine per assicurare la coerenza transazionale necessaria per eseguire la copia. L'uso dello snapshot consente di evitare problemi di blocco e concorrenza durante l'esecuzione di questi comandi. Se non è possibile creare uno snapshot, DBCC CLONEDATABASE avrà esito negativo. 

Vengono mantenuti blocchi a livello di database durante i passaggi del processo di copia seguenti:
- Convalidare il database di origine
- Ottenere il blocco S per il database di origine
- Creare uno snapshot del database di origine
- Creare un database clone (un database vuoto ereditato dal database modello)
- Ottenere il blocco X per il database di origine
- Copiare i metadati nel database clone
- Rilasciare tutti i blocchi di database

Non appena il comando è terminato, lo snapshot interno viene eliminato. Nei database clonati le opzioni DB_CHAINING e TRUSTWORTHY sono disattivate. 

## <a name="supported-objects"></a>Oggetti supportati
Nel database di destinazione possono essere clonati solo gli oggetti seguenti. Gli oggetti crittografati sono clonati, ma non sono utilizzabili nel database clone. Tutti gli oggetti che non sono elencati nella sezione seguente non sono supportati nel clone: 
- APPLICATION ROLE
- AVAILABILITY GROUP
- COLUMNSTORE INDEX
- CDB
- CDC
- CLR (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 e versioni successive)
- DATABASE PROPERTIES
- DEFAULT
- FILES AND FILEGROUPS
- Full-text (a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU2)
- FUNCTION
- INDEX
- LOGIN
- PARTITION FUNCTION
- PARTITION SCHEME
- PROCEDURE   
> [!NOTE]   
> Le procedure [!INCLUDE[tsql](../../includes/tsql-md.md)] sono supportate in tutte le versioni a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2. Le procedure CLR sono supportate a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3. Le procedure compilate in modo nativo sono supportate a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1.  

- QUERY STORE (a partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1)   
> [!NOTE]   
> I dati di archivio query vengono copiati solo se sono abilitati nel database di origine. Per copiare le statistiche di runtime più recenti come parte dell'archivio query, eseguire sp_query_store_flush_db per cancellare le statistiche di runtime nell'archivio query prima di eseguire DBCC CLONEDATABASE.  

- ROLE
- RULE
- SCHEMA
- SEQUENCE
- SPATIAL INDEX
- STATISTICS
- SYNONYM
- TABLE
- MEMORY OPTIMIZED TABLES (solo in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 e versioni successive).
- FILESTREAM AND FILETABLE OBJECTS (a partire da [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3, [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 e versioni successive). 
- TRIGGER
- TYPE
- UPGRADED DB
- USER
- VIEW
- XML INDEX
- XML SCHEMA COLLECTION  

## <a name="permissions"></a>Autorizzazioni  
È richiesta l'appartenenza al ruolo predefinito del server **sysadmin** .

## <a name="error-log-messages"></a>Messaggi del registro errori
I messaggi seguenti sono un esempio di quelli registrati nel registro errori durante il processo di clonazione:

```
2018-03-26 15:33:56.05 spid53 Database cloning for 'sourcedb' has started with target as 'sourcedb_clone'.

2018-03-26 15:33:56.46 spid53 Starting up database 'sourcedb_clone'.

2018-03-26 15:33:57.80 spid53 Setting database option TRUSTWORTHY to OFF for database 'sourcedb_clone'.

2018-03-26 15:33:57.80 spid53 Setting database option DB_CHAINING to OFF for database 'sourcedb_clone'.

2018-03-26 15:33:57.88 spid53 Starting up database 'sourcedb_clone'.

2018-03-26 15:33:57.91 spid53 Database 'sourcedb_clone' is a cloned database. A cloned database should be used for diagnostic purposes only and is not supported for use in a production environment.

2018-03-26 15:33:57.92 spid53 Database cloning for 'sourcedb' has finished. Cloned database is 'sourcedb_clone'.
```

## <a name="database-properties"></a>Proprietà dei database
`DATABASEPROPERTYEX('dbname', 'IsClone')` restituisce 1 se il database è stato generato utilizzando DBCC CLONEDATABASE.

`DATABASEPROPERTYEX('dbname', 'IsVerifiedClone')` restituisce 1 se il database è stato verificato usando con WITH VERIFY_CLONEDB.

## <a name="examples"></a>Esempi  
  
### <a name="a-creating-a-clone-of-a-database-that-includes-schema-statistics-and-query-store"></a>R. Creazione del clone di un database che include schema, statistiche e archivio query 
L'esempio seguente crea un clone del database AdventureWorks che include schema, statistiche e dati di Query Store ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 e versioni successive)

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone);    
GO 
```  
  
### <a name="b-creating-a-schema-only-clone-of-a-database-without-statistics"></a>B. Creazione di un clone solo schema di un database senza statistiche 
L'esempio seguente crea un clone del database AdventureWorks che non include le statistiche ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 CU3 e versioni successive)

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH NO_STATISTICS;    
GO 
```  

### <a name="c-creating-a-schema-only-clone-of-a-database-without-statistics-and-query-store"></a>C. Creazione di un clone solo schema di un database senza statistiche e archivio query 
L'esempio seguente crea un clone del database AdventureWorks che non include le statistiche e i dati di Query Store ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 e versioni successive)

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH NO_STATISTICS, NO_QUERYSTORE;    
GO 
```  

### <a name="d-creating-a-clone-of-a-database-that-is-verified-for-production-use"></a>D. Creazione di un clone di un database verificato per l'uso in produzione
L'esempio seguente crea un clone solo schema del database AdventureWorks senza statistiche e dati di Query Store verificato per l'uso come database di produzione ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 e versioni successive).

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH VERIFY_CLONEDB;    
GO 
```  
  
### <a name="e-creating-a-clone-of-a-database-that-is-verified-for-production-use-that-includes-a-backup-of-the-cloned-database"></a>E. Creazione di un clone di un database verificato per l'uso in produzione che include un backup del database clonato
L'esempio seguente crea un clone solo schema del database AdventureWorks senza statistiche e dati dell'archivio query verificato per l'uso come database di produzione.  Verrà anche creato un backup verificato del database clonato ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 e versioni successive).

```sql  
DBCC CLONEDATABASE (AdventureWorks, AdventureWorks_Clone) WITH VERIFY_CLONEDB, BACKUP_CLONEDB;    
GO 
```

## <a name="see-also"></a>Vedere anche
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)    
[Come generare uno script dei metadati del database necessari per creare un database solo statistiche in SQL Server](https://support.microsoft.com/help/914288)   

