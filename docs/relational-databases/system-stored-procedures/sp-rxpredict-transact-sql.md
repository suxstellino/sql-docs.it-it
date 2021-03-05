---
title: sp_rxPredict
description: sp_rxPredict genera un valore stimato per un input specificato costituito da un modello di apprendimento automatico archiviato in un formato binario in un database SQL Server.
ms.date: 03/31/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: machine-learning-services
ms.topic: reference
f1_keywords:
- sp_rxPredict
- sp_rxPredict_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_rxPredict procedure
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 6f4d7ee0ea6f9630942dc12baf5b8ed68cbee4d5
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186043"
---
# <a name="sp_rxpredict"></a>sp_rxPredict  
[!INCLUDE [SQL Server 2016 Windows only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]

Genera un valore stimato per un input specificato costituito da un modello di apprendimento automatico archiviato in un formato binario in un database SQL Server.

Fornisce l'assegnazione dei punteggi ai modelli di apprendimento automatico R e Python in tempo quasi reale. `sp_rxPredict` è un stored procedure fornito come wrapper per la `rxPredict` funzione R in [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler) e [MicrosoftML](/r-server/r-reference/microsoftml/microsoftml-package)e la funzione Python [rx_predict](/machine-learning-server/python-reference/revoscalepy/rx-predict) in [revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) e [MicrosoftML](/machine-learning-server/python-reference/microsoftml/microsoftml-package). Viene scritto in C++ ed è ottimizzato in modo specifico per le operazioni di assegnazione dei punteggi.

Anche se il modello deve essere creato con R o Python, una volta serializzato e archiviato in un formato binario in un'istanza del motore di database di destinazione, può essere utilizzato da tale istanza del motore di database anche quando l'integrazione con R o Python non è installata. Per ulteriori informazioni, vedere Assegnazione di [punteggi in tempo reale con sp_rxPredict](../../machine-learning/predictions/real-time-scoring.md).

## <a name="syntax"></a>Sintassi

```
sp_rxPredict  ( @model, @input )
```

### <a name="arguments"></a>Argomenti

**model**

Modello sottoposto a training in un formato supportato. 

**input**

Una query SQL valida

### <a name="return-values"></a>Valori restituiti

Viene restituita una colonna score, nonché tutte le colonne pass-through dell'origine dati di input.
È possibile restituire colonne con punteggio aggiuntivo, ad esempio intervallo di confidenza, se l'algoritmo supporta la generazione di tali valori.

## <a name="remarks"></a>Commenti

Per abilitare l'utilizzo del stored procedure, SQLCLR deve essere abilitato nell'istanza di.

> [!NOTE]
> L'abilitazione di questa opzione comporta implicazioni sulla sicurezza. Utilizzare un'implementazione alternativa, ad esempio la funzione di [stima Transact-SQL](../../t-sql/queries/predict-transact-sql.md?view=sql-server-2017&preserve-view=true) , se non è possibile abilitare SQLCLR nel server.

L'utente deve disporre dell' `EXECUTE` autorizzazione per il database.

### <a name="supported-algorithms"></a>Algoritmi supportati

Per creare ed eseguire il training del modello, usare uno degli algoritmi supportati per R o Python, fornito da [SQL Server Machine Learning Services (r o Python)](../../machine-learning/sql-server-machine-learning-services.md), [SQL Server 2016 R Services](../../machine-learning/r/sql-server-r-services.md), [SQL Server machine learning Server (autonomo) (r o python)](../../machine-learning/r/r-server-standalone.md)o [SQL Server 2016 R Server (standalone)](../../machine-learning/r/r-server-standalone.md?view=sql-server-2016&preserve-view=true).

#### <a name="r-revoscaler-models"></a>R: modelli RevoScaleR

  + [rxLinMod](/machine-learning-server/r-reference/revoscaler/rxlinmod)
  + [rxLogit](/machine-learning-server/r-reference/revoscaler/rxlogit)
  + [rxBTrees](/machine-learning-server/r-reference/revoscaler/rxbtrees)
  + [rxDtree](/machine-learning-server/r-reference/revoscaler/rxdtree)
  + [rxdForest](/machine-learning-server/r-reference/revoscaler/rxdforest)

#### <a name="r-microsoftml-models"></a>R: modelli MicrosoftML

  + [rxFastTrees](/machine-learning-server/r-reference/microsoftml/rxfasttrees)
  + [rxFastForest](/machine-learning-server/r-reference/microsoftml/rxfastforest)
  + [rxLogisticRegression](/machine-learning-server/r-reference/microsoftml/rxlogisticregression)
  + [rxOneClassSvm](/machine-learning-server/r-reference/microsoftml/rxoneclasssvm)
  + [rxNeuralNet](/machine-learning-server/r-reference/microsoftml/rxneuralnet)
  + [rxFastLinear](/machine-learning-server/r-reference/microsoftml/rxfastlinear)

#### <a name="r-transformations-supplied-by-microsoftml"></a>R: trasformazioni fornite da MicrosoftML

  + [featurizeText](/machine-learning-server/r-reference/microsoftml/rxfasttrees)
  + [concat](/machine-learning-server/r-reference/microsoftml/concat)
  + [categorical](/machine-learning-server/r-reference/microsoftml/categorical)
  + [categoricalHash](/machine-learning-server/r-reference/microsoftml/categoricalHash)
  + [selectFeatures](/machine-learning-server/r-reference/microsoftml/selectFeatures)

#### <a name="python-revoscalepy-models"></a>Python: modelli revoscalepy

  + [rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod)
  + [rx_logit](/machine-learning-server/python-reference/revoscalepy/rx-logit)
  + [rx_btrees](/machine-learning-server/python-reference/revoscalepy/rx-btrees)
  + [rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-dtree)
  + [rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-dforest)


#### <a name="python-microsoftml-models"></a>Python: modelli microsoftml

  + [rx_fast_trees](/machine-learning-server/python-reference/microsoftml/rx-fast-trees)
  + [rx_fast_forest](/machine-learning-server/python-reference/microsoftml/rx-fast-forest)
  + [rx_logistic_regression](/machine-learning-server/python-reference/microsoftml/rx-logistic-regression)
  + [rx_oneclass_svm](/machine-learning-server/python-reference/microsoftml/rx-oneclass-svm)
  + [rx_neural_network](/machine-learning-server/python-reference/microsoftml/rx-neural-network)
  + [rx_fast_linear](/machine-learning-server/python-reference/microsoftml/rx-fast-linear)

#### <a name="python-transformations-supplied-by-microsoftml"></a>Python: trasformazioni fornite da microsoftml

  + [featurize_text](/machine-learning-server/python-reference/microsoftml/rx-fast-trees)
  + [concat](/machine-learning-server/python-reference/microsoftml/concat)
  + [categorical](/machine-learning-server/python-reference/microsoftml/categorical)
  + [categorical_hash](/machine-learning-server/python-reference/microsoftml/categorical-hash)
  
### <a name="unsupported-model-types"></a>Tipi di modelli non supportati

I tipi di modello seguenti non sono supportati:

+ Modelli con gli `rxGlm` `rxNaiveBayes` algoritmi o in RevoScaleR
+ Modelli PMML in R
+ Modelli creati con altre librerie di terze parti 

## <a name="examples"></a>Esempio

```sql
DECLARE @model = SELECT @model 
FROM model_table 
WHERE model_name = 'rxLogit trained';

EXEC sp_rxPredict @model = @model,
@inputData = N'SELECT * FROM data';
```

Oltre a essere una query SQL valida, i dati di input in *\@ inputData* devono includere le colonne compatibili con le colonne nel modello archiviato.

`sp_rxPredict` supporta solo i tipi di colonna .NET seguenti: Double, float, short, ushort, Long, ULONG e String. Potrebbe essere necessario filtrare i tipi non supportati nei dati di input prima di usarli per l'assegnazione dei punteggi in tempo reale. 

  Per informazioni sui tipi SQL corrispondenti, vedere [Mapping del tipo SQL-CLR](/dotnet/framework/data/adonet/sql/linq/sql-clr-type-mapping) o [Mapping dei dati dei parametri CLR](../clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md).
