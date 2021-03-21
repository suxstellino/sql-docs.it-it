---
description: bcp_bind
title: bcp_bind | Microsoft Docs
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_bind
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_bind function
ms.assetid: 6e335a5c-64b2-4bcf-a88f-35dc9393f329
author: markingmyname
ms.author: maghan
ms.custom: ''
ms.reviewer: ''
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3d73f055a77bb120d8c56425426273d6a06412b0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749641"
---
# <a name="bcp_bind"></a>bcp_bind

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Vengono associati dati provenienti da una variabile di programma a una colonna di tabella per eseguire la copia bulk in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="syntax"></a>Sintassi

```  
  
RETCODE bcp_bind (  
        HDBC hdbc,
        LPCBYTE pData,  
        INT cbIndicator,  
        DBINT cbData,  
        LPCBYTE pTerm,  
        INT cbTerm,  
        INT eDataType,  
        INT idxServerCol);  
```  
  
## <a name="arguments"></a>Argomenti

 *hdbc*  
 Handle di connessione ODBC abilitato per la copia bulk.  
  
 *pData*  
 Puntatore ai dati copiati. Se *eDataType* è SQLTEXT, SQLNTEXT, SQLXML, SQLUDT, SQLCHARACTER, SQLVARCHAR, SQLVARBINARY, SqlBinary, SQLNCHAR o SQLIMAGE, *pData* può essere null. Un valore NULL *pData* indica che i valori di dati Long verranno inviati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in blocchi utilizzando [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md). L'utente deve impostare *pData* su null solo se la colonna corrispondente al campo associato utente è una colonna BLOB, in caso contrario **bcp_bind** avrà esito negativo.  
  
 Se presenti nei dati, gli indicatori vengono visualizzati in memoria direttamente prima dei dati. Il parametro *pData* punta alla variabile indicatore in questo caso e la larghezza dell'indicatore, il parametro *cbIndicator* , viene utilizzata dalla copia bulk per indirizzare correttamente i dati utente.  
  
 *cbIndicator*  
 Lunghezza, espressa in byte, di un indicatore di lunghezza o Null per i dati della colonna. Valori di lunghezza di indicatore validi sono 0 (nel caso in cui non venga utilizzato un indicatore), 1, 2, 4 o 8. Gli indicatori vengono visualizzati in memoria direttamente prima di qualsiasi dato. La definizione del tipo di struttura seguente, ad esempio, potrebbe essere utilizzata per inserire valori integer in una tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante la copia bulk:  
  
```
typedef struct tagBCPBOUNDINT  
    {  
    int iIndicator;  
    int Value;  
    } BCPBOUNDINT;  
```  
  
 Nel caso di esempio, il parametro *pData* verrebbe impostato sull'indirizzo di un'istanza dichiarata della struttura, ovvero l'indirizzo del membro della struttura BCPBOUNDINT *iIndicator* . Il parametro *cbIndicator* verrà impostato sulla dimensione di un numero intero (sizeof (int)) e il parametro *cbData* verrà nuovamente impostato sulla dimensione di un numero intero (sizeof (int)). Per eseguire una copia bulk di una riga nel server contenente un valore NULL per la colonna associata, il valore del membro *iIndicator* dell'istanza deve essere impostato su SQL_NULL_DATA.  
  
 *cbData*  
 Numero di byte dei dati nella variabile di programma che non include la lunghezza di un carattere di terminazione o di un indicatore di lunghezza o Null.  
  
 L'impostazione di *cbData* su SQL_NULL_DATA indica che tutte le righe copiate nel server contengono un valore null per la colonna.  
  
 L'impostazione di *cbData* su SQL_VARLEN_DATA indica che il sistema utilizzerà un carattere di terminazione della stringa o un altro metodo per determinare la lunghezza dei dati copiati.  
  
 Per tipi di dati a lunghezza fissa, ad esempio numeri interi, il tipo di dati indica la lunghezza dei dati al sistema. Pertanto, per i tipi di dati a lunghezza fissa, *cbData* può essere SQL_VARLEN_DATA o la lunghezza dei dati in modo sicuro.  
  
 Per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i tipi di dati character e binary, *cbData* può essere SQL_VARLEN_DATA, SQL_NULL_DATA, un valore positivo o 0. Se *cbData* è SQL_VARLEN_DATA, il sistema utilizza un indicatore di lunghezza/null (se presente) o una sequenza di caratteri di terminazione per determinare la lunghezza dei dati. Se vengono forniti entrambi, il sistema utilizza quello che comporta la copia del minori numero di dati. Se *cbData* è SQL_VARLEN_DATA, il tipo di dati della colonna è di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo carattere o binario e non viene specificato né un indicatore di lunghezza né una sequenza di caratteri di terminazione, il sistema restituisce un messaggio di errore.  
  
 Se *cbData* è 0 o un valore positivo, il sistema utilizza *cbData* come lunghezza dei dati. Tuttavia, se, oltre a un valore *cbData* positivo, viene specificato un indicatore di lunghezza o una sequenza di caratteri di terminazione, il sistema determina la lunghezza dei dati utilizzando il metodo che comporta la copia del minor numero di dati.  
  
 Il valore del parametro *cbData* rappresenta il numero di byte di dati. Se i dati di tipo carattere sono rappresentati da caratteri wide Unicode, un valore positivo per il parametro *cbData* rappresenta il numero di caratteri moltiplicato per la dimensione in byte di ogni carattere.  
  
 *pTerm*  
 Puntatore al modello di byte, se presente, che contrassegna la fine di questa variabile di programma. Le stringhe ANSI e MBCS C, ad esempio, presentano generalmente un carattere di terminazione di 1 byte (\0).  
  
 Se non è presente alcun carattere di terminazione per la variabile, impostare *pTerm* su null.  
  
 È possibile utilizzare una stringa vuota ("") per definire il carattere di terminazione Null C come carattere di terminazione della variabile di programma. Poiché la stringa vuota con terminazione null costituisce un singolo byte (byte del carattere di terminazione), impostare *cbTerm* su 1. Per indicare, ad esempio, che la stringa in *szName* è con terminazione null e che il carattere di terminazione deve essere usato per indicare la lunghezza:  
  
```
bcp_bind(hdbc, szName, 0,  
   SQL_VARLEN_DATA, "", 1,  
   SQLCHARACTER, 2)  
```

 Un formato senza terminazione di questo esempio potrebbe indicare che i 15 caratteri devono essere copiati dalla variabile *szName* alla seconda colonna della tabella associata:  

```
bcp_bind(hdbc, szName, 0, 15,
   NULL, 0, SQLCHARACTER, 2)  
```

 L'API della copia bulk esegue la conversione dei caratteri da Unicode a MBCS in base alle necessità. Verificare che la stringa di byte del carattere di terminazione e la lunghezza della stringa di byte siano impostate correttamente. Per indicare, ad esempio, che la stringa in *szName* è una stringa di caratteri wide Unicode, terminata dal valore del terminatore null Unicode:  
  
```  
bcp_bind(hdbc, szName, 0,
   SQL_VARLEN_DATA, L"",  
   sizeof(WCHAR), SQLNCHAR, 2)  
```  
  
 Se la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] colonna associata è un carattere wide, non viene eseguita alcuna conversione su [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md). Se la colonna di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è un tipo di carattere MBCS, la conversione da carattere wide a carattere multibyte viene eseguita quando i dati vengono inviati a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *cbTerm*  
 Numero dei byte presenti nel carattere di terminazione per la variabile di programma, se presente. Se non è presente alcun carattere di terminazione per la variabile, impostare *cbTerm* su 0.  

*eDataType* Tipo di dati C della variabile di programma. I dati della variabile di programma vengono convertiti nel tipo della colonna di database. Se questo parametro è 0, non viene eseguita alcuna conversione.  

Il parametro *eDataType* viene enumerato in base ai [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] token del tipo di dati in sqlncli. h e non negli enumeratori dei tipi di dati ODBC C. È ad esempio possibile specificare un numero intero a due byte, SQL_C_SHORT di tipo ODBC, utilizzando il tipo specifico di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQLINT2.  

[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] è stato introdotto il supporto per i token del tipo di dati SQLXML e SQLUDT nel parametro **_eDataType_** .  

La tabella seguente elenca i tipi di dati enumerati validi e i tipi di dati ODBC C corrispondenti.

|eDataType|Tipo C|
|-----------------------|------------|  
|SQLTEXT|char *|  
|SQLNTEXT|wchar_t *|  
|SQLCHARACTER|char *|  
|SQLBIGCHAR|char *|  
|SQLVARCHAR|char *|  
|SQLBIGVARCHAR|char *|  
|SQLNCHAR|wchar_t *|  
|SQLNVARCHAR|wchar_t *|  
|SQLBINARY|unsigned char *|  
|SQLBIGBINARY|unsigned char *|  
|SQLVARBINARY|unsigned char *|  
|SQLBIGVARBINARY|unsigned char *|  
|SQLBIT|char|  
|SQLBITN|char|  
|SQLINT1|char|  
|SQLINT2|short int|  
|SQLINT4|INT|  
|SQLINT8|_int64|  
|SQLINTN|*cbIndicator*<br /> 1: SQLINT1<br /> 2: SQLINT2<br /> 4: SQLINT4<br /> 8: SQLINT8|  
|SQLFLT4|float|  
|SQLFLT8|float|  
|SQLFLTN|*cbIndicator*<br /> 4: SQLFLT4<br /> 8: SQLFLT8|  
|SQLDECIMALN|SQL_NUMERIC_STRUCT|  
|SQLNUMERICN|SQL_NUMERIC_STRUCT|  
|SQLMONEY|DBMONEY|  
|SQLMONEY4|DBMONEY4|  
|SQLMONEYN|*cbIndicator*<br /> 4: SQLMONEY4<br /> 8: SQLMONEY|  
|SQLTIMEN|SQL_SS_TIME2_STRUCT|  
|SQLDATEN|SQL_DATE_STRUCT|  
|SQLDATETIM4|DBDATETIM4|  
|SQLDATETIME|DBDATETIME|  
|SQLDATETIMN|*cbIndicator*<br /> 4: SQLDATETIM4<br /> 8: SQLDATETIME|  
|SQLDATETIME2N|SQL_TIMESTAMP_STRUCT|  
|SQLDATETIMEOFFSETN|SQL_SS_TIMESTAMPOFFSET_STRUCT|  
|SQLIMAGE|unsigned char *|  
|SQLUDT|unsigned char *|  
|SQLUNIQUEID|SQLGUID|  
|SQLVARIANT|*Qualsiasi tipo di dati ad eccezione di:*<br />-   text<br />-   ntext<br />-   image<br />-   varchar(max)<br />-   varbinary(max)<br />-   nvarchar(max)<br />-   xml<br />-   timestamp|  
|SQLXML|*Tipi di dati C supportati:*<br />-   char*<br />-   wchar_t *<br />-   unsigned char *|  

*idxServerCol su* Posizione ordinale della colonna nella tabella di database in cui vengono copiati i dati. La prima colonna di una tabella è la colonna 1. La posizione ordinale di una colonna viene segnalata da [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md).  
  
## <a name="returns"></a>Restituisce

 SUCCEED o FAIL.

## <a name="remarks"></a>Commenti

Utilizzare **bcp_bind** per un modo rapido ed efficiente per copiare dati da una variabile di programma in una tabella in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

Chiamare [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md) prima di chiamare questa o qualsiasi altra funzione di copia bulk. La chiamata di **bcp_init** imposta la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabella di destinazione per la copia bulk. Quando si chiama **bcp_init** per l'uso con **bcp_bind** e [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md), il **bcp_init** parametro _szDataFile_ , che indica il file di dati, è impostato su null. il **bcp_init** parametro _eDirection_ è impostato su DB_IN.  

Eseguire una chiamata di **bcp_bind** separata per ogni colonna della [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabella in cui si desidera copiare. Una volta effettuate le chiamate **bcp_bind** necessarie, chiamare **bcp_sendrow** per inviare una riga di dati dalle variabili di programma a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La riassociazione della colonna non è supportata.

Quando si desidera [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eseguire il commit delle righe già ricevute, chiamare [bcp_batch](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-batch.md). Ad esempio, chiamare **bcp_batch** una volta per ogni 1000 righe inserite o in qualsiasi altro intervallo.  

Quando non sono presenti altre righe da inserire, chiamare [bcp_done](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-done.md). In caso contrario, viene generato un errore.

Le impostazioni dei parametri di controllo, specificate con [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md), non hanno alcun effetto sui trasferimenti **bcp_bind** righe.  

Se *pData* per una colonna è impostato su null perché il relativo valore verrà fornito dalle chiamate a [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md), anche tutte le colonne successive con *eDataType* impostato su SQLtext, SQLNTEXT, SQLXML, SQLUDT, SQLCHARACTER, SQLVARCHAR, SQLVARBINARY, SqlBinary, SQLNCHAR o SQLIMAGE devono essere associate con *pData* impostato su null e anche i rispettivi valori devono essere forniti dalle chiamate al **bcp_moretext**.  

Per i nuovi tipi di valore di grandi dimensioni, ad esempio **varchar (max)**, **varbinary (max)** o **nvarchar (max)**, è possibile utilizzare SQLCHARACTER, SQLVARCHAR, SQLVARBINARY, SqlBinary e SQLNCHAR come indicatori di tipo nel parametro *eDataType* .  

Se *cbTerm* è diverso da 0, qualsiasi valore (1, 2, 4 o 8) è valido per il prefisso (*cbIndicator*). In questa situazione, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] native client verrà cercata la terminazione, il calcolo della lunghezza dei dati rispetto al carattere di terminazione (*i*) e l'impostazione di *cbData* sul valore minore di i e sul valore di prefix.  

Se *cbTerm* è 0 e *cbIndicator* (il prefisso) non è 0, *cbIndicator* deve essere 8. Il prefisso a 8 byte può assumere i valori seguenti:  

- 0xFFFFFFFFFFFFFFFF indica un valore Null per il campo  

- 0xFFFFFFFFFFFFFFFE viene considerato un valore di prefisso speciale, che viene usato per inviare in modo efficiente i dati in blocchi al server. Il formato dei dati con questo prefisso speciale è:  

- <SPECIAL_PREFIX> \<0 or more  DATA CHUNKS> <ZERO_CHUNK> dove:  

- SPECIAL_PREFIX è 0xFFFFFFFFFFFFFFFE  

- DATA_CHUNK è un prefisso a 4 byte che contiene la lunghezza del blocco, seguito dai dati effettivi la cui lunghezza è specificata nel prefisso a 4 byte.  

- ZERO_CHUNK è un valore a 4 byte che contiene tutti gli zeri (00000000) che indicano la fine dei dati.  

- Qualsiasi altra lunghezza di 8 byte valida viene considerata come una lunghezza di dati normale.  

 La chiamata di [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md) quando si utilizza **bcp_bind** genera un errore.  
  
## <a name="bcp_bind-support-for-enhanced-date-and-time-features"></a>Supporto di bcp_bind per le caratteristiche avanzate di data e ora

Per informazioni sui tipi utilizzati con il parametro *eDataType* per i tipi data/ora, vedere la pagina relativa alle [modifiche di copia bulk per i tipi di data e ora avanzati &#40;OLE DB e&#41;ODBC](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md).  

Per ulteriori informazioni, vedere [miglioramenti di data e ora &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  

## <a name="example"></a>Esempio
  
```
#include sql.h  
#include sqlext.h  
#include odbcss.h  
// Variables like henv not specified.  
HDBC      hdbc;  
char         szCompanyName[MAXNAME];  
DBINT      idCompany;  
DBINT      nRowsProcessed;  
DBBOOL      bMoreData;  
char*      pTerm = "\t\t";  
  
// Application initiation, get an ODBC environment handle, allocate the  
// hdbc, and so on.  
...
  
// Enable bulk copy prior to connecting on allocated hdbc.  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_BCP, (SQLPOINTER) SQL_BCP_ON,  
   SQL_IS_INTEGER);  
  
// Connect to the data source; return on error.  
if (!SQL_SUCCEEDED(SQLConnect(hdbc, _T("myDSN"), SQL_NTS,  
   _T("myUser"), SQL_NTS, _T("myPwd"), SQL_NTS)))  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Initialize bcp.
if (bcp_init(hdbc, "comdb..accounts_info", NULL, NULL  
   DB_IN) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Bind program variables to table columns.
if (bcp_bind(hdbc, (LPCBYTE) &idCompany, 0, sizeof(DBINT), NULL, 0,  
   SQLINT4, 1)    == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
if (bcp_bind(hdbc, (LPCBYTE) szCompanyName, 0, SQL_VARLEN_DATA,  
   (LPCBYTE) pTerm, strnlen(pTerm, sizeof(pTerm)), SQLCHARACTER, 2) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
while (TRUE)  
   {  
   // Retrieve and process program data.
   if ((bMoreData = getdata(&idCompany, szCompanyName)) == TRUE)  
      {  
      // Send the data.
      if (bcp_sendrow(hdbc) == FAIL)  
         {  
         // Raise error and return.  
         return;  
         }  
      }  
   else  
      {  
      // Break out of loop.  
      break;  
      }  
   }  
  
// Terminate the bulk copy operation.  
if ((nRowsProcessed = bcp_done(hdbc)) == -1)  
   {  
   printf_s("Bulk-copy unsuccessful.\n");  
   return;  
   }  
  
printf_s("%ld rows copied.\n", nRowsProcessed);  
```  
  
## <a name="see-also"></a>Vedere anche

 [Funzioni di copia bulk](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)
