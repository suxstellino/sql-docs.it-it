---
description: FULLTEXTCATALOGPROPERTY (Transact-SQL)
title: FULLTEXTCATALOGPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FULLTEXTCATALOGPROPERTY_TSQL
- FULLTEXTCATALOGPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- full-text catalogs [SQL Server], properties
- FULLTEXTCATALOGPROPERTY function
- status information [SQL Server], full-text catalogs
ms.assetid: f841dc79-2044-4863-aff0-56b8bb61f250
author: cawrites
ms.author: chadam
ms.openlocfilehash: d49757197a66a0af05a0115cf4b16d6d589acc13
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237495"
---
# <a name="fulltextcatalogproperty-transact-sql"></a>FULLTEXTCATALOGPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Restituisce informazioni sulle proprietà del catalogo full-text in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
FULLTEXTCATALOGPROPERTY ('catalog_name' ,'property')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
  
> [!NOTE]  
>  In una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le proprietà seguenti verranno rimosse: **LogSize** e **PopulateStatus**. Evitare di utilizzare queste proprietà in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui vengono utilizzate.  
  
_catalog\_name_  
Espressione che contiene il nome del catalogo full-text.  
  
_property_  
Espressione che contiene il nome della proprietà di catalogo full-text. Nella tabella seguente vengono descritte le proprietà e le informazioni restituite.  
  
|Proprietà|Descrizione|  
|--------------|-----------------|  
|**AccentSensitivity**|Impostazione relativa alla distinzione tra caratteri accentati e non accentati.<br /><br /> 0 = distinzione dei caratteri accentati/non accentati disattivata<br /><br /> 1 = distinzione dei caratteri accentati/non accentati attivata|  
|**IndexSize**|Dimensioni logiche in megabyte del catalogo full-text. Include le dimensioni della frase chiave semantica e degli indici di somiglianza del documento.<br /><br /> Per ulteriori informazioni, vedere la sezione "Note" più avanti in questo argomento.|  
|**ItemCount**|Numero di elementi indicizzati, inclusi tutti gli indici full-text, di frasi chiave e di somiglianza del documento in un catalogo|  
|**LogSize**|Supportato unicamente per compatibilità con le versioni precedenti. Restituisce sempre 0.<br /><br /> Dimensioni in byte del set completo dei log degli errori associati a un catalogo full-text del servizio [!INCLUDE[msCoName](../../includes/msconame-md.md)] Search.|  
|**MergeStatus**|Indica se è in corso un'unione nell'indice master.<br /><br /> 0 = unione nell'indice master non in corso<br /><br /> 1 = unione nell'indice master in corso|  
|**PopulateCompletionAge**|Differenza espressa in secondi tra il completamento dell'ultimo popolamento di indici full-text e la data 01/01/1990 00.00.00.<br /><br /> Aggiornato solo per ricerche per indicizzazione complete o incrementali. Restituisce 0 se non si verifica alcun popolamento.|  
|**PopulateStatus**|0 = inattivo<br /><br /> 1 = popolamento completo in corso<br /><br /> 2 = sospeso<br /><br /> 3 = rallentato<br /><br /> 4 = Recupero in corso<br /><br /> 5 = Chiusura<br /><br /> 6= popolamento incrementale in corso<br /><br /> 7 = compilazione dell'indice in corso<br /><br /> 8 = disco pieno (sospeso)<br /><br /> 9 = rilevamento modifiche|  
|**UniqueKeyCount**|Numero di chiavi univoche nel catalogo full-text.|  
|**ImportStatus**|Indica se il catalogo full-text viene importato.<br /><br /> 0 = Il catalogo full-text non viene importato.<br /><br /> 1 = Il catalogo full-text viene importato.|  
  
## <a name="return-types"></a>Tipi restituiti  
**int**  
  
## <a name="exceptions"></a>Eccezioni  
Restituisce NULL in caso di errore o se un chiamante non ha l'autorizzazione necessaria per visualizzare l'oggetto.  
  
In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] un utente può visualizzare solo i metadati delle entità a protezione diretta di cui è proprietario o per cui ha autorizzazioni. Di conseguenza, le funzioni predefinite di creazione dei metadati come FULLTEXTCATALOGPROPERTY possono restituire NULL se l'utente non ha alcuna autorizzazione per l'oggetto. Per altre informazioni, vedere [sp_help_fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md).  
  
## <a name="remarks"></a>Osservazioni  
FULLTEXTCATALOGPROPERTY ('_catalog\_name_','**IndexSize**') analizza solo i frammenti con stato 4 o 6, come descritto in [sys.fulltext_index_fragments](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md). Tali frammenti fanno parte dell'indice logico. Di conseguenza, la proprietà **IndexSize** restituisce solo le dimensioni dell'indice logico. 

Durante un unione degli indici, tuttavia, le dimensioni effettive dell'indice potrebbero essere doppie rispetto a quelle logiche. Per trovare le dimensioni effettive utilizzate da un indice full-text durante un merge, usare la stored procedure di sistema [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md). Tale procedura analizza tutti i frammenti associati a un indice full-text. 

Il popolamento full-text potrebbe non riuscire se si limita l'aumento delle dimensioni del file di catalogo full-text e non si concede spazio sufficiente per il processo di unione. In questo caso, FULLTEXTCATALOGPROPERTY ('_catalog\_name_','**IndexSize**') restituisce 0 e nel log full-text viene scritto l'errore seguente:  
  
`Error: 30059, Severity: 16, State: 1. A fatal error occurred during a full-text population and caused the population to be cancelled. Population type is: FULL; database name is FTS_Test (id: 13); catalog name is t1_cat (id: 5); table name t1 (id: 2105058535). Fix the errors that are logged in the full-text crawl log. Then, resume the population. The basic Transact-SQL syntax for this is: ALTER FULLTEXT INDEX ON table_name RESUME POPULATION.`  
  
È importante che le applicazioni non attendano in un ciclo limitato che la proprietà **PopulateStatus** diventi inattiva. Il passaggio allo stato inattivo indica che il popolamento è stato completato. Questo controllo sottrae cicli della CPU dal database e dai processi di ricerca full-text e provoca timeout. È in genere consigliabile controllare la proprietà **PopulateStatus** corrispondente a livello di tabella, **TableFullTextPopulateStatus** nella funzione di sistema OBJECTPROPERTYEX. Questa e le altre nuove proprietà full-text disponibili per la funzione OBJECTPROPERTYEX forniscono informazioni sulla granularità relative alle tabelle di indicizzazione full-text. Per altre informazioni, vedere [OBJECTPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/objectpropertyex-transact-sql.md).  
  
## <a name="examples"></a>Esempi  
Nell'esempio seguente viene restituito il numero di voci indicizzate full-text del catalogo full-text `Cat_Desc`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT fulltextcatalogproperty('Cat_Desc', 'ItemCount');  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
[FULLTEXTSERVICEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)   
[Funzioni per i metadati &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
[sp_help_fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)  
  
  
