---
description: 'IRowsetFastLoad:: commit (provider OLE DB di Native Client)'
title: 'IRowsetFastLoad:: commit (provider OLE DB di Native Client) | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- IRowsetFastLoad::Commit (OLE DB)
apitype: COM
helpviewer_keywords:
- Commit method
ms.assetid: 19de9128-b91a-4626-847f-af721edaa24e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5e12a0c7ce8b92b430284ede1f62047abcb7954b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104741001"
---
# <a name="irowsetfastloadcommit-native-client-ole-db-provider"></a>IRowsetFastLoad:: commit (provider OLE DB di Native Client)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contrassegna la fine di un batch di righe inserite e scrive le righe nella tabella di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per gli esempi, vedere [Eseguire una copia bulk dei dati usando IRowsetFastLoad &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md) e [Inviare dati BLOB a SQL Server usando IROWSETFASTLOAD e ISEQUENTIALSTREAM &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-how-to/send-blob-data-to-sql-server-using-irowsetfastload-and-isequentialstream-ole-db.md).  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
HRESULT Commit(  
      BOOL fDone);  
```  
  
## <a name="arguments"></a>Argomenti  
 *fDone*[in]  
 Se impostato su FALSE, il set di righe resta valido e può essere utilizzato dal consumer per l'inserimento di altre righe. Se impostato su TRUE, il set di righe non è più valido e il consumer non può inserire altre righe.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 S_OK  
 Il metodo è riuscito e tutti i dati inseriti sono stati scritti nella tabella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 E_FAIL  
 Si è verificato un errore specifico del provider. Recuperare informazioni relative al testo dell'errore specifico dal provider.  
  
 E_UNEXPECTED  
 Il metodo è stato chiamato su un set di righe della copia bulk precedentemente invalidato dal metodo **IRowsetFastLoad::Commit**.  
  
## <a name="remarks"></a>Osservazioni  
 Un set di righe della [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] copia bulk del provider OLE DB di Native client si comporta come un set di righe in modalità di aggiornamento ritardato. Quando l'utente inserisce dati di riga nel set di righe, le righe inserite vengono gestite analogamente agli inserimenti in sospeso di un set di righe che supporta **IRowsetUpdate**.  
  
 Il consumer deve chiamare il metodo **Commit** sul set di righe della copia bulk per scrivere le righe inserite nella tabella [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esattamente come quando si usa il metodo **IRowsetUpdate::Update** per inviare le righe in sospeso a un'istanza di SQL Server.  
  
 Se il consumer rilascia il riferimento al set di righe della copia bulk senza chiamare il metodo **Commit**, tutte le righe inserite e non scritte in precedenza andranno perse.  
  
 Il consumer può raggruppare le righe inserite chiamando il metodo **Commit** con l'argomento *fDone* impostato su FALSE. Quando *fDone* è impostato su TRUE, il set di righe non è più valido. Un set di righe della copia bulk non valido supporta solo l'interfaccia **ISupportErrorInfo** e il metodo **IRowsetFastLoad::Release**.  
  
## <a name="see-also"></a>Vedere anche  
 [IRowsetFastLoad &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-ole-db.md)  
  
  
