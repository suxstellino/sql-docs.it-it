---
description: COLUMNPROPERTY (Transact-SQL)
title: COLUMNPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COLUMNPROPERTY
- COLUMNPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- column properties [SQL Server]
- parameters [SQL Server], properties
- COLUMNPROPERTY function
ms.assetid: 2408c264-6eca-4120-bb71-df043c7c2792
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7d2827c3945423125262a8f4e6842ef255c863e0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161248"
---
# <a name="columnproperty-transact-sql"></a>COLUMNPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Questa funzione restituisce informazioni di colonna o parametro.
  
![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
COLUMNPROPERTY ( id , column , property )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
*id*  
[Espressione](../../t-sql/language-elements/expressions-transact-sql.md) che include l'identificatore (ID) della tabella o procedura.
  
*column*  
Espressione che include il nome di colonna o di parametro.
  
*property*  
Per l'argomento *id*, l'argomento *property* specifica il tipo di informazioni che verranno restituite dalla funzione `COLUMNPROPERTY`. I valori possibili per l'argomento *property* sono i seguenti:
  
|valore|Descrizione|Valore restituito|  
|---|---|---|
|**AllowsNull**|Ammette valori Null.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido.|  
|**ColumnId**|Valore di ID di colonna corrispondente a **sys.columns.column_id**.|ID colonna<br /><br /> **Nota:** quando si eseguono query su più colonne, potrebbero apparire spazi vuoti nella sequenza dei valori di ID di colonna.|  
|**FullTextTypeColumn**|L'elemento TYPE COLUMN nella tabella in cui sono contenute le informazioni sui tipi di documenti di *column*.|ID di TYPE COLUMN full-text per l'espressione del nome della colonna passata come secondo parametro della funzione.|  
|**GeneratedAlwaysType**|Valore di colonna generato dal sistema. Corrisponde a **sys.columns.generated_always_type**|**Si applica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] e versioni successive.<br /><br /> 0: non sempre generato<br /><br /> 1: sempre generato all'inizio della riga<br /><br /> 2: sempre generato alla fine della riga|  
|**IsColumnSet**|La colonna è un set di colonne. Per altre informazioni, vedere [Usare set di colonne](../../relational-databases/tables/use-column-sets.md).|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido.|  
|**IsComputed**|La colonna è una colonna calcolata.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido.|  
|**IsCursorType**|Il parametro di procedura è di tipo CURSOR.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido.|  
|**IsDeterministic**|La colonna è tipo deterministico. Questa proprietà viene applicata solo alle colonne calcolate e alle colonne di una vista.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido. Non si tratta di una colonna calcolata o di una colonna di vista.|  
|**IsFulltextIndexed**|La colonna è stata registrata per l'indicizzazione full-text.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido.|  
|**IsHidden**|Valore di colonna generato dal sistema. Corrisponde a **sys.columns.is_hidden**|**Si applica a**: [!INCLUDE[ssCurrentLong](../../includes/sscurrent-md.md)] e versioni successive.<br /><br /> 0: non nascosto<br /><br /> 1: nascosto|  
|**IsIdentity**|La colonna utilizza la proprietà IDENTITY.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido.|  
|**IsIdNotForRepl**|La colonna controlla l'impostazione IDENTITY_INSERT.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido.|  
|**IsIndexable**|La colonna può essere indicizzata.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido.|  
|**IsOutParam**|Il parametro di procedura è di output.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido.|  
|**IsPrecise**|La colonna è precisa. Questa proprietà viene applicata solo a colonne di tipo deterministico.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido. Non si tratta di una colonna di tipo deterministico.|  
|**IsRowGuidCol**|La colonna è di tipo di dati **uniqueidentifier** e viene definita con la proprietà ROWGUIDCOL.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido.|  
|**IsSparse**|La colonna è una colonna di tipo sparse. Per altre informazioni, vedere [Usare le colonne di tipo sparse](../../relational-databases/tables/use-sparse-columns.md).|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido.|  
|**IsSystemVerified**|Le proprietà di determinismo e precisione della colonna possono essere verificate dal [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Questa proprietà è applicabile solo alle colonne calcolate e alle colonne di viste.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido.|  
|**IsXmlIndexable**|La colonna XML può essere utilizzata in un indice XML|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido.|  
|**Precisione**|Lunghezza del tipo di dati della colonna o del parametro.|Lunghezza del tipo di dati specificato per la colonna.<br /><br /> -1: **xml** o tipi di dati per valori di grandi dimensioni<br /><br /> NULL: input non valido.|  
|**Ridimensionamento**|Scala del tipo di dati della colonna o del parametro.|Valore di scalabilità<br /><br /> NULL: input non valido.|  
|**StatisticalSemantics**|La colonna è abilitata per l'indicizzazione semantica.|1: TRUE<br /><br /> 0: FALSE|  
|**SystemDataAccess**|La colonna deriva da una funzione che accede ai dati nei cataloghi di sistema o nelle tabelle virtuali di sistema di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa proprietà è applicabile solo alle colonne calcolate e alle colonne di viste.|1 TRUE (accesso di sola lettura).<br /><br /> 0: FALSE<br /><br /> NULL: input non valido.|  
|**UserDataAccess**|La colonna deriva da una funzione che accede ai dati nelle tabelle utente, incluse le tabelle temporanee e le viste, archiviate nell'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Questa proprietà è applicabile solo alle colonne calcolate e alle colonne di viste.|1 TRUE (accesso di sola lettura).<br /><br /> 0: FALSE<br /><br /> NULL: input non valido.|  
|**UsesAnsiTrim**|ANSI_PADDING è stato impostato su On al momento della creazione della tabella. Questa proprietà viene applicata solo alle colonne o ai parametri di tipo **char** o **varchar**.|1: TRUE<br /><br /> 0: FALSE<br /><br /> NULL: input non valido.|  
  
## <a name="return-types"></a>Tipi restituiti
 **int**  
  
## <a name="exceptions"></a>Eccezioni  
Restituisce NULL in caso di errore o se un chiamante non ha l'autorizzazione necessaria per visualizzare l'oggetto.
  
Un utente può visualizzare esclusivamente i metadati delle entità a sicurezza diretta di cui è proprietario o per cui ha ricevuto un'autorizzazione. Di conseguenza, le funzioni predefinite di creazione dei metadati come `COLUMNPROPERTY` possono restituire NULL se l'utente non ha l'autorizzazione corretta per l'oggetto. Per altre informazioni, vedere [Configurazione della visibilità dei metadati](../../relational-databases/security/metadata-visibility-configuration.md).
  
## <a name="remarks"></a>Osservazioni  
Durante il controllo della proprietà deterministica di una colonna, è necessario per prima cosa verificare se la colonna è calcolata. L'argomento **IsDeterministic** restituisce NULL per le colonne non calcolate. È possibile specificare le colonne calcolate come colonne di indice.
  
## <a name="examples"></a>Esempi  
Nell'esempio viene restituita la lunghezza della colonna `LastName`.
  
```sql
USE AdventureWorks2012;  
GO  
SELECT COLUMNPROPERTY( OBJECT_ID('Person.Person'),'LastName','PRECISION')AS 'Column Length';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Column Length
-------------
50
```  
  
## <a name="see-also"></a>Vedere anche
[Funzioni per i metadati &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)  
[TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md)
  
  
