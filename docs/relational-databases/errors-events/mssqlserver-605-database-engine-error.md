---
description: MSSQLSERVER_605
title: MSSQLSERVER_605 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 605 (Database Engine error)
ms.assetid: d8d3a22e-1ff8-48a4-891f-4c8619437e24
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d2ff4a40812fdedba5d3ed6b0f8b198f497a64a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211399"
---
# <a name="mssqlserver_605"></a>MSSQLSERVER_605
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|605|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|WRONGPAGE|  
|Testo del messaggio|Impossibile recuperare la pagina logica %S_PGID nel database %d, poiché appartiene all'unità di allocazione %I64d, non a %I64d.|  
  
## <a name="explanation"></a>Spiegazione  
Questo errore in genere indica il danneggiamento di una pagina o un'allocazione nel database specificato. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rileva il danneggiamento durante la lettura di pagine appartenenti a una tabella seguendo i collegamenti delle pagine o usando la mappa di allocazione degli indici (IAM, Index Allocation Map). Tutte le pagine allocate a una tabella devono appartenere a una delle unità di allocazione associate alla tabella. Se l'ID dell'unità di allocazione incluso nell'intestazione della pagina non corrisponde a uno degli ID di unità di allocazione associati alla tabella, viene generata questa eccezione. Il primo ID di unità di allocazione elencato nel messaggio di errore è l'ID presente nell'intestazione di pagina, mentre il secondo è quello associato alla tabella.  
  
**Errori di danneggiamento dei dati**  
  
Un livello di gravità pari a 21 indica un potenziale danneggiamento dei dati. Le possibili cause sono una catena di pagine o una mappa di allocazione degli indici danneggiata oppure una voce non valida nella vista del catalogo [sys.objects](~/relational-databases/system-catalog-views/sys-objects-transact-sql.md) per l'oggetto. Questi errori spesso sono causati da errori hardware o dei driver di dispositivi disco.  
  
**Errori transitori**  
  
Un livello di gravità pari a 12 indica un potenziale errore transitorio, ovvero un errore che si verifica nella cache e non implica danneggiamento di dati sul disco. Gli errori 605 di tipo transitorio possono essere causati dalle condizioni seguenti:  
  
-   Invio di una notifica anomala a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] da parte del sistema operativo per informare che è stata completata un'operazione di I/O. Il messaggio di errore viene visualizzato anche se non si è effettivamente verificato alcun danneggiamento di dati.  
  
 - Esecuzione di una query con l'hint di Query Optimizer NOLOCK o impostazione del livello di isolamento della transazione su READ UNCOMMITTED. Quando una query che usa un livello di isolamento della transazione NOLOCK o READ UNCOMMITTED tenta di leggere dati spostati o modificati da un altro utente, si verifica un errore 605. Per verificare che si tratti di un errore 605 transitorio, eseguire di nuovo la query in un secondo momento. 
  
In generale, se l'errore si verifica durante l'accesso ai dati ma le successive operazioni DBCC CHECKDB vengono completate senza errori, l'errore 605 era probabilmente transitorio.  
  
## <a name="user-action"></a>Azione dell'utente  
Se l'errore 605 non è temporaneo, il problema è grave e deve essere corretto eseguendo una delle operazioni seguenti:  
  
1.  Identificare le tabelle associate alle unità di allocazione specificate nel messaggio eseguendo la query seguente. Sostituire `allocation_unit_id` con le unità di allocazione specificate nel messaggio di errore.  
  
    ```sql  
    USE`database_name`;  
  
    GO  
  
    SELECT au.allocation_unit_id, OBJECT_NAME(p.object_id) AS table_name, fg.name AS filegroup_name,  
  
    au.type_desc AS allocation_type, au.data_pages, partition_number  
  
    FROM sys.allocation_units AS au  
  
    JOIN sys.partitions AS p ON au.container_id = p.partition_id  
  
    JOIN sys.filegroups AS fg ON fg.data_space_id = au.data_space_id  
  
    WHERE au.allocation_unit_id = `allocation_unit_id` OR au.allocation_unit_id = `allocation_unit_id`  
  
    ORDER BY au.allocation_unit_id;  
  
    GO  
    ```
  
2.  Eseguire DBCC CHECKTABLE senza una clausola REPAIR nella tabella associata al secondo ID di unità di allocazione specificato nel messaggio di errore.  
  
3.  Eseguire DBCC CHECKDB senza una clausola REPAIR il prima possibile, per determinare l'entità del danno all'intero database.  
  
4.  Verificare se nel log degli errori sono presenti altri errori spesso associati all'errore 605 e verificare se nel registro eventi di Windows sono presenti segnalazioni di eventuali problemi relativi al sistema o all'hardware. Risolvere tutti i problemi relativi all'hardware indicati nei log.  
  
Se il problema non è hardware, effettuare una delle operazioni seguenti:  
  
1.  Ripristinare il database da un backup valido noto. Per ripristinare solo le pagine danneggiate, è possibile utilizzare la caratteristica di backup per il ripristino di pagina.  
  
2.  Per correggere il danneggiamento, eseguire DBCC CHECKDB con la clausola REPAIR consigliata dall'operazione DBCC CHECKDB eseguita nel passaggio 3. Se l'esecuzione di DBCC CHECKDB con una clausola REPAIR non consente di risolvere il problema, contattare il personale del supporto tecnico. Accertarsi di avere l'output di DBCC CHECKDB disponibile per la consultazione.  
  
    > [!CAUTION]  
    > Se non si è certi dell'effetto prodotto sui dati dall'esecuzione di DBCC CHECKDB con la clausola REPAIR, contattare il personale del supporto tecnico prima di eseguire l'istruzione.  
  
## <a name="see-also"></a>Vedere anche  
[DBCC CHECKTABLE &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checktable-transact-sql.md)  
  
