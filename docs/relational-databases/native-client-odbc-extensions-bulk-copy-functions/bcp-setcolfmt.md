---
description: bcp_setcolfmt
title: bcp_setcolfmt | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_setcolfmt
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_setcolfmt function
ms.assetid: afb47987-39e7-4079-ad66-e0abf4d4c72b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0e87855f1094d3ac274abdb3acd38612fd0b3dd5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749031"
---
# <a name="bcp_setcolfmt"></a>bcp_setcolfmt
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  La funzione **bcp_setcolfmt** sostituisce l' [bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md). Quando si specificano le regole di confronto delle colonne, è necessario utilizzare la funzione **bcp_setcolfmt** . [bcp_setbulkmode](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-setbulkmode.md) può essere utilizzato per specificare più di un formato di colonna.  
  
 Questa funzione fornisce un approccio flessibile alla definizione del formato delle colonne in un'operazione di copia bulk. La funzione viene utilizzata per impostare singoli attributi di formato di colonna. Ogni chiamata a **bcp_setcolfmt** imposta un attributo di formato della colonna.  
  
 La funzione **bcp_setcolfmt** specifica il formato di origine o di destinazione dei dati in un file utente. Se utilizzato come formato di origine, **bcp_setcolfmt** specifica il formato di un file di dati esistente utilizzato come origine dati di dati in una copia bulk in una tabella in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Quando viene utilizzato come formato di destinazione, il file di dati viene creato utilizzando i formati di colonna specificati con **bcp_setcolfmt**.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
RETCODE bcp_setcolfmt (  
        HDBC hdbc,  
        INT field,  
        INT property,  
        void* pValue,  
        INT cbValue);  
```  
  
## <a name="arguments"></a>Argomenti  
 *hdbc*  
 Handle di connessione ODBC abilitato per la copia bulk.  
  
 *campo*  
 Numero di colonna ordinale per cui viene impostata la proprietà.  
  
 *property*  
 Una delle costanti di proprietà. Le costanti della proprietà sono definite nella tabella seguente.  
  
|Proprietà|Valore|Descrizione|  
|--------------|-----------|-----------------|  
|BCP_FMT_TYPE|BYTE|Tipo di dati della colonna nel file utente. Se differisce dal tipo di dati della colonna corrispondente nella tabella del database, la copia bulk converte i dati, se possibile.<br /><br /> Il parametro BCP_FMT_TYPE viene enumerato in base ai token dei tipi di dati in sqlncli.h e non in base agli enumeratori dei tipi di dati C ODBC. È possibile, ad esempio, specificare una stringa di caratteri SQL_C_CHAR di tipo ODBC utilizzando il tipo SQLCHARACTER specifico di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Per specificare la rappresentazione predefinita dei dati per il tipo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], impostare questo parametro su 0.<br /><br /> Per una copia bulk dal SQL Server in un file, quando BCP_FMT_TYPE è SqlDecimal o SQLNUMERIC, se la colonna di origine non è **Decimal** o **numeric**, vengono utilizzate la precisione e la scala predefinite. In caso contrario, se la colonna di origine è **Decimal** o **numeric**, vengono utilizzate la precisione e la scala della colonna di origine.|  
|BCP_FMT_INDICATOR_LEN|INT|Lunghezza in byte dell'indicatore (prefisso).<br /><br /> Lunghezza, espressa in byte, di un indicatore di lunghezza o Null nei dati della colonna. I valori validi per la lunghezza dell'indicatore sono 0 (quando non si utilizza alcun indicatore), 1, 2 o 4.<br /><br /> Per specificare l'utilizzo di un indicatore di copia bulk predefinito, impostare questo parametro su SQL_VARLEN_DATA.<br /><br /> Gli indicatori vengono visualizzati in memoria direttamente prima dei dati e nel file di dati immediatamente prima dei dati a cui si riferiscono.<br /><br /> Se si utilizzano più modalità per specificare la lunghezza delle colonne del file di dati, ad esempio un indicatore e una lunghezza di colonna massima o un indicatore e una sequenza di caratteri di terminazione, la copia bulk sceglie quella che comporta la copia del minor numero di dati.<br /><br /> I file di dati generati dalla copia bulk quando il formato dei dati non viene modificato dall'utente contengono indicatori se la lunghezza dei dati di colonna può variare o se la colonna può accettare NULL come valore.|  
|BCP_FMT_DATA_LEN|DBINT|Lunghezza in byte dei dati (lunghezza di colonna)<br /><br /> Lunghezza massima, espressa in byte, dei dati della colonna nel file utente, esclusa la lunghezza di eventuali caratteri di terminazione o di indicatori di lunghezza.<br /><br /> L'impostazione di BCP_FMT_DATA_LEN su SQL_NULL_DATA indica che tutti i valori nella colonna del file di dati sono o devono essere impostati su NULL.<br /><br /> L'impostazione di BCP_FMT_DATA_LEN su SQL_VARLEN_DATA indica che il sistema deve determinare la lunghezza dei dati in ogni colonna. Per alcune colonne, ciò può indicare che viene generato un indicatore di lunghezza o Null da anteporre ai dati in una copia da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o che l'indicatore è previsto nei dati copiati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Per i tipi di dati character e binary di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], BCP_FMT_DATA_LEN può essere SQL_VARLEN_DATA, SQL_NULL_DATA, 0 o un valore positivo. Se BCP_FMT_DATA_LEN è SQL_VARLEN_DATA, il sistema utilizza l'indicatore di lunghezza, se disponibile, o una sequenza di caratteri di terminazione per determinare la lunghezza dei dati. Se vengono specificati sia un indicatore di lunghezza che una sequenza di caratteri di terminazione, la copia bulk utilizza la modalità che comporta la copia del minor numero di dati. Se BCP_FMT_DATA_LEN è SQL_VARLEN_DATA, il tipo di dati è un tipo character o binary di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e non viene specificato né un indicatore di lunghezza né una sequenza di caratteri di terminazione, il sistema restituisce un messaggio di errore.<br /><br /> Se BCP_FMT_DATA_LEN è 0 o un valore positivo, viene utilizzato come lunghezza massima dei dati. Se, tuttavia, oltre a un valore positivo per BCP_FMT_DATA_LEN viene specificato un indicatore di lunghezza o una sequenza di caratteri di terminazione, il sistema determina la lunghezza dei dati utilizzando il metodo che comporta la copia del minor numero di dati.<br /><br /> Il valore di BCP_FMT_DATA_LEN rappresenta il numero di byte dei dati. Se i dati di tipo carattere vengono rappresentati come caratteri estesi Unicode, un valore positivo per il parametro BCP_FMT_DATA_LEN rappresenta il numero di caratteri moltiplicato per le dimensioni, espresse in byte, di ogni carattere.|  
|BCP_FMT_TERMINATOR|LPCBYTE|Puntatore alla sequenza di caratteri di terminazione (ANSI o Unicode in base alle esigenze) da utilizzare per la colonna. Questo parametro risulta particolarmente utile per i dati di tipo carattere, in quanto tutti gli altri tipi hanno una lunghezza fissa o, nel caso dei dati binari, richiedono un indicatore di lunghezza per registrare in modo accurato il numero di byte presenti.<br /><br /> Per evitare di terminare i dati estratti o per indicare che i dati di un file utente non devono essere terminati, impostare questo parametro su NULL.<br /><br /> Se si utilizzano più modalità per definire la lunghezza delle colonne di un file utente, ad esempio un carattere di terminazione e un indicatore di lunghezza o un carattere di terminazione e una lunghezza di colonna massima, la copia bulk sceglierà quella che comporta la copia del minor numero di dati.<br /><br /> L'API della copia bulk esegue la conversione dei caratteri da Unicode a MBCS in base alle necessità. Verificare attentamente che la stringa di byte del carattere di terminazione e la lunghezza della stringa di byte siano impostate correttamente.|  
|BCP_FMT_SERVER_COL|INT|Posizione ordinale della colonna nel database.|  
|BCP_FMT_COLLATION|LPCSTR|Nome delle regole di confronto.|  
  
 *pValue*  
 Puntatore al valore da associare alla *Proprietà*. Consente di impostare singolarmente ogni proprietà di formato di colonna.  
  
 *cbValue*  
 Lunghezza in byte del buffer delle proprietà.  
  
## <a name="returns"></a>Restituisce  
 SUCCEED o FAIL.  
  
## <a name="remarks"></a>Commenti  
 Questa funzione sostituisce la funzione **bcp_colfmt** . Tutte le funzionalità di **bcp_colfmt** sono disponibili nella **bcp_setcolfmt** funzione. Sono inoltre supportate le regole di confronto delle colonne. È consigliabile impostare gli attributi di formato di colonna seguenti nell'ordine indicato:  
  
 BCP_FMT_SERVER_COL  
  
 BCP_FMT_DATA_LEN  
  
 BCP_FMT_TYPE  
  
 La funzione **bcp_setcolfmt** consente di specificare il formato del file utente per le copie bulk. Per la copia bulk, un formato contiene le parti seguenti:  
  
-   Un mapping dalle colonne del file utente alle colonne del database.  
  
-   Il tipo di dati di ogni colonna del file utente.  
  
-   La lunghezza dell'indicatore facoltativo per ogni colonna.  
  
-   La lunghezza massima dei dati per ogni colonna del file utente.  
  
-   La sequenza di byte di terminazione facoltativa per ogni colonna.  
  
-   La lunghezza della sequenza di byte di terminazione facoltativa.  
  
 Ogni chiamata a **bcp_setcolfmt** specifica il formato per una colonna del file utente. Ad esempio, per modificare le impostazioni predefinite per tre colonne in un file di dati utente a cinque colonne, chiamare prima [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md)**(5)**, quindi chiamare **bcp_setcolfmt** cinque volte, con tre di queste chiamate che impostano il formato personalizzato. Per le due chiamate rimanenti, impostare BCP_FMT_TYPE su 0 e impostare rispettivamente BCP_FMT_INDICATOR_LENGTH, BCP_FMT_DATA_LEN e *cbValue* su 0, SQL_VARLEN_DATA e 0. Questa procedura consente di copiare tutte e cinque le colonne, tre con il formato personalizzato e due con il formato predefinito.  
  
 Prima di chiamare **bcp_setcolfmt**, è necessario chiamare la funzione **bcp_columns** .  
  
 È necessario chiamare **bcp_setcolfmt** una volta per ogni proprietà di ogni colonna del file utente.  
  
 Non è necessario copiare tutti i dati di un file utente nella tabella di SQL Server. Per ignorare una colonna, specificarne il formato dei dati impostando il parametro BCP_FMT_SERVER_COL su 0. Se si desidera ignorare una colonna, è necessario specificarne il tipo.  
  
 È possibile utilizzare la funzione [bcp_writefmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-writefmt.md) per salvare in modo permanente la specifica di formato.  
  
## <a name="bcp_setcolfmt-support-for-enhanced-date-and-time-features"></a>Supporto di bcp_setcolfmt per le caratteristiche avanzate di data e ora  
 I tipi utilizzati con la proprietà BCP_FMT_TYPE per i tipi data/ora sono specificati nelle [modifiche della copia bulk per i tipi di data e ora avanzati &#40;OLE DB e ODBC&#41;](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md).  
  
 Per ulteriori informazioni, vedere [miglioramenti di data e ora &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di copia bulk](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
