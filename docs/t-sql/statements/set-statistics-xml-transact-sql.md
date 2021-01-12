---
description: SET STATISTICS XML (Transact-SQL)
title: SET STATISTICS XML (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET_STATISTICS_XML_TSQL
- SET STATISTICS XML
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], statement processing
- STATISTICS XML option
- SET STATISTICS XML statement
- statements [SQL Server], statistical information
- XML [SQL Server], statement execution information
ms.assetid: 2b6d4c5a-a7f5-4dd1-b10a-7632265b1af7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b99a042e889c2de3baa5c544e2255a1f8bceb855
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98079770"
---
# <a name="set-statistics-xml-transact-sql"></a>SET STATISTICS XML (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Fa sì che Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esegua istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] e generi informazioni dettagliate sull'esecuzione delle istruzioni in un documento XML ben definito.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
SET STATISTICS XML { ON | OFF }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni
 L'opzione SET STATISTICS XML viene impostata in fase di esecuzione, non in fase di analisi.  
  
 Se SET STATISTICS XML è impostata su ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce informazioni sull'esecuzione di ogni istruzione. Quando l'opzione viene impostata su ON, vengono restituite informazioni su tutte le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] successive fino a quando l'opzione non viene impostata su OFF. Non è necessario che SET STATISTICS XML sia l'unica istruzione in un batch.  
  
 SET STATISTICS XML restituisce l'output come tipo **nvarchar(max)** per le applicazioni, come l'utilità **sqlcmd**, in cui l'output XML viene successivamente usato da altri strumenti per visualizzare ed elaborare le informazioni del piano di query.  
  
 SET STATISTICS XML restituisce le informazioni sotto forma di documenti XML. Ogni istruzione successiva all'istruzione SET STATISTICS XML ON è rappresentata nell'output da un documento specifico. Ogni documento contiene il testo dell'istruzione, seguito da informazioni dettagliate sui passaggi dell'esecuzione. L'output include informazioni ottenute durante l'esecuzione, quali i costi, gli indici utilizzati e i tipi di operazioni eseguite, l'ordine di join, il numero di esecuzioni di un'operazione fisica, il numero di righe prodotto da ogni operatore fisico e altro ancora.  
  
 Il documento contenente l'XML Schema per l'output XML di SET STATISTICS XML viene copiato, durante l'installazione, in una directory locale del computer in cui è installato Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si trova nell'unità contenente i file di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], nel percorso seguente:  
  
 \Microsoft SQL Server\100\Tools\Binn\schemas\sqlserver\2004\07\showplan\showplanxml.xsd  
  
 Lo schema Showplan è inoltre reperibile in [questo sito Web](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409).  
  
 SET STATISTICS PROFILE e SET STATISTICS XML sono complementari. La prima crea output di testo, la seconda output XML. Nelle prossime versioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le informazioni sui piani di esecuzione delle query verranno visualizzate solo mediante l'istruzione SET STATISTICS XML e non mediante l'istruzione SET STATISTICS PROFILE.  
  
> [!NOTE]  
>  Se l'opzione **Includi piano di esecuzione effettivo** è selezionata in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], l'opzione SET non genera alcun output di Showplan XML. Prima di usare l'opzione SET, deselezionare il pulsante **Includi piano di esecuzione effettivo**.  
  
## <a name="permissions"></a>Autorizzazioni  
 Per utilizzare SET STATISTICS XML e visualizzare l'output, è necessario che gli utenti dispongano delle autorizzazioni seguenti:  
  
-   Autorizzazioni appropriate per l'esecuzione delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Autorizzazione SHOWPLAN su tutti i database contenenti oggetti a cui viene fatto riferimento nelle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Per le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che non producono set di risultati STATISTICS XML, sono necessarie solo le autorizzazioni appropriate per l'esecuzione delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)]. Per le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] che producono set di risultati STATISTICS XML, devono venire superati i controlli sia dell'autorizzazione di esecuzione delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] sia dell'autorizzazione SHOWPLAN. Negli altri casi l'esecuzione delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] verrà annullata e non verranno generate informazioni Showplan.  
  
## <a name="examples"></a>Esempi  
 Nelle due istruzioni seguenti vengono utilizzate le impostazioni dell'istruzione SET STATISTICS XML per illustrare l'analisi e l'ottimizzazione dell'utilizzo degli indici nelle query da parte di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La prima query utilizza l'operatore di confronto uguale a (=) nella clausola WHERE in una colonna indicizzata. La seconda query utilizza l'operatore LIKE nella clausola WHERE. In tal modo viene imposta l'esecuzione da parte di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] di un'analisi di indice cluster per individuare i dati che soddisfano la condizione della clausola WHERE. I valori negli attributi **EstimateRows** e **EstimatedTotalSubtreeCost** della prima query indicizzata sono inferiori, a indicare che la query è stata elaborata molto più rapidamente e con un numero di risorse inferiore rispetto alla query non indicizzata.  
  
```sql
USE AdventureWorks2012;  
GO  
SET STATISTICS XML ON;  
GO  
-- First query.  
SELECT BusinessEntityID   
FROM HumanResources.Employee  
WHERE NationalIDNumber = '509647174';  
GO  
-- Second query.  
SELECT BusinessEntityID, JobTitle   
FROM HumanResources.Employee  
WHERE JobTitle LIKE 'Production%';  
GO  
SET STATISTICS XML OFF;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [SET SHOWPLAN_XML &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-xml-transact-sql.md)   
 [Utilità sqlcmd](../../tools/sqlcmd-utility.md)  
  
  
