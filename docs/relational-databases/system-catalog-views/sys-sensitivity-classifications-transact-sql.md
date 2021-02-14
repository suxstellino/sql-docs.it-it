---
description: sys.sensitivity_classifications (Transact-SQL)
title: sys.sensitivity_classifications (Transact-SQL) | Microsoft Docs
ms.date: 03/25/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
ms.custom: ''
ms.author: mibar
author: barmichal
f1_keywords:
- 'sys.sensitivity_classifications '
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sensitivity_classifications statement
- dropping labels
- drop labels
- removing labels
- remove labels
- classification [SQL]
- labels [SQL]
- information types
- rank
monikerRange: '>= sql-server-ver15 || = azuresqldb-current || = azure-sqldw-latest'
ms.openlocfilehash: 1945926624b6f0a9c996c0be624e32b49e07345b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343137"
---
# <a name="syssensitivity_classifications-transact-sql"></a>sys.sensitivity_classifications (Transact-SQL)
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Restituisce una riga per ogni elemento classificato nel database.

|Nome colonna|Tipo di dati|Descrizione|
|-----------------|---------------|-----------------|  
|**class**|**int**|Identifica la classe dell'elemento in cui esiste la classificazione. Avrà sempre il valore 1 (che rappresenta una colonna)|  
|**class_desc**|**varchar (16)**|Descrizione della classe dell'elemento in cui esiste la classificazione. il valore sarà sempre *OBJECT_OR_COLUMN*|  
|**major_id**|**int**|Rappresenta l'ID della tabella contenente la colonna classificata, corrispondente a sys. All _objects. object_id|  
|**minor_id**|**int**|Rappresenta l'ID della colonna in cui esiste la classificazione, corrispondente a sys. All _columns. column_id|   
|**label**|**sysname**|Etichetta (leggibile) assegnata per la classificazione di riservatezza|  
|**label_id**|**sysname**|ID associato all'etichetta, che può essere usato da un sistema di protezione delle informazioni, ad esempio Azure Information Protection (AIP)|  
|**information_type**|**sysname**|Tipo di informazioni (leggibile) assegnato per la classificazione di riservatezza|  
|**information_type_id**|**sysname**|ID associato al tipo di informazioni, che può essere usato da un sistema di protezione delle informazioni, ad esempio Azure Information Protection (AIP)|  
|**Rank**|**int**|Valore numerico del rango: <br><br>0 per nessuno<br>10 per basso<br>20 per media<br>30 per alta<br>40 per CRITICAL| 
|**rank_desc**|**sysname**|Rappresentazione testuale del rango:  <br><br>NONE, LOW, MEDIUM, HIGH, CRITICAL|  
| &nbsp; | &nbsp; | &nbsp; |

## <a name="remarks"></a>Commenti  

- Questa visualizzazione fornisce visibilità sullo stato di classificazione del database. Può essere utilizzato per la gestione delle classificazioni dei database, nonché per la generazione di report.
- Attualmente è supportata solo la classificazione delle colonne del database.
 
## <a name="examples"></a>Esempi

### <a name="a-listing-all-classified-columns-and-their-corresponding-classification"></a>R. Elenco di tutte le colonne classificate e della classificazione corrispondente

Nell'esempio seguente viene restituita una tabella in cui sono elencati il nome della tabella, il nome della colonna, l'etichetta, l'ID dell'etichetta, il tipo di informazioni, l'ID del tipo di informazione, il rango e la descrizione di rango per ogni colonna classificata nel

> [!NOTE]
> Label è una parola chiave per Azure sinapsi Analytics.

```sql
SELECT
    SCHEMA_NAME(sys.all_objects.schema_id) as SchemaName,
    sys.all_objects.name AS [TableName], sys.all_columns.name As [ColumnName],
    [Label], [Label_ID], [Information_Type], [Information_Type_ID], [Rank], [Rank_Desc]
FROM
          sys.sensitivity_classifications
left join sys.all_objects on sys.sensitivity_classifications.major_id = sys.all_objects.object_id
left join sys.all_columns on sys.sensitivity_classifications.major_id = sys.all_columns.object_id
                         and sys.sensitivity_classifications.minor_id = sys.all_columns.column_id
```

## <a name="permissions"></a>Autorizzazioni  
 Richiede l'autorizzazione **View any Sensitivity Classification** . 
 
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Per altre informazioni, vedere [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  

## <a name="see-also"></a>Vedere anche  

[ADD SENSITIVITY CLASSIFICATION (Transact-SQL)](../../t-sql/statements/add-sensitivity-classification-transact-sql.md)

[DROP SENSITIVITY CLASSIFICATION (Transact-SQL)](../../t-sql/statements/drop-sensitivity-classification-transact-sql.md)

[Introduzione a SQL Information Protection](/azure/azure-sql/database/data-discovery-and-classification-overview)