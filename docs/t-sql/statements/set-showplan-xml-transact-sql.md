---
description: SET SHOWPLAN_XML (Transact-SQL)
title: SET SHOWPLAN_XML (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET SHOWPLAN_XML
- SET_SHOWPLAN_XML_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- statements [SQL Server], estimates
- SET SHOWPLAN_XML statement
- execution information and estimates [SQL Server]
- statements [SQL Server], information without processing
- XML [SQL Server], statement execution information
- SHOWPLAN_XML option
- estimated execution information [SQL Server]
ms.assetid: a467a1b3-10a5-43c4-9085-13d8aed549c9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d0065350fbf3c3c774602f28aeb98d6e7685df4c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206957"
---
# <a name="set-showplan_xml-transact-sql"></a>SET SHOWPLAN_XML (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Impedisce l'esecuzione delle istruzioni [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Microsoft [!INCLUDE[tsql](../../includes/tsql-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce invece informazioni dettagliate sulla modalità di esecuzione delle istruzioni sotto forma di documento XML ben definito.

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql
SET SHOWPLAN_XML { ON | OFF }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni

L'opzione SET SHOWPLAN_XML viene impostata in fase di esecuzione, non in fase di analisi.

Quando l'opzione SET SHOWPLAN_XML è impostata su ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce le informazioni sul piano di esecuzione per ogni istruzione, senza eseguirla. Le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] non vengono eseguite. Quando l'opzione è impostata su ON, vengono restituite informazioni del piano di esecuzione su tutte le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)] successive fino a quando l'opzione non viene reimpostata su OFF. Se, ad esempio, si esegue un'istruzione CREATE TABLE quando l'opzione SET SHOWPLAN_XML è impostata su ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restituisce un messaggio di errore per una successiva istruzione SELECT che interessa la stessa tabella. La tabella specificata non esiste. I successivi riferimenti a tale tabella pertanto hanno esito negativo. Quando l'opzione SET SHOWPLAN_XML è impostata su OFF, le istruzioni vengono eseguite da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] senza la generazione di alcun report.

SET SHOWPLAN_XML è stata creata specificatamente per la restituzione di output come **nvarchar(max)** per applicazioni quali l'utilità **sqlcmd**, in cui l'output XML viene successivamente usato da altri strumenti per visualizzare ed elaborare le informazioni del piano della query.

> [!NOTE]
> La vista a gestione dinamica, **sys.dm_exec_query_plan**, restituisce le stesse informazioni di SET SHOWPLAN XML nel tipo di dati **xml**. Queste informazioni sono restituite dalla colonna **query_plan** di **sys.dm_exec_query_plan**. Per altre informazioni, vedere [sys.dm_exec_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md).

Non è possibile specificare SET SHOWPLAN_XML all'interno di una stored procedure. Deve essere l'unica istruzione in un batch.

SET SHOWPLAN_XML restituisce le informazioni come set di documenti XML. Ogni batch dopo l'istruzione SET SHOWPLAN_XML ON viene restituito nell'output da un unico documento. Ogni documento contiene il testo delle istruzioni nel batch, seguito dai dettagli dei passaggi dell'esecuzione. Nel documento vengono illustrati i costi stimati, il numero di righe, gli indici utilizzati e i tipi di operatori eseguiti, l'ordine di join e ulteriori informazioni sui piani di esecuzione.

> [!NOTE]
> Se l'opzione **Includi piano di esecuzione effettivo** è selezionata in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], l'opzione SET non genera alcun output di Showplan XML. Prima di usare l'opzione SET, deselezionare il pulsante **Includi piano di esecuzione effettivo**.

### <a name="location-of-showplan-output"></a>Percorso dell'output SHOWPLAN

Il documento contenente l'XML Schema per l'output XML di SET SHOWPLAN_XML viene copiato durante l'installazione in una directory locale nel computer in cui è installato Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il documento è reperibile nell'unità contenente i file di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], in un percorso simile al seguente:

- `\Microsoft SQL Server\130\Tools\Binn\schemas\sqlserver\2004\07\showplan\showplanxml.xsd`

Nel percorso precedente, il nodo `130\` viene usato da SQL Server 2016. Il numero 130 è derivato dal primo nodo del valore restituito da `SELECT @@VERSION`, che è 13. Per SQL Server 2017 viene usato il percorso `140\`, poiché il primo nodo del relativo valore @@VERSION è 14. Per SQL Server 2019 il primo valore da @@VERSION è 15.

Lo schema Showplan è inoltre reperibile in [questo sito Web](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409).

## <a name="permissions"></a>Autorizzazioni

Per poter utilizzare SET SHOWPLAN_XML, è necessario disporre delle autorizzazioni sufficienti per eseguire le istruzioni in cui SET SHOWPLAN_XM viene eseguito, nonché l'autorizzazione SHOWPLAN per tutti i database contenenti oggetti di riferimento.

Per poter generare uno Showplan con le istruzioni SELECT, INSERT, UPDATE, DELETE, EXEC *stored_procedure* ed EXEC *user_defined_function*, l'utente deve avere:

- Autorizzazioni appropriate per l'esecuzione delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)].

- Autorizzazione SHOWPLAN su tutti i database contenenti oggetti a cui fanno riferimento le istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)], ad esempio tabelle, viste e così via.

Per tutte le altre istruzioni, ad esempio DDL, USE *database_name*, SET, DECLARE, SQL dinamico e così via sono necessarie soltanto le autorizzazioni per l'esecuzione delle istruzioni [!INCLUDE[tsql](../../includes/tsql-md.md)].

## <a name="examples"></a>Esempi

Nelle due istruzioni seguenti vengono utilizzate le impostazioni dell'opzione SET SHOWPLAN_XML per illustrare l'analisi e l'ottimizzazione dell'utilizzo degli indici nelle query in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

La prima query utilizza l'operatore di confronto uguale a (=) nella clausola WHERE in una colonna indicizzata. La seconda query utilizza l'operatore LIKE nella clausola WHERE. In tal modo viene imposta l'esecuzione di un'analisi di indice cluster in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per individuare i dati che soddisfano la condizione della clausola WHERE. I valori negli attributi **EstimateRows** e **TotalSubtreeCost** della prima query indicizzata sono inferiori, a indicare che la query è stata elaborata molto più rapidamente e con un numero di risorse inferiore rispetto alla query non indicizzata.

```sql
USE AdventureWorks2012;
GO
SET SHOWPLAN_XML ON;
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
SET SHOWPLAN_XML OFF;
```

## <a name="see-also"></a>Vedere anche

[Istruzioni SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)
