---
title: Compatibilità FILESTREAM | Microsoft Docs
description: FILESTREAM archivia i dati nel file system. Informazioni sulle linee guida, sulle limitazioni e sui suggerimenti da tenere presenti quando si usa FILESTREAM con diverse funzionalità di SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: filestream
ms.topic: conceptual
helpviewer_keywords:
- FILESTREAM [SQL Server], other SQL Server features and
- FILESTREAM [SQL Server], limitations
ms.assetid: d2c145dc-d49a-4f5b-91e6-89a2b0adb4f3
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 53ea4630f147ce576ad071d1ac593e00452944a3
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250632"
---
# <a name="filestream-compatibility-with-other-sql-server-features"></a>Compatibilità FILESTREAM con altre funzionalità di SQL Server

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Poiché i dati FILESTREAM sono nel file system, in questo argomento vengono fornite alcune considerazioni, linee guida e limitazioni per l'utilizzo di FILESTREAM con le funzionalità di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]indicate di seguito.  
  
-   [SQL Server Integration Services (SSIS)](#ssis)  
  
-   [Query distribuite e server collegati](#distqueries)  
  
-   [Crittografia](#encryption)  
  
-   [snapshot del database](#DatabaseSnapshot)  
  
-   [Replica](#Replication)  
  
-   [Log shipping](#LogShipping)  
  
-   [Mirroring del database](#DatabaseMirroring)  
  
-   [Indicizzazione full-text](#FullText)  
  
-   [Clustering di failover](#FailoverClustering)  
  
-   [SQL Server Express](#SQLServerExpress)  
  
-   [Database indipendenti](#contained)  
  
##  <a name="sql-server-integration-services-ssis"></a><a name="ssis"></a> SQL Server Integration Services (SSIS)  
 In SQL Server Integration Services (SSIS) i dati FILESTREAM vengono gestiti nel flusso di dati in modo analogo agli altri dati BLOB, utilizzando il tipo di dati SSIS DT_IMAGE.  
  
 È possibile utilizzare la trasformazione Importa colonna per caricare file dal file system e inserirli in una colonna FILESTREAM. È inoltre possibile utilizzare la trasformazione Esporta colonna per estrarre i file da una colonna FILESTREAM e inserirli in un'altra posizione nel file system.  
  
##  <a name="distributed-queries-and-linked-servers"></a><a name="distqueries"></a> Query distribuite e server collegati  
 È possibile utilizzare i dati FILESTREAM tramite query distribuite e server collegati trattandoli come dati **varbinary(max)** . Non è possibile utilizzare la funzione FILESTREAM **PathName()** in query distribuite in cui viene utilizzato un nome in quattro parti, anche quando il nome si riferisce al server locale. Tuttavia, è possibile utilizzare **PathName()** nella query interna di una query pass-through in cui viene utilizzato **OPENQUERY()** .  
  
##  <a name="encryption"></a><a name="encryption"></a> Crittografia  
 I dati FILESTREAM non vengono crittografati anche se è abilitata la crittografia dei dati trasparenti.  
  
##  <a name="database-snapshots"></a><a name="DatabaseSnapshot"></a> Snapshot di database  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta [snapshot del database](../../relational-databases/databases/database-snapshots-sql-server.md) per i filegroup FILESTREAM. Se un filegroup FILESTREAM è incluso in una clausola CREATE DATABASE ON, l'istruzione non verrà eseguita e verrà restituito un errore.  
  
 Se si sta utilizzando FILESTREAM, è possibile creare snapshot del database di filegroup standard (non FILESTREAM). I filegroup FILESTREAM sono contrassegnati come offline per questi snapshot del database.  
  
 Un'istruzione SELECT eseguita in una tabella FILESTREAM in uno snapshot del database non deve includere una colonna FILESTREAM. In caso contrario, verrà restituito il seguente messaggio di errore:  
  
 `Could not continue scan with NOLOCK due to data movement.`  
  
##  <a name="replication"></a><a name="Replication"></a> Replication  
 Una colonna **varbinary(max)** che ha l'attributo FILESTREAM abilitato sul server di pubblicazione può essere replicata in un sottoscrittore con o senza l'attributo FILESTREAM. Per specificare la modalità di replica della colonna, usare la finestra di dialogo **Proprietà articolo - \<Article>** oppure il parametro @schema_option di [sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) o [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). I dati replicati in una colonna **varbinary(max)** che non dispone dell'attributo FILESTREAM non devono superare il limite di 2 GB per quel tipo di dati; in caso contrario, viene generato un errore di runtime. Si consiglia di replicare l'attributo FILESTREAM, a meno che non si stia effettuando la replica dei dati in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. La replica di tabelle con colonne FILESTREAM in Sottoscrittori [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] non è supportata, indipendentemente dall'opzione di schema specificata.  
  
> [!NOTE]  
>  La replica di valori di dati di dimensioni elevate da [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] a Sottoscrittori [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] è limitata a dati con dimensioni massime pari a 256 MB. Per ulteriori informazioni, vedere la pagina Web [Specifiche di capacità massima](../../sql-server/maximum-capacity-specifications-for-sql-server.md).  
  
### <a name="considerations-for-transactional-replication"></a>Considerazioni sulla replica transazionale  
 Se si utilizzano colonne FILESTREAM in tabelle pubblicate per replica transazionale, considerare quanto riportato di seguito.  
  
-   Se in una tabella qualsiasi sono incluse colonne con attributo FILESTREAM, non è possibile usare i valori *database snapshot* o *database snapshot character* per la proprietà @sync_method di [sp_addpublication](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md).  
  
-   L'opzione max text repl size consente di specificare la quantità massima di dati che è possibile inserire in una colonna pubblicata per la replica. Questa opzione può essere utilizzata per controllare le dimensioni dei dati FILESTREAM replicati.  
  
-   Se si specifica l'opzione dello schema per replicare l'attributo FILESTREAM, ma si esclude la colonna **uniqueidentifier** richiesta da FILESTREAM o si specifica di non replicare il vincolo UNIQUE per la colonna, la replica dell'attributo FILESTREAM non viene effettuata. La colonna viene replicata solo come **varbinary(max)** .  
  
### <a name="considerations-for-merge-replication"></a>Considerazioni per la replica di tipo merge  
 Se si utilizzano colonne FILESTREAM in tabelle pubblicate per replica di tipo merge, considerare quanto riportato di seguito.  
  
-   Le repliche di tipo merge e FILESTREAM richiedono una colonna di tipo di dati **uniqueidentifier** per identificare ogni riga in una tabella. La replica di tipo merge consente di aggiungere automaticamente una colonna se la tabella non ne contiene neanche una. La replica di tipo merge richiede l'impostazione della proprietà ROWGUIDCOL per la colonna e che vi sia un'impostazione predefinita di NEWID () o NEWSEQUENTIALID (). Oltre a questi requisiti, è necessario che per FILESTREAM vi sia un vincolo UNIQUE definito per la colonna. In base a questi requisiti si verificano le seguenti conseguenze:  
  
    -   Se si aggiunge una colonna FILESTREAM a una tabella che è già pubblicata per la replica di tipo merge, verificare che la colonna **uniqueidentifier** abbia un vincolo UNIQUE. In caso contrario, aggiungere un vincolo denominato alla tabella nel database di pubblicazione. Per impostazione predefinita questa modifica dello schema verrà pubblicata tramite la replica e applicata a ogni database di sottoscrizione.  
  
         Se si aggiunge manualmente un vincolo UNIQUE come descritto e si desidera rimuovere una replica di tipo merge, è necessario prima rimuovere il vincolo UNIQUE; in caso contrario, la rimozione della replica non verrà effettuata.  
  
    -   Per impostazione predefinita, la replica di tipo merge consente di utilizzare NEWSEQUENTIALID () perché può fornire migliori prestazioni rispetto a NEWID (). Se si aggiunge una colonna **uniqueidentifier** a una tabella che sarà pubblicata per la replica di tipo merge, specificare NEWSEQUENTIALID () come impostazione predefinita.  
  
-   La replica di tipo merge include un'ottimizzazione per la replica di tipi di oggetto di grandi dimensioni. Questa ottimizzazione è controllata dal parametro @stream_blob_columns di [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Se si imposta l'opzione dello schema per replicare l'attributo FILESTREAM, il valore del parametro @stream_blob_columns è impostato su **true**. L'override di questa ottimizzazione può essere eseguito utilizzando [sp_changemergearticle](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Questa stored procedure consente di impostare @stream_blob_columns su **false**. Se si aggiunge una colonna FILESTREAM a una tabella che è già pubblicata per la replica di tipo merge, si consiglia di impostare l'opzione su **true** utilizzando sp_changemergearticle.  
  
-   L'abilitazione dell'opzione dello schema per FILESTREAM dopo la creazione di un articolo potrebbe non consentire la replica se i dati in una colonna FILESTREAM superano 2 GB e si verifica un conflitto durante tale operazione. Se le probabilità che ciò si verifichi sono alte, è consigliabile rimuovere e creare di nuovo l'articolo della tabella con l'opzione dello schema FILESTREAM appropriata abilitata al momento della creazione.  
  
-   La replica di tipo merge può sincronizzare i dati FILESTREAM su una connessione HTTPS utilizzando [Sincronizzazione Web](../../relational-databases/replication/web-synchronization-for-merge-replication.md). Questi dati non possono superare il limite di 50 MB per la sincronizzazione Web; in caso contrario, viene generato un errore di runtime.  
  
##  <a name="log-shipping"></a><a name="LogShipping"></a> Log shipping  
 Il[log shipping](../../database-engine/log-shipping/about-log-shipping-sql-server.md) supporta FILESTREAM. Sia i server primari che secondari devono eseguire [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]o una versione più recente e avere FILESTREAM abilitato.  
  
##  <a name="database-mirroring"></a><a name="DatabaseMirroring"></a> Mirroring del database  
 Il mirroring del database non supporta FILESTREAM. Non è possibile creare un filegroup FILESTREAM nel server principale, né configurare il mirroring per un database in cui sono contenuti filegroup FILESTREAM.  
  
##  <a name="full-text-indexing"></a><a name="FullText"></a> Indicizzazione full-text  
 [L'indicizzazione full-text](../../relational-databases/search/populate-full-text-indexes.md) viene eseguita per una colonna FILESTREAM nello stesso modo in cui avviene per una colonna **varbinary(max)** . La tabella FILESTREAM deve avere una colonna che contiene l'estensione del nome file per ogni BLOB FILESTREAM. Per altre informazioni, vedere [Esecuzione della query con ricerca Full-Text](../../relational-databases/search/query-with-full-text-search.md), [Configurazione e gestione di filtri per la ricerca](../../relational-databases/search/configure-and-manage-filters-for-search.md) e [sys.fulltext_document_types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md).  
  
 Il motore full-text consente di indicizzare il contenuto dei BLOB FILESTREAM. L'indicizzazione di file di immagini, ad esempio, potrebbe non essere utile. Un BLOB FILESTREAM viene reindicizzato quando viene aggiornato.  
  
##  <a name="failover-clustering"></a><a name="FailoverClustering"></a> Clustering di failover  
 Per il clustering di failover, i filegroup FILESTREAM devono essere inseriti su un disco condiviso. FILESTREAM deve essere abilitato su ogni nodo nel cluster che ospiterà l'istanza FILESTREAM. Per altre informazioni, vedere [Configurazione di FILESTREAM in un cluster di failover](../../relational-databases/blob/set-up-filestream-on-a-failover-cluster.md).  
  
##  <a name="sql-server-express"></a><a name="SQLServerExpress"></a> SQL Server Express  
 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] supporta FILESTREAM. Il limite delle dimensioni di 10 GB non include il contenitore dei dati FILESTREAM.  
  
##  <a name="contained-databases"></a><a name="contained"></a> Database indipendenti  
 La funzionalità FILESTREAM richiede alcuni interventi di configurazione all'esterno del database. Pertanto un database in cui si utilizza FILESTREAM o FileTable non è completamente indipendente.  
  
 È possibile impostare l'indipendenza del database su PARTIAL se si desidera utilizzare alcune funzionalità dei database indipendenti, ad esempio gli utenti indipendenti. In tal caso, tuttavia, è necessario tenere presente che alcune impostazioni del database non sono contenute nel database e non vengono spostate automaticamente quando viene spostato il database.  
  
## <a name="see-also"></a>Vedere anche  
 [Dati BLOB &#40;Binary Large Object&#41; &#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)  
  
