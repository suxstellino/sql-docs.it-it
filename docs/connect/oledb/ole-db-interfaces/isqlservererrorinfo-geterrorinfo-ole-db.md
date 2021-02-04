---
title: ISQLServerErrorInfo::GetErrorInfo (OLE DB Driver) | Microsoft Docs
description: Informazioni sul modo in cui il metodo ISQLServerErrorInfo::GetErrorInfo restituisce un puntatore a una struttura SSERRORINFO di OLE DB Driver per SQL Server con i dettagli errore di SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISQLServerErrorInfo::GetErrorInfo (OLE DB)
apitype: COM
helpviewer_keywords:
- GetErrorInfo method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c23db8a2472f236e0a9733b46fdc0d338d6b6f70
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183760"
---
# <a name="isqlservererrorinfogeterrorinfo-ole-db"></a>ISQLServerErrorInfo::GetErrorInfo (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Restituisce un puntatore a una struttura SSERRORINFO di OLE DB Driver per SQL Server contenente i dettagli sugli errori di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 OLE DB Driver per SQL Server definisce l'interfaccia degli errori **ISQLServerErrorInfo**. Questa interfaccia restituisce i dettagli di un errore di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], inclusi la gravità e lo stato.  

  
## <a name="syntax"></a>Sintassi  
  
```  
  
HRESULT GetErrorInfo(  
   SSERRORINFO**ppSSErrorInfo,  
   OLECHAR**ppErrorStrings);  
```  
  
## <a name="arguments"></a>Argomenti  
 *ppSSErrorInfo*[out]  
 Puntatore a una struttura SSERRORINFO. Se il metodo non riesce o non sono disponibili informazioni di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] associate all'errore, il provider non alloca memoria e verifica che l'argomento *ppSSErrorInfo* sia un puntatore Null nell'output.  
  
 *ppErrorStrings*[out]  
 Puntatore a un puntatore stringa carattere Unicode. Se il metodo non riesce o non sono disponibili informazioni di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] associate a un errore, il provider non alloca memoria e verifica che l'argomento *ppErrorStrings* sia un puntatore Null nell'output. Liberando l'argomento *ppErrorStrings* con il metodo **IMalloc::Free**, vengono liberati i tre singoli membri della stringa della struttura SSERRORINFO restituita, in quanto la memoria è allocata in un blocco.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 S_OK  
 Il metodo è riuscito.  
  
 E_INVALIDARG  
 L'argomento *ppSSErrorInfo* o *ppErrorStrings* era NULL.  
  
 E_OUTOFMEMORY  
 OLE DB Driver per SQL Server non è riuscito ad allocare una quantità di memoria sufficiente per completare la richiesta.  
  
## <a name="remarks"></a>Osservazioni  
 Il driver OLE DB per SQL Server alloca memoria per le stringhe SSERRORINFO e OLECHAR restituite tramite i puntatori passati dal consumer. Il consumer deve deallocare questa memoria tramite il metodo **IMalloc::Free** quando l'accesso ai dati dell'errore non è più necessario.  
  
 La struttura SSERRORINFO viene definita nel modo seguente:  
  
```  
typedef struct tagSSErrorInfo  
   {  
   LPOLESTR pwszMessage;  
   LPOLESTR pwszServer;  
   LPOLESTR pwszProcedure;  
   LONG lNative;  
   BYTE bState;  
   BYTE bClass;  
   WORD wLineNumber;  
   }  
SSERRORINFO;  
```  
  
|Membro|Descrizione|  
|------------|-----------------|  
|*pwszMessage*|Messaggio di errore di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Il messaggio viene restituito attraverso il metodo **IErrorInfo::GetDescription**.|  
|*pwszServer*|Nome dell'istanza di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] in cui si è verificato l'errore.|  
|*pwszProcedure*|Nome della stored procedure che genera l'errore se esso si è verificato all'interno della stessa, in caso contrario, una stringa vuota.|  
|*lNative*|Numero di errore di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Il numero di errore è identico a quello restituito nel parametro *plNativeError* del metodo **ISQLErrorInfo::GetSQLInfo**.|  
|*bState*|Stato dell'errore di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|*bClass*|Gravità dell'errore di [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|*wLineNumber*|Quando applicabile, riga di una stored procedure [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] che ha generato il messaggio di errore. Se non è coinvolta alcuna procedura, il valore predefinito è 1.|  
  
 I puntatori nella struttura fanno riferimento agli indirizzi nella stringa restituita nell'argomento *ppErrorStrings*.  
  
## <a name="see-also"></a>Vedere anche  
 [RAISERROR &#40;Transact-SQL&#41;](../../../t-sql/language-elements/raiserror-transact-sql.md)  
  
