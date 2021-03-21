---
description: bcp_control
title: bcp_control | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_control
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_control function
ms.assetid: 32187282-1385-4c52-9134-09f061eb44f5
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 335dd32c4d3ba62c9aae09cc38adb4383d1f71fd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749581"
---
# <a name="bcp_control"></a>bcp_control
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Modifica le impostazioni predefinite per vari parametri di controllo per una copia bulk tra un file e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
RETCODE bcp_control (  
        HDBC hdbc,  
        INT eOption,  
        void* iValue);  
```  
  
## <a name="arguments"></a>Argomenti  
 *hdbc*  
 Handle di connessione ODBC abilitato per la copia bulk.  
  
 *eOption*  
 I valori validi sono i seguenti:  
  
 BCPABORT  
 Arresta un'operazione di copia bulk già in corso. Chiamare **bcp_control** con un *eOption* di BCPABORT da un altro thread per arrestare un'operazione di copia bulk in esecuzione. Il parametro *iValue* viene ignorato.  
  
 BCPBATCH  
 Indica il numero di righe per batch. L'impostazione predefinita è 0 e indica tutte le righe di una tabella, quando i dati vengono estratti, o tutte le righe nel file di dati dell'utente, quando i dati vengono copiati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un valore inferiore a 1 comporta la reimpostazione di BCPBATCH sul valore predefinito.  
  
 BCPDELAYREADFMT  
 Un valore booleano, se impostato su true, causerà la lettura [bcp_readfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-readfmt.md) in fase di esecuzione. Se false (impostazione predefinita), bcp_readfmt leggerà immediatamente il file di formato. Si verificherà un errore di sequenza se BCPDELAYREADFMT è true e si chiama bcp_columns o bcp_setcolfmt.  
  
 Si verificherà un errore di sequenza anche se si chiama `bcp_control(hdbc,` BCPDELAYREADFMT `, (void *)FALSE)` dopo avere chiamato `bcp_control(hdbc,` BCPDELAYREADFMT `, (void *)TRUE)` e bcp_writefmt.  
  
 Per altre informazioni, vedere [Metadata Discovery](../../relational-databases/native-client/features/metadata-discovery.md).  
  
 BCPFILECP  
 *iValue* contiene il numero della tabella codici per il file di dati. È possibile specificare il numero della tabella codici, ad esempio 1252 o 850, o uno dei valori indicati di seguito:  
  
 BCPFILE_ACP: i dati contenuti nel file utilizzano la tabella codici di Microsoft Windows® del client.  
  
 BCPFILE_OEMCP: i dati contenuti nel file utilizzano la tabella codici OEM del client (impostazione predefinita).  
  
 BCPFILE_RAW: i dati contenuti nel file utilizzano la tabella codici di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 BCPFILEFMT  
 Numero di versione del formato del file di dati. Può essere 80 ( [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] ), 90 ( [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ), 100 ( [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] ), 110 ( [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ) o 120 ( [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ). 120 è il valore predefinito. Questo valore è utile per l'esportazione e l'importazione di dati in formati supportati da una versione precedente del server. Ad esempio, per importare i dati ottenuti da una colonna di testo di un [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] server in una colonna **varchar (max)** di un [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Server o versione successiva, è necessario specificare 80. Analogamente, se si specifica 80 quando si esportano dati da una colonna **varchar (max)** , questo verrà salvato esattamente come le colonne di testo vengono salvate nel [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] formato e possono essere importate in una colonna di testo di un [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Server.  
  
 BCPFIRST  
 Indica la prima riga di dati del file o della tabella da copiare. Il valore predefinito è 1. Un valore minore di 1 reimposta l'opzione sul valore predefinito.  
  
 BCPFIRSTEX  
 Per le operazioni BCP out, specifica la prima riga della tabella di database da copiare nel file di dati.  
  
 Per le operazioni BCP in, specifica la prima riga del file di dati da copiare nella tabella di database.  
  
 È previsto che il parametro *iValue* sia l'indirizzo di un intero con segno a 64 bit contenente il valore. Il valore massimo che è possibile passare a BCPFIRSTEX è 2^63-1.  
  
 BCPFMTXML  
 Specifica che il file di formato generato deve essere in formato XML. Per impostazione predefinita è disattivato.  
  
 I file in formato XML offrono una maggiore flessibilità sebbene con alcuni vincoli aggiuntivi. Diversamente dai file nei formati precedenti, non è ad esempio possibile specificare contemporaneamente il prefisso e il carattere di terminazione per un campo.  
  
> [!NOTE]  
>  I file in formato XML sono supportati solo quando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene installato insieme a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
 BCPHINTS  
 *iValue* contiene un puntatore alla stringa di caratteri SQLTCHAR. Tale stringa specifica hint di elaborazione della copia bulk di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o un'istruzione Transact-SQL che restituisce un set di risultati. Se viene specificata un'istruzione Transact-SQL che restituisce più set di risultati, vengono ignorati tutti i set di risultati successivi al primo. Per ulteriori informazioni sugli hint di elaborazione per la copia bulk, vedere [utilità bcp](../../tools/bcp-utility.md).  
  
 BCPKEEPIDENTITY  
 Quando *iValue* è true, specifica che le funzioni di copia bulk inseriscono i valori dei dati forniti per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le colonne definite con un vincolo Identity. Il file di input deve fornire valori per le colonne di identità. Se questa impostazione non è disponibile, per le righe inserite vengono generati nuovi valori Identity. Eventuali dati presenti nel file per le colonne di identità vengono ignorati.  
  
 BCPKEEPNULLS  
 Specifica se i valori di dati vuoti nel file verranno convertiti in valori NULL nella tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Quando *iValue* è true, i valori vuoti verranno convertiti in valori null nella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabella. L'impostazione predefinita prevede che i valori vuoti vengano convertiti in un valore predefinito, se presente, per la colonna nella tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 BCPLAST  
 Indica l'ultima riga da copiare. Il valore predefinito prevede che vengano copiate tutte le righe. Un valore inferiore a 1 reimposta l'opzione sul valore predefinito.  
  
 BCPLASTEX  
 Per le operazioni BCP out, specifica l'ultima riga della tabella di database da copiare nel file di dati.  
  
 Per le operazioni BCP in, specifica l'ultima riga del file di dati da copiare nella tabella di database.  
  
 È previsto che il parametro *iValue* sia l'indirizzo di un intero con segno a 64 bit contenente il valore. Il valore massimo che è possibile passare a BCPLASTEX è 2^63-1.  
  
 BCPMAXERRS  
 Indica il numero massimo di errori che si possono verificare prima che l'operazione di copia bulk venga annullata. Il valore predefinito è 10. un valore minore di 1 Reimposta questa opzione sul valore predefinito. La copia bulk impone un massimo di 65.535 errori. Il tentativo di impostare questa opzione su un valore maggiore di 65.535 comporta l'impostazione dell'opzione su 65.535.  
  
 BCPODBC  
 Se è TRUE, specifica che i valori **DateTime** e **smalldatetime** salvati in formato carattere utilizzeranno il prefisso e il suffisso della sequenza di escape del timestamp ODBC. L'opzione BCPODBC si applica solo ai DB_OUT.  
  
 Se è FALSE, un valore **DateTime** che rappresenta il 1 ° gennaio 1997 viene convertito nella stringa di caratteri: 1997-01-01 00:00:00.000. Se è TRUE, lo stesso valore **DateTime** viene rappresentato come: {ts ' 1997-01-01 00:00:00.000'}.  
  
 BCPROWCOUNT  
 Restituisce il numero di righe interessate dall'ultima operazione BCP o da quella corrente.  
  
 BCPTEXTFILE  
 Quando è impostato su TRUE, specifica che il file di dati è un file di testo, anziché un file binario. Se il file è un file di testo, BCP determina se è Unicode controllando il marcatore di byte Unicode nei primi due byte del file di dati.  
  
 BCPUNICODEFILE  
 Quando è impostato su TRUE, specifica che il file di input è un file Unicode.  
  
 *iValue*  
 Valore per l'oggetto *eOption* specificato. *iValue* è un valore integer (LONGLONG) di cui viene eseguito il cast a un puntatore void per consentire l'espansione futura a valori di bit 64.  
  
## <a name="returns"></a>Restituisce  
 SUCCEED o FAIL.  
  
## <a name="remarks"></a>Commenti  
 Questa funzione imposta vari parametri di controllo per le operazioni di copia bulk, inclusi il numero di errori consentito prima dell'annullamento di una copia bulk, i numeri della prima e dell'ultima riga da copiare da un file di dati e le dimensioni del batch.  
  
 Questa funzione viene utilizzata anche per specificare l'istruzione SELECT quando si esegue una copia bulk del set di risultati di un'istruzione SELECT da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Impostare *eOption* su BCPHINTS e impostare *iValue* su un puntatore a una stringa SQLTCHAR che contiene l'istruzione SELECT.  
  
 Questi parametri di controllo sono significativi solo in caso di copia tra un file utente e una tabella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le impostazioni dei parametri di controllo non hanno effetto sulle righe copiate in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md).  
  
## <a name="example"></a>Esempio  
  
```  
// Variables like henv not specified.  
SQLHDBC      hdbc;  
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
if (bcp_init(hdbc, _T("address"), _T("address.add"), _T("addr.err"),  
   DB_IN) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Set the number of rows per batch.   
if (bcp_control(hdbc, BCPBATCH, (void*) 1000) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Set file column count.   
if (bcp_columns(hdbc, 1) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Set the file format.   
if (bcp_colfmt(hdbc, 1, 0, 0, SQL_VARLEN_DATA, '\n', 1, 1)  
   == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Execute the bulk copy.   
if (bcp_exec(hdbc, &nRowsProcessed) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
printf_s("%ld rows processed by bulk copy.", nRowsProcessed);  
  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di copia bulk](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
