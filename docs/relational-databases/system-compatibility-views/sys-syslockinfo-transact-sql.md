---
description: sys.syslockinfo (Transact-SQL)
title: sys.syslockinfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- syslockinfo_TSQL
- sys.syslockinfo_TSQL
- sys.syslockinfo
- syslockinfo
dev_langs:
- TSQL
helpviewer_keywords:
- syslockinfo system table
- sys.syslockinfo compatibility view
ms.assetid: d8cae434-807a-473e-b94f-f7a0e1b2daf0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: d552fdbb75c4aa1cb786025a8d1c866b89135e12
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099075"
---
# <a name="syssyslockinfo-transact-sql"></a>sys.syslockinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene informazioni su tutte le richieste di blocco concesse, in fase di conversione e in attesa.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
> [!IMPORTANT]  
>  Questa funzionalità è stata modificata rispetto alle versioni precedenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per ulteriori informazioni, vedere [modifiche di rilievo apportate alle funzionalità di motore di database in SQL Server 2016](../../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md).  
  
|Nome colonna|Tipo di dati|Descrizione|  
|-----------------|---------------|-----------------|  
|**rsc_text**|**nchar(32)**|Descrizione in formato testo di una risorsa di blocco. Include una parte del nome della risorsa.|  
|**rsc_bin**|**binary(16)**|Risorsa di blocco binaria. Include l'effettiva risorsa di blocco presente in Gestione blocchi. Questa colonna è inclusa per gli strumenti che conoscono il formato della risorsa di blocco per la generazione di una risorsa di blocco formattata e per l'esecuzione di self join in **syslockinfo**.|  
|**rsc_valblk**|**binary(16)**|Gruppo di valori del blocco. Alcuni tipi di risorsa possono includere dati aggiuntivi nella risorsa di blocco per cui non viene eseguito l'hashing da parte di Gestione blocchi al fine di determinare la proprietà di una determinata risorsa di blocco. Ad esempio, i blocchi a livello di pagina non appartengono a un ID di oggetto particolare. Per fini di escalation dei blocchi o altri fini. tuttavia, è possibile inserire nel gruppo di valori del blocco l'ID di oggetto di un blocco a livello di pagina.|  
|**rsc_dbid**|**smallint**|ID di database associato alla risorsa.|  
|**rsc_indid**|**smallint**|ID di indice associato alla risorsa, se appropriato.|  
|**rsc_objid**|**int**|ID di oggetto associato alla risorsa, se appropriato.|  
|**rsc_type**|**tinyint**|Tipo di risorsa:<br /><br /> 1 = Risorsa NULL (non utilizzata)<br /><br /> 2 = database<br /><br /> 3 = File<br /><br /> 4 = Indice<br /><br /> 5 = Tabella<br /><br /> 6 = Pagina<br /><br /> 7 = Chiave<br /><br /> 8 = Extent<br /><br /> 9 = RID (ID di riga)<br /><br /> 10 = Applicazione|  
|**rsc_flag**|**tinyint**|Flag delle risorse interne.|  
|**req_mode**|**tinyint**|Modalità di richiesta di blocco. Questa colonna indica la modalità di blocco del richiedente e rappresenta la modalità concessa, in fase di conversione o in attesa.<br /><br /> 0 = NULL. Non è consentito l'accesso alla risorsa. Funge da segnaposto.<br /><br /> 1 = Sch-S (stabilità schema). Impedisce che un elemento dello schema, ad esempio una tabella o un indice, venga eliminato mentre in una sessione viene mantenuto attivo un blocco di stabilità dello schema su tale elemento.<br /><br /> 2 = Sch-M (modifica schema). Deve essere impostato in tutte le sessioni in cui si desidera modificare lo schema della risorsa specificata. Assicura che nessun'altra sessione faccia riferimento all'oggetto specificato.<br /><br /> 3 = S (condiviso). La sessione attiva dispone dell'accesso condiviso alla risorsa.<br /><br /> 4 = U (aggiornamento). Indica un blocco di aggiornamento acquisito su risorse che potrebbero essere aggiornate. Viene utilizzato per evitare una forma comune di deadlock che si verifica quando in più sessioni vengono bloccate risorse che potrebbero essere aggiornate in futuro.<br /><br /> 5 = X (esclusivo). La sessione attiva dispone dell'accesso esclusivo alla risorsa.<br /><br /> 6 = IS (preventivo condiviso). Indica l'intenzione di impostare blocchi condivisi (S) su alcune risorse subordinate nella gerarchia dei blocchi.<br /><br /> 7 = IU (preventivo aggiornamento). Indica l'intenzione di impostare blocchi di aggiornamento (U) su alcune risorse subordinate nella gerarchia dei blocchi.<br /><br /> 8 = IX (preventivo esclusivo). Indica l'intenzione di impostare blocchi esclusivi (X) su alcune risorse subordinate nella gerarchia dei blocchi.<br /><br /> 9 = SIU (condiviso preventivo aggiornamento). Indica l'accesso condiviso a una risorsa con l'intenzione di acquisire blocchi di aggiornamento su risorse subordinate nella gerarchia dei blocchi.<br /><br /> 10 = SIX (condiviso preventivo esclusivo). Indica l'accesso condiviso a una risorsa con l'intenzione di acquisire blocchi esclusivi su risorse subordinate nella gerarchia dei blocchi.<br /><br /> 11 = UIX (aggiornamento preventivo esclusivo). Indica un blocco di aggiornamento attivato su una risorsa con l'intenzione di acquisire blocchi esclusivi su risorse subordinate nella gerarchia dei blocchi.<br /><br /> 12 = BU. Utilizzato dalle operazioni bulk.<br /><br /> 13 = RangeS_S (blocco condiviso intervalli di chiavi e risorsa). Indica un'analisi di intervallo serializzabile.<br /><br /> 14 = RangeS_U (blocco condiviso intervalli di chiavi e aggiornamento risorsa). Indica un'analisi di aggiornamento serializzabile.<br /><br /> 15 = RangeI_N (blocco inserimento intervalli di chiavi e risorsa Null). Utilizzato per verificare gli intervalli prima di inserire una nuova chiave in un indice.<br /><br /> 16 = RangeI_S. Blocco conversione intervalli di chiavi, creato da una sovrapposizione di blocchi RangeI_N e S.<br /><br /> 17 = RangeI_U. Blocco conversione intervalli di chiavi, creato da una sovrapposizione di blocchi RangeI_N e U.<br /><br /> 18 = RangeI_X. Blocco conversione intervalli di chiavi, creato da una sovrapposizione di blocchi RangeI_N e X.<br /><br /> 19 = RangeX_S. Blocco conversione intervalli di chiavi, creato da una sovrapposizione di blocchi RangeI_N e RangeS_S.<br /><br /> 20 = RangeX_U. Blocco conversione intervalli di chiavi, creato da una sovrapposizione di blocchi RangeI_N e RangeS_U.<br /><br /> 21 = RangeX_X (blocco esclusivo intervalli di chiavi e risorsa). Si tratta di un blocco di conversione utilizzato quando viene aggiornata una chiave in un intervallo.|  
|**req_status**|**tinyint**|Stato della richiesta di blocco:<br /><br /> 1 = Concessa<br /><br /> 2 = In fase di conversione<br /><br /> 3 = In attesa|  
|**req_refcnt**|**smallint**|Numero dei riferimenti di blocco. Ogni volta che una transazione richiede un blocco su una determinata risorsa, il numero dei riferimenti viene incrementato. Il blocco può essere rilasciato solo quando il numero dei riferimenti è uguale a 0.|  
|**req_cryrefcnt**|**smallint**|Riservata per utilizzi futuri. È sempre impostata su 0.|  
|**req_lifetime**|**int**|Mappa di bit del ciclo di vita dei blocchi. Durante determinati processi di elaborazione delle query, è necessario mantenere i blocchi sulle risorse fino a quando in Query Processor non è stata completata una particolare fase della query. La mappa di bit del ciclo di vita dei blocchi viene utilizzata da Query Processor e dallo strumento di gestione delle transazioni per indicare i gruppi di blocchi che è possibile rilasciare quando una determinata fase di una query viene completata. Alcuni bit della mappa vengono utilizzati per indicare i blocchi che vengono mantenuti attivi fino al termine di una transazione, anche se il relativo numero di riferimenti è uguale a 0.|  
|**req_spid**|**int**|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ID processo interno della sessione che richiede il blocco.|  
|**req_ecid**|**int**|ID del contesto di esecuzione (ECID). Viene utilizzato per indicare il thread di un'operazione parallela che è il proprietario di un determinato blocco.|  
|**req_ownertype**|**smallint**|Tipo di oggetto associato al blocco:<br /><br /> 1 = Transazione<br /><br /> 2 = Cursore<br /><br /> 3 = Sessione<br /><br /> 4 = Sessione esclusiva<br /><br /> I tipi di oggetto 3 e 4 rappresentano versioni particolari di blocchi di sessione che consentono di tenere traccia rispettivamente di blocchi a livello di database e di filegroup.|  
|**req_transactionID**|**bigint**|ID di transazione univoco usato in **syslockinfo** e nell'evento del profiler|  
|**req_transactionUOW**|**uniqueidentifier**|Identifica l'ID di unità di lavoro (UOW, Unit Of Work) della transazione DTC. Per transazioni non MS DTC, l'unità UOW è impostata su 0.|  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione VIEW SERVER STATE per il server.  
  
## <a name="see-also"></a>Vedere anche  
 [Mapping di tabelle di sistema a viste di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Viste della compatibilità &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
