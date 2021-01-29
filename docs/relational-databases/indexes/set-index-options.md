---
description: Impostare le opzioni di indice
title: Impostare le opzioni di indice | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- ALLOW_ROW_LOCKS option
- SORT_IN_TEMPDB option
- DROP_EXISTING clause
- large data, indexes
- PAD_INDEX
- STATISTICS_NORECOMPUTE
- MAXDOP index option, setting
- index options [SQL Server]
- MAXDOP index option
- IGNORE_DUP_KEY option
- ALLOW_PAGE_LOCKS option
- OPTIMIZE_FOR_SEQUENTIAL_KEY option
- ONLINE
ms.assetid: 7969af33-e94c-41f7-ab89-9d9a2747cd5c
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f2101cd82c0b03eab9c884e8266e4ad01bea1923
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048821"
---
# <a name="set-index-options"></a>Impostare le opzioni di indice

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

In questo argomento si illustra come modificare le proprietà di un indice in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].

 **Contenuto dell'articolo**

- **Prima di iniziare:**

   [Limitazioni e restrizioni](#Restrictions)

   [Sicurezza](#Security)

- **Per modificare le proprietà di un indice utilizzando:**

   [SQL Server Management Studio](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni

- Le opzioni seguenti vengono applicate immediatamente all'indice tramite la clausola SET nell'istruzione ALTER INDEX: ALLOW_PAGE_LOCKS, ALLOW_ROW_LOCKS, OPTIMIZE_FOR_SEQUENTIAL_KEY, IGNORE_DUP_KEY e STATISTICS_NORECOMPUTE.
- Durante la ricompilazione di un indice tramite ALTER INDEX REBUILD o CREATE INDEX WITH DROP_EXISTING, è possibile impostare le opzioni PAD_INDEX, FILLFACTOR, SORT_IN_TEMPDB, IGNORE_DUP_KEY, STATISTICS_NORECOMPUTE, ONLINE, ALLOW_ROW_LOCKS, ALLOW_PAGE_LOCKS, MAXDOP e DROP_EXISTING (solo CREATE INDEX).

### <a name="security"></a><a name="Security"></a> Sicurezza

#### <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni

È richiesta l'autorizzazione ALTER per la tabella o la vista.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio

### <a name="to-modify-the-properties-of-an-index-in-table-designer"></a>Per modificare le proprietà di un indice in Progettazione tabelle

1. In Esplora oggetti fare clic sul segno più per espandere il database contenente la tabella in cui si desidera modificare le proprietà di un indice.
2. Fare clic sul segno più per espandere la cartella **Tabelle** .
3. Fare clic con il pulsante destro del mouse sulla tabella in cui si vogliono modificare le proprietà di un indice e scegliere **Progetta**.
4. Scegliere **Indici/chiavi** nel menu **Progettazione tabelle**.
5. Selezionare l'indice che si desidera modificare. Le relative proprietà saranno visualizzate nella griglia principale.
6. Modificare le impostazioni di tutte le proprietà per personalizzare l'indice.
7. Fare clic su **Close**.
8. Selezionare **Salva** table_name **dal menu**_File_.

### <a name="to-modify-the-properties-of-an-index-in-object-explorer"></a>Per modificare le proprietà di un indice in Esplora oggetti

1. In Esplora oggetti fare clic sul segno più per espandere il database contenente la tabella in cui si desidera modificare le proprietà di un indice.
2. Fare clic sul segno più per espandere la cartella **Tabelle** .
3. Fare clic sul segno più per espandere la tabella nella quale modificare le proprietà di un indice.
4. Fare clic sul segno più per espandere la cartella **Indici** .
5. Fare clic con il pulsante destro del mouse sull'indice di cui si vogliono modificare le proprietà e scegliere **Proprietà**.
6. In **Selezione pagina** selezionare **Opzioni**.
7. Modificare le impostazioni di tutte le proprietà per personalizzare l'indice.
8. Per aggiungere, rimuovere o modificare la posizione di una colonna dell'indice, selezionare la pagina **Generale** della finestra di dialogo **Proprietà indice -** _nome_indice_. Per altre informazioni, vedere [Index Properties F1 Help](../../relational-databases/indexes/index-properties-f1-help.md)

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL

### <a name="to-see-the-properties-of-all-the-indexes-in-a-table"></a>Per visualizzare le proprietà di tutti gli indici in una tabella

L'esempio seguente mostra le proprietà di tutti gli indici in una tabella nel database AdventureWorks.

```sql
SELECT i.name AS index_name
   , i.type_desc
   , i.is_unique
   , ds.type_desc AS filegroup_or_partition_scheme
   , ds.name AS filegroup_or_partition_scheme_name
   , i.ignore_dup_key
   , i.is_primary_key
   , i.is_unique_constraint
   , i.fill_factor
   , i.is_padded
   , i.is_disabled
   , i.allow_row_locks
   , i.allow_page_locks
   , i.has_filter
   , i.filter_definition
FROM sys.indexes AS i
   INNER JOIN sys.data_spaces AS ds
      ON i.data_space_id = ds.data_space_id
   WHERE is_hypothetical = 0 AND i.index_id <> 0
       AND i.object_id = OBJECT_ID('HumanResources.Employee')
;
```

#### <a name="to-set-the-properties-of-an-index"></a>Per visualizzare le proprietà di un indice

L'esempio seguente imposta le proprietà degli indici nel database AdventureWorks.

[!code-sql[IndexDDL#AlterIndex4](../../relational-databases/indexes/codesnippet/tsql/set-index-options_1.sql)]

[!code-sql[IndexDDL#AlterIndex2](../../relational-databases/indexes/codesnippet/tsql/set-index-options_2.sql)]

Per altre informazioni, vedere [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).
