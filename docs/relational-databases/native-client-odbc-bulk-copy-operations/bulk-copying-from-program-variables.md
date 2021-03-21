---
description: Copia bulk da variabili di programma
title: Copia bulk da variabili di programma | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- bulk copy [ODBC], program variables
- bcp_bind function
- mapping data types [ODBC]
- SQL Server Native Client ODBC driver, bulk copy
- data types [ODBC], bulk copying
- ODBC, bulk copy operations
- program variables [ODBC]
ms.assetid: e4284a1b-7534-4b34-8488-b8d05ed67b8c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 92ba50ba85056dc9a55bc68f91a370015ec91f2a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754771"
---
# <a name="bulk-copying-from-program-variables"></a>Copia bulk da variabili di programma
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  È possibile eseguire una copia bulk direttamente dalle variabili di programma. Dopo aver allocato le variabili in modo che contengano i dati per una riga e chiamando [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md) per avviare la copia bulk, chiamare [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) per ogni colonna per specificare il percorso e il formato della variabile di programma da associare alla colonna. Riempire ogni variabile con i dati, quindi chiamare [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md) per inviare una riga di dati al server. Ripetere il processo di riempimento delle variabili e chiamando **bcp_sendrow** fino a quando tutte le righe non sono state inviate al server, quindi chiamare [bcp_done](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-done.md) per specificare che l'operazione è stata completata.  
  
 Il **bcp_bind** parametro _pData_ contiene l'indirizzo della variabile da associare alla colonna. I dati di ogni colonna possono essere archiviati in due modi:  
  
-   Allocando una variabile in modo da contenere i dati.  
  
-   Allocando una variabile indicatore seguita immediatamente dalla variabile dati.  
  
 La variabile indicatore indica la lunghezza dei dati per le colonne a lunghezza variabile e indica inoltre i valori NULL se la colonna ammette valori NULL. Se viene utilizzata solo una variabile dati, l'indirizzo di questa variabile viene archiviato nel parametro **bcp_bind**_pData_ . Se viene utilizzata una variabile indicatore, l'indirizzo della variabile indicatore viene archiviato nel parametro **bcp_bind**_pData_ . Le funzioni di copia bulk calcolano il percorso della variabile dati aggiungendo i parametri **bcp_bind**_cbIndicator_ e *pData* .  
  
 **bcp_bind** supporta tre metodi per la gestione dei dati a lunghezza variabile:  
  
-   Utilizzare *cbData* solo con una variabile di dati. Inserire la lunghezza dei dati in *cbData*. Ogni volta che viene eseguita la copia bulk delle modifiche alla lunghezza dei dati, chiamare [bcp_collen](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-collen.md)per reimpostare *cbData*. Se viene utilizzato uno degli altri due metodi, specificare SQL_VARLEN_DATA per *cbData*. Se tutti i valori dei dati forniti per una colonna sono NULL, specificare SQL_NULL_DATA per *cbData*.  
  
-   Utilizzare variabili indicatore. Non appena un nuovo valore dei dati viene spostato nella variabile dati, archiviare la lunghezza del valore nella variabile indicatore. Se viene utilizzato uno degli altri due metodi, specificare 0 per *cbIndicator*.  
  
-   Utilizzare i puntatori ai caratteri di terminazione. Caricare il **bcp_bind** parametro _pTerm_ con l'indirizzo dello schema di bit che termina i dati. Se viene utilizzato uno degli altri due metodi, specificare NULL per *pTerm*.  
  
 Tutti e tre questi metodi possono essere utilizzati nella stessa chiamata di **bcp_bind** , nel qual caso viene utilizzata la specifica che comporta la copia del minor numero di dati.  
  
 Il parametro di _tipo_ **bcp_bind** utilizza DB-Library identificatori del tipo di dati e non gli identificatori del tipo di dati ODBC. DB-Library identificatori del tipo di dati sono definiti in sqlncli. h per l'utilizzo con la funzione di **BCP_BIND** ODBC.  
  
 Le funzioni di copia bulk non supportano tutti i tipi di dati C ODBC. Le funzioni di copia bulk, ad esempio, non supportano la struttura ODBC SQL_C_TYPE_TIMESTAMP, pertanto utilizzare [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md) o [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) per convertire i dati SQL_TYPE_TIMESTAMP odbc in una variabile SQL_C_CHAR. Se quindi si usa **bcp_bind** con un parametro di *tipo* SQLCHARACTER per associare la variabile a una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] colonna **DateTime** , le funzioni di copia bulk convertono la clausola di escape timestamp nella variabile di tipo carattere nel formato DateTime appropriato.  
  
 Nella tabella seguente vengono elencati i tipi di dati consigliati da utilizzare nel mapping da un tipo di dati SQL ODBC a un tipo di dati [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Tipo di dati SQL ODBC|Tipo di dati C ODBC|parametro di *tipo* bcp_bind|Tipo di dati di SQL Server|  
|-----------------------|----------------------|--------------------------------|--------------------------|  
|SQL_CHAR|SQL_C_CHAR|SQLCHARACTER|**character**<br /><br /> **char**|  
|SQL_VARCHAR|SQL_C_CHAR|SQLCHARACTER|**varchar**<br /><br /> **character varying**<br /><br /> **char varying**<br /><br /> **sysname**|  
|SQL_LONGVARCHAR|SQL_C_CHAR|SQLCHARACTER|**text**|  
|SQL_WCHAR|SQL_C_WCHAR|SQLNCHAR|**nchar**|  
|SQL_WVARCHAR|SQL_C_WCHAR|SQLNVARCHAR|**nvarchar**|  
|SQL_WLONGVARCHAR|SQL_C_WCHAR|SQLNTEXT|**ntext**|  
|SQL_DECIMAL|SQL_C_CHAR|SQLCHARACTER|**decimal**<br /><br /> **Dec**<br /><br /> **money**<br /><br /> **smallmoney**|  
|SQL_NUMERIC|SQL_C_NUMERIC|SQLNUMERICN|**numeric**|  
|SQL_BIT|SQL_C_BIT|SQLBIT|**bit**|  
|SQL_TINYINT (con segno)|SQL_C_SSHORT|SQLINT2|**smallint**|  
|SQL_TINYINT (senza segno)|SQL_C_UTINYINT|SQLINT1|**tinyint**|  
|SQL_SMALL_INT (con segno)|SQL_C_SSHORT|SQLINT2|**smallint**|  
|SQL_SMALL_INT (senza segno)|SQL_C_SLONG|SQLINT4|**int**<br /><br /> **integer**|  
|SQL_INTEGER (con segno)|SQL_C_SLONG|SQLINT4|**int**<br /><br /> **integer**|  
|SQL_INTEGER (senza segno)|SQL_C_CHAR|SQLCHARACTER|**decimal**<br /><br /> **Dec**|  
|SQL_BIGINT (con segno e senza segno)|SQL_C_CHAR|SQLCHARACTER|**bigint**|  
|SQL_REAL|SQL_C_FLOAT|SQLFLT4|**real**|  
|SQL_FLOAT|SQL_C_DOUBLE|SQLFLT8|**float**|  
|SQL_DOUBLE|SQL_C_DOUBLE|SQLFLT8|**float**|  
|SQL_BINARY|SQL_C_BINARY|SQLBINARY|**binary**<br /><br /> **timestamp**|  
|SQL_VARBINARY|SQL_C_BINARY|SQLBINARY|**varbinary**<br /><br /> **variabili binarie**|  
|SQL_LONGVARBINARY|SQL_C_BINARY|SQLBINARY|**image**|  
|SQL_TYPE_DATE|SQL_C_CHAR|SQLCHARACTER|**datetime**<br /><br /> **smalldatetime**|  
|SQL_TYPE_TIME|SQL_C_CHAR|SQLCHARACTER|**datetime**<br /><br /> **smalldatetime**|  
|SQL_TYPE_TIMESTAMP|SQL_C_CHAR|SQLCHARACTER|**datetime**<br /><br /> **smalldatetime**|  
|SQL_GUID|SQL_C_GUID|SQLUNIQUEID|**uniqueidentifier**|  
|SQL_INTERVAL_|SQL_C_CHAR|SQLCHARACTER|**char**|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non dispone di tipi di dati con segno **tinyint**, unsigned **smallint** o unsigned **int** . Per evitare la perdita di valori dei dati quando si esegue la migrazione di questi tipi di dati, creare la tabella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con il tipo di dati integer successivo più grande. Per impedire agli utenti di aggiungere in un secondo momento valori non compresi nell'intervallo consentito dal tipo di dati originale, applicare una regola alla colonna [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per limitare i valori consentiti all'intervallo supportato dal tipo di dati originale:  
  
```  
CREATE TABLE Sample_Ints(STinyIntCol   SMALLINT,  
USmallIntCol INT)  
GO  
CREATE RULE STinyInt_Rule  
AS   
@range >= -128 AND @range <= 127  
GO  
CREATE RULE USmallInt_Rule  
AS   
@range >= 0 AND @range <= 65535  
GO  
sp_bindrule STinyInt_Rule, 'Sample_Ints.STinyIntCol'  
GO  
sp_bindrule USmallInt_Rule, 'Sample_Ints.USmallIntCol'  
GO  
```  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta direttamente i tipi di dati intervallo. Tuttavia, un'applicazione può archiviare le sequenze di escape dell'intervallo come stringhe di caratteri in una colonna [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di tipo carattere. Tali valori potranno essere letti dall'applicazione per un utilizzo futuro ma non potranno essere utilizzati nelle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 Le funzioni di copia bulk possono essere utilizzate per caricare rapidamente i dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] letti da un'origine dati ODBC. Utilizzare [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md) per associare le colonne di un set di risultati alle variabili di programma, quindi utilizzare **bcp_bind** per associare le stesse variabili di programma a un'operazione di copia bulk. Se si chiama [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md) o **SQLFetch** , viene recuperata una riga di dati dall'origine dati ODBC nelle variabili di programma e viene chiamato [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md) esegue la copia bulk dei dati dalle variabili di programma a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Un'applicazione può utilizzare la funzione [bcp_colptr](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colptr.md) ogni volta che è necessario modificare l'indirizzo della variabile di dati originariamente specificata nel parametro **bcp_bind** _pData_ . Un'applicazione può utilizzare la funzione [bcp_collen](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-collen.md) ogni volta che è necessario modificare la lunghezza dei dati specificata originariamente nel parametro **bcp_bind**_cbData_ .  
  
 Non è possibile leggere i dati da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nelle variabili di programma utilizzando la copia bulk. Non esiste una funzione simile a "bcp_readrow". È possibile solo inviare i dati dall'applicazione al server.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione di operazioni di copia bulk &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)  
  
  
