---
description: 'IBCPSession:: BCPColumns (provider OLE DB Native Client)'
title: 'IBCPSession:: BCPColumns (provider OLE DB Native Client) | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- IBCPSession::BCPColumns (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPColumns method
ms.assetid: c338abe8-9e30-4853-a7c6-b1a6c00095e1
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b9f4904df18917efac3e8a503c59d3a2d3b07dde
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749391"
---
# <a name="ibcpsessionbcpcolumns-native-client-ole-db-provider"></a>IBCPSession:: BCPColumns (provider OLE DB Native Client)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Imposta il numero di campi da associare alle colonne di una tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
HRESULT BCPColumns(   
      DBCOUNTITEM nColumns);  
```  
  
## <a name="remarks"></a>Osservazioni  
 Chiama internamente [IBCPSession::BCPColFmt](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md) per impostare i valori predefiniti per i dati dei campi. Questi valori predefiniti vengono ottenuti dalle informazioni sulle colonne di SQL Server recuperate internamente dal provider quando si specifica il nome di tabella tramite [IBCPSession::BCPInit](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpinit-ole-db.md).  
  
> [!NOTE]  
>  È possibile chiamare questo metodo solo dopo avere chiamato **BCPInit** con un nome di file valido.  
  
 È consigliabile chiamare questo metodo solo se si intende utilizzare un formato di file utente diverso da quello predefinito. Per ulteriori informazioni su una descrizione del formato di file utente predefinito, vedere il metodo **BCPInit**.  
  
 Dopo avere chiamato il metodo **BCPColumns**, è necessario chiamare il metodo **BCPColFmt** per ogni colonna del file utente per definire in modo completo un formato di file personalizzato.  
  
## <a name="arguments"></a>Argomenti  
 *nColumns*[in]  
 Numero totale di campi nel file utente. Anche se si prepara la copia bulk dei dati dal file utente in una tabella di SQL Server e non si prevede di copiare tutti i campi del file utente, è comunque necessario impostare l'argomento *nColumns* sul numero totale di campi del file utente. I campi ignorati possono quindi essere specificati tramite **BCPColFmt**.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 S_OK  
 Il metodo è riuscito.  
  
 E_FAIL  
 Si è verificato un errore specifico del provider. Per informazioni dettagliate, usare l'interfaccia [ISQLServerErrorInfo](isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 E_UNEXPECTED  
 La chiamata al metodo non era prevista. Non è stato ad esempio chiamato il metodo **BCPInit** prima della chiamata a questo metodo. Si verifica inoltre quando questo metodo viene chiamato più volte per un'operazione di copia bulk.  
  
 E_OUTOFMEMORY  
 Errore di memoria insufficiente.  
  
## <a name="see-also"></a>Vedere anche  
 [IBCPSession &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-ole-db.md)   
 [Esecuzione di operazioni di copia bulk](../../relational-databases/native-client/features/performing-bulk-copy-operations.md)  
  
