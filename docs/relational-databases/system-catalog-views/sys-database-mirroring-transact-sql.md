---
description: sys.database_mirroring (Transact-SQL)
title: sys.database_mirroring (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.database_mirroring
- database_mirroring
- sys.database_mirroring_TSQL
- database_mirroring_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_mirroring catalog view
ms.assetid: 480de2b0-2c16-497d-a6a3-bf7f52a7c9a0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6e6d3eb98654d2fd5cf539b3052e9b96f877fa37
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209501"
---
# <a name="sysdatabase_mirroring-transact-sql"></a>sys.database_mirroring (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una riga per ogni database nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se il database non è ONLINE o se il mirroring del database non è abilitato, i valori di tutte le colonne tranne database_id saranno NULL.  
  
 Per visualizzare la riga relativa a un database diverso dal database master o tempdb, è necessario essere il proprietario del database o avere almeno l'autorizzazione ALTER ANY DATABASE o VIEW ANY DATABASE a livello di server oppure l'autorizzazione CREATE DATABASE nel database master. Per visualizzare i valori non NULL in un database mirror, è necessario essere un membro del ruolo predefinito del server **sysadmin** .  
  
> [!NOTE]  
>  Se un database non partecipa al mirroring, tutte le colonne con prefisso "mirroring_" sono NULL.  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|ID del database. È univoco in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**mirroring_guid**|**uniqueidentifier**|ID della relazione di mirroring.<br /><br /> NULL = il database non è accessibile o non è con mirroring.<br /><br /> Nota: se il database non partecipa al mirroring, tutte le colonne con prefisso "mirroring_" sono NULL.|  
|**mirroring_state**|**tinyint**|Stato del database mirror e della sessione di mirroring del database.<br /><br /> 0 = sospeso<br /><br /> 1 = Disconnesso dall'altro partner<br /><br /> 2 = Sincronizzazione in corso<br /><br /> 3 = Failover in sospeso<br /><br /> 4 = Sincronizzato<br /><br /> 5 = I partner non sono sincronizzati. Il failover ora non è possibile.<br /><br /> 6 = i partner sono sincronizzati. Il failover è possibile. Per informazioni sui requisiti per il failover, vedere [modalità operative per il mirroring del database](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).<br /><br /> NULL = Database inaccessibile o non sottoposto a mirroring.|  
|**mirroring_state_desc**|**nvarchar(60)**|Descrizione dello stato del database mirror e della sessione di mirroring del database. I possibili valori sono i seguenti:<br /><br /> DISCONNECTED<br /><br /> SYNCHRONIZED<br /><br /> SYNCHRONIZING<br /><br /> PENDING_FAILOVER<br /><br /> SUSPENDED<br /><br /> UNSYNCHRONIZED<br /><br /> SYNCHRONIZED<br /><br /> NULL<br /><br /> Per altre informazioni, vedere [Stati di mirroring &#40;SQL Server&#41;](../../database-engine/database-mirroring/mirroring-states-sql-server.md).|  
|**mirroring_role**|**tinyint**|Ruolo corrente svolto dal database locale nella sessione di mirroring del database.<br /><br /> 1 = Database principale<br /><br /> 2 = Database mirror<br /><br /> NULL = Database inaccessibile o non sottoposto a mirroring.|  
|**mirroring_role_desc**|**nvarchar(60)**|Descrizione del ruolo svolto dal database locale nel mirroring. I possibili valori sono i seguenti:<br /><br /> PRINCIPAL<br /><br /> MIRROR|  
|**mirroring_role_sequence**|**int**|Numero di scambi di ruolo dei partner del mirroring dovuti a un failover o a un servizio forzato.<br /><br /> NULL = Database inaccessibile o non sottoposto a mirroring.|  
|**mirroring_safety_level**|**tinyint**|Impostazione di protezione per gli aggiornamenti nel database mirror:<br /><br /> 0 = Stato sconosciuto<br /><br /> 1 = Disattivata [asincrona]<br /><br /> 2 = Completa [sincrona]<br /><br /> NULL = Database inaccessibile o non sottoposto a mirroring.|  
|**mirroring_safety_level_desc**|**nvarchar(60)**|Impostazione di sicurezza delle transazioni per gli aggiornamenti nel database mirror. I possibili valori sono i seguenti:<br /><br /> UNKNOWN<br /><br /> OFF<br /><br /> FULL<br /><br /> NULL|  
|**mirroring_safety_sequence**|**int**|Aggiorna il numero di sequenza per le modifiche apportate al livello di sicurezza delle transazioni.<br /><br /> NULL = Database inaccessibile o non sottoposto a mirroring.|  
|**mirroring_partner_name**|**nvarchar(128)**|Nome server del partner di mirroring di database.<br /><br /> NULL = Database inaccessibile o non sottoposto a mirroring.|  
|**mirroring_partner_instance**|**nvarchar(128)**|Nome dell'istanza e nome del computer per l'altro partner. I client utilizzano queste informazioni per connettersi al partner se questo diventa il server principale.<br /><br /> NULL = Database inaccessibile o non sottoposto a mirroring.|  
|**mirroring_witness_name**|**nvarchar(128)**|Nome del server di controllo del mirroring.<br /><br /> NULL = Non esiste alcun server di controllo.|  
|mirroring_witness_state|**tinyint**|Stato del server di controllo del mirroring nella sessione di mirroring del database. I possibili valori sono i seguenti:<br /><br /> 0 = Sconosciuto<br /><br /> 1 = Connesso<br /><br /> 2 = Disconnesso<br /><br /> NULL = Non esiste alcun server di controllo del mirroring, il database non è online oppure il database non è sottoposto a mirroring.|  
|**mirroring_witness_state_desc**|**nvarchar(60)**|Descrizione dello stato. I possibili valori sono i seguenti:<br /><br /> UNKNOWN<br /><br /> CONNECTED<br /><br /> DISCONNECTED<br /><br /> NULL|  
|**mirroring_failover_lsn**|**numeric(25,0)**|Numero di sequenza del file di log (LSN) del record del log delle transazioni più recente di cui è certo il salvataggio sul disco per entrambi i partner. Dopo un failover, il **mirroring_failover_lsn** viene utilizzato dai partner come punto di riconciliazione in corrispondenza del quale il nuovo server mirror inizia la sincronizzazione del nuovo database mirror con il nuovo database principale.|  
|**mirroring_connection_timeout**|**int**|Timeout della connessione per il mirroring, espresso in secondi. Numero di secondi di attesa della risposta da parte di un partner o del server di controllo del mirroring prima che venga considerato non disponibile. Il valore di timeout predefinito è di 10 secondi.<br /><br /> NULL = Database inaccessibile o non sottoposto a mirroring.|  
|**mirroring_redo_queue**|**int**|Quantità massima del log di cui il database mirror esegue il rollforward. Se mirroring_redo_queue_type è impostato su UNLIMITED (impostazione predefinita), la colonna è NULL. La colonna è NULL anche se il database non è online.<br /><br /> Negli altri casi la colonna contiene la quantità massima del log espressa in MB. Quando viene raggiunta la quantità massima, il log viene sospeso temporaneamente nel server principale mentre il server mirror si aggiorna. Questa funzionalità limita il tempo di failover.<br /><br /> Per altre informazioni, vedere [Stimare l'interruzione del servizio durante il cambio di ruolo &#40;mirroring del database&#41;](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md).|  
|**mirroring_redo_queue_type**|**nvarchar(60)**|UNLIMITED indica che il mirroring non impedisce l'esecuzione della coda rollforward. Si tratta dell'impostazione predefinita.<br /><br /> MB per le dimensioni massime della coda rollforward in megabyte. Se le dimensioni della coda sono state specificate in KB o GB, [!INCLUDE[ssDE](../../includes/ssde-md.md)] converte il valore in MB.<br /><br /> Se il database non è online, la colonna è NULL.|  
|**mirroring_end_of_log_lsn**|**numeric(25,0)**|La fine del log locale è stata scaricata sul disco. Questo è paragonabile all'LSN finalizzato dal server mirror (vedere la colonna **mirroring_failover_lsn** ).|  
|**mirroring_replication_lsn**|**numeric(25,0)**|Il valore LSN massimo che la replica può inviare.|  
  
## <a name="permissions"></a>Autorizzazioni  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Vedere anche  
 [Viste del catalogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.database_mirroring_witnesses &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)   
 [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)   
 [Viste del catalogo di database e file &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)   
 [Domande frequenti sull'esecuzione di query sul catalogo di sistema di SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
