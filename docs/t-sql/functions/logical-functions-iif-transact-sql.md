---
description: Funzioni logiche - IIF (Transact-SQL)
title: IIF (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IIF_TSQL
- IIF
dev_langs:
- TSQL
helpviewer_keywords:
- IIF function
ms.assetid: e3ccf8ed-1cec-43ac-90b7-d8597c24b050
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8dd7e4faa4c8e175987254981f09d78da9445d1a
ms.sourcegitcommit: e40e75055c1435c5e3f9b6e3246be55526807b4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2021
ms.locfileid: "98151279"
---
# <a name="logical-functions---iif-transact-sql"></a>Funzioni logiche - IIF (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Viene restituito uno di due valori a seconda che l'espressione booleana sia true o false in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
IIF( boolean_expression, true_value, false_value )
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *boolean_expression*  
 Espressione booleana valida.  
  
 Se questo argomento non è un'espressione booleana, viene generato un errore di sintassi.  
  
 *true_value*  
 Valore da restituire se *boolean_expression* restituisce true.  
  
 *false_value*  
 Valore da restituire se *boolean_expression* restituisce false.  
  
## <a name="return-types"></a>Tipi restituiti  
 Restituisce il tipo di dati con precedenza maggiore nei tipi *true_value* e *false_value*. Per altre informazioni, vedere [Precedenza dei tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="remarks"></a>Osservazioni  
 IIF è un modo abbreviato per scrivere un'espressione CASE. Valuta l'espressione booleana passata come primo argomento, quindi restituisce uno dei due argomenti in base al risultato della valutazione. Viene quindi restituito *true_value* se l'espressione booleana è true, mentre viene restituito *false_value* se l'espressione booleana è false o sconosciuta. *true_value* e *false_value* possono essere di qualsiasi tipo. Le stesse regole applicate all'espressione CASE per espressioni booleane, gestione di valori Null e tipi restituiti vengono applicate anche a IIF. Per altre informazioni, vedere [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md).  
  
 La conversione di IIF in CASE influisce inoltre su altri aspetti del comportamento di questa funzione. Poiché le espressioni CASE possono essere nidificate solo fino a livello 10, anche le istruzioni IIF possono essere nidificate fino al livello massimo 10. Inoltre l'istruzione IIF viene eseguita in modalità remota in altri server come un'espressione CASE equivalente dal punto di vista semantico, con tutti i comportamenti di un'espressione CASE eseguita in modalità remota.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-simple-iif-example"></a>R. Esempio semplice di IIF  
  
```sql  
DECLARE @a INT = 45, @b INT = 40;
SELECT [Result] = IIF( @a > @b, 'TRUE', 'FALSE' );
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
--------  
TRUE  
```  
  
### <a name="b-iif-with-null-constants"></a>B. IIF con costanti NULL  
  
```sql 
SELECT [Result] = IIF( 45 > 30, NULL, NULL );
```  
  
 Il risultato di questa istruzione è un errore.  
  
### <a name="c-iif-with-null-parameters"></a>C. IIF con parametri NULL  
  
```sql  
DECLARE @P INT = NULL, @S INT = NULL;  
SELECT [Result] = IIF( 45 > 30, @P, @S );
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
--------  
NULL  
```  
  
## <a name="see-also"></a>Vedere anche  
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [CHOOSE &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-choose-transact-sql.md)  
  
  
