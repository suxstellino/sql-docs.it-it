---
description: bcp_moretext
title: bcp_moretext | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_moretext
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_moretext function
ms.assetid: 23e98015-a8e4-4434-9b3f-9c7350cf965f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 70a364306f2498e6a9f504bcbb2d1c119c746d9d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749531"
---
# <a name="bcp_moretext"></a>bcp_moretext
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Invia parte di un valore del tipo di dati Long a lunghezza variabile a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
RETCODE bcp_moretext (  
        HDBC hdbc,  
        DBINT cbData,  
        LPCBYTE pData);  
```  
  
## <a name="arguments"></a>Argomenti  
 *hdbc*  
 Handle di connessione ODBC abilitato per la copia bulk.  
  
 *cbData*  
 Numero di byte di dati copiati in SQL Server dai dati a cui viene fatto riferimento da *pData*. Un valore di SQL_NULL_DATA indica NULL.  
  
 *pData*  
 Puntatore al blocco di dati Long a lunghezza variabile supportati da inviare a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="returns"></a>Restituisce  
 SUCCEED o FAIL.  
  
## <a name="remarks"></a>Commenti  
 Questa funzione può essere utilizzata in combinazione con [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) e [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md) per copiare i valori dei dati Long a lunghezza variabile per SQL Server in diversi blocchi più piccoli. **bcp_moretext** può essere utilizzato con colonne con i tipi di dati SQL Server seguenti: **Text**, **ntext**, **Image**, **varchar (max)**, **nvarchar (max)**, **varbinary (max)**, tipo definito dall'utente (UDT) e XML. **bcp_moretext** non supporta le conversioni dei dati, i dati forniti devono corrispondere al tipo di dati della colonna di destinazione.  
  
 Se **bcp_bind** viene chiamato con un parametro *pData* non null per i tipi di dati supportati da **bcp_moretext**, **bcp_sendrow** invia l'intero valore di dati, indipendentemente dalla lunghezza. Se, tuttavia, **bcp_bind** dispone di un parametro *pData* null per i tipi di dati supportati, è possibile utilizzare **bcp_moretext** per copiare i dati immediatamente dopo un risultato restituito da **bcp_sendrow** a indicare che tutte le colonne associate con dati presenti sono state elaborate.  
  
 Se si utilizza **bcp_moretext** per inviare una colonna con tipo di dati supportato in una riga, è necessario utilizzarla anche per inviare tutte le altre colonne con tipo di dati supportato nella riga. Non è possibile ignorare alcuna colonna. I tipi di dati supportati sono SQLTEXT, SQLNTEXT, SQLIMAGE, SQLUDT e SQLXML. Anche SQLCHARACTER, SQLVARCHAR, SQNCHAR, SQLBINARY e SQLVARBINARY rientrano in questa categoria se la colonna è di tipo varchar(max), nvarchar(max) o varbinary(max), rispettivamente.  
  
 La chiamata di **bcp_bind** o [bcp_collen](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-collen.md) imposta la lunghezza totale di tutte le parti di dati da copiare nella colonna SQL Server. Un tentativo di invio SQL Server un numero di byte maggiore di quello specificato nella chiamata a **bcp_bind** o **bcp_collen** genera un errore. Questo errore si verifica, ad esempio, in un'applicazione che ha utilizzato **bcp_collen** per impostare la lunghezza dei dati disponibili per una colonna SQL Server **testo** su 4500, quindi chiamata **bcp_moretext** cinque volte, indicando a ogni chiamata che la lunghezza del buffer di dati era 1000 byte.  
  
 Se una riga copiata contiene più di una colonna Long a lunghezza variabile, **bcp_moretext** invia prima i dati alla colonna con la numerazione ordinale più bassa, seguita dalla successiva colonna con numerazione ordinale più bassa e così via. Una corretta impostazione della lunghezza totale dei dati previsti è importante. Non è possibile segnalare, al di fuori dell'impostazione della lunghezza, che tutti i dati per una colonna sono stati ricevuti dalla copia bulk.  
  
 Quando si inviano valori **var (max)** al server utilizzando bcp_sendrow e bcp_moretext, non è necessario chiamare bcp_collen per impostare la lunghezza della colonna. Al contrario, solo per questi tipi, il valore viene terminato chiamando bcp_sendrow con una lunghezza pari a zero.  
  
 Un'applicazione in genere chiama **bcp_sendrow** e **bcp_moretext** all'interno dei cicli per inviare un certo numero di righe di dati. Ecco una descrizione di come eseguire questa operazione per una tabella contenente due colonne di **testo** :  
  
```  
while (there are still rows to send)  
{  
bcp_sendrow(...);  
  
for (all the data in the first varbinary(max) column)  
bcp_moretext(...);  
bcp_moretext(hdbc, 0, NULL);  
  
for (all the data in the second varbinary(max) column)  
bcp_moretext(...);  
bcp_moretext(hdbc, 0, NULL);  
  
}  
  
```  
  
## <a name="example"></a>Esempio  
 Questo esempio illustra come usare **bcp_moretext** con **bcp_bind** e **bcp_sendrow**:  
  
```  
// Variables like henv not specified.  
HDBC      hdbc;  
DBINT      idRow = 5;  
char*      pPart1 = "This text value isn't very long,";  
char*      pPart2 = " but it's broken into three parts";  
char*      pPart3 = " anyhow.";  
DBINT      cbAllParts;  
DBINT      nRowsProcessed;  
  
// Application initiation, get an ODBC environment handle, allocate the  
// hdbc, and so on.  
...   
  
// Enable bulk copy prior to connecting on allocated hdbc.  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_BCP, (SQLPOINTER) SQL_BCP_ON,  
   SQL_IS_INTEGER);  
  
// Connect to the data source, return on error.  
if (!SQL_SUCCEEDED(SQLConnect(hdbc, _T("myDSN"), SQL_NTS,  
   _T("myUser"), SQL_NTS, _T("myPwd"), SQL_NTS)))  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Initialize bulk copy.   
if (bcp_init(hdbc, "comdb..articles", NULL, NULL, DB_IN) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Bind program variables to table columns.   
if (bcp_bind(hdbc, (LPCBYTE) &idRow, 0, SQL_VARLEN_DATA, NULL, 0,  
   SQLINT4, 1)    == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
cbAllParts = (DBINT) (strnlen(pPart1, sizeof(pPart1) + 1) + strnlen(pPart2, sizeof(pPart2) + 1) + strnlen(pPart3, sizeof(pPart3) + 1));  
if (bcp_bind(hdbc, NULL, 0, cbAllParts, NULL, 0, SQLTEXT, 2) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Send this row, with the text value broken into three chunks.   
if (bcp_sendrow(hdbc) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
if (bcp_moretext(hdbc, (DBINT) strnlen(pPart1, sizeof(pPart1) + 1), pPart1) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
if (bcp_moretext(hdbc, (DBINT) strnlen(pPart2, sizeof(pPart2) + 1), pPart2) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
if (bcp_moretext(hdbc, (DBINT) strnlen(pPart3, sizeof(pPart3) + 1), pPart3) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// All done. Get the number of rows processed (should be one).  
nRowsProcessed = bcp_done(hdbc);  
  
// Carry on.  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di copia bulk](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
