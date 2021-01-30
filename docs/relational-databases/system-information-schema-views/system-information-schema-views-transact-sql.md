---
description: Viste degli schemi delle informazioni di sistema (Transact-SQL)
title: Viste degli schemi delle informazioni di sistema (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- information schema views
- schemas [SQL Server], information schema views
- metadata [SQL Server], views
- views [SQL Server], information schema
- system views [SQL Server], information schema
ms.assetid: 7e9f1dfe-27e9-40e7-8fc7-bfc5cae6be10
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a39ec8c22b4267e1c6b6bc2f213775836b073a48
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185479"
---
# <a name="system-information-schema-views-transact-sql"></a>Viste degli schemi delle informazioni di sistema (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Una vista dello schema delle informazioni rappresenta uno dei metodi disponibili in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per ottenere metadati. Le viste degli schemi delle informazioni offrono una panoramica interna dei metadati di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] indipendente dalle tabelle di sistema, nonché garantiscono il corretto funzionamento delle applicazioni anche se sono state apportate modifiche significative alle tabelle di sistema sottostanti. Le viste degli schemi delle informazioni incluse in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono conformi alla definizione dello standard ISO per INFORMATION_SCHEMA.

> [!IMPORTANT]
> Alle viste degli schemi delle informazioni sono state apportate alcune modifiche che non garantiscono la compatibilità con le versioni precedenti. Tali modifiche sono descritte negli argomenti specifici relativi alle viste interessate.

Per i riferimenti al server corrente, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta i nomi composti da tre parti. La stessa convenzione di denominazione viene adottata anche dallo standard ISO. I nomi utilizzati nelle due convenzioni di denominazione sono tuttavia diversi. Le viste degli schemi delle informazioni sono definite in uno schema speciale denominato INFORMATION_SCHEMA. Questo schema è incluso in ogni database. Ogni vista dello schema delle informazioni include metadati per tutti gli oggetti dati archiviati nel database specifico. Nella tabella seguente sono riportate le relazioni tra i nomi di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e i nomi SQL standard.

|Nome di SQL Server|Nome SQL standard equivalente|
|---------------------|-----------------------------------------------|
|Database|Catalogo|
|SCHEMA|SCHEMA|
|Oggetto|Oggetto|
|Tipo di dati definito dall'utente|Dominio|

Questa convenzione di mapping dei nomi è valida per le viste di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compatibili con lo standard ISO riportate di seguito.

:::row:::
    :::column:::
        [CHECK_CONSTRAINTS](../../relational-databases/system-information-schema-views/check-constraints-transact-sql.md)

        [COLUMN_DOMAIN_USAGE](../../relational-databases/system-information-schema-views/column-domain-usage-transact-sql.md)

        [COLUMN_PRIVILEGES](../../relational-databases/system-information-schema-views/column-privileges-transact-sql.md)

        [COLUMNS](../../relational-databases/system-information-schema-views/columns-transact-sql.md)

        [CONSTRAINT_COLUMN_USAGE](../../relational-databases/system-information-schema-views/constraint-column-usage-transact-sql.md)

        [CONSTRAINT_TABLE_USAGE](../../relational-databases/system-information-schema-views/constraint-table-usage-transact-sql.md)

        [DOMAIN_CONSTRAINTS](../../relational-databases/system-information-schema-views/domain-constraints-transact-sql.md)

        [DOMINI](../../relational-databases/system-information-schema-views/domains-transact-sql.md)

        [KEY_COLUMN_USAGE](../../relational-databases/system-information-schema-views/key-column-usage-transact-sql.md)

        [PARAMETERS](../../relational-databases/system-information-schema-views/parameters-transact-sql.md)
    :::column-end:::
    :::column:::
        [REFERENTIAL_CONSTRAINTS](../../relational-databases/system-information-schema-views/referential-constraints-transact-sql.md)

        [ROUTINES](../../relational-databases/system-information-schema-views/routines-transact-sql.md)

        [ROUTINE_COLUMNS](../../relational-databases/system-information-schema-views/routine-columns-transact-sql.md)

        [SCHEMATA](../../relational-databases/system-information-schema-views/schemata-transact-sql.md)

        [TABLE_CONSTRAINTS](../../relational-databases/system-information-schema-views/table-constraints-transact-sql.md)

        [TABLE_PRIVILEGES](../../relational-databases/system-information-schema-views/table-privileges-transact-sql.md)

        [TABLES](../../relational-databases/system-information-schema-views/tables-transact-sql.md)

        [VIEW_COLUMN_USAGE](../../relational-databases/system-information-schema-views/view-column-usage-transact-sql.md)

        [VIEW_TABLE_USAGE](../../relational-databases/system-information-schema-views/view-table-usage-transact-sql.md)

        [VIEWS](../../relational-databases/system-information-schema-views/views-transact-sql.md)
    :::column-end:::
:::row-end:::

Alcune viste contengono inoltre riferimenti a classi di dati diverse, ad esempio dati di tipo carattere o dati binari.

Quando si fa riferimento alle viste dello schema delle informazioni, è necessario utilizzare un nome completo che includa il nome schema `INFORMATION_SCHEMA`. Ad esempio:

```sql
SELECT TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME, COLUMN_DEFAULT
FROM AdventureWorks2012.INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = N'Product';
```

## <a name="permissions"></a>Autorizzazioni  
La visibilità dei metadati nelle viste degli schemi delle informazioni è limitata alle entità a protezione diretta di cui l'utente è proprietario o per le quali dispone di autorizzazioni. Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).

> [!NOTE]  
> Le viste degli schemi delle informazioni sono definite a livello di server e pertanto non possono essere negate nel contesto di un database utente. Per REVOCAre o NEGAre l'accesso (SELECT), è necessario utilizzare il database master. Per impostazione predefinita, il ruolo public dispone dell'autorizzazione SELECT per tutte le viste degli schemi delle informazioni, ma il contenuto è limitato alle regole di visibilità dei metadati.

## <a name="see-also"></a>Vedere anche

- [Viste di sistema &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)
- [Tipi di dati &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
- [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md) 
