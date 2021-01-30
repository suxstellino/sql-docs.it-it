---
description: sp_serveroption (Transact-SQL)
title: sp_serveroption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/11/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_serveroption_TSQL
- sp_serveroption
dev_langs:
- TSQL
helpviewer_keywords:
- 7343 (Database Engine error)
- sp_serveroption
ms.assetid: 47d04a2b-dbf0-4f15-bd9b-81a2efc48131
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ec6dfda1d4e12b190999b82e16c27b5127d5af6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190774"
---
# <a name="sp_serveroption-transact-sql"></a>sp_serveroption (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Imposta le opzioni per server remoti e server collegati.  
  
 
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sp_serveroption [@server = ] 'server'   
      ,[@optname = ] 'option_name'       
      ,[@optvalue = ] 'option_value' ;  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @server = ] 'server'` Nome del server per il quale impostare l'opzione. *server* è di tipo **sysname** e non prevede alcun valore predefinito.  
  
`[ @optname = ] 'option_name'` Opzione da impostare per il server specificato. *option_name* è di tipo **varchar (** 35 **)** e non prevede alcun valore predefinito. *option_name* può essere uno dei valori seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**regole di confronto compatibili**|Influisce sull'esecuzione delle query distribuite in server collegati. Se questa opzione è impostata su **true**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] presuppone che tutti i caratteri del server collegato siano compatibili con il server locale, relativamente al set di caratteri e alla sequenza delle regole di confronto (o ordinamento). Ciò consente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di inviare al provider i confronti per colonne di tipo carattere. Se questa opzione non è impostata, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valuta sempre i confronti per le colonne di tipo carattere localmente.<br /><br /> Impostare questa opzione solo se nell'origine dei dati corrispondente al server collegato il set di caratteri e il tipo di ordinamento corrispondono a quelli del server locale.|  
|**nome delle regole di confronto**|Specifica il nome delle regole di confronto utilizzate dall'origine dati remota se **Usa regole di confronto remote** è **true** e l'origine dati non è un' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] origine dati. È necessario specificare il nome di uno dei set di regole di confronto supportate da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Utilizzare questa opzione per accedere a un'origine dei dati OLE DB diversa da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]che utilizza regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> Il server collegato deve supportare regole di confronto singole da utilizzare per tutte le colonne del server. Non impostare questa opzione quando il server collegato supporta più regole di confronto nella stessa origine dei dati oppure non è possibile stabilire se le regole di confronto del server collegato corrispondono a una delle regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**timeout connessione**|Valore di timeout in secondi per la connessione a un server collegato.<br /><br /> Se è **0**, usare il valore predefinito **sp_configure** .|  
|**accesso ai dati**|Consente di attivare e disabilitare un server collegato per l'accesso a query distribuite. Può essere utilizzato solo per le voci **sys. Server** aggiunte tramite **sp_addlinkedserver**.|  
|**dist**|Server di distribuzione.|  
|**convalida differita dello schema**|Determina se lo schema delle tabelle remote viene controllato.<br /><br /> Se **true**, ignora il controllo dello schema delle tabelle remote all'inizio della query.|  
|**pub**|Server di pubblicazione.|  
|**timeout query**|Timeout per le query eseguite nel server collegato.<br /><br /> Se è **0**, usare il valore predefinito **sp_configure** .|  
|**RPC**|Abilita l'esecuzione di chiamate RPC dal server specificato.|  
|**RPC in uscita**|Abilita l'esecuzione di chiamate RPC al server specificato.|  
|**sub**|Sottoscrittore.|  
|**sistema**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**Usa regole di confronto remote**|Determina se vengono utilizzate le regole di confronto di una colonna remota o di un server locale.<br /><br /> Se **true**, le regole di confronto delle colonne remote vengono utilizzate per le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] origini dati e le regole di confronto specificate nel **nome delle regole di confronto** vengono utilizzate per le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] origini dati non.<br /><br /> Se **false**, le query distribuite utilizzeranno sempre le regole di confronto predefinite del server locale, mentre il **nome delle regole** di confronto e le regole di confronto delle colonne remote vengono ignorati. Il valore predefinito è **false**. Il valore **false** è compatibile con la semantica delle regole di confronto utilizzata in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7,0.|  
|**remote proc transaction promotion**|Questa opzione consente di proteggere le azioni di una procedura da server a server tramite una transazione MS DTC ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator). Quando questa opzione è impostata su TRUE (o ON), la chiamata di una stored procedure remota comporta l'avvio di una transazione distribuita e l'integrazione della transazione in MS DTC. L'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui viene chiamata la stored procedure remota corrisponde all'origine della transazione e ne controlla il completamento. Quando per la connessione viene successivamente eseguita un'istruzione COMMIT TRANSACTION o ROLLBACK TRANSACTION, l'istanza di controllo richiede che il completamento della transazione distribuita nei computer interessati venga gestito da MS DTC.<br /><br /> Dopo l'avvio di una transazione distribuita [!INCLUDE[tsql](../../includes/tsql-md.md)], è possibile chiamare stored procedure remote in altre istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definite come server collegati. Tutti i server collegati sono integrati nella transazione distribuita [!INCLUDE[tsql](../../includes/tsql-md.md)]. MS DTC garantisce inoltre che la transazione venga completata in ogni server collegato.<br /><br /> Se questa opzione è impostata su FALSE (o OFF), una transazione locale non sarà promossa a transazione distribuita durante una chiamata di procedura remota in un server collegato.<br /><br /> Se prima di effettuare una chiamata di una procedura da server a server la transazione è già una transazione distribuita, questa opzione non ha alcun effetto. La chiamata di procedura nel server collegato verrà eseguita nella stessa transazione distribuita.<br /><br /> Se prima di effettuare una chiamata di una procedura da server a server non vi sono transazioni attive nella connessione, questa opzione non ha alcun effetto. La procedura viene quindi eseguita nel server collegato senza transazioni attive.<br /><br /> Il valore predefinito per questa opzione è TRUE (o ON).|  
  
`[ @optvalue = ] 'option_value'` Specifica se il *option_name* deve essere abilitato (**true** o **on**) o disabilitato (**false** o **off**). *option_value* è di tipo **varchar (** 10 **)** e non prevede alcun valore predefinito.  
  
 *option_value* può essere un numero intero non negativo per le opzioni **timeout connessione** e **timeout query** . Per l'opzione **nome delle regole di confronto** , *option_value* può essere un nome di regole di confronto o null.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="remarks"></a>Commenti  
 Se l'opzione relativa alla **compatibilità delle regole di confronto** è impostata su true, il nome delle regole di **confronto** verrà impostato automaticamente su null. Se il **nome delle regole di confronto** è impostato su un valore non null, le regole di **confronto compatibili** automaticamente verranno impostate su false.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'autorizzazione ALTER ANY LINKED SERVER per il server.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene configurata la compatibilità delle regole di confronto tra un server collegato corrispondente a un'altra istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], `SEATTLE3`, e l'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
USE master;  
EXEC sp_serveroption 'SEATTLE3', 'collation compatible', 'true';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure per query distribuite &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_adddistpublisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_dropdistpublisher &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
