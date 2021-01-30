---
description: sp_helpconstraint (Transact-SQL)
title: sp_helpconstraint (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpconstraint
- sp_helpconstraint_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpconstraint
ms.assetid: 29d6cd36-535d-4765-bca8-62f9d9886ff5
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dddf290bd1b22f0ec25f9ea03dd2e9766e184308
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176453"
---
# <a name="sp_helpconstraint-transact-sql"></a>sp_helpconstraint (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Restituisce un elenco di tutti i tipi di vincoli, con i relativi nomi definiti dall'utente o dal sistema, le colonne in cui sono stati definiti e l'espressione che li definisce (solo per i vincoli DEFAULT e CHECK).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_helpconstraint [ @objname = ] 'table'   
     [ , [ @nomsg = ] 'no_message' ]   
```  
  
## <a name="arguments"></a>Argomenti  
`[ @objname = ] 'table'` Tabella in cui vengono restituite le informazioni sui vincoli. La tabella specificata deve essere locale rispetto al database corrente. *Table* è di **tipo nvarchar (776)** e non prevede alcun valore predefinito.  
  
`[ @nomsg = ] 'no_message'` È un parametro facoltativo che stampa il nome della tabella. *no_message* è di tipo **varchar (5)** e il valore predefinito è **msg**. **nomsg Annulla** disattiva la stampa.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 **sp_helpconstraint** Visualizza una colonna indicizzata in ordine decrescente se ha partecipato alle chiavi primarie. Nel set di risultati il nome di tali colonne viene seguito da un segno meno (-). Nel caso di colonne indicizzate in ordine crescente, come per impostazione predefinita, viene invece visualizzato solo il nome delle colonne.  
  
## <a name="remarks"></a>Commenti  
 L'esecuzione di **sp_help**_tabella_ riporta tutte le informazioni relative alla tabella specificata. Per visualizzare solo le informazioni sui vincoli, utilizzare **sp_helpconstraint**.  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo **public** .  
  
## <a name="examples"></a>Esempi  
 Nell'esempio seguente vengono illustrati tutti i vincoli per la tabella `Product`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_helpconstraint 'Production.Product';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Stored procedure di motore di database &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.key_constraints &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-key-constraints-transact-sql.md)   
 [sys.check_constraints &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md)   
 [sys.default_constraints &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-default-constraints-transact-sql.md)  
  
  
