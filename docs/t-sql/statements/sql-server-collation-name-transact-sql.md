---
description: Nome delle regole di confronto di SQL Server (Transact-SQL)
title: Nome delle regole di confronto di SQL Server (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- collations [SQL Server], SQL collations
- SQL collations
- names [SQL Server], collations
ms.assetid: 56483d24-add7-483d-9b96-c6fda460ddbc
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8fa8fc550e166b24fcbadd1ca38587a63bc34204
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98080101"
---
# <a name="sql-server-collation-name-transact-sql"></a>Nome delle regole di confronto di SQL Server (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Singola stringa che specifica il nome regole di confronto per le regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sono supportate le regole di confronto di Windows. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è inoltre supportato un numero limitato (<80) di regole di confronto definite regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sviluppate prima che le regole di confronto di Windows venissero supportate in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vengono ancora supportate per la compatibilità con le versioni precedenti, ma è consigliabile non utilizzarle per nuovi progetti di sviluppo. Per altre informazioni sulle regole di confronto di Windows, vedere [Nome delle regole di confronto di Windows](../../t-sql/statements/windows-collation-name-transact-sql.md).

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql
<SQL_collation_name> :: =
SQL_SortRules[_Pref]_CPCodepage_<ComparisonStyle>

<ComparisonStyle> ::=
_CaseSensitivity_AccentSensitivity | _BIN
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*SortRules* è una stringa che identifica l'alfabeto o la lingua di cui vengono applicate le regole di ordinamento quando si specifica l'ordinamento del dizionario, ad esempio Latin1_General o Polish.

**Pref** specifica come preferenza l'uso del maiuscolo. Anche se nel confronto non viene applicata la distinzione tra maiuscole e minuscole, la versione maiuscola di una lettera precede la versione minuscola, se non vengono applicate altre distinzioni.

*Codepage* specifica un numero composto da 1 a 4 cifre che identifica la tabella codici usata dalle regole di confronto. **CP1** specifica la tabella codici 1252, mentre per tutte le altre tabelle codici viene specificato il numero completo corrispondente. Ad esempio, **CP1251** specifica la tabella codici 1251 e **CP850** specifica la tabella codici 850.

*CaseSensitivity*
**CI** specifica che non viene applicata la distinzione tra maiuscole e minuscole, mentre **CS** indica che viene applicata.

*AccentSensitivity*
**AI** specifica che non viene applicata la distinzione tra caratteri accentati e non accentati, mentre **AS** indica che viene applicata.

**BIN** specifica che deve essere usato il tipo di ordinamento binario.

## <a name="remarks"></a>Commenti

Per elencare le regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supportati dal server, eseguire la query seguente.

```sql
SELECT * FROM sys.fn_helpcollations()
WHERE name LIKE 'SQL%';
```

> [!NOTE]
> Per l'ID del tipo di ordinamento 80, usare le regole di confronto di Windows desiderate con la tabella codici 1250 e l'ordinamento binario, ad esempio Albanian_BIN, Croatian_BIN, Czech_BIN, Romanian_BIN, Slovak_BIN, Slovenian_BIN.

## <a name="see-also"></a>Vedere anche

- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [Costanti](../../t-sql/data-types/constants-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
- [tabella](../../t-sql/data-types/table-transact-sql.md)
- [sys.fn_helpcollations](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)
