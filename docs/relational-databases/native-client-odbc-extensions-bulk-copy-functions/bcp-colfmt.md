---
description: bcp_colfmt
title: bcp_colfmt | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_colfmt
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_colfmt function
ms.assetid: 5c3b6299-80c7-4e84-8e69-4ff33009548e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8e014d9298fd3d756e0d6a85349949ee52a813ec
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749621"
---
# <a name="bcp_colfmt"></a>bcp_colfmt
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Specifica il formato di origine o di destinazione dei dati in un file utente. Quando viene utilizzato come formato di origine, **bcp_colfmt** specifica il formato di un file di dati esistente utilizzato come origine dei dati in una copia bulk in una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabella. Quando viene utilizzato come formato di destinazione, il file di dati viene creato utilizzando i formati di colonna specificati con **bcp_colfmt**.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
RETCODE bcp_colfmt (  
        HDBC hdbc,  
        INT idxUserDataCol,  
        BYTE eUserDataType,  
        INT cbIndicator,  
        DBINT cbUserData,  
        LPCBYTE pUserDataTerm,  
        INT cbUserDataTerm,  
        INT idxServerCol);  
```  
  
## <a name="arguments"></a>Argomenti  
 *hdbc*  
 Handle di connessione ODBC abilitato per la copia bulk.  
  
 *idxUserDataCol*  
 Numero ordinale di colonna nel file di dati dell'utente per il quale viene specificato il formato. La prima colonna è 1.  
  
 *eUserDataType*  
 Tipo di dati della colonna nel file utente. Se è diverso dal tipo di dati della colonna corrispondente nella tabella di database (*idxServerColumn*), la copia bulk converte i dati, se possibile.  
  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] è stato introdotto il supporto per i token del tipo di dati SQLXML e SQLUDT nel parametro *eUserDataType* .  
  
 Il parametro *eUserDataType* viene enumerato in base ai [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] token del tipo di dati in sqlncli. h e non negli enumeratori dei tipi di dati ODBC C. È ad esempio possibile specificare una stringa di caratteri SQL_C_CHAR di tipo ODBC utilizzando il tipo SQLCHARACTER specifico di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Per specificare la rappresentazione predefinita dei dati per il tipo di dati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], impostare questo parametro su 0.  
  
 Per una copia bulk [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un file, quando *eUserDataType* è SqlDecimal o SQLNUMERIC:  
  
-   Se la colonna di origine non è **Decimal** o **numeric**, vengono utilizzate la precisione e la scala predefinite.  
  
-   Se la colonna di origine è **Decimal** o **numeric**, vengono utilizzate la precisione e la scala della colonna di origine.  
  
 *cbIndicator*  
 Lunghezza, espressa in byte, di un indicatore di lunghezza o Null nei dati della colonna. Valori di lunghezza di indicatore validi sono 0 (nel caso in cui non venga utilizzato un indicatore), 1, 2, 4 o 8.  
  
 Per specificare l'utilizzo di un indicatore di copia bulk predefinito, impostare questo parametro su SQL_VARLEN_DATA.  
  
 Gli indicatori vengono visualizzati in memoria direttamente prima dei dati e nel file di dati immediatamente prima dei dati a cui si riferiscono.  
  
 Se si utilizzano più modalità per specificare la lunghezza delle colonne del file di dati, ad esempio un indicatore e una lunghezza di colonna massima o un indicatore e una sequenza di caratteri di terminazione, la copia bulk sceglie quella che comporta la copia del minor numero di dati.  
  
 I file di dati generati dalla copia bulk quando il formato dei dati non viene modificato dall'utente contengono indicatori se la lunghezza dei dati di colonna può variare o se la colonna può accettare NULL come valore.  
  
 *cbUserData*  
 Lunghezza massima, espressa in byte, dei dati della colonna nel file utente, senza includere la lunghezza di un carattere di terminazione o di un indicatore di lunghezza.  
  
 L'impostazione di *cbUserData* su SQL_NULL_DATA indica che tutti i valori nella colonna del file di dati sono o devono essere impostati su null.  
  
 L'impostazione di *cbUserData* su SQL_VARLEN_DATA indica che il sistema deve determinare la lunghezza dei dati in ogni colonna. Per alcune colonne, ciò può indicare che viene generato un indicatore di lunghezza o Null da anteporre ai dati in una copia da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o che l'indicatore è previsto nei dati copiati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] i tipi di dati character e binary, *cbUserData* può essere SQL_VARLEN_DATA, SQL_NULL_DATA, 0 o un valore positivo. Se *cbUserData* è SQL_VARLEN_DATA, il sistema utilizza l'indicatore di lunghezza, se presente, o una sequenza di caratteri di terminazione per determinare la lunghezza dei dati. Se vengono specificati sia un indicatore di lunghezza che una sequenza di caratteri di terminazione, la copia bulk utilizza la modalità che comporta la copia del minor numero di dati. Se *cbUserData* è SQL_VARLEN_DATA, il tipo di dati è di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo carattere o binario e non viene specificato né un indicatore di lunghezza né una sequenza di caratteri di terminazione, il sistema restituisce un messaggio di errore.  
  
 Se *cbUserData* è 0 o un valore positivo, il sistema usa *cbUserData* come lunghezza massima dei dati. Se, tuttavia, oltre a un *cbUserData* positivo, viene specificato un indicatore di lunghezza o una sequenza di caratteri di terminazione, il sistema determina la lunghezza dei dati usando il metodo che comporta la copia della quantità minima di dati.  
  
 Il valore *cbUserData* rappresenta il numero di byte dei dati. Se i dati di tipo carattere sono rappresentati da caratteri wide Unicode, un valore positivo per il parametro *cbUserData* rappresenta il numero di caratteri moltiplicato per la dimensione, espressa in byte, di ogni carattere.  
  
 *pUserDataTerm*  
 Sequenza di caratteri di terminazione da utilizzare per la colonna. Questo parametro risulta particolarmente utile per i dati di tipo carattere, in quanto tutti gli altri tipi hanno una lunghezza fissa o, nel caso dei dati binari, richiedono un indicatore di lunghezza per registrare in modo accurato il numero di byte presenti.  
  
 Per evitare di terminare i dati estratti o per indicare che i dati di un file utente non devono essere terminati, impostare questo parametro su NULL.  
  
 Se si utilizzano più modalità per definire la lunghezza delle colonne di un file utente, ad esempio un carattere di terminazione e un indicatore di lunghezza o un carattere di terminazione e una lunghezza di colonna massima, la copia bulk sceglierà quella che comporta la copia del minor numero di dati.  
  
 L'API della copia bulk esegue la conversione dei caratteri da Unicode a MBCS in base alle necessità. Verificare attentamente che la stringa di byte del carattere di terminazione e la lunghezza della stringa di byte siano impostate correttamente.  
  
 *pUserDataTerm*  
 Lunghezza, espressa in byte, della sequenza di caratteri di terminazione da utilizzare per la colonna. Se non sono presenti caratteri di terminazione nei dati o non si desidera includerli, impostare questo valore su 0.  
  
 *idxServerCol*  
 Posizione ordinale della colonna nella tabella di database. Il numero della prima colonna è 1. La posizione ordinale di una colonna viene segnalata da [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md).  
  
 Se questo valore è 0, la copia bulk ignora la colonna nel file di dati.  
  
## <a name="returns"></a>Restituisce  
 SUCCEED o FAIL.  
  
## <a name="remarks"></a>Commenti  
 La funzione **bcp_colfmt** consente di specificare il formato del file utente per le copie bulk. Per la copia bulk, un formato contiene le parti seguenti:  
  
-   Un mapping dalle colonne del file utente alle colonne del database.  
  
-   Il tipo di dati di ogni colonna del file utente.  
  
-   La lunghezza dell'indicatore facoltativo per ogni colonna.  
  
-   La lunghezza massima dei dati per ogni colonna del file utente.  
  
-   La sequenza di byte di terminazione facoltativa per ogni colonna.  
  
-   La lunghezza della sequenza di byte di terminazione facoltativa.  
  
 Ogni chiamata a **bcp_colfmt** specifica il formato per una colonna del file utente. Ad esempio, per modificare le impostazioni predefinite per tre colonne in un file di dati utente a cinque colonne, chiamare prima [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md)**(5)**, quindi chiamare **bcp_colfmt** cinque volte, con tre di queste chiamate che impostano il formato personalizzato. Per le due chiamate rimanenti, impostare *eUserDataType* su 0, quindi impostare *cbIndicator*, *cbUserData* e *cbUserDataTerm* su 0, SQL_VARLEN_DATA e 0 rispettivamente. Questa procedura consente di copiare tutte e cinque le colonne, tre con il formato personalizzato e due con il formato predefinito.  
  
 Per *cbIndicator*, un valore pari a 8 per indicare che un tipo di valore di grandi dimensioni è ora valido. Se si specifica il prefisso per un campo la cui colonna corrispondente è un nuovo tipo max, può essere impostato solo su 8. Per informazioni dettagliate, vedere [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md).  
  
 La funzione **bcp_columns** deve essere chiamata prima di qualsiasi chiamata a **bcp_colfmt**.  
  
 È necessario chiamare **bcp_colfmt** una volta per ogni colonna del file utente.  
  
 La chiamata di **bcp_colfmt** più di una volta per ogni colonna del file utente genera un errore.  
  
 Non è necessario copiare tutti i dati di un file utente nella tabella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per ignorare una colonna, specificare il formato dei dati per la colonna, impostando il parametro *idxServerCol su* su 0. Se si desidera ignorare una colonna, è necessario specificarne il tipo.  
  
 È possibile utilizzare la funzione [bcp_writefmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-writefmt.md) per salvare in modo permanente la specifica di formato.  
  
## <a name="bcp_colfmt-support-for-enhanced-date-and-time-features"></a>Supporto di bcp_colfmt per le caratteristiche avanzate di data e ora  
 Per informazioni sui tipi utilizzati con il parametro *eUserDataType* per i tipi data/ora, vedere la pagina relativa alle [modifiche di copia bulk per i tipi di data e ora avanzati &#40;OLE DB e&#41;ODBC](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md).  
  
 Per ulteriori informazioni, vedere [miglioramenti di data e ora &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di copia bulk](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
