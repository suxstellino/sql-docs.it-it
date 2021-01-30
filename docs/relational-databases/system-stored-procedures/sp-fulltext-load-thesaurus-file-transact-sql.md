---
description: sp_fulltext_load_thesaurus_file (Transact-SQL)
title: sp_fulltext_load_thesaurus_file (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_load_thesaurus_file
- sp_fulltext_load_thesaurus_file_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_load_thesaurus_file
- full-text indexes [SQL Server], thesaurus files
- thesaurus [full-text search], editing
ms.assetid: 73a309c3-6d22-42dc-a6fe-8a63747aa2e4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 129bc6385628ef44d5103ddbac5f8ee7f51889ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185411"
---
# <a name="sp_fulltext_load_thesaurus_file-transact-sql"></a>sp_fulltext_load_thesaurus_file (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fa in modo che l'istanza del server analizzi e carichi i dati dal file del thesaurus che corrisponde alla lingua per cui è specificato l'identificatore LCID. Questa stored procedure risulta utile dopo avere eseguito l'aggiornamento di un file del thesaurus. L'esecuzione di **sp_fulltext_load_thesaurus_file** comporta la ricompilazione di query full-text che utilizzano il thesaurus dell'LCID specificato.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
sys.sp_fulltext_load_thesaurus_file lcid [ , @loadOnlyIfNotLoaded  = action ]   
```  
  
## <a name="arguments"></a>Argomenti  
 *lcid*  
 Valore intero che esegue il mapping dell'identificatore delle impostazioni locali (LCID) della lingua per cui si desidera caricare la definizione XML del thesaurus. Per ottenere gli LCID delle lingue disponibili in un'istanza del server, utilizzare la sys.fulltext_languages &#40;vista del catalogo [&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md) .  
  
 **\@**  =  *azione* loadOnlyIfNotLoaded  
 Specifica se il file del thesaurus viene caricato nelle tabelle interne del thesaurus anche se è già stato caricato. *Action* è uno dei seguenti:  
  
|valore|Definizione|  
|-----------|----------------|  
|**0**|Il file del thesaurus viene caricato indipendentemente dal fatto che sia già caricato. Questo è il comportamento predefinito di **sp_fulltext_load_thesaurus_file**.|  
|1|Il file del thesaurus viene caricato solo se non è ancora caricato.|  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 Nessuno  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 I file del thesaurus vengono caricati automaticamente da query full-text che utilizzano il thesaurus. Per evitare questo primo influsso sulle prestazioni delle query full-text, è consigliabile eseguire **sp_fulltext_load_thesaurus_file**.  
  
 Utilizzare [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)'**update_languages**' per aggiornare l'elenco di lingue registrate con la ricerca full-text.  
  
## <a name="permissions"></a>Autorizzazioni  
 Solo i membri del ruolo predefinito del server **sysadmin** o dell'amministratore di sistema possono eseguire il **sp_fulltext_load_thesaurus_file** stored procedure.  
  
 Solo gli amministratori di sistema possono aggiornare, modificare o eliminare i file del thesaurus.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-load-a-thesaurus-file-even-if-it-is-already-loaded"></a>R. Caricamento di un file del thesaurus anche se è già caricato  
 Nell'esempio seguente viene analizzato e caricato il file del thesaurus inglese.  
  
```sql
EXEC sys.sp_fulltext_load_thesaurus_file 1033;
```  
  
### <a name="b-load-a-thesaurus-file-only-if-it-is-not-yet-loaded"></a>B. Caricamento di un file del thesaurus solo se non è ancora caricato  
 Nell'esempio seguente viene analizzato e caricato il file del thesaurus arabo, a meno che non sia già caricato.  
  
```sql
EXEC sys.sp_fulltext_load_thesaurus_file 1025, @loadOnlyIfNotLoaded = 1;
```  

## <a name="see-also"></a>Vedere anche

[FULLTEXTSERVICEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)  
[Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
[Configurare e gestire i file del thesaurus per la ricerca full-text](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md)
