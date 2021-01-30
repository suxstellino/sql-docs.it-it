---
description: semanticsimilaritytable (Transact-SQL)
title: SEMANTICSIMILARITYTABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- semanticsimilaritytable
- semanticsimilaritytable_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- semanticsimilaritytable function
ms.assetid: b49d40ab-7552-438b-ad67-6237dcccb75b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 532ec509de094db343406b6cb0f9f0cca924a724
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194561"
---
# <a name="semanticsimilaritytable-transact-sql"></a>semanticsimilaritytable (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Restituisce una tabella di zero, una o più righe per documenti il cui contenuto nelle colonne specificate è semanticamente simile a un determinato documento.  
  
 A questa funzione del set di righe è possibile fare riferimento nella clausola FROM di un'istruzione SELECT come normale nome di tabella.  

 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```sql  
SEMANTICSIMILARITYTABLE  
    (  
    table,  
    { column | (column_list) | * },  
    source_key  
    )  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Argomenti  
 **tabella**  
 Nome di una tabella per cui è abilitata l'indicizzazione full-text e semantica.  
  
 Questo nome può essere costituito da una a quattro parti, ma non è consentito un nome di server remoto.  
  
 **column**  
 Nome della colonna indicizzata per cui restituire risultati. Per la colonna deve essere abilitata l'indicizzazione semantica.  
  
 **column_list**  
 Indica diverse colonne, separate da una virgola e racchiuse tra parentesi. Per tutte le colonne deve essere abilitata l'indicizzazione semantica.  
  
 **\** _  
 Indica che tutte le colonne per cui l'indicizzazione semantica è abilitata sono incluse.  
  
 _ *source_key**  
 Chiave univoca per la riga per richiedere risultati per una riga specifica.  
  
 Laddove possibile, la chiave viene convertita in modo implicito nel tipo della chiave univoca full-text nella tabella di origine. La chiave può essere specificata come costante o variabile, ma non può essere un'espressione o il risultato di una sottoquery scalare.  
  
## <a name="table-returned"></a>Tabella restituita  
 Nella tabella seguente vengono descritte le informazioni su documenti simili o correlati restituiti da questa funzione per i set di righe.  
  
 Se i risultati vengono richiesti per più di una colonna, i documenti corrispondenti vengono restituiti in base a ogni colonna.  
  
|Nome della colonna|Tipo|Descrizione|  
|------------------|----------|-----------------|  
|**source_column_id**|**int**|ID della colonna da cui è stato utilizzato un documento di origine per la ricerca di documenti simili.<br /><br /> Vedere le funzioni COL_NAME e COLUMNPROPERTY per informazioni dettagliate su come recuperare il nome di colonna da column_id e viceversa.|  
|**matched_column_id**|**int**|ID della colonna da cui è stato trovato un documento simile.<br /><br /> Vedere le funzioni COL_NAME e COLUMNPROPERTY per informazioni dettagliate su come recuperare il nome di colonna da column_id e viceversa.|  
|**matched_document_key**|**\** _<br /><br /> Questa chiave corrisponde al tipo della chiave univoca nella tabella di origine.|Valore della chiave univoca di estrazione full-text e semantica della riga o del documento individuato come simile al documento specificato nella query.|  
|_ *Punteggio**|**REAL**|Valore relativo per la somiglianza del documento nella sua relazione con tutti gli altri documenti simili.<br /><br /> Il valore è un valore decimale frazionario compreso nell'intervallo [0.0, 1.0], dove un punteggio maggiore rappresenta una corrispondenza più vicina e 1.0 costituisce un punteggio perfetto.|  
  
## <a name="general-remarks"></a>Osservazioni generali  
 Per altre informazioni, vedere [trovare documenti simili e correlati con la ricerca semantica](../../relational-databases/search/find-similar-and-related-documents-with-semantic-search.md).  
  
## <a name="limitations-and-restrictions"></a>Limitazioni e restrizioni  
 Non è possibile eseguire una query su diverse colonne per ottenere documenti simili. La funzione **SEMANTICSIMILARITYTABLE** recupera solo documenti simili dalla stessa colonna della colonna di origine, identificata dall'argomento **source_key** .  
  
## <a name="metadata"></a>Metadati  
 Per informazioni generali e sullo stato relative all'estrazione e al popolamento della somiglianza semantica, eseguire una query sulle DMV seguenti:  
  
-   [sys.dm_db_fts_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-fts-index-physical-stats-transact-sql.md)  
  
-   [sys.dm_fts_semantic_similarity_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-semantic-similarity-population-transact-sql.md)  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 Sono necessarie autorizzazioni SELECT per la tabella di base in cui sono stati creati gli indici full-text e semantico.  
  
## <a name="examples"></a>Esempio  
 Nell'esempio seguente vengono recuperati i primi 10 candidati simili a un candidato specificato dalla tabella HumanResources.JobCandidate nel database di esempio AdventureWorks2012.  
  
```scr  
SELECT TOP(10) KEY_TBL.matched_document_key AS Candidate_ID  
FROMSEMANTICSIMILARITYTABLE  
    (  
    HumanResources.JobCandidate,  
    Resume,  
    @CandidateID  
    ) AS KEY_TBL  
ORDER BY KEY_TBL.score DESC;  
  
```  
  
  
