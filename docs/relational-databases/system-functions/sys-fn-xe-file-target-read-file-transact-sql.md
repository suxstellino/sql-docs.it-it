---
description: sys.fn_xe_file_target_read_file (Transact-SQL)
title: sys.fn_xe_file_target_read_file (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_xe_file_target_read_file_TSQL
- fn_xe_file_target_read_file
- sys.fn_xe_file_target_read_file_TSQL
- sys.fn_xe_file_target_read_file
dev_langs:
- TSQL
helpviewer_keywords:
- extended events [SQL Server], functions
- fn_xe_file_target_read_file function
- sys.fn_xe_file_target_read_file function
ms.assetid: cc0351ae-4882-4b67-b0d8-bd235d20c901
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a592609cce6805105e61679e54857312e6296b6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187371"
---
# <a name="sysfn_xe_file_target_read_file-transact-sql"></a>sys.fn_xe_file_target_read_file (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Legge file creati dalla destinazione asincrona dei file degli eventi estesi. Viene restituito un evento per riga in formato XML.  
  
> [!WARNING]  
>  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] accettano i risultati della traccia generati in formato XEL e xem. [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Gli eventi estesi supportano solo i risultati della traccia in formato XEL. È consigliabile utilizzare SQL Server Management Studio per leggere i risultati della traccia in formato XEL.    
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sys.fn_xe_file_target_read_file ( path, mdpath, initial_file_name, initial_offset )  
```  
  
## <a name="arguments"></a>Argomenti  
 *path*  
 Percorso dei file da leggere. il *percorso* può contenere caratteri jolly e includere il nome di un file. *path* è di **tipo nvarchar (260)**. Non prevede alcun valore predefinito. Nel contesto del database SQL di Azure, questo valore è un URL HTTP di un file in archiviazione di Azure.
  
 *mdpath*  
 Percorso del file di metadati che corrisponde al file o ai file specificati dall'argomento *path* . *mdpath* è di **tipo nvarchar (260)**. Non prevede alcun valore predefinito. A partire da SQL Server 2016, questo parametro può essere specificato come null.
  
> [!NOTE]  
>  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] non richiede il parametro *mdpath* . È tuttavia disponibile per la compatibilità con i file di log generati in versioni precedenti di SQL Server.  
  
 *initial_file_name*  
 Primo file da leggere dal *percorso*. *initial_file_name* è di **tipo nvarchar (260)**. Non prevede alcun valore predefinito. Se viene specificato **null** come argomento, vengono letti tutti i file trovati nel *percorso* .  
  
> [!NOTE]  
>  *initial_file_name* e *initial_offset* sono argomenti abbinati. Se si specifica un valore per uno dei due argomenti, è necessario specificare un valore anche per l'altro.  
  
 *initial_offset*  
 Utilizzato per specificare l'ultimo offset letto precedentemente e consente di ignorare tutti gli eventi fino all'offset (incluso). L'enumerazione degli eventi inizia dopo l'offset specificato. *initial_offset* è di tipo **bigint**. Se viene specificato **null** come argomento, viene letto l'intero file.  
  
## <a name="table-returned"></a>Tabella restituita  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|module_guid|**uniqueidentifier**|GUID del modulo dell'evento. Non ammette i valori Null.|  
|package_guid|**uniqueidentifier**|GUID del pacchetto dell'evento. Non ammette i valori Null.|  
|object_name|**nvarchar(256)**|Nome dell'evento. Non ammette i valori Null.|  
|event_data|**nvarchar(max)**|Contenuto dell'evento in formato XML. Non ammette i valori Null.|  
|file_name|**nvarchar(260)**|Nome del file che contiene l'evento. Non ammette i valori Null.|  
|file_offset|**bigint**|Offset del blocco nel file che contiene l'evento. Non ammette i valori Null.|  
|timestamp_utc|**datetime2**|**SI APPLICA A**: [!INCLUDE[ssSQLv14](../../includes/sssqlv14-md.md)] e versioni successive e [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br />Data e ora (fuso orario UTC) dell'evento. Non ammette i valori Null.|  

  
## <a name="remarks"></a>Commenti  
 La lettura di set di risultati di grandi dimensioni tramite l'esecuzione di **sys.fn_xe_file_target_read_file** in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] può causare un errore. Usare i **Risultati in modalità file** (**CTRL + MAIUSC + F**) per esportare set di risultati di grandi dimensioni in un file e leggere il file con un altro strumento.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-retrieving-data-from-file-targets"></a>R. Recupero di dati da destinazioni di file  
 Nell'esempio seguente vengono restituite tutte le righe di tutti i file. Nell'esempio le destinazioni di file e i metafile si trovano nella cartella della traccia in C:\unità.  
  
```  
SELECT * FROM sys.fn_xe_file_target_read_file('C:\traces\*.xel', 'C:\traces\metafile.xem', null, null);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Viste a gestione dinamica degli eventi estesi](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)   
 [Viste del catalogo degli eventi estesi &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [Eventi estesi](../../relational-databases/extended-events/extended-events.md)  
  
  
