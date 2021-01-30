---
description: syspublications (vista di sistema) (Transact-SQL)
title: syspublications (vista di sistema) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- syspublications
- syspublications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syspublications view
ms.assetid: e5f57c32-efc0-4455-a74f-684dc2ae51f8
author: stevestein
ms.author: sstein
ms.openlocfilehash: 9a17f9371d82ca50a1a27ef0acb0e7281a26fb92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181562"
---
# <a name="syspublications-system-view-transact-sql"></a>syspublications (vista di sistema) (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista **syspublications** espone le informazioni sulla pubblicazione. Questa vista è archiviata nel database di distribuzione.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**description**|**nvarchar(255)**|Voce descrittiva per la pubblicazione.|  
|**nome**|**sysname**|Nome univoco associato alla pubblicazione.|  
|**pubid**|**int**|Colonna Identity che include un ID univoco per la pubblicazione.|  
|**repl_freq**|**tinyint**|Frequenza della replica:<br /><br /> **0** = basato sulla transazione (transazionale).<br /><br /> **1** = aggiornamento tabella pianificato (snapshot).|  
|**Stato**|**tinyint**|Stato della pubblicazione:<br /><br /> **0** = inattivo.<br /><br /> **1** = attivo.|  
|**sync_method**|**tinyint**|Metodo di sincronizzazione:<br /><br /> **0** = utilità per la copia bulk nativa (BCP).<br /><br /> **1** = bcp di tipo carattere.<br /><br /> **3** = simultanea, ovvero viene utilizzata l'utilità bcp nativa, ma durante lo snapshot le tabelle non vengono bloccate.<br /><br /> **4** = concurrent_c, che significa che viene utilizzata l'utilità bcp di tipo carattere, ma durante lo snapshot le tabelle non vengono bloccate.|  
|**snapshot_jobid**|**binary(16)**|Identifica il processo di agente pianificato per la generazione dello snapshot iniziale.|  
|**independent_agent**|**bit**|Specifica se per la pubblicazione è disponibile un agente di distribuzione autonomo.<br /><br /> **0** = la pubblicazione utilizza un agente di distribuzione condiviso e ogni coppia database del server di pubblicazione/database del Sottoscrittore dispone di un solo agente condiviso.<br /><br /> **1** = è presente una agente di distribuzione autonoma per questa pubblicazione.|  
|**immediate_sync**|**bit**|Indica se i file di sincronizzazione vengono creati o ricreati ogni volta che viene eseguita la agente di snapshot, dove **1** indica che vengono creati ogni volta che viene eseguito l'agente.|  
|**enabled_for_internet**|**bit**|Indica se i file di sincronizzazione per la pubblicazione sono esposti a Internet tramite FTP (file Transfer Protocol) e altri servizi, dove **1** indica che è possibile accedervi da Internet.|  
|**allow_push**|**bit**|Indica se le sottoscrizioni push sono consentite nella pubblicazione, dove **1** indica che sono consentite.|  
|**allow_pull**|**bit**|Indica se le sottoscrizioni pull sono consentite nella pubblicazione, dove **1** indica che sono consentite.|  
|**allow_anonymous**|**bit**|Indica se sono consentite sottoscrizioni anonime nella pubblicazione, dove **1** indica che sono consentite.|  
|**immediate_sync_ready**|**bit**|Indica se lo snapshot è stato generato dall'agente snapshot e se è pronto per l'utilizzo nelle nuove sottoscrizioni. Questo valore risulta significativo solo per le pubblicazioni ad aggiornamento immediato. **1** indica che lo snapshot è pronto.|  
|**allow_sync_tran**|**bit**|Specifica se è consentito creare sottoscrizioni ad aggiornamento immediato per la pubblicazione. **1** indica che sono consentite sottoscrizioni ad aggiornamento immediato.|  
|**autogen_sync_procs**|**bit**|Specifica se la stored procedure di sincronizzazione per sottoscrizioni ad aggiornamento immediato viene generata nel server di pubblicazione. **1** indica che viene generato nel server di pubblicazione.|  
|**conservazione**|**int**|Periodo di tempo, espresso in ore, per cui le modifiche della pubblicazione vengono mantenute nel database di distribuzione.|  
|**allow_queued_tran**|**bit**|Specifica se disabilitare l'inserimento in coda delle modifiche apportate nel Sottoscrittore finché non è possibile applicarle al server di pubblicazione. Se è **1**, le modifiche nel Sottoscrittore vengono accodate.|  
|**snapshot_in_defaultfolder**|**bit**|Specifica se i file di snapshot sono archiviati nella cartella predefinita. Se è **0**, i file di snapshot sono stati archiviati nel percorso alternativo specificato da *alternate_snapshot_folder*. Se è 1, i file di snapshot sono disponibili nella cartella predefinita.|  
|**alt_snapshot_folder**|**nvarchar (510)**|Specifica la posizione della cartella alternativa per lo snapshot.|  
|**pre_snapshot_script**|**nvarchar (510)**|Specifica un puntatore al percorso di un file con **estensione SQL** . L'agente di distribuzione esegue lo script pre-snapshot prima dell'esecuzione degli script di oggetti replicati in fase di applicazione di uno snapshot in un Sottoscrittore.|  
|**post_snapshot_script**|**nvarchar (510)**|Specifica un puntatore al percorso di un file con **estensione SQL** . L'agente di distribuzione esegue lo script post-snapshot dopo l'applicazione di tutti gli altri script di oggetti replicati e dei dati durante una sincronizzazione iniziale.|  
|**compress_snapshot**|**bit**|Specifica che lo snapshot scritto nella posizione *alt_snapshot_folder* deve essere compresso nel [!INCLUDE[msCoName](../../includes/msconame-md.md)] formato CAB. **1** indica che lo snapshot verrà compresso.|  
|**ftp_address**|**sysname**|Indirizzo di rete del servizio FTP per il server di distribuzione. Specifica la posizione in cui i file di snapshot della pubblicazione possono essere prelevati dall'agente di distribuzione.|  
|**ftp_port**|**int**|Numero di porta del servizio FTP per il server di distribuzione. Specifica la posizione in cui i file di snapshot della pubblicazione possono essere prelevati dall'agente di distribuzione.|  
|**ftp_subdirectory**|**nvarchar (510)**|Specifica la posizione in cui i file di snapshot possono essere prelevati dall'agente di distribuzione se la pubblicazione supporta la propagazione degli snapshot tramite FTP.|  
|**ftp_login**|**nvarchar(256)**|Nome utente utilizzato per la connessione al servizio FTP.|  
|**ftp_password**|**nvarchar (1048)**|Password dell'utente utilizzata per la connessione al servizio FTP.|  
|**allow_dts**|**bit**|Specifica se la pubblicazione consente trasformazioni [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Data Transformation Services (DTS). **1** specifica che sono consentite le trasformazioni DTS.|  
|**allow_subscription_copy**|**bit**|Specifica se la funzionalità che consente di copiare i database di sottoscrizione che sottoscrivono la pubblicazione è abilitata. **1** indica che è consentita la copia.|  
|**centralized_conflicts**|**bit**|Specifica se i record dei conflitti vengono archiviati nel server di pubblicazione:<br /><br /> **0** = i record dei conflitti vengono archiviati sia nel server di pubblicazione che nel Sottoscrittore che ha causato il conflitto.<br /><br /> **1** = i record dei conflitti vengono archiviati nel server di pubblicazione.|  
|**conflict_retention**|**int**|Specifica il periodo di memorizzazione dei record dei conflitti espresso in giorni.|  
|**conflict_policy**|**int**|Specifica i criteri di risoluzione dei conflitti adottati quando viene utilizzata l'opzione per Sottoscrittori ad aggiornamento in coda. I possibili valori sono i seguenti:<br /><br /> **1** = il conflitto viene vinto dal server di pubblicazione.<br /><br /> **2** = il conflitto viene vinto dal Sottoscrittore.<br /><br /> **3** = la sottoscrizione viene reinizializzata.|  
|**queue_type**|**int**|Specifica il tipo di coda da utilizzare. I possibili valori sono i seguenti:<br /><br /> **1** = MSMQ, che utilizza [!INCLUDE[msCoName](../../includes/msconame-md.md)] Accodamento messaggi per archiviare le transazioni.<br /><br /> **2** =. SQL, che usa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per archiviare le transazioni.<br /><br /> Nota: l'utilizzo [!INCLUDE[msCoName](../../includes/msconame-md.md)] di Accodamento messaggi è stato deprecato e non è più supportato.|  
|**ad_guidname**|**sysname**|Specifica se la pubblicazione è pubblicata in [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory. Un valore GUID valido indica che la pubblicazione è pubblicata in Active Directory e il GUID è l'objectGUID dell'oggetto pubblicazione di Active Directory corrispondente. Se è NULL, la pubblicazione non è pubblicata in Active Directory.<br /><br /> Nota: la pubblicazione in Active Directory non è più supportata.|  
|**backward_comp_level**|**int**|Livello di compatibilità del database. I possibili valori sono i seguenti:<br /><br /> **90**  =  [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .<br /><br /> **100**  =  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] .|  
|**allow_initialize_from_backup**|**bit**|Indica se i Sottoscrittori possono inizializzare una sottoscrizione della pubblicazione da un backup anziché da uno snapshot iniziale. **1** indica che le sottoscrizioni possono essere inizializzate da un backup e **0** indica che non è possibile. Per altre informazioni, vedere [Inizializzazione di una sottoscrizione transazionale senza uno snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).|  
|**min_autonosync_lsn**|**binario (1)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**replicate_ddl**|**int**|Indica se per la pubblicazione è supportata la replica dello schema.<br /><br /> **1** = le istruzioni DDL eseguite nel server di pubblicazione vengono replicate.<br /><br /> **0** = indica che le istruzioni DDL non vengono replicate. Per altre informazioni, vedere [Apportare modifiche allo schema nei database di pubblicazione](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).|  
|**options**|**int**|Mappa di bit che specifica le opzioni di pubblicazione aggiuntive. I possibili valori bit per bit sono i seguenti:<br /><br /> **0x1** : abilitata per la replica peer-to-peer.<br /><br /> **0x2** : consente di pubblicare solo le modifiche locali per la replica peer-to-peer.<br /><br /> **0x4** : abilitata per i [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sottoscrittori non.<br /><br /> **0x8** : abilitata per il rilevamento dei conflitti peer-to-peer.|  
|**originator_id**|**smallint**|Identifica ogni nodo in una topologia di replica peer-to-peer per consentire il rilevamento dei conflitti. Per altre informazioni, vedere [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).|  
  
## <a name="see-also"></a>Vedere anche  
 [Tabelle di replica &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Stored procedure per la replica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_addpublication &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)   
 [sp_changepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)   
 [sp_helppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md)  
  
  
