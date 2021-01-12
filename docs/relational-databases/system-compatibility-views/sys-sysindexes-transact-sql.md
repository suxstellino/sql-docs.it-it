---
description: sys.sysindexes (Transact-SQL)
title: Indici sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysindexes
- sysindexes_TSQL
- sys.sysindexes
- sys.sysindexes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysindexes system table
- sys.sysindexes compatibility view
ms.assetid: f483d89c-35c4-4a08-8f8b-737fd80d13f5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cf8982115b1a4399c327aefc66a5e99a1d46c575
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094194"
---
# <a name="syssysindexes-transact-sql"></a>sys.sysindexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Include una riga per ogni indice e tabella del database corrente. Gli indici XML non sono supportati in questa vista. Le tabelle e gli indici partizionati non sono completamente supportati in questa visualizzazione. utilizzare invece la vista del catalogo [sys. Indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) .  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**id**|**int**|ID della tabella alla quale appartiene l'indice.|  
|**Stato**|**int**|Informazioni sullo stato del sistema.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**first**|**binary(6)**|Puntatore alla prima pagina o alla pagina radice.<br /><br /> Non usato quando **indid** = 0.<br /><br /> NULL = l'indice viene partizionato quando **indid** > 1.<br /><br /> NULL = la tabella viene partizionata quando **indid** è 0 o 1.|  
|**indid**|**smallint**|ID dell'indice:<br /><br /> 0 = heap<br /><br /> 1 = indice cluster<br /><br /> >1 = indice non cluster|  
|**radice**|**binary(6)**|Per **indid** >= 1, **root** è il puntatore alla pagina radice.<br /><br /> Non usato quando **indid** = 0.<br /><br /> NULL = l'indice viene partizionato quando **indid** > 1.<br /><br /> NULL = la tabella viene partizionata quando **indid** è 0 o 1.|  
|**minlen**|**smallint**|Dimensioni minime di una riga.|  
|**keycnt**|**smallint**|Numero di chiavi.|  
|**GroupID**|**smallint**|ID del filegroup in cui l'oggetto è stato creato.<br /><br /> NULL = l'indice viene partizionato quando **indid** > 1.<br /><br /> NULL = la tabella viene partizionata quando **indid** è 0 o 1.|  
|**dpages**|**int**|Per **indid** = 0 o **indid** = 1, **dpages** è il conteggio delle pagine di dati utilizzate.<br /><br /> Per **indid** > 1, **dpages** è il numero di pagine di indice utilizzate.<br /><br /> 0 = l'indice viene partizionato quando **indid** > 1.<br /><br /> 0 = la tabella viene partizionata quando **indid** è 0 o 1.<br /><br /> Non restituisce risultati precisi se si verifica un overflow della riga.|  
|**riservati**|**int**|Per **indid** = 0 o **indid** = 1, **reserved** è il numero di pagine allocate per tutti gli indici e i dati della tabella.<br /><br /> Per **indid** > 1, **riservato** è il numero di pagine allocate per l'indice.<br /><br /> 0 = l'indice viene partizionato quando **indid** > 1.<br /><br /> 0 = la tabella viene partizionata quando **indid** è 0 o 1.<br /><br /> Non restituisce risultati precisi se si verifica un overflow della riga.|  
|**utilizzato**|**int**|Per **indid** = 0 o **indid** = 1, **used** è il conteggio delle pagine totali utilizzate per tutti i dati di indice e di tabella.<br /><br /> Per **indid** > 1, **usato** è il numero di pagine utilizzate per l'indice.<br /><br /> 0 = l'indice viene partizionato quando **indid** > 1.<br /><br /> 0 = la tabella viene partizionata quando **indid** è 0 o 1.<br /><br /> Non restituisce risultati precisi se si verifica un overflow della riga.|  
|**rowcnt**|**bigint**|Conteggio delle righe a livello di dati basato su **indid** = 0 e **indid** = 1.<br /><br /> 0 = l'indice viene partizionato quando **indid** > 1.<br /><br /> 0 = la tabella viene partizionata quando **indid** è 0 o 1.|  
|**rowmodctr**|**int**|Conta il numero totale di righe inserite, eliminate o aggiornate a partire dall'ultimo aggiornamento delle statistiche per la tabella.<br /><br /> 0 = l'indice viene partizionato quando **indid** > 1.<br /><br /> 0 = la tabella viene partizionata quando **indid** è 0 o 1.<br /><br /> In [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] e versioni successive, **rowmodctr** non è completamente compatibile con le versioni precedenti. Per altre informazioni, vedere la sezione Osservazioni.|  
|**reserved3**|**int**|Viene restituito 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**reserved4**|**int**|Viene restituito 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xmaxlen**|**smallint**|Dimensioni massime di una riga.|  
|**maxirow**|**smallint**|Dimensioni massime di una riga di indice non foglia.<br /><br /> In [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] e versioni successive, **maxirow** non è completamente compatibile con le versioni precedenti.|  
|**OrigFillFactor**|**tinyint**|Valore del fattore di riempimento originale utilizzato durante la creazione dell'indice. Questo valore non viene mantenuto. Può tuttavia risultare utile se è necessario ricreare un indice e non si ricorda il fattore di riempimento utilizzato.|  
|**StatVersion**|**tinyint**|Viene restituito 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**reserved2**|**int**|Viene restituito 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**FirstIAM**|**binary(6)**|NULL = l'indice viene partizionato.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**impid**|**smallint**|Flag di implementazione dell'indice.<br /><br /> Viene restituito 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**lockflags**|**smallint**|Utilizzata per vincolare le granularità dei blocchi considerati per un indice. Per ridurre al minimo il costo di blocco, è possibile ad esempio impostare una tabella di ricerca essenzialmente di sola lettura per l'esecuzione del blocco solo a livello di tabella.|  
|**pgmodctr**|**int**|Viene restituito 0.<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**chiavi**|**varbinary(816)**|Elenco degli ID delle colonne che costituiscono la chiave dell'indice.<br /><br /> Restituisce NULL.<br /><br /> Per visualizzare le colonne chiave dell'indice, utilizzare [sys.sysindexkeys](../../relational-databases/system-compatibility-views/sys-sysindexkeys-transact-sql.md).|  
|**nome**|**sysname**|Nome dell'indice o della statistica. Restituisce NULL quando **indid** = 0. Modificare l'applicazione in uso in modo da eseguire la ricerca di un nome di heap NULL.|  
|**statblob**|**image**|BLOB (Binary Large Object) per statistiche.<br /><br /> Restituisce NULL.|  
|**maxlen**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**rows**|**int**|Conteggio delle righe a livello di dati basato su **indid** = 0 e **indid** = 1 e il valore viene ripetuto per **indid** >1.|  
  
## <a name="remarks"></a>Osservazioni  
 Le colonne definite come riservate non devono essere utilizzate.  
  
 Le colonne **dpages**, **reserved** e **used** non restituiranno risultati accurati se la tabella o l'indice contiene dati nell'unità di allocazione ROW_OVERFLOW. I conteggi delle pagine per ogni indice vengono inoltre rilevati separatamente e per la tabella di base non vengono aggregati. Per visualizzare i conteggi delle pagine, utilizzare le viste del catalogo [sys.allocation_units](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md) o [sys.](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md) Partitions oppure la vista a gestione dinamica [sys.dm_db_partition_stats](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md) .  
  
 In SQL Server 2000 e versioni precedenti, il [!INCLUDE[ssDE](../../includes/ssde-md.md)] gestisce contatori delle modifiche a livello di riga. Questi contatori vengono ora gestiti a livello di colonna. Pertanto, la colonna **rowmodctr** viene calcolata e produce risultati simili ai risultati nelle versioni precedenti, ma non sono esatti.  
  
 Se si usa il valore in **rowmodctr** per determinare quando aggiornare le statistiche, prendere in considerazione le soluzioni seguenti:  
  
-   Non eseguire alcuna operazione. Il nuovo valore di **rowmodctr** consente spesso di determinare quando aggiornare le statistiche perché il comportamento è ragionevolmente vicino ai risultati delle versioni precedenti.  
  
-   Utilizzare AUTO_UPDATE_STATISTICS. Per ulteriori informazioni, vedere [statistiche](../../relational-databases/statistics/statistics.md).  
  
-   Determinare il momento dell'aggiornamento delle statistiche in base a un intervallo di tempo. Ad esempio, ogni ora, ogni giorno o ogni settimana.  
  
-   Determinare il momento dell'aggiornamento delle statistiche utilizzando informazioni a livello di applicazione. Ad esempio, ogni volta che il valore massimo di una colonna **Identity** viene modificato di più di 10.000 o ogni volta che viene eseguita un'operazione di inserimento bulk.  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Mapping di tabelle di sistema a viste di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  
