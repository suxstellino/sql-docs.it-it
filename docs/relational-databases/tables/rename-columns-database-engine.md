---
description: Ridenominazione di colonne (motore di database)
title: Rinominare colonne (motore di database) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- columns [SQL Server], names
- renaming columns
- column names [SQL Server]
ms.assetid: 7c71ec9f-0180-4398-b32a-4bfb7592e75d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ef2568fbb27a2139896cb18fd99fb9e65c403d8f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212024"
---
# <a name="rename-columns-database-engine"></a>Ridenominazione di colonne (motore di database)


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

È possibile rinominare un nome tabella in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].

**Contenuto dell'articolo**

- **Prima di iniziare:**

   [Limitazioni e restrizioni](#Restrictions)

   [Sicurezza](#Security)

- **Per rinominare colonne utilizzando:**

   [SQL Server Management Studio](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni

Se una colonna viene ridenominata, i riferimenti a tale colonna non vengono ridenominati automaticamente ed è necessario modificare manualmente tutti gli oggetti che fanno riferimento alla colonna rinominata. Se, ad esempio, si rinomina una colonna di una tabella a cui viene fatto riferimento all'interno di un trigger, è necessario modificare il trigger in base al nuovo nome della colonna. Usare [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) per elencare le dipendenze dall'oggetto prima di rinominarlo.

### <a name="security"></a><a name="Security"></a> Sicurezza

#### <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni

È necessario disporre dell'autorizzazione ALTER per l'oggetto.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio

### <a name="to-rename-a-column-using-object-explorer"></a>Per rinominare una colonna utilizzando Esplora oggetti

1. In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].
2. In **Esplora oggetti** fare clic con il pulsante destro del mouse sulla tabella in cui si vuole rinominare le colonne, quindi selezionare **Rinomina**.
3. Digitare un nuovo nome colonna.

### <a name="to-rename-a-column-using-table-designer"></a>Per rinominare una colonna utilizzando Progettazione tabelle

1. In **Esplora oggetti** fare clic con il pulsante destro del mouse sulla tabella di cui si vuole rinominare le colonne e selezionare **Progetta**.
2. In **Nome colonna**, selezionare il nome da cambiare e digitarne uno nuovo.
3. Nel menu **File** fare clic su **Salva** _nome tabella_.

> [!NOTE]
> Per cambiare il nome di una colonna, è anche possibile utilizzare la scheda **Proprietà colonne** . A tale scopo, selezionare la colonna di cui si desidera cambiare il nome e digitare un nuovo valore per **Nome**.

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL

**Per rinominare una colonna**

### <a name="to-rename-a-column"></a>Per rinominare una colonna

L'esempio seguente rinomina la colonna `TerritoryID` nella tabella `Sales.SalesTerritory` in `TerrID` nel database AdventureWorks.

```sql
EXEC sp_rename 'Sales.SalesTerritory.TerritoryID', 'TerrID', 'COLUMN';
```

Per altre informazioni, vedere [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md).
