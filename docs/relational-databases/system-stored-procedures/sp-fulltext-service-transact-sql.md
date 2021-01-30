---
description: sp_fulltext_service (Transact-SQL)
title: sp_fulltext_service (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_service
- sp_fulltext_service_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], properties
- sp_fulltext_service
- Full-Text Search Upgrade Option
ms.assetid: 17a91433-f9b6-4a40-88c4-8c704ec2de9f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6fbbbe3451994687542f755b4d48953c7dbbe2e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187326"
---
# <a name="sp_fulltext_service-transact-sql"></a>sp_fulltext_service (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica le proprietà del server di ricerca full-text per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_fulltext_service [ [@action=] 'action'   
     [ , [ @value= ] value ] ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @action = ] 'action'` Proprietà da modificare o reimpostare. *Action* è di **tipo nvarchar (100) e** non prevede alcun valore predefinito. Per un elenco di proprietà di *c*, le relative descrizioni e i valori che è possibile impostare, vedere la tabella sotto l'argomento *value* . Questo argomento restituisce le proprietà seguenti: tipo di dati, valore corrente, valore minimo o massimo e valore che indica se l'oggetto è deprecato, se pertinente.  
  
`[ @value = ] value` Valore della proprietà specificata. il *valore* è **sql_variant** e il valore predefinito è null. Se @value è null, **sp_fulltext_service** restituisce l'impostazione corrente. In questa tabella sono elencate le proprietà, le descrizioni e i valori che è possibile impostare.  
  
> [!NOTE]  
>  Le azioni seguenti verranno rimosse in una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **clean_up**, **connect_timeout**, **data_timeout** e **resource_usage**. Evitare di utilizzare queste azioni in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui attualmente vengono utilizzate.  
  
|Azione|Tipo di dati|Descrizione|  
|------------|---------------|-----------------|  
|**clean_up**|**int**|Supportato unicamente per compatibilità con le versioni precedenti. Il valore corrisponde sempre a 0.|  
|**connect_timeout**|**int**|Supportato unicamente per compatibilità con le versioni precedenti. Il valore corrisponde sempre a 0.|  
|**data_timeout**|**int**|Supportato unicamente per compatibilità con le versioni precedenti. Il valore corrisponde sempre a 0.|  
|**load_os_resources**|**int**|Indica se i word breaker, gli stemmer e i filtri del sistema operativo vengono registrati e utilizzati con questa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Uno dei valori possibili:<br /><br /> 0 = Utilizza solo i filtri e i word breaker specifici per questa istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> 1 = Vengono caricati i filtri e i word breaker del sistema operativo.<br /><br /> Per impostazione predefinita, questa proprietà è disabilitata per evitare modifiche accidentali del sistema operativo. L'abilitazione dell'utilizzo delle risorse del sistema operativo consente l'accesso alle risorse per le lingue e i tipi di documenti registrati nel servizio di indicizzazione [!INCLUDE[msCoName](../../includes/msconame-md.md)] per cui non è installata una risorsa specifica dell'istanza. Se si Abilita il caricamento delle risorse del sistema operativo, verificare che le risorse del sistema operativo siano binari firmati attendibili; in caso contrario, non possono essere caricati quando **verify_signature** (vedere di seguito) è impostato su 1.|  
|**master_merge_dop**|**int**|Specifica il numero di thread che deve essere utilizzato dal processo di unione nell'indice master. Questo valore non deve superare il numero di CPU o di core della CPU disponibili.<br /><br /> Quando questo argomento non viene specificato, il servizio utilizza il minore di 4 o il numero di CPU o di core della CPU disponibili.|  
|**pause_indexing**|**int**|Specifica se l'indicizzazione full-text deve essere sospesa, se è attualmente in esecuzione, o ripresa, se è attualmente sospesa.<br /><br /> 0 = Riprende le attività di indicizzazione full-text per l'istanza del server.<br /><br /> 1 = Sospende le attività di indicizzazione full-text per l'istanza del server.|  
|**resource_usage**|**int**|Non ha alcuna funzione in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] e versioni successive e viene ignorata.|  
|**update_languages**|NULL|Aggiorna l'elenco di lingue e filtra quelle registrate per la ricerca full-text. Le lingue vengono specificate quando si configura l'indicizzazione e nelle query full-text. I filtri vengono utilizzati dall'host del daemon di filtri per estrarre informazioni testuali dai formati di file corrispondenti, ad esempio docx archiviati in tipi di dati, ad esempio **varbinary**, **varbinary (max)**, **Image** o **XML**, per l'indicizzazione full-text.<br /><br /> Per altre informazioni, vedere [Visualizzazione o modifica di word breaker e filtri registrati](../../relational-databases/search/view-or-change-registered-filters-and-word-breakers.md).|  
|**upgrade_option**|**int**|Controlla il modo in cui viene eseguita la migrazione di indici full-text durante l'aggiornamento di un database da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] a una versione successiva. Questa proprietà si applica ai casi in cui viene eseguito l'aggiornamento tramite il collegamento di un database, il ripristino di un backup di database o di un backup di file oppure la copia del database tramite la Copia guidata database.<br /><br /> Uno dei valori possibili:<br /><br /> 0 = I cataloghi full-text vengono ricompilati utilizzando i nuovi word breaker migliorati. La ricompilazione degli indici può richiedere tempo e dopo l'aggiornamento potrebbe essere necessaria una quantità significativa di CPU e di memoria.<br /><br /> 1= I cataloghi full-text vengono reimpostati. [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] I file del catalogo full-text vengono rimossi, ma i metadati per i cataloghi e per gli indici full-text vengono mantenuti. Dopo l'aggiornamento, in tutti gli indici full-text il rilevamento delle modifiche viene disabilitato e le ricerche per indicizzazione non vengono avviate automaticamente. Il catalogo resterà vuoto fino a quando non si eseguirà manualmente un popolamento completo al termine dell'aggiornamento.<br /><br /> 2 = I cataloghi full-text vengono importati. In genere, l'importazione è molto più veloce della ricompilazione. Se ad esempio si utilizza solo una CPU, l'importazione è di circa 10 volte più veloce della ricompilazione. Tuttavia, un catalogo full-text importato non utilizza i word breaker nuovi e migliorati, pertanto potrebbe essere necessario ricompilare i cataloghi full-text.<br /><br /> Nota: la ricompilazione può essere eseguita in modalità a thread multipli e, se sono disponibili più di 10 CPU, la ricompilazione potrebbe essere eseguita più velocemente dell'importazione se si consente a ricompila di usare tutte le CPU.<br /><br /> Se un catalogo full-text non è disponibile, gli indici full-text associati vengono ricreati. Questa opzione è disponibile solo per i database di [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .<br /><br /> Per informazioni sulla scelta dell'opzione di aggiornamento full-text, vedere[Aggiornare la ricerca full-text](../../relational-databases/search/upgrade-full-text-search.md).<br /><br /> Nota: per impostare questa proprietà in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , utilizzare la proprietà **opzione di aggiornamento full-text** . Per altre informazioni, vedere [Gestione e monitoraggio della ricerca full-text per un'istanza del server](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md).|  
|**verify_signature**|**int**|Indica se solo i file binari firmati vengono caricati dal motore di ricerca full-text. Per impostazione predefinita vengono caricati solo i file binari firmati attendibili.<br /><br /> 1 = Verifica che vengano caricati solo i file binari firmati trusted (impostazione predefinita).<br /><br /> 0 = Non verifica se i file binari sono firmati.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **serveradmin** o l'amministratore di sistema possono eseguire **sp_fulltext_service**.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-updating-the-list-of-registered-languages"></a>R. Aggiornamento dell'elenco di lingue registrate  
 Nell'esempio seguente viene aggiornato l'elenco di lingue registrate per la ricerca full-text.  
  
```  
EXEC sp_fulltext_service 'update_languages';  
GO  
```  
  
### <a name="b-changing-the-full-text-upgrade-option-to-reset-full-text-catalogs"></a>B. Modifica dell'opzione di aggiornamento full-text per reimpostare i cataloghi full-text  
 Nell'esempio seguente viene modificata l'opzione di aggiornamento full-text per reimpostare i cataloghi full-text. I cataloghi vengono rimossi completamente. Nell'esempio vengono specificate le parole chiave facoltative `@action` e `@value`.  
  
```  
EXEC sp_fulltext_service @action='upgrade_option', @value=1;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Ricerca full-text](../../relational-databases/search/full-text-search.md)   
 [FULLTEXTSERVICEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
