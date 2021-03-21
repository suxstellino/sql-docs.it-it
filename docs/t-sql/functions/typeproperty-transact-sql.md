---
description: TYPEPROPERTY (Transact-SQL)
title: TYPEPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TYPEPROPERTY
- TYPEPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server], data types
- data types [SQL Server], status information
- TYPEPROPERTY function
ms.assetid: bc311c80-bac5-46ab-a5c8-68b1c6bbf24a
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7caea50cb3235c8995b60ebb73206ed6db7d5361
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750891"
---
# <a name="typeproperty-transact-sql"></a>TYPEPROPERTY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Restituisce informazioni su un tipo di dati.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
TYPEPROPERTY (type , property)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *type*  
 Nome del tipo di dati.  
  
 *property*  
 Tipo di informazioni da restituire per il tipo di dati. I possibili valori di *property* sono i seguenti.  
  
|Proprietà|Descrizione|Valore restituito|  
|--------------|-----------------|--------------------|  
|**AllowsNull**|Il tipo di dati ammette valori Null.|1 = True<br /><br /> 0 = False<br /><br /> NULL = Tipo di dati non trovato.|  
|**OwnerId**|Proprietario del tipo.<br /><br /> Nota: il proprietario dello schema non deve necessariamente corrispondere al proprietario del tipo.|Valore diverso da Null = ID utente del database del proprietario del tipo.<br /><br /> NULL = Tipo non supportato oppure ID di tipo non valido.|  
|**Precisione**|Precisione del tipo di dati.|Numero di cifre o caratteri.<br /><br /> -1 = **xml** oppure tipo di dati per valori di grandi dimensioni<br /><br /> NULL = Tipo di dati non trovato.|  
|**Ridimensionamento**|Scala del tipo di dati.|Numero di posizioni decimali per il tipo di dati.<br /><br /> NULL = Tipo di dati diverso da **numeric** oppure non trovato.|  
|**UsesAnsiTrim**|In fase di creazione del tipo di dati l'opzione per il riempimento ANSI era impostata su ON.|1 = True<br /><br /> 0 = False<br /><br /> NULL = Tipo di dati non trovato oppure diverso da binary o string.|  
  
## <a name="return-types"></a>Tipi restituiti  
 **int**  
  
## <a name="exceptions"></a>Eccezioni  
 Restituisce NULL in caso di errore o se un chiamante non dispone dell'autorizzazione necessaria per visualizzare l'oggetto.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] un utente può visualizzare esclusivamente i metadati delle entità a sicurezza diretta di cui è proprietario o per cui ha ricevuto un'autorizzazione. Di conseguenza, le funzioni predefinite di creazione dei metadati come TYPEPROPERTY possono restituire NULL se l'utente non dispone di alcuna autorizzazione per l'oggetto. Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-identifying-the-owner-of-a-data-type"></a>R. Identificazione del proprietario di un tipo di dati  
 Nell'esempio seguente viene restituito il proprietario di un tipo di dati.  
  
```sql
SELECT TYPEPROPERTY(SCHEMA_NAME(schema_id) + '.' + name, 'OwnerId') AS owner_id, name, system_type_id, user_type_id, schema_id  
FROM sys.types;  
```  
  
### <a name="b-returning-the-precision-of-the-tinyint-data-type"></a>B. Restituzione della precisione del tipo di dati tinyint  
 Nell'esempio seguente viene restituita la precisione o il numero di cifre per il tipo di dati `tinyint`.  
  
```sql
SELECT TYPEPROPERTY( 'tinyint', 'PRECISION');  
```  
  
## <a name="see-also"></a>Vedere anche  
 [TYPE_ID &#40;Transact-SQL&#41;](../../t-sql/functions/type-id-transact-sql.md)   
 [TYPE_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/type-name-transact-sql.md)   
 [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md)   
 [Funzioni per i metadati &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)  
  
  

