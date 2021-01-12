---
description: sys.dm_clr_appdomains (Transact-SQL)
title: sys.dm_clr_appdomains (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_clr_appdomains
- sys.dm_clr_appdomains
- dm_clr_appdomains_TSQL
- sys.dm_clr_appdomains_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_appdomains dynamic management dynamic management view
ms.assetid: 9fe0d4fd-950a-4274-a493-85e776278045
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ae934fc3b9d8f7664e2542450ee1456298548c85
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097771"
---
# <a name="sysdm_clr_appdomains-transact-sql"></a>sys.dm_clr_appdomains (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una riga per ogni dominio dell'applicazione nel server. Il dominio applicazione (**AppDomain**) è un costrutto nella [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] Common Language Runtime (CLR) che rappresenta l'unità di isolamento per un'applicazione. È possibile utilizzare questa visualizzazione per comprendere e risolvere i problemi relativi agli oggetti di integrazione CLR in esecuzione in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Esistono diversi tipi di oggetti di database gestito dell'integrazione con CLR. Per informazioni generali su questi oggetti, vedere [compilazione di oggetti di database con l'integrazione con Common Language Runtime (CLR)](../../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md). Ogni volta che questi oggetti vengono eseguiti, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Crea un **AppDomain** in cui è possibile caricare ed eseguire il codice richiesto. Il livello di isolamento per un **AppDomain** è un **AppDomain** per database per proprietario. Ovvero, tutti gli oggetti CLR di proprietà di un utente vengono sempre eseguiti nello stesso **AppDomain** per database (se un utente registra oggetti di database CLR in database diversi, gli oggetti di database CLR verranno eseguiti in domini applicazione diversi). Un **AppDomain** non viene eliminato definitivamente al termine dell'esecuzione del codice. ma viene memorizzato nella cache per le future esecuzioni. In questo modo le prestazioni risultano migliorate.  
  
 Per ulteriori informazioni, vedere [domini applicazione](../../sql-server/what-s-new-in-sql-server-2016.md).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**appdomain_address**|**varbinary (8)**|Indirizzo del **dominio applicazione**. Tutti gli oggetti di database gestiti di proprietà di un utente vengono sempre caricati nello stesso **AppDomain**. È possibile utilizzare questa colonna per cercare tutti gli assembly attualmente caricati in questo **AppDomain** in **sys.dm_clr_loaded_assemblies**.|  
|**appdomain_id**|**int**|ID del **dominio AppDomain**. Ogni **AppDomain** ha un ID univoco.|  
|**appdomain_name**|**varchar (386)**|Nome dell' **AppDomain** assegnato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**creation_time**|**datetime**|Ora di creazione dell' **AppDomain** . Poiché gli **AppDomain** vengono memorizzati nella cache e riutilizzati per migliorare le prestazioni, **creation_time** non è necessariamente il momento in cui il codice è stato eseguito.|  
|**db_id**|**int**|ID del database in cui è stato creato l' **AppDomain** . Il codice archiviato in due database diversi non può condividere un **AppDomain**.|  
|**user_id**|**int**|ID dell'utente i cui oggetti possono essere eseguiti in questo **AppDomain**.|  
|**state**|**nvarchar(128)**|Descrittore per lo stato corrente del **dominio AppDomain**. Un AppDomain può trovarsi in stati diversi dalla creazione all'eliminazione. Per ulteriori informazioni, vedere la sezione Osservazioni di questo argomento.|  
|**strong_refcount**|**int**|Numero di riferimenti sicuri a questo **AppDomain**. Riflette il numero di batch attualmente in esecuzione che utilizzano questo **AppDomain**. Si noti che l'esecuzione di questa visualizzazione creerà un **refcount sicuro**; anche se non è attualmente in esecuzione codice, **strong_refcount** avrà un valore pari a 1.|  
|**weak_refcount**|**int**|Numero di riferimenti deboli a questo **AppDomain**. Indica il numero di oggetti all'interno dell' **AppDomain** memorizzati nella cache. Quando si esegue un oggetto di database gestito, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo memorizza nella cache all'interno dell' **AppDomain** per un riutilizzo futuro. In questo modo le prestazioni risultano migliorate.|  
|**cost**|**int**|Costo del **dominio AppDomain**. Maggiore è il costo, più è probabile che questo **AppDomain** debba essere scaricato con un numero eccessivo di richieste di memoria. Il costo dipende in genere dalla quantità di memoria necessaria per ricreare questo **AppDomain**.|  
|**value**|**int**|Valore del **dominio AppDomain**. Più basso è il valore, più è probabile che questo **AppDomain** debba essere scaricato sotto pressione di memoria. Il valore dipende in genere dal numero di connessioni o batch che utilizzano questo **AppDomain**.|  
|**total_processor_time_ms**|**bigint**|Tempo totale del processore, in millisecondi, utilizzato da tutti i thread durante l'esecuzione nel dominio dell'applicazione corrente dall'avvio del processo. Equivale a **System. AppDomain. MonitoringTotalProcessorTime**.|  
|**total_allocated_memory_kb**|**bigint**|Dimensioni totali, in kilobyte, di tutte le allocazioni di memoria eseguite dal dominio dell'applicazione dalla sua creazione, senza sottrarre la memoria raccolta. Equivale a **System. AppDomain. MonitoringTotalAllocatedMemorySize**.|  
|**survived_memory_kb**|**bigint**|Numero di kilobyte rimanenti dall'ultima raccolta di blocco completa e a cui fa riferimento il dominio dell'applicazione corrente. Equivale a **System. AppDomain. MonitoringSurvivedMemorySize**.|  
  
## <a name="remarks"></a>Osservazioni  
 Esiste una relazione uno-a-uno tra **dm_clr_appdomains. appdomain_address** e **dm_clr_loaded_assemblies. appdomain_address**.  
  
 Le tabelle seguenti elencano i valori di **stato** possibili, le relative descrizioni e quando si verificano nel ciclo di vita dell' **AppDomain** . È possibile usare queste informazioni per seguire la vita di un **AppDomain** e per controllare lo scaricamento di istanze di **AppDomain** sospette o ripetitive, senza dover analizzare il registro eventi di Windows.  
  
## <a name="appdomain-initialization"></a>Inizializzazione di AppDomain  
  
|State|Descrizione|  
|-----------|-----------------|  
|E_APPDOMAIN_CREATING|È in corso la creazione del **dominio AppDomain** .|  
  
## <a name="appdomain-usage"></a>Utilizzo di AppDomain  
  
|State|Descrizione|  
|-----------|-----------------|  
|E_APPDOMAIN_SHARED|**AppDomain** di runtime è pronto per l'uso da parte di più utenti.|  
|E_APPDOMAIN_SINGLEUSER|**AppDomain** è pronto per l'utilizzo nelle operazioni DDL. Queste ultime differiscono da E_APPDOMAIN_SHARED per il fatto che gli AppDomain condivisi vengono utilizzati per le esecuzioni di integrazione di CLR e non per operazioni DDL. Tali AppDomain sono isolati da altre operazioni simultanee.|  
|E_APPDOMAIN_DOOMED|Il **dominio AppDomain** è pianificato per essere scaricato, ma al momento sono in esecuzione thread.|  
  
## <a name="appdomain-cleanup"></a>Eliminazione di AppDomain  
  
|State|Descrizione|  
|-----------|-----------------|  
|E_APPDOMAIN_UNLOADING|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha richiesto che CLR scarichi l' **AppDomain**, in genere perché l'assembly che contiene gli oggetti di database gestiti è stato modificato o eliminato.|  
|E_APPDOMAIN_UNLOADED|CLR ha scaricato l' **AppDomain**. Questo è in genere il risultato di una procedura di escalation dovuta a **ThreadAbort**, **OutOfMemory** o a un'eccezione non gestita nel codice utente.|  
|E_APPDOMAIN_ENQUEUE_DESTROY|**AppDomain** è stato scaricato in CLR e impostato per essere eliminato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|E_APPDOMAIN_DESTROY|È in corso l'eliminazione del **AppDomain** da parte di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|E_APPDOMAIN_ZOMBIE|**AppDomain** eliminato da. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tuttavia, non tutti i riferimenti a **AppDomain** sono stati eliminati.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE nel database.  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente viene illustrato come visualizzare i dettagli di un **AppDomain** per un determinato assembly:  
  
```  
select appdomain_id, creation_time, db_id, user_id, state  
from sys.dm_clr_appdomains a  
where appdomain_address =   
(select appdomain_address   
 from sys.dm_clr_loaded_assemblies  
   where assembly_id = 500);  
```  
  
 Nell'esempio seguente viene illustrato come visualizzare tutti gli assembly in un determinato **AppDomain**:  
  
```  
select a.name, a.assembly_id, a.permission_set_desc, a.is_visible, a.create_date, l.load_time   
from sys.dm_clr_loaded_assemblies as l   
inner join sys.assemblies as a  
on l.assembly_id = a.assembly_id  
where l.appdomain_address =   
(select appdomain_address   
from sys.dm_clr_appdomains  
where appdomain_id = 15);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.dm_clr_loaded_assemblies &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-clr-loaded-assemblies-transact-sql.md)   
 [Viste a gestione dinamica relative a Common Language Runtime &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
