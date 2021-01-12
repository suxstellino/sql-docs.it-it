---
description: SET SHOWPLAN_ALL (Transact-SQL)
title: SET SHOWPLAN_ALL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET SHOWPLAN_ALL
- SET_SHOWPLAN_ALL_TSQL
- SHOWPLAN_ALL
- SHOWPLAN_ALL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- statements [SQL Server], estimates
- execution information and estimates [SQL Server]
- statements [SQL Server], information without processing
- SET SHOWPLAN_ALL statement
- SHOWPLAN_ALL option
- canceling statement execution
- stopping statement execution
- estimated execution information [SQL Server]
ms.assetid: a500b682-bae4-470f-9e00-47de905b851b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6a8cef73df3a6ced1cac91c997848cf28b966042
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093331"
---
# <a name="set-showplan_all-transact-sql"></a>SET SHOWPLAN_ALL (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Impedisce l'esecuzione delle istruzioni [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Microsoft [!INCLUDE[tsql](../../includes/tsql-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce invece informazioni dettagliate sulla modalità di esecuzione delle istruzioni e una stima delle risorse necessarie per eseguire le istruzioni.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
SET SHOWPLAN_ALL { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni
 L'opzione SET SHOWPLAN_ALL viene impostata in fase di esecuzione, non in fase di analisi.  
  
 Quando l'opzione `SET SHOWPLAN_ALL` è impostata su ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce le informazioni di esecuzione per ciascuna istruzione, senza eseguirla. Le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] non vengono eseguite. Quando l'opzione è impostata su ON, vengono restituite informazioni su tutte le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] successive fino a quando l'opzione non viene reimpostata su OFF. Ad esempio se si esegue un'istruzione CREATE TABLE quando l'opzione `SET SHOWPLAN_ALL` è impostata su ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un messaggio di errore da una successiva istruzione SELECT che interessa la stessa tabella, per informare gli utenti che la tabella specificata non esiste. I successivi riferimenti a tale tabella pertanto hanno esito negativo. Quando l'opzione SET SHOWPLAN_ALL è impostata su OFF, le istruzioni vengono eseguite da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] senza la generazione di alcun report.  
  
 L'opzione `SET SHOWPLAN_ALL` è progettata per l'uso in applicazioni scritte per gestirne l'output. Usare SET SHOWPLAN_TEXT per ottenere output leggibile in applicazioni della riga di comando per Microsoft Win32, ad esempio l'utilità **osql**.  
  
 Non è possibile specificare entrambe le opzioni SET SHOWPLAN_TEXT e SET SHOWPLAN_ALL in una stored procedure. Devono essere inoltre le uniche istruzioni di un batch.  
  
 L'opzione SET SHOWPLAN_ALL restituisce informazioni sotto forma di un set di righe in un albero gerarchica che rappresenta i passaggi eseguiti da Query Processor di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per l'esecuzione delle varie istruzioni. Ogni istruzione restituita nell'output include una singola riga contenente il testo dell'istruzione seguita da alcune righe che includono i dettagli dei passaggi dell'esecuzione. Nella tabella seguente vengono illustrate le colonne incluse nell'output.  
  
|Nome colonna|Descrizione|  
|-----------------|-----------------|  
|**StmtText**|Per righe che non sono di tipo PLAN_ROW, questa colonna include il testo dell'istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)]. Per righe di tipo PLAN_ROW, include una descrizione dell'operazione. La colonna include l'operatore fisico e facoltativamente l'operatore logico. Può essere inoltre seguita da una descrizione determinata dall'operatore fisico. Per altre informazioni, vedere la [Guida di riferimento a operatori Showplan logici e fisici](../../relational-databases/showplan-logical-and-physical-operators-reference.md).|  
|**StmtId**|Numero dell'istruzione nel batch corrente.|  
|**NodeId**|ID del nodo nella query corrente.|  
|**Parent**|ID del nodo del passaggio padre.|  
|**PhysicalOp**|Algoritmo di implementazione fisica del nodo. Solo per righe di tipo PLAN_ROWS.|  
|**LogicalOp**|Operatore algebrico relazionale rappresentato dal nodo. Solo per righe di tipo PLAN_ROWS.|  
|**Argument**|Offre informazioni aggiuntive sull'operazione che viene eseguita. Il contenuto di questa colonna dipende dall'operatore fisico.|  
|**DefinedValues**|Include un elenco delimitato da virgole dei valori introdotti da questo operatore. Tali valori possono essere espressioni calcolate che erano incluse nella query corrente, ad esempio nell'elenco di selezione o nella clausola WHERE, oppure valori interni inseriti da Query Processor per l'elaborazione della query. È inoltre possibile fare riferimento a tali valori in un altro punto della query. Solo per righe di tipo PLAN_ROWS.|  
|**EstimateRows**|Numero stimato di righe restituite dall'operatore. Solo per righe di tipo PLAN_ROWS.|  
|**EstimateIO**|Costo* di I/O stimato per l'operatore. Solo per righe di tipo PLAN_ROWS.|  
|**EstimateCPU**|Costo* della CPU stimato per l'operatore. Solo per righe di tipo PLAN_ROWS.|  
|**AvgRowSize**|Dimensioni medie stimate (in byte) della riga che viene elaborata dall'operatore.|  
|**TotalSubtreeCost**|Costo* (cumulativo) stimato dell'operazione e delle operazioni figlio.|  
|**OutputList**|Include un elenco delimitato da virgole delle colonne previste dall'operazione corrente.|  
|**:::no-loc text="Warnings":::**|Include un elenco delimitato da virgole dei messaggi di avviso relativi all'operazione corrente. I messaggi di avviso possono includere la stringa "NO STATS:()" con un elenco di colonne. Tale messaggio indica che in Query Optimizer è stata tentata una decisione in base alle statistiche di questa colonna, ma non era disponibile alcuna statistica. Query Optimizer ha pertanto formulato un'ipotesi con cui potrebbe essere stato scelto un piano non efficiente per la query. Per altre informazioni sulla creazione o l'aggiornamento di statistiche di colonna, che agevolano la scelta di un piano di query più efficace in Query Optimizer, vedere [UPDATE STATISTICS](../../t-sql/statements/update-statistics-transact-sql.md). Questa colonna può includere facoltativamente la stringa "MISSING JOIN PREDICATE", a indicare che è stato eseguito un join tra tabelle senza specificare un predicato di join. In seguito all'eliminazione accidentale di un predicato di join, la query potrebbe richiedere tempi di esecuzione maggiori del previsto e restituire un set di risultati di dimensioni elevate. Se la colonna include tale stringa, verificare se l'assenza di un predicato di join è o meno voluta.|  
|**:::no-loc text="Type":::**|Tipo di nodo. Per il nodo padre di ogni query, è il tipo di istruzione [!INCLUDE[tsql](../../includes/tsql-md.md)], ad esempio SELECT, INSERT, EXECUTE e così via. Per sottonodi che rappresentano piani di esecuzione, il tipo è PLAN_ROW.|  
|**Parallel**|**0** = L'operatore non viene eseguito in parallelo.<br /><br /> **1** = L'operatore viene eseguito in parallelo.|  
|**EstimateExecutions**|Numero stimato di esecuzioni dell'operatore durante l'elaborazione della query corrente.|  
|||

 *Le unità di costo sono basate su una misurazione interna del tempo, non sul tempo dell'orologio. Si utilizzano per la determinazione del costo relativo di un piano rispetto ad altri piani.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per poter utilizzare SET SHOWPLAN_ALL, è necessario disporre delle autorizzazioni sufficienti per eseguire le istruzioni in cui SET SHOWPLAN_ALL viene eseguito, nonché l'autorizzazione SHOWPLAN per tutti i database contenenti oggetti di riferimento.  
  
 Per poter generare uno Showplan con le istruzioni SELECT, INSERT, UPDATE, DELETE, EXEC *stored_procedure* ed EXEC *user_defined_function*, l'utente deve avere:  
  
-   Autorizzazioni appropriate per l'esecuzione delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Autorizzazione SHOWPLAN su tutti i database contenenti oggetti a cui fanno riferimento le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)], ad esempio tabelle, viste e così via.  
  
 Per tutte le altre istruzioni, ad esempio DDL, USE *database_name*, SET, DECLARE, SQL dinamico e così via sono necessarie soltanto le autorizzazioni per l'esecuzione delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="examples"></a>Esempi  
 Nelle due istruzioni seguenti vengono utilizzate le impostazioni dell'opzione SET SHOWPLAN_ALL per illustrare l'analisi e l'ottimizzazione dell'utilizzo degli indici nelle query in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 La prima query utilizza l'operatore di confronto uguale a (=) nella clausola WHERE in una colonna indicizzata. I risultati sono il valore Clustered Index Seek nella colonna **LogicalOp** e il nome dell'indice nella colonna **Argument**.  
  
 La seconda query utilizza l'operatore LIKE nella clausola WHERE. In tal modo viene imposta l'esecuzione di un'analisi di indice cluster in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per individuare i dati che soddisfano la condizione della clausola WHERE. I risultati sono il valore Clustered Index Scan nella colonna **LogicalOp** con il nome dell'indice nella colonna **Argument** e il valore Filter nella colonna **LogicalOp** con la condizione della clausola WHERE nella colonna **Argument**.  
  
 I valori nelle colonne **EstimateRows** e **TotalSubtreeCost** della prima query indicizzata sono inferiori, a indicare che la query è stata elaborata molto più rapidamente e con un numero di risorse inferiore rispetto alla query non indicizzata.  
  
```sql
USE AdventureWorks2012;  
GO  
SET SHOWPLAN_ALL ON;  
GO  
-- First query.  
SELECT BusinessEntityID   
FROM HumanResources.Employee  
WHERE NationalIDNumber = '509647174';  
GO  
-- Second query.  
SELECT BusinessEntityID, EmergencyContactID   
FROM HumanResources.Employee  
WHERE EmergencyContactID LIKE '1%';  
GO  
SET SHOWPLAN_ALL OFF;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET SHOWPLAN_TEXT &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-text-transact-sql.md)   
 [SET SHOWPLAN_XML &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-xml-transact-sql.md)  
  
  
