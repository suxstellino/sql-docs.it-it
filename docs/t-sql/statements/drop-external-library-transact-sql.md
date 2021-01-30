---
description: DROP EXTERNAL LIBRARY (Transact-SQL)
title: DROP EXTERNAL LIBRARY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- DROP EXTERNAL LIBRARY
- DROP_EXTERNAL_LIBRARY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP EXTERNAL LIBRARY
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 681e7082c2019cba4924d96e1617dc0031a76c35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197582"
---
# <a name="drop-external-library-transact-sql"></a>DROP EXTERNAL LIBRARY (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Elimina una libreria di pacchetti esistente. Le librerie di pacchetti sono usate da runtime esterni supportati, ad esempio R, Python o Java.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
> [!NOTE]
> In SQL Server 2017 sono supportati il linguaggio R e la piattaforma Windows. R, Python e Java nelle piattaforme Windows e Linux sono supportati in SQL Server 2019 e versioni successive.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
> [!NOTE]
> In Istanza gestita di SQL di Azure sono supportati i linguaggi R e Python.
::: moniker-end

## <a name="syntax"></a>Sintassi

```syntaxsql
DROP EXTERNAL LIBRARY library_name
[ AUTHORIZATION owner_name ];
```

### <a name="arguments"></a>Argomenti

**library_name**

Specifica il nome di una libreria di pacchetti esistente.

Le librerie hanno un ambito di tipo utente. I nomi delle librerie devono essere univoci nel contesto di un utente o proprietario specifico.

**owner_name**

Specifica il nome dell'utente o del ruolo che è proprietario della libreria esterna.

I proprietari di database possono eliminare le librerie create da altri utenti.

## <a name="permissions"></a>Autorizzazioni

Per eliminare una libreria è necessario il privilegio ALTER ANY EXTERNAL LIBRARY. Per impostazione predefinita, anche un proprietario del database o il proprietario dell'oggetto può eliminare una libreria esterna.

### <a name="return-values"></a>Valori restituiti

Se l'istruzione ha esito positivo viene restituito un messaggio informativo.

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Osservazioni

A differenza di altre istruzioni `DROP` in SQL Server, questa istruzione supporta la specifica di una clausola di autorizzazione facoltativa. In questo modo gli utenti **dbo** o gli utenti con ruolo **db_owner** possono eliminare una libreria di pacchetti caricata nel database da un utente comune.

Una serie di pacchetti, detti *pacchetti di sistema*, sono preinstallati in un'istanza di SQL. L'utente non può aggiungere, aggiornare o rimuovere i pacchetti di sistema.

## <a name="examples"></a>Esempi

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
Aggiungere il pacchetto R personalizzato, `customPackage`, a un database:

```sql
CREATE EXTERNAL LIBRARY customPackage 
FROM (CONTENT = 'C:\temp\customPackage_v1.1.zip')
WITH (LANGUAGE = 'R');
GO
```
::: moniker-end

Eliminare la libreria `customPackage`.

```sql
DROP EXTERNAL LIBRARY customPackage;
```

## <a name="see-also"></a>Vedere anche

[CREATE EXTERNAL LIBRARY (Transact-SQL)](create-external-library-transact-sql.md)  
[ALTER EXTERNAL LIBRARY (Transact-SQL)](alter-external-library-transact-sql.md)  
[sys.external_library_files](../../relational-databases/system-catalog-views/sys-external-library-files-transact-sql.md)  
[sys.external_libraries](../../relational-databases/system-catalog-views/sys-external-libraries-transact-sql.md)  
