---
title: IBCPSession::BCPWriteFmt (OLE DB Driver) | Microsoft Docs
description: Uso di IBCPSession::BCPWriteFmt per salvare i file di formato in formato XML o testo (OLE DB)
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- IBCPSession::BCPWriteFmt (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPWriteFmt method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1a11d1a1ad8d2522886a8b90b92e93c4c3a7bc3b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197774"
---
# <a name="ibcpsessionbcpwritefmt-ole-db"></a>IBCPSession::BCPWriteFmt (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Scrive informazioni sul formato per ogni colonna nel file di formato.  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
HRESULT BCPWriteFmt(   
      const wchar_t *pwszFormatFile);  
```  
  
## <a name="remarks"></a>Osservazioni  
 Il file di formato specifica il formato dei dati di un file di dati creato dalla copia bulk. Le chiamate ai metodi [IBCPSession::BCPColumns](../../oledb/ole-db-interfaces/ibcpsession-bcpcolumns-ole-db.md) e [IBCPSession::BCPColFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md) definiscono il formato del file di dati. Il metodo **BCPWriteFmt** salva questa definizione nel file a cui fa riferimento l'argomento pwszFormatFile.  
  
 Il metodo **BCPWriteFmt** può salvare i file di formato in formato XML o testo. Il formato deve essere indicato utilizzando l'opzione di controllo BCP_OPTION_XML con il metodo [IBCPSession::BCPControl](../../oledb/ole-db-interfaces/ibcpsession-bcpcontrol-ole-db.md).  
  
 Per caricare un file di formato salvato, utilizzare il metodo [IBCPSession::BCPReadFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpreadfmt-ole-db.md).  
  
## <a name="arguments"></a>Argomenti  
 *pwszFormatFile*[in]  
 Percorso e nome del file contenente i valori di formato per il file di dati.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 S_OK  
 Il metodo è riuscito.  
  
 E_FAIL  
 Si è verificato un errore specifico del provider. Per informazioni dettagliate, usare l'interfaccia [ISQLServerErrorInfo](./isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 E_OUTOFMEMORY  
 Errore di memoria insufficiente.  
  
 E_UNEXPECTED  
 La chiamata al metodo non era prevista. Non è stato ad esempio chiamato il metodo [IBCPSession::BCPInit](../../oledb/ole-db-interfaces/ibcpsession-bcpinit-ole-db.md) prima della chiamata a questo metodo.  
  
## <a name="see-also"></a>Vedere anche  
 [IBCPSession &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md)   
 [Esecuzione di operazioni di copia bulk](../../oledb/features/performing-bulk-copy-operations.md) 
  
