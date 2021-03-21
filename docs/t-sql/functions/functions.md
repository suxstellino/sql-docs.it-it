---
description: Quali sono le funzioni del database SQL di Microsoft?
title: Quali sono le funzioni del database SQL di Microsoft? | Microsoft Docs
ms.custom: ''
ms.date: 06/28/2017
ms.prod: sql
ms.prod_service: synapse-analytics, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- built-in functions [SQL Server]
- function [SQL Server] See functions [SQL Server]
- functions [Transact-SQL]
- functions [SQL Server], about functions
- scalar functions
- functions [SQL Server]
ms.assetid: 17186213-5ab5-40b0-b470-b660af1ec44c
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 68ea2973865ed43d357c99e7ad285c021f68f9bc
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747381"
---
# <a name="what-are-the-sql-database-functions"></a>Quali sono le funzioni del database SQL?
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Informazioni sulle categorie di funzioni predefinite che è possibile usare con i database SQL. È possibile usare le funzioni predefinite o creare funzioni definite dall'utente.
  
## <a name="aggregate-functions"></a>Funzioni di aggregazione

Le funzioni di aggregazione eseguono un calcolo su un set di valori e restituiscono un singolo valore. Sono consentite nell'elenco di selezione o nella clausola HAVING di un'istruzione SELECT. È possibile usare un'aggregazione in combinazione con la clausola GROUP BY per calcolare l'aggregazione in categorie di righe. Usare la clausola OVER per calcolare l'aggregazione in un intervallo specifico di valori. La clausola OVER non può seguire le aggregazioni GROUPING o GROUPING_ID.

Tutte le funzioni di aggregazione sono deterministiche, ovvero restituiscono sempre lo stesso valore quando vengono eseguite negli stessi valori di input. Per altre informazioni, vedere [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).

## <a name="analytic-functions"></a>Funzioni analitiche
Le funzioni analitiche calcolano un valore di aggregazione basato su un gruppo di righe. Tuttavia, a differenza delle funzioni di aggregazione, le funzioni analitiche sono in grado di restituire più righe per ogni gruppo. È possibile usare le funzioni analitiche per calcolare medie mobili, totali parziali, percentuali o i primi N risultati all'interno di un gruppo.

## <a name="ranking-functions"></a>Funzioni di rango
Le funzioni di rango restituiscono un valore di rango per ogni riga di una partizione. In base alla funzione utilizzata, è possibile che venga assegnato lo stesso valore a più righe. Le funzioni di rango non sono deterministiche.

## <a name="rowset-functions"></a>Funzioni per i set di righe
Le funzioni per i set di righe restituiscono un oggetto che è possibile usare come i riferimenti a tabelle in un'istruzione SQL.

## <a name="scalar-functions"></a>Funzioni scalari
Sono applicate a un singolo valore e restituiscono un singolo valore. È possibile usare le funzioni scalari in tutte le posizioni in cui sono consentite espressioni.

### <a name="categories-of-scalar-functions"></a>Categorie di funzioni scalari
  
|Categoria di funzioni|Descrizione|  
|-----------------------|-----------------|  
|[Funzioni di configurazione](configuration-functions-transact-sql.md)|Restituiscono informazioni sulla configurazione corrente.|  
|[Funzioni di conversione](conversion-functions-transact-sql.md)|Supportano l'esecuzione del cast e la conversione del tipo di dati.|  
|[Funzioni per i cursori](cursor-functions-transact-sql.md)|Restituiscono informazioni sui cursori.|  
|[Funzioni e tipi di dati di data e ora](date-and-time-data-types-and-functions-transact-sql.md)|Eseguono operazioni su valori di input di data e ora e restituiscono valori stringa, numerici o di data e ora.|  
|[Funzioni JSON](json-functions-transact-sql.md)|Consentono di convalidare o modificare i dati JSON e di eseguire query su di essi.|  
|[Funzioni logiche](logical-functions-choose-transact-sql.md)|Eseguono operazioni logiche.|  
|[Funzioni matematiche](mathematical-functions-transact-sql.md)|Eseguono calcoli in base ai valori di input specificati come parametri per le funzioni e restituiscono valori numerici.|  
|[Funzioni per i metadati](metadata-functions-transact-sql.md)|Restituiscono informazioni sul database e sugli oggetti di database.|  
|[Funzioni di sicurezza](security-functions-transact-sql.md)|Restituiscono informazioni sugli utenti e sui ruoli.|  
|[Funzioni per i valori stringa](string-functions-transact-sql.md)|Eseguono operazioni sui valori di input di tipo stringa (**char** o **varchar**) e restituiscono un valore stringa o numerico.|  
|[Funzioni di sistema](../../relational-databases/system-functions/system-functions-category-transact-sql.md)|Eseguono operazioni e restituiscono informazioni su valori, oggetti e impostazioni in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[Funzioni statistiche di sistema](system-statistical-functions-transact-sql.md)|Restituiscono informazioni statistiche sul sistema.|  
|[Funzioni per i valori text e image](./text-and-image-functions-textptr-transact-sql.md)|Eseguono operazioni su valori di input o colonne di testo o immagini e restituiscono informazioni sul valore.|  
  
## <a name="function-determinism"></a>Determinismo delle funzioni  
 Le funzioni predefinite di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] possono essere deterministiche o non deterministiche. Sono deterministiche quando restituiscono sempre lo stesso risultato ogni volta che vengono chiamate con un set specifico di valori di input. Sono invece non deterministiche se restituiscono valori diversi per ogni chiamata con un set specifico di valori di input. Per altre informazioni, vedere [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)  
  
## <a name="function-collation"></a>Regole di confronto per le funzioni  
 Le funzioni che accettano una stringa di caratteri come input e restituiscono una stringa di caratteri come output usano per l'output le regole di confronto della stringa di input.  
  
 Le funzioni che accettano input di dati non di tipo carattere e restituiscono una stringa di caratteri usano per l'output le regole di confronto predefinite del database corrente.  
  
 Le funzioni che accettano input composti da più stringhe di caratteri e restituiscono una stringa di caratteri impostano le regole di confronto per l'output in base alle regole sulla precedenza delle regole di confronto. Per altre informazioni, vedere [Precedenza delle regole di confronto &#40;Transact-SQL&#41;](../../t-sql/statements/collation-precedence-transact-sql.md).  
  
## <a name="see-also"></a>Vedere anche  
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)   
 [Funzioni deterministiche e non deterministiche](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)   
 [Uso di stored procedure &#40;MDX&#41;](../../mdx/using-stored-procedures-mdx.md)  
  
