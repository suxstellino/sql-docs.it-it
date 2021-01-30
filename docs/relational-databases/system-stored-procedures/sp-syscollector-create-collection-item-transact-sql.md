---
description: sp_syscollector_create_collection_item (Transact-SQL)
title: sp_syscollector_create_collection_item (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_create_collection_item
- sp_syscollector_create_collection_item_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_create_collection_item
- data collector [SQL Server], stored procedures
ms.assetid: 60dacf13-ca12-4844-b417-0bc0a8bf0ddb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e58f8407c152be375099e9eb70e3fe63d0100e67
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207042"
---
# <a name="sp_syscollector_create_collection_item-transact-sql"></a>sp_syscollector_create_collection_item (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un elemento della raccolta in un set di raccolta definito dall'utente. Un elemento della raccolta definisce i dati da raccogliere e la frequenza di raccolta.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_syscollector_create_collection_item   
      [ @collection_set_id = ] collection_set_id   
    , [ @collector_type_uid = ] 'collector_type_uid'  
    , [ @name = ] 'name'   
    , [ [ @frequency = ] frequency ]  
    , [ @parameters = ] 'parameters'  
    , [ @collection_item_id = ] collection_item_id OUTPUT  
```  
  
## <a name="arguments"></a>Argomenti  
 [ @collection_set_id =] *collection_set_id*  
 Identificatore univoco locale del set di raccolta. *collection_set_id* è di **tipo int**.  
  
 [ @collector_type_uid =]'*collector_type_uid*'  
 GUID che identifica il tipo di agente di raccolta da utilizzare per questo elemento *collector_type_uid* è di tipo **uniqueidentifier** e non prevede alcun valore predefinito. Per un elenco di tipi di agenti di raccolta, eseguire una query sulla vista di sistema syscollector_collector_types.  
  
 [ @name =]'*nome*'  
 Nome dell'elemento della raccolta. *Name* è di **tipo sysname** e non può essere una stringa vuota o null.  
  
 il *nome* deve essere univoco. Per un elenco dei nomi degli elementi della raccolta correnti, eseguire una query sulla vista di sistema syscollector_collection_items.  
  
 [ @frequency =] *frequenza*  
 Utilizzato per specificare la frequenza di raccolta dei dati da parte di questo elemento della raccolta, espressa in secondi. *Frequency* è di **tipo int** e il valore predefinito è 5. Il valore minimo che è possibile specificare è 5 secondi.  
  
 Se il set di raccolta è impostato sulla modalità non in cache, la frequenza viene ignorata in quanto in questa modalità la raccolta e il caricamento dei dati vengono eseguiti in base alla pianificazione specificata per il set di raccolta. Per visualizzare la modalità di raccolta del set di raccolta, eseguire una query sulla vista di sistema [syscollector_collection_sets](../../relational-databases/system-catalog-views/syscollector-collection-sets-transact-sql.md) .  
  
 [ @parameters =]'*Parameters*'  
 Parametri di input per il tipo agente di raccolta. *Parameters* è di **XML** e il valore predefinito è null. Lo schema dei *parametri* deve corrispondere allo schema dei parametri del tipo di agente di raccolta.  
  
 [ @collection_item_id =] *collection_item_id*  
 Identificatore univoco che identifica l'elemento del set di raccolta. *collection_item_id* è di **tipo int** e ha output.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 **0** (esito positivo) o **1** (esito negativo)  
  
## <a name="remarks"></a>Commenti  
 È necessario eseguire sp_syscollector_create_collection_item nel contesto del database di sistema msdb.  
  
 Il set di raccolta al quale viene aggiunto l'elemento deve essere arrestato prima della creazione dell'elemento della raccolta. Non è possibile aggiungere elementi della raccolta ai set di raccolta di sistema.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per eseguire questa procedura, è richiesta l'appartenenza al ruolo predefinito del database dc_admin (con autorizzazione EXECUTE) .  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente viene creato un elemento della raccolta basato sul tipo di raccolta `Generic T-SQL Query Collector Type` e successivamente tale elemento viene aggiunto al set di raccolta denominato `Simple collection set test 2`. Per creare il set di raccolta specificato, eseguire l'esempio B in [sp_syscollector_create_collection_set &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md).  
  
```  
USE msdb;  
GO  
DECLARE @collection_item_id int;  
DECLARE @collection_set_id int = (SELECT collection_set_id   
                                  FROM syscollector_collection_sets  
                                  WHERE name = N'Simple collection set test 2');  
DECLARE @collector_type_uid uniqueidentifier =   
    (SELECT collector_type_uid  
     FROM syscollector_collector_types  
     WHERE name = N'Generic T-SQL Query Collector Type');  
DECLARE @params xml =   
    CONVERT(xml, N'\<ns:TSQLQueryCollector xmlns:ns="DataCollectorType">  
            <Query>  
                <Value>SELECT * FROM sys.objects</Value>  
                <OutputTable>MyOutputTable</OutputTable>  
            </Query>  
            <Databases>   
                <Database> UseSystemDatabases = "true"   
                           UseUserDatabases = "true"  
                </Database>  
            </Databases>  
         \</ns:TSQLQueryCollector>');  
  
EXEC sp_syscollector_create_collection_item  
    @collection_set_id = @collection_set_id,  
    @collector_type_uid = @collector_type_uid,  
    @name = 'My custom TSQL query collector item',  
    @frequency = 6000,  
    @parameters = @params,  
    @collection_item_id = @collection_item_id OUTPUT;  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Raccolta dati](../../relational-databases/data-collection/data-collection.md)   
 [sp_syscollector_update_collection_item &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syscollector-update-collection-item-transact-sql.md)   
 [sp_syscollector_delete_collection_item &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syscollector-delete-collection-item-transact-sql.md)   
 [syscollector_collector_types &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/syscollector-collector-types-transact-sql.md)   
 [sp_syscollector_create_collection_set &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md)   
 [syscollector_collection_items &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collection-items-transact-sql.md)  
  
  
