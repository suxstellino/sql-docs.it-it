---
description: COLLATE (Transact-SQL)
title: COLLATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COLLATE
- COLLATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- collations [SQL Server], COLLATE clause
- COLLATE clause
ms.assetid: 76763ac8-3e0d-4bbb-aa53-f5e7da021daa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d84dffab852ed071b75db0bef34d5db488a891f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184476"
---
# <a name="collate-transact-sql"></a>COLLATE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Definisce le regole di confronto di un database o di una colonna di database, o un'operazione di cast di regole di confronto se applicata all'espressione della stringa di caratteri. È possibile usare nomi di regole di confronto di Windows o SQL. Se non viene specificato durate la creazione del database, al database vengono assegnate le regole di confronto predefinite dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se non viene specificato durate la creazione della colonna della tabella, alla colonna vengono assegnate le regole di confronto predefinite del database.

![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintassi

```syntaxsql
COLLATE { <collation_name> | database_default }
<collation_name> :: =
    { Windows_collation_name } | { SQL_collation_name }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti

*collation_name* è il nome delle regole di confronto da applicare all'espressione o alla definizione di colonna o di database. In *collation_name* è possibile usare solo un *Windows_collation_name* o un *SQL_collation_name* specificato. *collation_name* deve essere un valore letterale. *collation_name* non può essere rappresentato da una variabile o un'espressione.

*Windows_collation_name* è il nome delle regole di confronto per un [Nome delle regole di confronto di Windows](../../t-sql/statements/windows-collation-name-transact-sql.md).

*SQL_collation_name* è il nome delle regole di confronto per un [Nome delle regole di confronto di SQL Server](../../t-sql/statements/sql-server-collation-name-transact-sql.md).

**database_default** specifica che la clausola COLLATE eredita le regole di confronto del database corrente.

## <a name="remarks"></a>Osservazioni

È possibile specificare la clausola COLLATE a vari livelli, Questi includono:

1. In fase di creazione o modifica di un database.

    La clausola COLLATE dell'istruzione `CREATE DATABASE` o `ALTER DATABASE` consente di specificare le regole di confronto predefinite del database. È inoltre possibile specificare una regola di confronto durante la creazione di un database tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Se non vengono specificate regole di confronto, al database verranno assegnate le regole di confronto predefinite dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

    > [!NOTE]
    > Le regole di confronto solo Unicode di Windows possono essere usate solo con la clausola COLLATE per applicare regole di confronto ai tipi di dati **nchar**, **nvarchar** e **ntext** su dati a livello di colonna e a livello di espressione. Queste regole non possono essere usate con la clausola COLLATE per definire o modificare le regole di confronto di un database o un'istanza del server.

2. In fase di creazione o modifica di una colonna di tabella.

    È possibile specificare le regole di confronto di ogni colonna contenente stringhe di caratteri tramite la clausola COLLATE dell'istruzione `CREATE TABLE` o `ALTER TABLE`. È inoltre possibile specificare una regola di confronto durante la creazione di una tabella tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Se non vengono specificate regole di confronto, alla colonna verranno assegnate le regole di confronto predefinite del database.

    È inoltre possibile usare l'opzione `database_default` nella clausola COLLATE per specificare che una colonna di una tabella temporanea usa le regole di confronto predefinite del database utente corrente per la connessione anziché quelle del database **tempdb**.

3. In fase di casting delle regole di confronto di un'espressione.

    Tramite la clausola COLLATE è possibile applicare un'espressione di caratteri a regole di confronto specifiche. Ai valori letterali e alle variabili di tipo carattere vengono assegnate le regole di confronto predefinite del database corrente. Ai riferimenti di colonna vengono assegnate le regole di confronto di definizione della colonna.

Le regole di confronto di un identificatore dipendono dal livello nel quale viene definito. Agli identificatori degli oggetti a livello di istanza, quali gli account di accesso e i nomi di database, vengono assegnate le regole di confronto predefinite dell'istanza. Agli identificatori degli oggetti di un database, quali tabelle, viste e nomi di colonna, vengono assegnate le regole di confronto predefinite del database. Ad esempio, due tabelle i cui nomi si differenziano soltanto per l'utilizzo del maiuscolo e del minuscolo possono essere create in un database con regole di confronto in cui l'uso di maiuscole e minuscole è rilevante, ma non in un database con regole di confronto in cui l'uso di maiuscole e minuscole non è rilevante. Per altre informazioni, vedere [Identificatori del database](../../relational-databases/databases/database-identifiers.md).

È possibile creare variabili, etichette GOTO, stored procedure temporanee e tabelle temporanee se il contesto di connessione è associato a un singolo database, e quindi fare riferimento ad esse quando il contesto passa a un altro database. Gli identificatori di variabili, le etichette GOTO, le stored procedure temporanee e le tabelle temporanee sono inclusi nelle regole di confronto predefinite dell'istanza del server.

È possibile applicare la clausola COLLATE solo ai tipi di dati **char**, **varchar**, **text**, **nchar**, **nvarchar** e **ntext**.

COLLATE usa *collate_name* per fare riferimento al nome delle regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o di Windows da applicare all'espressione, alla definizione di colonna o alla definizione del database. In *collation_name* è possibile usare solo un *Windows_collation_name* o un *SQL_collation_name* specificato e il parametro deve contenere un valore letterale. *collation_name* non può essere rappresentato da una variabile o un'espressione.

Le regole di confronto sono identificate in genere da un nome, ad eccezione che nel programma di installazione. Nel programma di installazione viene invece specificata la designazione delle regole di confronto radice (le impostazioni locali delle regole di confronto) per le regole di confronto di Windows, quindi vengono specificate le opzioni di ordinamento che possono rispettare o meno la distinzione tra maiuscole e minuscole e tra caratteri accentati e non accentati.

È possibile eseguire la funzione di sistema [fn_helpcollations](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md) per recuperare un elenco di tutti i nomi validi per le regole di confronto di Windows e SQL Server:

```sql
SELECT name, description
FROM fn_helpcollations();
```

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta solo le tabelle codici supportate dal sistema operativo corrente. Quando si esegue un'azione che dipende dalle regole di confronto, le regole di confronto di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzate dall'oggetto a cui si fa riferimento devono utilizzare una tabella codici supportata dal sistema operativo in esecuzione nel computer. Queste azioni includono:

- Impostazione delle regole di confronto predefinite per un database utilizzate durante la creazione o la modifica del database.
- Impostazione delle regole di confronto per una colonna utilizzate durante la creazione o la modifica di una tabella.
- Durante il ripristino o il collegamento di un database è necessario che le regole di confronto predefinite del database e quelle delle colonne o dei parametri di tipo **char**, **varchar** e **text** presenti nel database siano supportate dal sistema operativo.

> [!NOTE]
> Le conversioni tra tabelle codici sono supportate per i tipi di dati **char** e **varchar**, ma non per il tipo di dati **text**. L'eventuale perdita di dati che si verifica durante la conversione tra tabelle codici non viene segnalata.
>
> Se le regole di confronto specificate o adottate dall'oggetto cui viene fatto riferimento usano una tabella codici non supportata dai sistemi operativi Windows, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene visualizzato un errore.

## <a name="examples"></a>Esempi

### <a name="a-specifying-collation-during-a-select"></a>R. Specifica delle regole di confronto durante una SELEZIONE

Nell'esempio seguente viene creata la tabella semplice in cui vengono inserite 4 righe. Quindi vengono applicate due regole di confronto durante la selezione dei dati dalla tabella per dimostrare che `Chiapas` viene ordinato in modo differente.

```sql
CREATE TABLE Locations
(Place varchar(15) NOT NULL);
GO
INSERT Locations(Place) VALUES ('Chiapas'),('Colima')
                             , ('Cinco Rios'), ('California');
GO
--Apply an typical collation
SELECT Place FROM Locations
ORDER BY Place
COLLATE Latin1_General_CS_AS_KS_WS ASC;
GO
-- Apply a Spanish collation
SELECT Place FROM Locations
ORDER BY Place
COLLATE Traditional_Spanish_ci_ai ASC;
GO
```

Di seguito vengono riportati i risultati dalla prima query.

```output
Place
-------------
California
Chiapas
Cinco Rios
Colima
```

Di seguito vengono riportati i risultati dalla seconda query.

```output
Place
-------------
California
Cinco Rios
Colima
Chiapas
```

### <a name="b-additional-examples"></a>B. Esempi aggiuntivi

Per altri esempi in cui viene usata la clausola **COLLATE**, vedere [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md#examples), esempio **G. Creazione di un database e specifica di un nome delle regole di confronto e delle opzioni** e [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md#alter_column), esempio **V. Modifica delle regole di confronto di una colonna**.

## <a name="see-also"></a>Vedere anche

- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [Regole di confronto e supporto Unicode](../../relational-databases/collations/collation-and-unicode-support.md)
- [Precedenza delle regole di confronto](../../t-sql/statements/collation-precedence-transact-sql.md)
- [Costanti](../../t-sql/data-types/constants-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
- [Tipo di dati tabella](../../t-sql/data-types/table-transact-sql.md)
