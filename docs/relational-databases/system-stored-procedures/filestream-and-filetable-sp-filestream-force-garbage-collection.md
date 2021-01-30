---
description: sp_filestream_force_garbage_collection (Transact-SQL)
title: sp_filestream_force_garbage_collection (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_filestream_force_garbage_collection
- sp_filestream_force_garbage_collection_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- FILESTREAM [SQL Server]
- sp_filestream_force_garbage_collection
ms.assetid: 9d1efde6-8fa4-42ac-80e5-37456ffebd0b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0c234c5c03b00b25babbec0a4fbc197f235c966d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165246"
---
# <a name="sp_filestream_force_garbage_collection-transact-sql"></a>sp_filestream_force_garbage_collection (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Forza il Garbage Collector di FILESTREAM all'esecuzione, eliminando qualsiasi file FILESTREAM non necessario.  
  
 Non è possibile rimuovere un contenitore FILESTREAM finché il Garbage Collector non ha completato la rimozione di tutti i file eliminati all'interno di esso. Il Garbage Collector di FILESTREAM viene eseguito automaticamente. Tuttavia, se è necessario rimuovere un contenitore prima dell'esecuzione del Garbage Collector, è possibile usare sp_filestream_force_garbage_collection per eseguire manualmente il Garbage Collector.  
  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_filestream_force_garbage_collection
    [ @dbname = ]  'database_name'
    [ , [ @filename = ] 'logical_file_name' ]
```  
  
## <a name="arguments"></a>Argomenti  
 `[ @dbname = ]  'database_name'`  
 Indica il nome del database in cui eseguire il Garbage Collector.  
  
> [!NOTE]  
> `@dbname` è di tipo **sysname**. Se viene omesso, verrà considerato il database corrente.  
  
 `[ @filename = ] 'logical_file_name'`  
 Indica il nome logico del contenitore FILESTREAM in cui eseguire il Garbage Collector. `@filename` è facoltativo. Se non viene specificato alcun nome di file logico, il Garbage Collector pulisce tutti i contenitori FILESTREAM nel database specificato.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
  
| Valore | Descrizione |
| ----- | ----------- |   
|0|Operazione riuscita|  
|1|Operazione non riuscita|  
  
## <a name="result-sets"></a>Set di risultati  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|*file_name*|Indica il nome del contenitore FILESTREAM|  
|*num_collected_items*|Indica il numero di elementi FILESTREAM (file/directory) che sono stati sottoposti a Garbage Collection (eliminati) in questo contenitore.|  
|*num_marked_for_collection_items*|Indica il numero di elementi FILESTREAM (file/directory) che sono stati contrassegnati per il Garbage Collection in questo contenitore. Questi elementi non sono stati ancora eliminati, ma potrebbero essere idonei per l'eliminazione dopo la fase Garbage Collection.|  
|*num_unprocessed_items*|Indica il numero di elementi FILESTREAM (file o directory) idonei che non sono stati elaborati per il Garbage Collection in questo contenitore FILESTREAM. Gli elementi possono non essere elaborati per i vari motivi, tra cui:<br /><br /> File che devono essere bloccati perché non è stato eseguito un checkpoint o backup del log.<br /><br /> File nel modello di recupero FULL o BULK_LOGGED.<br /><br /> È presente una transazione attiva con esecuzione prolungata.<br /><br /> Il processo di lettura log repliche non è stato eseguito. Per ulteriori informazioni, vedere l' [Archivio FILESTREAM white paper SQL Server 2008](/previous-versions/sql/sql-server-2008/hh461480(v=msdn.10)) .|  
|*last_collected_xact_seqno*|Restituisce l'ultimo numero di sequenza del file di log (LSN) corrispondente fino a cui i file sono stati sottoposti a Garbage Collection per il contenitore FILESTREAM specificato.|  
  
## <a name="remarks"></a>Commenti  
 Esegue in modo esplicito l'attività Garbage Collector per FILESTREAM per il completamento nel database richiesto (e il contenitore FILESTREAM). I file che non sono più necessari vengono rimossi dal processo di Garbage Collection. Il tempo necessario per il completamento di questa operazione dipende dalle dimensioni dei dati FILESTREAM in tale database o contenitore nonché dalla quantità di attività DML recentemente eseguita sui dati FILESTREAM. Sebbene questa operazione possa essere eseguita con il database in modalità online, ciò potrebbe influire sulle prestazioni del database durante l'esecuzione a causa delle varie attività I/O eseguite dal processo di Garbage Collection.  
  
> [!NOTE]  
>  Si consiglia di eseguire questa operazione solo quando è necessario e in orario diverso da quello lavorativo.  
  
È possibile eseguire contemporaneamente più chiamate di questa stored procedure solo in contenitori separati o database separati.  

A causa delle operazioni a 2 fasi, il stored procedure deve essere eseguito due volte per eliminare effettivamente i file FILESTREAM sottostanti.  

Garbage Collection (GC) si basa sul troncamento del log. Di conseguenza, se i file sono stati eliminati di recente in un database utilizzando il modello di recupero con registrazione completa, sono GC-ed solo dopo che è stato eseguito un backup del log delle parti del log delle transazioni e la parte del log è contrassegnata come inattiva. In un database che utilizza un modello di recupero con registrazione minima, un troncamento del log si verifica dopo che `CHECKPOINT` è stato emesso un oggetto nel database.  


## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo del database db_owner.  
  
## <a name="examples"></a>Esempio  
 Negli esempi seguenti viene eseguito il Garbage Collector per i contenitori FILESTREAM nel database `FSDB`.  
  
### <a name="a-specifying-no-container"></a>R. Specifica di nessun contenitore  
  
```sql  
USE FSDB;  
GO  
EXEC sp_filestream_force_garbage_collection @dbname = N'FSDB';  
```  
  
### <a name="b-specifying-a-container"></a>B. Specifica di un contenitore  
  
```sql  
USE FSDB;  
GO  
EXEC sp_filestream_force_garbage_collection @dbname = N'FSDB',
    @filename = N'FSContainer';  
```  
  
## <a name="see-also"></a>Vedere anche  
[Filestream](../../relational-databases/blob/filestream-sql-server.md)
<br>[Tabelle FileTable](../../relational-databases/blob/filetables-sql-server.md)
<br>[DMV per FILESTREAM e tabelle FileTable (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
<br>[Viste del catalogo Filestream e FileTable (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
<br>[sp_kill_filestream_non_transacted_handles (Transact-SQL)](filestream-and-filetable-sp-kill-filestream-non-transacted-handles.md)
  
