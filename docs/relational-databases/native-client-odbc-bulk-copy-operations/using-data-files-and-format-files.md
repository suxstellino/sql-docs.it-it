---
description: Utilizzo di file di dati e file di formato
title: Uso di file di dati e file di formato | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- bulk copy [ODBC], file formats
- ODBC, functions
- SQL Server Native Client ODBC driver, bulk copy
- bulk copy [SQL Server Native Client]
- ODBC, bulk copy operations
- bulk copy [ODBC], data files
ms.assetid: c01b7155-3f0a-473d-90b7-87a97bc56ca5
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c9ca6f51e9d81f3b2f40b2d4c049ba100667dfd4
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746111"
---
# <a name="using-data-files-and-format-files"></a>Utilizzo di file di dati e file di formato
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Il programma per la copia bulk più semplice effettua le operazioni seguenti:  
  
1.  Chiama [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md) per specificare la copia bulk (set BCP_OUT) da una tabella o da una vista a un file di dati.  
  
2.  Chiama [bcp_exec](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-exec.md) per eseguire l'operazione di copia bulk.  
  
 Poiché il file di dati viene creato in modalità nativa, i dati di tutte le colonne nella tabella o nella vista vengono archiviati nel file di dati con lo stesso formato utilizzato nel database. Il file può essere quindi oggetto di copia bulk in un server utilizzando questi stessi passaggi e impostando DB_IN anziché DB_OUT. È possibile procedere in questo modo solo se le tabelle di origine e di destinazione hanno una struttura identica. Il file di dati risultante può essere anche un input per l'utilità **bcp** utilizzando l'opzione **/n** (modalità nativa).  
  
 Per eseguire la copia bulk dal set di risultati di un'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] anziché direttamente da una tabella o una vista:  
  
1.  Chiamare **bcp_init** per specificare la copia bulk, ma specificare null per il nome della tabella.  
  
2.  Chiamare [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) con *EOPTION* impostato su BCPHINTS e *iValue* impostato su un puntatore a una stringa SQLTCHAR contenente l'istruzione Transact-SQL.  
  
3.  Chiamare **bcp_exec** per eseguire l'operazione di copia bulk.  

 L'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] può essere qualsiasi istruzione che genera un set di risultati. Il file di dati creato contiene il primo set di risultati dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)]. Nella copia bulk viene ignorato qualsiasi set di risultati successivo al primo se tramite l'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] vengono generati più set di risultati.  
  
 Per creare un file di dati in cui i dati della colonna vengono archiviati in un formato diverso da quello della tabella, chiamare [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md) per specificare il numero di colonne che verranno modificate, quindi chiamare [bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md) per ogni colonna di cui si desidera modificare il formato. Questa operazione viene eseguita dopo aver chiamato **bcp_init** ma prima di chiamare **bcp_exec**. **bcp_colfmt** specifica il formato in cui i dati della colonna vengono archiviati nel file di dati. Può essere usato durante la copia bulk in o in uscita. È inoltre possibile utilizzare **bcp_colfmt** per impostare i terminatori di riga e di colonna. Se, ad esempio, i dati non contengono caratteri di tabulazione, è possibile creare un file delimitato da tabulazioni usando **bcp_colfmt** per impostare il carattere di tabulazione come carattere di terminazione per ogni colonna.  
  
 Quando si esegue la copia bulk e si utilizza **bcp_colfmt**, è possibile creare facilmente un file di formato che descrive il file di dati creato chiamando [bcp_writefmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-writefmt.md) dopo l'ultima chiamata al **bcp_colfmt**.  
  
 Quando si esegue la copia bulk da un file di dati descritto da un file di formato, leggere il file di formato chiamando [bcp_readfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-readfmt.md) dopo **bcp_init** ma prima di **bcp_exec**.  
  
 La funzione **bcp_control** controlla diverse opzioni durante la copia bulk in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da un file di dati. **bcp_control** imposta le opzioni, ad esempio il numero massimo di errori prima della terminazione, la riga del file in cui avviare la copia bulk, la riga in cui arrestare e le dimensioni del batch.  
  
## <a name="see-also"></a>Vedere anche  
 [Esecuzione di operazioni di copia bulk &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)  
  
  
