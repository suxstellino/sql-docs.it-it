---
description: 'IRowsetFastLoad:: InsertRow (provider OLE DB Native Client)'
title: 'IRowsetFastLoad:: InsertRow (provider OLE DB Native Client) | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- IRowsetFastLoad::InsertRow (OLE DB)
apitype: COM
helpviewer_keywords:
- InsertRow method
ms.assetid: 594d3461-34d2-41e7-8ad4-bd2753601ab6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d78beded73205abbfa266603c50d98fa4f35540e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755691"
---
# <a name="irowsetfastloadinsertrow-native-client-ole-db-provider"></a>IRowsetFastLoad:: InsertRow (provider OLE DB Native Client)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Aggiunge una riga al set di righe della copia bulk. Per gli esempi, vedere [Eseguire una copia bulk dei dati usando IRowsetFastLoad &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md) e [Inviare dati BLOB a SQL Server usando IROWSETFASTLOAD e ISEQUENTIALSTREAM &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-how-to/send-blob-data-to-sql-server-using-irowsetfastload-and-isequentialstream-ole-db.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
HRESULT InsertRow(  
      HACCESSOR hAccessor,  
      void* pData);  
```  
  
## <a name="arguments"></a>Argomenti  
 *hAccessor*[in]  
 Handle della funzione di accesso che definisce i dati delle righe per la copia bulk. La funzione di accesso a cui viene fatto riferimento è una funzione di accesso di riga, che specifica l'associazione alla memoria del consumer contenente valori di dati.  
  
 *pData*[in]  
 Puntatore alla memoria del consumer contenente valori di dati. Per altre informazioni, vedere le [strutture DBBINDING](/previous-versions/windows/desktop/ms716845(v=vs.85)).  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 S_OK  
 Il metodo è riuscito. I valori di stato associati per tutte le colonne hanno il valore DBSTATUS_S_OK o DBSTATUS_S_NULL.  
  
 E_FAIL  
 Si è verificato un errore. Le informazioni sull'errore sono disponibili nelle interfacce degli errori del set di righe.  
  
 E_INVALIDARG  
 L'argomento *pData* è stato impostato su un puntatore NULL.  
  
 E_OUTOFMEMORY  
 SQLNCLI11 non è stato in grado di allocare memoria sufficiente per completare la richiesta.  
  
 E_UNEXPECTED  
 Il metodo è stato chiamato su un set di righe della copia bulk precedentemente invalidato dal metodo [IRowsetFastLoad::Commit](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-commit-ole-db.md).  
  
 DB_E_BADACCESSORHANDLE  
 L'argomento *hAccessor* specificato dal consumer non è valido.  
  
 DB_E_BADACCESSORTYPE  
 La funzione di accesso specificata non è una funzione di accesso di riga o non specifica la memoria del consumer.  
  
## <a name="remarks"></a>Osservazioni  
 Un errore di conversione dei dati del consumer nel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo di dati di una colonna causa la restituzione di un E_FAIL dal [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider OLE DB di Native Client. I dati possono essere trasmessi a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con qualsiasi metodo **InsertRow** o solo con il metodo **Commit**. L'applicazione consumer può chiamare il metodo **InsertRow** diverse volte usando i dati errati prima di ricevere un avviso relativo all'errore di conversione del tipo di dati. Poiché il metodo **Commit** verifica che tutti i dati vengano specificati correttamente dal consumer, se necessario, il consumer può usare **Commit** in modo appropriato per convalidare i dati.  
  
 I [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] set di righe della copia bulk del provider OLE DB di Native Client sono di sola scrittura. Il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native client non espone metodi che consentono di eseguire query sul set di righe. Per terminare l'elaborazione, il consumer può rilasciare il riferimento all'interfaccia [IRowsetFastLoad](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-ole-db.md) senza chiamare il metodo **Commit**. Non sono disponibili funzioni per accedere alle righe inserite dal consumer nel set di righe e modificarne i valori o per rimuoverle singolarmente dal set di righe.  
  
 Le righe oggetto di copia bulk vengono formattate sul server per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le opzioni eventualmente impostate per la connessione o per la sessione, ad esempio ANSI_PADDING, influiscono sul formato di riga. Per impostazione predefinita, questa opzione è impostata su per tutte le connessioni effettuate tramite il [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] provider di OLE DB di Native Client.  
  
## <a name="see-also"></a>Vedere anche  
 [IRowsetFastLoad &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-ole-db.md)  
  
