---
description: '% (modulo) (Transact-SQL)'
title: '% (modulo) (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- modulo
- modulus
- '% (Modulo)'
- '% (Modulus)'
- MOD_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- '% (modulo operator)'
- '% (modulus operator)'
- remainder of division operation
- modulo operator (%)
- modulus operator (%)
ms.assetid: f93c662e-f405-486e-bf23-a2d03907b5bd
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 38a4cc536d506e9f70a68198a7a7ea5550c84eca
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748861"
---
# <a name="-modulus-transact-sql"></a>% (modulo) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Viene restituito il resto di una divisione.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql  
dividend % divisor  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 *dividend*  
 Espressione numerica da dividere. *dividend* deve essere un'[espressione](../../t-sql/language-elements/expressions-transact-sql.md) valida di uno dei tipi di dati presenti per le categorie di interi e di valuta o del tipo di dati **numeric**.  
  
 *divisor*  
 Espressione numerica per cui dividere il dividendo. *divisor* deve essere qualsiasi espressione valida di uno dei tipi di dati presenti per le categorie di interi e di valuta o del tipo di dati **numeric**.  
  
## <a name="result-types"></a>Tipi restituiti  
 Dipendenti dai tipi di dati dei due argomenti.  
  
## <a name="remarks"></a>Commenti  
 L'operatore aritmetico modulo può essere usato nell'elenco di selezione dell'istruzione SELECT con una qualsiasi combinazione di nomi di colonna, costanti numeriche o qualsiasi espressione valida delle categorie di tipi di dati integer e monetary o del tipo di dati **numeric**.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-simple-example"></a>R. Esempio semplice  
 Nell'esempio seguente viene diviso il numero 38 per 5. Viene restituito 7 come parte intera del risultato e viene illustrata la restituzione del resto di 3 da parte del modulo.  
  
```sql  
SELECT 38 / 5 AS Integer, 38 % 5 AS Remainder;
```  
  
### <a name="b-example-using-columns-in-a-table"></a>B. Esempio di utilizzo di colonne in una tabella  
 Nell'esempio seguente viene restituito il numero di serie del prodotto, il prezzo unitario del prodotto e il modulo (resto) della divisione tra il prezzo di ogni prodotto convertito in un valore intero e il numero di prodotti ordinati.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT TOP(100)ProductID, UnitPrice, OrderQty,  
   CAST((UnitPrice) AS INT) % OrderQty AS Modulo  
FROM Sales.SalesOrderDetail;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Esempi: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] e [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-simple-example"></a>C. Esempio semplice  
 L'esempio seguente visualizza i risultati per l'operatore `%` quando si divide 3 per 2.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT TOP(1) 3%2 FROM dimEmployee;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------   
1         
```  
  
## <a name="see-also"></a>Vedere anche  
 [Funzioni predefinite &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [%= &#40;Assegnazione modulo&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/modulo-equals-transact-sql.md)   
 [Operatori composti &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  


