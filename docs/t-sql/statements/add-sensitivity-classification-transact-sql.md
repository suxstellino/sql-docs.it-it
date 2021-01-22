---
description: ADD SENSITIVITY CLASSIFICATION (Transact-SQL)
title: ADD SENSITIVITY CLASSIFICATION (Transact-SQL)
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
f1_keywords:
- ADD SENSITIVITY CLASSIFICATION
- ADD_SENSITIVITY_CLASSIFICATION
helpviewer_keywords:
- ADD SENSITIVITY CLASSIFICATION statement
- add labels
- adding labels
- adding labels
- classification [SQL]
- labels [SQL]
- information types
- data classification
- rank
ms.custom: ''
ms.date: 06/10/2020
monikerRange: " >= sql-server-linux-ver15 || >= sql-server-ver15 || = azuresqldb-current"
ms.openlocfilehash: 8aa8b00a9a24513eb925ef113db94caaea0cfe50
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688836"
---
# <a name="add-sensitivity-classification-transact-sql"></a>ADD SENSITIVITY CLASSIFICATION (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Aggiunge i metadati sulla classificazione di riservatezza a una o più colonne di database. La classificazione può includere un'etichetta di riservatezza e un tipo di informazioni.

Per SQL Server, questa istruzione è stata introdotta con SQL Server 2019.

Classificare i dati sensibili nell'ambiente di database consente di ottenere visibilità estesa e una protezione superiore. Per altre informazioni, vedere [Introduzione a SQL Information Protection](/azure/azure-sql/database/data-discovery-and-classification-overview)

## <a name="syntax"></a>Sintassi

```syntaxsql
    ADD SENSITIVITY CLASSIFICATION TO
    <object_name> [, ...n ]
    WITH ( <sensitivity_option> [, ...n ] )

<object_name> ::=
{
    [schema_name.]table_name.column_name
}

<sensitivity_option> ::=  
{
    LABEL = string |
    LABEL_ID = guidOrString |
    INFORMATION_TYPE = string |
    INFORMATION_TYPE_ID = guidOrString |
    RANK = NONE | LOW | MEDIUM | HIGH | CRITICAL
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti  

*object_name* ([schema_name.]table_name.column_name)

È il nome della colonna di database da classificare. Attualmente è supportata solo la classificazione delle colonne.
    - *schema_name* (facoltativo): si tratta del nome dello schema a cui appartiene la colonna classificata.
    - *table_name*: si tratta del nome della tabella a cui appartiene la colonna classificata.
    - *column_name*: si tratta del nome della colonna classificata.

*LABEL*

È il nome leggibile dell'etichetta di riservatezza. Le etichette di riservatezza rappresentano il livello di riservatezza dei dati archiviati nella colonna di database.

*LABEL_ID*

È un identificatore associato all'etichetta di riservatezza. Viene spesso usato dalle piattaforme centralizzate di protezione delle informazioni per identificare in modo univoco le etichette nel sistema.

*INFORMATION_TYPE*

È il nome leggibile del tipo di informazioni. I tipi di informazioni vengono usati per descrivere il tipo di dati da archiviare nella colonna di database.

*INFORMATION_TYPE_ID*

È un identificatore associato al tipo di informazioni. Viene spesso usato dalle piattaforme centralizzate di protezione delle informazioni per identificare in modo univoco i tipi di informazioni nel sistema.

*RANK*

È un identificatore basato su un set predefinito di valori che definiscono il livello di riservatezza. Usato da altri servizi, ad esempio Advanced Threat Protection, per rilevare le anomalie in base alla classificazione.

## <a name="remarks"></a>Osservazioni  

- A un singolo oggetto è possibile aggiungere una sola classificazione. Aggiungendo una classificazione a un oggetto già classificato, la classificazione esistente verrà sovrascritta.
- È possibile classificare più oggetti usando un singola istruzione `ADD SENSITIVITY CLASSIFICATION`.
- La vista di sistema [sys.sensitivity_classifications](../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md) può essere usata per recuperare le informazioni sulla classificazione di riservatezza per un database.

## <a name="permissions"></a>Autorizzazioni

È necessaria l'autorizzazione ALTER ANY SENSITIVITY CLASSIFICATION. L'autorizzazione ALTER ANY SENSITIVITY CLASSIFICATION è implicita nell'autorizzazione del database ALTER o nell'autorizzazione del server CONTROL SERVER.

## <a name="examples"></a>Esempi  

### <a name="a-classifying-two-columns"></a>R. Classificazione di due colonne

L'esempio seguente classifica le colonne **dbo.sales.price** e **dbo.sales.discount** con l'etichetta di riservatezza **Highly Confidential**, la classificazione **Critical** e il tipo di informazioni **Financial**.

```sql
ADD SENSITIVITY CLASSIFICATION TO
    dbo.sales.price, dbo.sales.discount
    WITH ( LABEL='Highly Confidential', INFORMATION_TYPE='Financial', RANK=CRITICAL )
```  

### <a name="b-classifying-only-a-label"></a>B. Classificazione di una sola etichetta

L'esempio seguente classifica la colonna **dbo.customer.comments** con l'etichetta **Confidential** e l'ID etichetta **643f7acd-776a-438d-890c-79c3f2a520d6**. Il tipo di informazioni non è classificato per questa colonna.

```sql
ADD SENSITIVITY CLASSIFICATION TO
    dbo.customer.comments
    WITH ( LABEL='Confidential', LABEL_ID='643f7acd-776a-438d-890c-79c3f2a520d6' )
```  

## <a name="see-also"></a>Vedere anche

- [DROP SENSITIVITY CLASSIFICATION (Transact-SQL)](../../t-sql/statements/drop-sensitivity-classification-transact-sql.md)
- [sys.sensitivity_classifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md)
- [Autorizzazioni (Motore di database)](../../relational-databases/security/permissions-database-engine.md)
- [Introduzione a SQL Information Protection](/azure/azure-sql/database/data-discovery-and-classification-overview)