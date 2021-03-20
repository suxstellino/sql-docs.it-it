---
description: Ridenominazione di tabelle (motore di database)
title: Rinominare tabelle (motore di database) | Microsoft Docs
ms.custom: ''
ms.date: 02/23/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- table renaming [SQL Server]
- table names [SQL Server]
- tables [SQL Server], Visual Database Tools
- renaming tables
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c1f38616cf33ee2a759ada71b7873d1a0c0e3c20
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752381"
---
# <a name="rename-tables-database-engine"></a>Ridenominazione di tabelle (motore di database)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Rinominare una tabella in SQL Server o nel database SQL di Azure.

Per rinominare una tabella in Azure Synapse Analytics o in Parallel Data Warehouse, usare l'istruzione [RENAME OBJECT](../../t-sql/statements/rename-transact-sql.md) di T-SQL. 
  
> [!CAUTION]  
>  Fare attenzione prima di rinominare una tabella. Se query, viste, funzioni definite dall'utente, stored procedure o programmi esistenti fanno riferimento a tale tabella, la modifica del nome renderà questi oggetti non validi.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per rinominare una tabella:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
 Se una tabella viene ridenominata, i riferimenti a tale tabella non vengono ridenominati automaticamente ed è necessario modificare manualmente tutti gli oggetti che fanno riferimento alla tabella rinominata. Se, ad esempio, si rinomina una tabella a cui viene fatto riferimento all'interno di un trigger, è necessario modificare il trigger in base al nuovo nome della tabella. Utilizzare [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) per elencare le dipendenze della tabella prima di rinominarla.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessario disporre dell'autorizzazione ALTER per la tabella.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
  
#### <a name="to-rename-a-table"></a>Per rinominare una tabella  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse sulla tabella da rinominare, quindi selezionare **Progetta** dal menu di scelta rapida.  
  
2.  Scegliere **Proprietà** dal menu **Visualizza**.  
  
3.  Nella finestra **Proprietà** digitare un nuovo nome per la tabella nel campo relativo al valore **Nome** .  
  
4.  Per annullare questa azione, premere ESC prima di uscire dal campo.  
  
5.  Scegliere **Salva** _nome tabella_ dal menu **File**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
  
#### <a name="to-rename-a-table"></a>Per rinominare una tabella  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Nell'esempio seguente la tabella `SalesTerritory` viene rinominata in `SalesTerr` nello schema `Sales` . Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    EXEC sp_rename 'Sales.SalesTerritory', 'SalesTerr';  
    ```  
  
 Per altri esempi, vedere [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md).  
  
  
