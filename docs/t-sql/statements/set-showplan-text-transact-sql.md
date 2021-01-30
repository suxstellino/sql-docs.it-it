---
description: SET SHOWPLAN_TEXT (Transact-SQL)
title: SET SHOWPLAN_TEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SHOWPLAN_TEXT
- SET_SHOWPLAN_TEXT_TSQL
- SET SHOWPLAN_TEXT
- SHOWPLAN_TEXT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- statements [SQL Server], estimates
- execution information and estimates [SQL Server]
- statements [SQL Server], information without processing
- SET SHOWPLAN_TEXT statement
- canceling statement execution
- SHOWPLAN_TEXT option
- stopping statement execution
- estimated execution information [SQL Server]
ms.assetid: 2c4f3fc8-ff2c-4790-8b74-e7e8ef58f9a6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 418326bb3fcfe0edd07e88405bfb9d216fb8538e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189442"
---
# <a name="set-showplan_text-transact-sql"></a>SET SHOWPLAN_TEXT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Impedisce l'esecuzione delle istruzioni [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Microsoft [!INCLUDE[tsql](../../includes/tsql-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce invece informazioni dettagliate sulla modalità di esecuzione delle istruzioni.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
SET SHOWPLAN_TEXT { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni
 L'opzione SET SHOWPLAN_TEXT viene impostata in fase di esecuzione, non in fase di analisi.  
  
 Quando l'opzione SET SHOWPLAN_TEXT è impostata su ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce informazioni di esecuzione per ogni istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)] senza eseguire l'istruzione. Quando l'opzione è impostata su ON, vengono restituite informazioni del piano di esecuzione su tutte le istruzioni [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] successive fino a quando l'opzione non viene reimpostata su OFF. Se, ad esempio, si esegue un'istruzione CREATE TABLE quando l'opzione SET SHOWPLAN_TEXT è impostata su ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un messaggio di errore di una successiva istruzione SELECT che interessa la stessa tabella, per informare gli utenti che la tabella specificata non esiste. I successivi riferimenti a tale tabella pertanto hanno esito negativo. Quando l'opzione SET SHOWPLAN_TEXT è impostata su OFF, le istruzioni vengono eseguite da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] senza la generazione di un report contenente informazioni del piano di esecuzione.  
  
 SET SHOWPLAN_TEXT è stata creata specificatamente per la restituzione di output leggibile in applicazioni del prompt dei comandi per Microsoft Win32, ad esempio l'utilità **sqlcmd**. L'opzione SET SHOWPLAN_ALL restituisce un output più dettagliato per l'utilizzo in programmi per la gestione di output.  
  
 Non è possibile specificare SET SHOWPLAN_TEXT e SET SHOWPLAN_ALLL all'interno di una stored procedure. Devono essere le uniche istruzioni in un batch.  
  
 L'opzione SET SHOWPLAN_TEXT restituisce informazioni sotto forma di un set di righe in un albero gerarchico che rappresenta i passaggi eseguiti da Query Processor di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per l'esecuzione delle varie istruzioni. Ogni istruzione restituita nell'output include una singola riga contenente il testo dell'istruzione seguita da alcune righe che includono i dettagli dei passaggi dell'esecuzione. Nella tabella seguente vengono illustrate le colonne incluse nell'output.  
  
|Nome colonna|Descrizione|  
|-----------------|-----------------|  
|**StmtText**|Per righe che non sono di tipo PLAN_ROW, questa colonna include il testo dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)]. Per righe di tipo PLAN_ROW, include una descrizione dell'operazione. La colonna include l'operatore fisico e facoltativamente l'operatore logico. Può essere inoltre seguita da una descrizione determinata dall'operatore fisico. Per altre informazioni sugli operatori fisici, vedere la colonna **Argument** in [SET SHOWPLAN_ALL &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-all-transact-sql.md).|  
|||

 Per altre informazioni sugli operatori fisici e logici che possono essere visualizzati nell'output Showplan, vedere [Guida di riferimento a operatori Showplan logici e fisici](../../relational-databases/showplan-logical-and-physical-operators-reference.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 Per poter utilizzare SET SHOWPLAN_TEXT, è necessario disporre delle autorizzazioni sufficienti per eseguire le istruzioni in cui SET SHOWPLAN_TEXT viene eseguito, nonché l'autorizzazione SHOWPLAN per tutti i database contenenti oggetti di riferimento.  
  
 Per poter generare uno Showplan con le istruzioni SELECT, INSERT, UPDATE, DELETE, EXEC *stored_procedure* ed EXEC *user_defined_function*, l'utente deve avere:  
  
-   Autorizzazioni appropriate per l'esecuzione delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Autorizzazione SHOWPLAN su tutti i database contenenti oggetti a cui fanno riferimento le istruzioni Transact-SQL, ad esempio tabelle, viste e così via.  
  
 Per tutte le altre istruzioni, ad esempio DDL, USE *database_name*, SET, DECLARE, SQL dinamico e così via sono necessarie soltanto le autorizzazioni per l'esecuzione delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="examples"></a>Esempi  
 In questo esempio viene illustrato l'utilizzo degli indici in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] durante l'elaborazione di istruzioni.  
  
 Questa query utilizza un indice:  
  
```sql
USE AdventureWorks2012;  
GO  
SET SHOWPLAN_TEXT ON;  
GO  
SELECT *  
FROM Production.Product   
WHERE ProductID = 905;  
GO  
SET SHOWPLAN_TEXT OFF;  
GO  
```  
  
 Set di risultati:  
  
```  
StmtText                                             
---------------------------------------------------  
SELECT *  
FROM Production.Product   
WHERE ProductID = 905;   
  
StmtText                                                                                                                                                                                        
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------  
|--Clustered Index Seek(OBJECT:([AdventureWorks2012].[Production].[Product].[PK_Product_ProductID]), SEEK:([AdventureWorks2012].[Production].[Product].[ProductID]=CONVERT_IMPLICIT(int,[@1],0)) ORDERED FORWARD)   
```  
  
 Questa query non utilizza un indice:  
  
```sql
USE AdventureWorks2012;  
GO  
SET SHOWPLAN_TEXT ON;  
GO  
SELECT *  
FROM Production.ProductCostHistory  
WHERE StandardCost < 500.00;  
GO  
SET SHOWPLAN_TEXT OFF;  
GO  
```  
  
 Set di risultati:  
  
```  
StmtText                                                                  
------------------------------------------------------------------------  
SELECT *  
FROM Production.ProductCostHistory  
WHERE StandardCost < 500.00;   
  
StmtText                                                                                                                                                                                                  
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------  
|--Clustered Index Scan(OBJECT:([AdventureWorks2012].[Production].[ProductCostHistory].[PK_ProductCostHistory_ProductCostID]), WHERE:([AdventureWorks2012].[Production].[ProductCostHistory].[StandardCost]<[@1]))  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Operatori &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET SHOWPLAN_ALL &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-all-transact-sql.md)  
  
  
