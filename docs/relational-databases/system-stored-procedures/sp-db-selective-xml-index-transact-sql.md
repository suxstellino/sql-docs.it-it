---
description: sp_db_selective_xml_index (Transact-SQL)
title: sp_db_selective_xml_index (Transact-SQL)
ms.custom: ''
ms.date: 02/11/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_db_selective_xml_index_TSQL
- sp_db_selective_xml_index
dev_langs:
- TSQL
helpviewer_keywords:
- sp_db_selective_xml_index procedure
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5df370a674026b4c6ebc7eb59985505821e3b028
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489445"
---
# <a name="sp_db_selective_xml_index-transact-sql"></a>sp_db_selective_xml_index (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Abilita e disabilita la funzionalità degli indici XML selettivi in un database [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se chiamata senza parametri, la stored procedure restituisce 1 se l'indice XML selettivo è abilitato in un database specifico.  
  
> [!NOTE]  
>  Per disabilitare l'indice XML selettivo utilizzando questo stored procedure, è necessario attivare la modalità di recupero con registrazione minima utilizzando il comando [ALTER database set Options &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) .  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
      sys.sp_db_selective_xml_index[[ @dbname = ] 'dbname'],   
[[ @selective_xml_index = ] 'selective_xml_index']  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @ dbname = ] 'dbname'` Nome del database in cui abilitare o disabilitare l'indice XML selettivo. Se *dbname* è null, viene utilizzato il database corrente. *@dbname* è di **tipo sysname**.


`[ @selective_xml_index = ] 'selective_xml_index'` Determina se abilitare o disabilitare l'indice. Valori consentiti:' on ',' off ',' true ',' false '. Se viene passato un altro valore tranne ' on ',' true ',' off ' o ' false ', verrà generato un errore. *@selective_xml_index* è di tipo **varchar (6)**.

  
## <a name="return-code-values"></a>Valori del codice restituito  
 **1** se l'indice XML selettivo è abilitato in un database specifico, **0** se disabilitato.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-enable-selective-xml-index-functionality"></a>R. Abilitare la funzionalità degli indici XML selettivi  
 Nell'esempio seguente viene abilitato l'indice XML selettivo nel database corrente.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = NULL  
  , @selective_xml_index = N'on';  
GO  
```  
  
 Nell'esempio seguente viene abilitato l'indice XML selettivo nel database AdventureWorks2012.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = N'AdventureWorks2012'  
  , @selective_xml_index = N'true';  
GO  
```  
  
### <a name="b-disable-selective-xml-index-functionality"></a>B. Disabilitare la funzionalità degli indici XML selettivi  
 Nell'esempio seguente viene disabilitato l'indice XML selettivo nel database corrente.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = NULL  
  , @selective_xml_index = N'off';  
GO  
```  
  
 Nell'esempio seguente viene disabilitato l'indice XML selettivo nel database AdventureWorks2012.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = N'AdventureWorks2012'  
  , @selective_xml_index = N'false';  
GO  
```  
  
### <a name="c-detect-if-selective-xml-index-is-enabled"></a>C. Rilevare se l'indice XML selettivo è abilitato  
 Nell'esempio seguente viene rilevato se l'indice XML selettivo è abilitato. Restituisce 1 se l'indice XML selettivo è abilitato.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Indici XML selettivi &#40;SXI&#41;](../../relational-databases/xml/selective-xml-indexes-sxi.md)  
   