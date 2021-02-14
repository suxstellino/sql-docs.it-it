---
description: Funzionamento delle operazioni sugli indici online
title: Funzionamento delle operazioni sugli indici online | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- online index operations
- source indexes [SQL Server]
- pre-existing indexes [SQL Server]
- target indexes [SQL Server]
- temporary mapping index [SQL Server]
- index temporary mappings [SQL Server]
ms.assetid: eef0c9d1-790d-46e4-a758-d0bf6742e6ae
author: MikeRayMSFT
ms.author: mikeray
ms.prod_service: database-engine, sql-database
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 17f070e71fe485b49eebdb441a75b809d701068f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351216"
---
# <a name="how-online-index-operations-work"></a>Funzionamento delle operazioni sugli indici online
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  In questo argomento vengono descritte le strutture esistenti durante un'operazione sugli indici online e illustra le attività associate a tali strutture.  
  
## <a name="online-index-structures"></a>Strutture degli indici online  
 Per consentire attività utente simultanee durante un'operazione DDL (Data Definition Language) sull'indice, durante l'operazione sull'indice online vengono usate le strutture seguenti: indici di origine e preesistenti, destinazione e un indice di mapping temporaneo per la ricompilazione di un heap o l'eliminazione di un indice cluster online.  
  
-   **Indici di origine e preesistenti**  
  
     L'origine è rappresentata dalla tabella originale o dai dati dell'indice cluster. Gli indici preesistenti sono indici non cluster associati alla struttura di origine. Se ad esempio l'operazione sull'indice online consiste nella ricompilazione di un indice cluster con quattro indici non cluster associati, l'origine è l'indice cluster esistente e gli indici preesistenti sono gli indici non cluster.  
  
     Gli indici preesistenti sono disponibili agli utenti simultanei per operazioni di selezione, inserimento, aggiornamento ed eliminazione, inclusi inserimenti bulk (supportati ma non consigliati) e aggiornamenti impliciti mediante trigger e vincoli di integrità referenziale. Tutti gli indici preesistenti sono disponibili per l'esecuzione di query e ricerche. Ciò significa che possono essere selezionati da Query Optimizer e, se necessario, specificati in hint per l'indice.  
  
-   **Destinazione**  
  
     La destinazione o le destinazioni sono rappresentate dal nuovo indice (o heap) o da un set di nuovi indici creati o ricompilati. Le operazioni utente di inserimento, aggiornamento ed eliminazione sull'origine vengono applicate da [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] alla destinazione durante l'operazione sull'indice. Se ad esempio l'operazione sull'indice online consiste nella ricompilazione di un indice cluster, la destinazione è l'indice cluster ricompilato. [!INCLUDE[ssDE](../../includes/ssde-md.md)] non ricompila indici non cluster quando viene ricompilato un indice cluster.  
  
     Nell'indice di destinazione non viene eseguita la ricerca durante l'elaborazione delle istruzioni SELECT finché non viene eseguito il commit dell'operazione. L'indice viene contrassegnato internamente come di sola scrittura.  
  
-   **Indice di mapping temporaneo**  
  
     Per le operazioni sugli indici online che consistono nella creazione, nell'eliminazione o nella ricompilazione di un indice cluster è inoltre necessario un indice di mapping temporaneo che viene utilizzato dalle transazioni simultanee per determinare i record da eliminare nei nuovi indici ricompilati quando vengono aggiornate o eliminate righe nella tabella sottostante. Questo indice non cluster viene creato contemporaneamente al nuovo indice cluster (o heap) e non richiede un'operazione di ordinamento separata. Le transazioni simultanee mantengono l'indice di mapping temporaneo in tutte le operazioni di inserimento, aggiornamento ed eliminazione.  
  
## <a name="online-index-activities"></a>Attività sugli indici online  
 Durante un'operazione sugli indici online semplice, ad esempio la creazione di un indice cluster in una tabella non indicizzata (heap), l'origine e la destinazione passano attraverso tre fasi: preparatoria, di compilazione e finale.  
  
 Nella figura seguente si illustra il processo per la creazione di un indice cluster iniziale online. L'oggetto di origine, l'heap, non presenta altri indici. Per ogni fase sono illustrate le attività sulle strutture di origine e di destinazione, nonché le operazioni utente simultanee di selezione, inserimento, aggiornamento ed eliminazione. Le fasi preparatoria, di compilazione e finale sono indicate insieme alle modalità di blocco utilizzate in ogni fase.  
  
 ![Attività eseguite durante l'operazione sull'indice online](../../relational-databases/indexes/media/online-index.gif "Attività eseguite durante l'operazione sull'indice online")  
  
## <a name="source-structure-activities"></a>Attività relative alla struttura di origine  
 Nella tabella seguente sono incluse le attività relative alle strutture di origine durante ogni fase dell'operazione sull'indice e la relativa strategia di blocco.  
  
|Fase|Attività di origine|Blocchi di origine|  
|-----------|---------------------|------------------|  
|Preparazione<br /><br /> Fase breve|Preparazione dei metadati di sistema per la creazione della nuova struttura vuota dell'indice.<br /><br /> Viene definito uno snapshot della tabella, ovvero viene utilizzato il controllo delle versioni delle righe per garantire la consistenza in lettura a livello di transazione.<br /><br /> Le operazioni di scrittura di utenti simultanei sull'origine vengono bloccate per un breve periodo di tempo.<br /><br /> Non sono consentite operazioni DDL simultanee, ad eccezione della creazione di più indici non cluster.|S (condiviso) nella tabella*<br /><br /> IS (preventivo condiviso)<br /><br /> INDEX_BUILD_INTERNAL_RESOURCE\*\*|  
|Compilare<br /><br /> Fase principale|I dati vengono sottoposti ad analisi, ordinati, uniti e inseriti nella destinazione in operazioni di caricamento bulk.<br /><br /> Le operazioni di selezione, inserimento, aggiornamento ed eliminazione di utenti simultanei vengono applicate agli indici preesistenti e ai nuovi indici compilati.|IS<br /><br /> INDEX_BUILD_INTERNAL_RESOURCE**|  
|Finale<br /><br /> Fase breve|Prima dell'avvio di questa fase, è necessario che tutte le transazioni di aggiornamento di cui non è stato eseguito il commit vengano completate. A seconda del blocco acquisito, tutte le nuove transazioni utente di lettura o di scrittura vengono bloccate per un breve periodo di tempo finché questa fase non viene completata.<br /><br /> I metadati di sistema vengono aggiornati per sostituire l'origine con la destinazione.<br /><br /> Se necessario, l'origine viene eliminata, ad esempio dopo la ricompilazione o l'eliminazione di un indice cluster.|INDEX_BUILD_INTERNAL_RESOURCE**<br /><br /> S nella tabella se viene creato un indice non cluster.\*<br /><br /> SCH-M (modifica dello schema) se viene eliminata la struttura di origine (indice o tabella).\*|  
  
 \* L'operazione sull'indice attende il completamento delle transazioni di aggiornamento di cui non è stato eseguito il commit prima di acquisire il blocco S o SCH-M nella tabella. Se viene eseguita una query con esecuzione prolungata, l'operazione sull'indice online resta in attesa fino al completamento della query.
  
 ** Con il blocco di risorsa INDEX_BUILD_INTERNAL_RESOURCE viene impedita l'esecuzione di operazioni DDL (Data Definition Language) simultanee sulle strutture di origine e preesistenti mentre è in corso l'operazione sull'indice. Questo blocco impedisce ad esempio la ricompilazione simultanea di due indici nella stessa tabella. Nonostante questo blocco di risorsa sia associato al blocco SCH-M, non impedisce l'esecuzione di istruzioni DML.  
  
 Nella tabella precedente è incluso un solo blocco condiviso (S) acquisito durante la fase di compilazione di un'operazione sugli indici online relativa a un singolo indice. Quando si compilano o ricompilano indici cluster e non cluster, in una singola operazione sugli indici online, ad esempio durante la creazione dell'indice cluster iniziale in una tabella contenente uno o più indici non cluster, vengono acquisiti due blocchi S di breve durata durante la fase di compilazione seguiti da blocchi preventivi condivisi (IS) di lunga durata. Viene innanzitutto acquisito un blocco S per la creazione dell'indice cluster e, quando la creazione dell'indice cluster viene completata, viene acquisito un altro blocco S di breve durata per la creazione degli indici non cluster. Dopo la creazione degli indici non cluster, il blocco S viene abbassato di livello e portato a blocco IS fino alla fase finale dell'operazione sull'indice online.  

Per altre informazioni sulla modalità di utilizzo dei blocchi e sul modo in cui è possibile gestirli, vedere gli [argomenti](../../t-sql/statements/alter-table-index-option-transact-sql.md#arguments).

### <a name="target-structure-activities"></a>Attività relative alla struttura di destinazione  
 Nella tabella seguente sono incluse le attività relative alla struttura di destinazione durante ogni fase dell'operazione sull'indice e la relativa strategia di blocco.  
  
|Fase|Attività di destinazione|Blocchi di destinazione|  
|-----------|---------------------|------------------|  
|Preparazione|Il nuovo indice viene creato e impostato in sola scrittura.|IS|  
|Compilare|Vengono inseriti i dati dall'origine.<br /><br /> Vengono applicate le modifiche utente (inserimenti, aggiornamenti, eliminazioni) apportate all'origine.<br /><br /> Questa attività è visibile all'utente.|IS|  
|Finale|Vengono aggiornati i metadati dell'indice.<br /><br /> Viene impostato lo stato lettura/scrittura per l'indice.|S<br /><br /> o<br /><br /> SCH-M|  
  
 Con le istruzioni SELECT eseguite dall'utente non è possibile accedere alla destinazione finché l'operazione sull'indice non è stata completata.  
  
 Dopo il completamento della fase preparatoria e della fase finale, i piani di aggiornamento e di query archiviati nella cache delle procedure vengono invalidati e per le query successive viene usato il nuovo indice.  
  
 La durata di un cursore dichiarato in una tabella oggetto di un'operazione su un indice online è limitata dalle fasi dell'operazione sull'indice. I cursori di aggiornamento vengono invalidati in corrispondenza di ogni fase, mentre i cursori di sola lettura vengono invalidati solo dopo la fase finale.  
  
## <a name="related-content"></a>Contenuto correlato  
 [Eseguire operazioni online sugli indici](../../relational-databases/indexes/perform-index-operations-online.md)  
  
 [Linee guida per le operazioni sugli indici online](../../relational-databases/indexes/guidelines-for-online-index-operations.md)  
  
## <a name="next-steps"></a>Passaggi successivi

[Opzioni di indice ALTER TABLE](../../t-sql/statements/alter-table-index-option-transact-sql.md#arguments)
