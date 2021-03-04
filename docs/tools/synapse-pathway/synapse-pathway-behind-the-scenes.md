---
title: Anteprima del percorso sinapsi di Azure in background.
description: Approfondimento tecnico sul modo in cui il percorso sinapsi di Azure converte il codice.
author: anshul82-ms
ms.author: anrampal.
ms.prod: sql
ms.technology: Azure Synapse Pathway
ms.topic: conceptual
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-concept
ms.openlocfilehash: dbd362e53b5bfcd916c53e90d6f66c8fb44f0374
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839209"
---
# <a name="azure-synapse-pathway-preview-behind-the-scenes"></a>Anteprima dei percorsi delle sinapsi di Azure
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

L'obiettivo del percorso della sinapsi di Azure è mantenere lo scopo funzionale del codice originale durante l'ottimizzazione per la sinapsi SQL. Il percorso sinapsi usa un processo in tre fasi per la conversione del codice SQL da un sistema di origine a SQL sinapsi di Azure.

Ogni fase conserva e aumenta la conoscenza dell'origine, inclusi i metadati specifici dell'origine, per garantire la massima qualità della traduzione.

 ![Percorso delle sinapsi di Azure.](./media/technical-deep-dive/behind-the-scene.png)

## <a name="stage-1--lexing-and-parsing"></a>Fase 1: Lexing e analisi

L'analisi del linguaggio SQL è un problema che è stato risolto molte volte. Sono disponibili molti parser commerciali e open source che contribuiscono al processo sottostante di acquisizione di un'istruzione di origine, alla suddivisione dei token logici e quindi all'esecuzione in base a regole set o parser per garantire la coerenza della lingua. 

Il percorso sinapsi definisce le grammatiche di origine che consentono allo strumento di identificare ed elaborare l'input SQL in un albero AST (Abstract Syntax Tree) potenziato usato per un'ulteriore elaborazione. 

## <a name="stage-2---augmented-abstract-syntax-tree-ast"></a>Fase 2: albero sintattico astratto (AST) ampliato

Il percorso sinapsi definisce una rappresentazione comune di tutti gli oggetti in un albero sintattico astratto (AST) potenziato. Il percorso di sinapsi AST include istruzioni aggiuntive o metadati frammentati usati per prendere la decisione appropriata durante la conversione del codice tra i due sistemi.

Non solo verificando che un token sia una funzione ma piuttosto il requisito del tipo di sistema di origine, i componenti di generazione degli script possono prendere decisioni più intelligenti sulla conversione in sinapsi SQL.

Ad esempio, la funzione di origine per la funzione Absolute è definita come segue:

```sql  
ABS( float_expression ) 
```

Azure sinapsi SQL definisce la funzione assoluta come:

```sql  
ABS ( numeric_expression )  
```

In questo semplice caso, il percorso sinapsi comprende che la conversione in sinapsi SQL da float a numeric è una [conversione](../../t-sql/functions/cast-and-convert-transact-sql?view=azure-sqldw-latest#implicit-conversions) implicita e non richiede ulteriori operazioni di cast del tipo. Conversione di codice semplice, pulita ed efficace.

La conservazione di queste metainformazioni sulle istruzioni e sui frammenti di origine consente le differenze strutturali tra le piattaforme, conversioni nella logica di rifiuto esplicito per i predicati della condizione di ricerca in una clausola WHERE, ad esempio.

## <a name="stage-3---syntax-generation"></a>Fase 3: generazione della sintassi

L'ultima fase del processo consiste nel generare la sintassi per sinapsi SQL. Usando la struttura AST generata dai file di origine, il percorso sinapsi scrive ogni oggetto DDL in un singolo file. I generatori di sintassi utilizzano una conoscenza approfondita della piattaforma di destinazione per l'ottimizzazione delle istruzioni.

Un modello comune illustrato negli scenari di caricamento dei dati consiste, ad esempio, nell'eliminare innanzitutto tutto il contenuto di una tabella di staging e quindi caricare i dati da un'altra tabella di staging in modalità di inserimento/selezione.

```sql  
DELETE staging.table1 ALL;
INSERT INTO staging.table1…
FROM staging.table2;
```

Sinapsi SQL dispone di un percorso ottimizzato per questo scenario, ovvero un [create table come SELECT](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-develop-ctas). L'istruzione CTAS è un'operazione basata su batch e con registrazione minima che conduce a prestazioni elevate usando l'infrastruttura di calcolo disponibile. Senza queste informazioni su sinapsi SQL, gli strumenti spesso producono un'istruzione TRUNCATE e INSERT/SELECT.

```sql  
TRUNCATE TABLE staging.table1;
INSERT INTO staging.table1…
FROM staging.table2;
```

Sebbene non sia negativo, questo codice può essere ottimizzato in una tabella DROP e CTAS per ottenere prestazioni più elevate.

```sql  
DROP TABLE staging.table1;
CREATE TABLE staging.table1
WITH
(
    -- Derived from the original table definition 
    DISTRIBUTION = HASH(column1),
    -- Derived from the original table definition
    CLUSTERED COLUMNSTORE INDEX
)
AS SELECT  * FROM staging.table2;
```

## <a name="next-steps"></a>Passaggi successivi

- [Scarica il percorso delle sinapsi di Azure](synapse-pathway-download.md)
- [Domande frequenti](pathway-faq.md)

