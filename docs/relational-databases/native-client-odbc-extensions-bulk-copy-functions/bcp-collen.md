---
description: bcp_collen
title: bcp_collen | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_collen
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_collen function
ms.assetid: faaf1f7a-81f2-4852-a178-56602c33673a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cd081df01f461b98a64ea7185b4aefb03d2f8661
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749611"
---
# <a name="bcp_collen"></a>bcp_collen
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Imposta la lunghezza dei dati nella variabile di programma per la copia bulk corrente in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
RETCODE bcp_collen (  
        HDBC hdbc,  
        DBINT cbData,  
        INT idxServerCol);  
```  
  
## <a name="arguments"></a>Argomenti  
 *hdbc*  
 Handle di connessione ODBC abilitato per la copia bulk.  
  
 *cbData*  
 Lunghezza dei dati nella variabile di programma esclusa la lunghezza dei caratteri di terminazione o degli indicatori di lunghezza. L'impostazione di *cbData* su SQL_NULL_DATA indica che tutte le righe copiate nel server contengono un valore null per la colonna. L'impostazione di cbData su SQL_VARLEN_DATA indica che verrà utilizzato un carattere di terminazione della stringa o un altro metodo per determinare la lunghezza dei dati copiati. Se sono presenti sia un indicatore di lunghezza che un carattere di terminazione, il sistema utilizzerà il metodo che comporta la copia del minor numero di dati.  
  
 *idxServerCol*  
 Posizione ordinale della colonna nella tabella in cui vengono copiati i dati. La prima colonna è 1. La posizione ordinale di una colonna viene segnalata da [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md).  
  
## <a name="returns"></a>Restituisce  
 SUCCEED o FAIL.  
  
## <a name="remarks"></a>Commenti  
 La funzione **bcp_collen** consente di modificare la lunghezza dei dati nella variabile di programma per una determinata colonna durante la copia dei dati in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md).  
  
 Inizialmente, la lunghezza dei dati viene determinata quando viene chiamato [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) . Se la lunghezza dei dati viene modificata tra le chiamate a **bcp_sendrow** e non viene utilizzato alcun prefisso di lunghezza o carattere di terminazione, è possibile chiamare **bcp_collen** per reimpostare la lunghezza. La chiamata successiva a **bcp_sendrow** utilizza la lunghezza impostata dalla chiamata al **bcp_collen**.  
  
 È necessario chiamare **bcp_collen** una volta per ogni colonna della tabella di cui si desidera modificare la lunghezza dei dati.  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni di copia bulk](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
