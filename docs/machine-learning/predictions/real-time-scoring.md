---
title: Assegnazione dei punteggi in tempo reale tramite sp_rxPredict
description: Istruzioni per l'assegnazione dei punteggi in tempo reale con la stored procedure di sistema sp_rxPredict in SQL Server per ottenere previsioni ad alte prestazioni o punteggi per i carichi di lavoro di previsione.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 04/05/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 7761957f1627d7fbf2e881c99f83ab38b8a808ac
ms.sourcegitcommit: ab0c654d924eeb5647e47444abb59d934345b205
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2021
ms.locfileid: "106450192"
---
# <a name="real-time-scoring-with-sp_rxpredict-in-sql-server"></a>Assegnazione dei punteggi in tempo reale con sp_rxPredict in SQL Server
[!INCLUDE[sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

Istruzioni per l'assegnazione dei punteggi in tempo reale con la stored procedure di sistema [sp_rxPredict](../../relational-databases/system-stored-procedures/sp-rxpredict-transact-sql.md) in SQL Server per ottenere previsioni ad alte prestazioni o punteggi per i carichi di lavoro di previsione.

Il Punteggio in tempo reale con `sp_rxPredict` è indipendente dal linguaggio e viene eseguito senza dipendenze dai runtime di R o Python in [Machine Learning Services](../sql-server-machine-learning-services.md) o [Machine Learning server](../r/r-server-standalone.md). Usando un modello creato e sottoposto a training usando le funzioni Microsoft e serializzato in un formato binario in SQL Server, è possibile usare il punteggio in tempo reale per generare risultati stimati in nuovi input di dati in istanze di SQL Server in cui non è installato il componente aggiuntivo R o Python.

## <a name="how-real-time-scoring-works"></a>Funzionamento dell'assegnazione dei punteggi in tempo reale

Il Punteggio in tempo reale è supportato su tipi di modello specifici in base alle funzioni in [RevoScaleR](../r/ref-r-revoscaler.md) o [MicrosoftML](../r/ref-r-microsoftml.md) in R o [revoscalepy](../python/ref-py-revoscalepy.md) o [MicrosoftML](../python/ref-py-microsoftml.md) in Python. USA librerie C++ native per generare punteggi in base all'input dell'utente fornito a un modello di Machine Learning archiviato in un formato binario speciale.

Poiché un modello sottoposto a training può essere utilizzato per l'assegnazione dei punteggi senza dover chiamare un runtime di linguaggio esterno in [Machine Learning Services](../sql-server-machine-learning-services.md) o [Machine Learning server](../r/r-server-standalone.md), viene ridotto l'overhead di più processi.

L'assegnazione dei punteggi in tempo reale è un processo in più passaggi:

1. Si Abilita la stored procedure che esegue il punteggio in base al database.
2. Il modello con training preliminare viene caricato in formato binario.
3. Si forniscono nuovi dati di input a cui assegnare un punteggio, sia in formato tabulare che come righe singole, come input per il modello.
4. Per generare i punteggi, chiamare la stored procedure [sp_rxPredict](../../relational-databases/system-stored-procedures/sp-rxpredict-transact-sql.md).

## <a name="prerequisites"></a>Prerequisiti

+ [Abilitare l'integrazione CLR in SQL Server](../../relational-databases/clr-integration/clr-integration-enabling.md).

+ [Abilitare l'assegnazione dei punteggi in tempo reale](#bkmk_enableRtScoring).

+ Il training del modello deve essere eseguito in anticipo tramite uno degli algoritmi **rx** supportati. Per informazioni dettagliate, vedere [algoritmi supportati](../../relational-databases/system-stored-procedures/sp-rxpredict-transact-sql.md?view=sql-server-ver15#supported-algorithms) per `sp_rxPredict` .

+ Serializzare il modello usando [rxSerialize](/machine-learning-server/r-reference/revoscaler/rxserializemodel) per R o [rx_serialize_model](/machine-learning-server/python-reference/revoscalepy/rx-serialize-model) per Python. Queste funzioni di serializzazione sono state ottimizzate per supportare l'assegnazione rapida dei punteggi.

+ Salvare il modello nell'istanza del motore di database da cui si vuole chiamarlo. Per questa istanza non è necessario avere l'estensione di runtime R o Python.

> [!Note]
> L'assegnazione dei punteggi in tempo reale è attualmente ottimizzata per stime rapide su set di dati più piccoli, che vanno da poche righe a centinaia di migliaia di righe. Nei set di grandi dimensioni potrebbe essere più veloce usare [rxPredict](/machine-learning-server/r-reference/revoscaler/rxpredict).

<a name ="bkmk_enableRtScoring"></a>

## <a name="enable-real-time-scoring"></a>Abilitare l'assegnazione dei punteggi in tempo reale

Abilitare questa funzionalità per ogni database che si desidera utilizzare per l'assegnazione dei punteggi. L'amministratore del server deve eseguire l'utilità da riga di comando RegisterRExt.exe, inclusa nel pacchetto RevoScaleR.

> [!CAUTION]
> Per consentire il funzionamento del punteggio in tempo reale, è necessario abilitare la funzionalità CLR SQL nell'istanza di e il database deve essere contrassegnato come Trustworthy. Quando si esegue lo script, queste operazioni vengono eseguite automaticamente. Tuttavia, prima di eseguire questa operazione considerare attentamente le implicazioni di sicurezza aggiuntive.

1. Aprire un prompt dei comandi con privilegi elevati e passare alla cartella in cui si trova RegisterRExt.exe. Il percorso seguente può essere usato in un'installazione predefinita:

    `<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\`

2. Eseguire il comando seguente, sostituendo il nome dell'istanza e il database di destinazione in cui si vogliono abilitare le stored procedure estese:

    `RegisterRExt.exe /installRts [/instance:name] /database:databasename`

    Ad esempio, per aggiungere la stored procedure estesa al database CLRPredict nell'istanza predefinita, digitare:

    `RegisterRExt.exe /installRts /database:CLRPRedict`

    Il nome dell'istanza è facoltativo se il database si trova nell'istanza predefinita. Se si usa un'istanza denominata, specificare il nome dell'istanza.

3. RegisterRExt.exe crea gli oggetti seguenti:

    + Assembly attendibili
    + Stored procedure `sp_rxPredict`
    + Nuovo ruolo del database `rxpredict_users`, che l'amministratore del database può usare per concedere l'autorizzazione agli utenti che usano la funzionalità di assegnazione dei punteggi in tempo reale.

4. Aggiungere tutti gli utenti che devono eseguire `sp_rxPredict` al nuovo ruolo.

> [!NOTE]
>
> In SQL Server 2017 e versioni successive sono disponibili misure di sicurezza aggiuntive per evitare problemi con l'integrazione CLR. Queste misure impongono anche ulteriori restrizioni sull'uso di questa stored procedure.

## <a name="disable-real-time-scoring"></a>Disabilitare l'assegnazione dei punteggi in tempo reale

Per disabilitare la funzionalità di assegnazione dei punteggi in tempo reale, aprire un prompt dei comandi con privilegi elevati ed eseguire il comando seguente: `RegisterRExt.exe /uninstallrts /database:<database_name> [/instance:name]`

## <a name="example"></a>Esempio

In questo esempio vengono descritti i passaggi necessari per preparare e salvare un modello per la stima in **tempo reale** e viene fornito un esempio in R di come chiamare la funzione da T-SQL.

### <a name="step-1-prepare-and-save-the-model"></a>Passaggio 1. Preparare e salvare il modello

Il formato binario richiesto da sp\_rxPredict è uguale al formato necessario per usare la funzione PREDICT. Includere quindi nel codice R una chiamata a [rxSerializeModel](/machine-learning-server/r-reference/revoscaler/rxserializemodel) e assicurarsi di specificare `realtimeScoringOnly = TRUE`, come nell'esempio seguente:

```R
model <- rxSerializeModel(model.name, realtimeScoringOnly = TRUE)
```

### <a name="step-2-call-sp_rxpredict"></a>Passaggio 2. Chiamare sp_rxPredict

`sp_rxPredict` viene chiamata come qualsiasi altra stored procedure. Nella versione corrente la stored procedure accetta solo due parametri: _\@model_ per il modello in formato binario e _\@inputData_ per i dati da usare per l'assegnazione dei punteggi, definiti come query SQL valida.

Poiché il formato binario è identico a quello usato dalla funzione PREDICT, è possibile usare i modelli e la tabella dati dell'esempio precedente.

```sql
DECLARE @irismodel varbinary(max)
SELECT @irismodel = [native_model_object] from [ml_models]
WHERE model_name = 'iris.dtree' 
AND model_version = 'v1''

EXEC sp_rxPredict
@model = @irismodel,
@inputData = N'SELECT * FROM iris_rx_data'
```

> [!NOTE]
> La chiamata `sp_rxPredict` ha esito negativo se i dati di input per l'assegnazione dei punteggi non includono colonne che soddisfano i requisiti del modello. Attualmente sono supportati solo i tipi di dati .NET seguenti: double, float, short, ushort, long, ulong e string.
>
> Potrebbe quindi essere necessario filtrare ed escludere i tipi non supportati nei dati di input prima di usarli per l'assegnazione dei punteggi in tempo reale.
>
> Per informazioni sui tipi SQL corrispondenti, vedere [Mapping del tipo SQL-CLR](/dotnet/framework/data/adonet/sql/linq/sql-clr-type-mapping) o [Mapping dei dati dei parametri CLR](../../relational-databases/clr-integration-database-objects-types-net-framework/mapping-clr-parameter-data.md).

## <a name="next-steps"></a>Passaggi successivi

+ [Assegnazione dei punteggi nativa tramite la funzione PREDICT T-SQL con Machine Learning in SQL](native-scoring-predict-transact-sql.md)
+ [sp_rxPredict](../../relational-databases/system-stored-procedures/sp-rxpredict-transact-sql.md)
+ [Machine Learning SQL](../index.yml)