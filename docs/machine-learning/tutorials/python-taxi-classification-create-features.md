---
title: 'Esercitazione su Python: Creare funzionalità di dati'
titleSuffix: SQL machine learning
description: Nella terza parte di questa serie di esercitazioni in cinque parti si aggiungeranno calcoli alle stored procedure da usare nei modelli di Machine Learning Python con il Machine Learning di SQL.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 630a5db712e3bd6e288c0ac12ca767a07c3ac4ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339713"
---
# <a name="python-tutorial-create-data-features-using-t-sql"></a>Esercitazione su Python: Creare caratteristiche dei dati usando T-SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Nella terza parte di questa serie di esercitazioni in cinque parti si apprenderà come creare funzionalità dai dati non elaborati tramite una funzione [!INCLUDE[tsql](../../includes/tsql-md.md)]. La funzione verrà quindi chiamata da una stored procedure SQL per creare una tabella contenente i valori della funzionalità.

Il processo di *definizione delle funzionalità*, creando funzionalità dai dati non elaborati, è un passaggio critico nella modellazione dell'analisi avanzata.

Contenuto dell'articolo:

> [!div class="checklist"]
> + Modificare una funzione personalizzata per calcolare la distanza della corsa
> + Salvare le funzionalità usando un'altra funzione personalizzata

Nella [prima parte](python-taxi-classification-introduction.md) sono stati installati i prerequisiti ed è stato ripristinato il database di esempio.

Nella [seconda parte](python-taxi-classification-explore-data.md) sono stati esaminati i dati di esempio e sono stati generati alcuni tracciati.

Nella [quarta parte](python-taxi-classification-train-model.md) verranno caricati i moduli e verranno chiamate le funzioni necessarie per la creazione e il training del modello usando una stored procedure di SQL Server.

Nella [quinta parte](python-taxi-classification-deploy-model.md) si apprenderà come rendere operativi i modelli sottoposti a training e salvati nella quarta parte.

## <a name="define-the-function"></a>Definire la funzione

I valori di distanza inclusi nei dati originali si basano sulla distanza registrata dal tassametro e non rappresentano necessariamente la distanza geografica o la distanza percorsa. Pertanto è necessario calcolare la distanza diretta tra i punti di inizio e fine della corsa, usando le coordinate disponibili nel set di dati di origine NYC Taxi. È possibile farlo usando la [formula dell'emisenoverso](https://en.wikipedia.org/wiki/Haversine_formula) in una funzione [!INCLUDE[tsql](../../includes/tsql-md.md)] personalizzata.

Si userà una funzione T-SQL personalizzata, _fnCalculateDistance_, per calcolare la distanza con la formula dell'emisenoverso e una seconda funzione T-SQL personalizzata, _fnEngineerFeatures_, per creare una tabella contenente tutte le funzionalità.

### <a name="calculate-trip-distance-using-fncalculatedistance"></a>Calcolare la distanza della corsa usando fnCalculateDistance

1. La funzione _fnCalculateDistance_ è inclusa nel database di esempio. Dedicare un attimo di tempo a esaminare il codice.
  
2. In [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]espandere **Programmabilità**, **Funzioni** e quindi **Funzioni a valori scalari**.
   Fare clic con il pulsante destro del mouse su _fnCalculateDistance_ e selezionare **Modifica** per aprire lo script [!INCLUDE[tsql](../../includes/tsql-md.md)] in una nuova finestra Query.
  
   ```sql
   CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)
   -- User-defined function that calculates the direct distance between two geographical coordinates
   RETURNS float
   AS
   BEGIN
     DECLARE @distance decimal(28, 10)
     -- Convert to radians
     SET @Lat1 = @Lat1 / 57.2958
     SET @Long1 = @Long1 / 57.2958
     SET @Lat2 = @Lat2 / 57.2958
     SET @Long2 = @Long2 / 57.2958
     -- Calculate distance
     SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
     --Convert to miles
     IF @distance <> 0
     BEGIN
       SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
     END
     RETURN @distance
   END
   GO
   ```

**Note:**

+ La funzione è una funzione con valori scalari che restituisce un singolo valore di dati di un tipo predefinito.
+ Accetta come input valori di latitudine e longitudine, ottenuti dalle località di inizio e fine della corsa. La formula dell'emisenoverso converte le posizioni in radianti e usa tali valori per calcolare la distanza diretta in miglia tra queste due posizioni.

Per aggiungere il valore calcolato a una tabella da usare per il training del modello è possibile usare un'altra funzione, _fnEngineerFeatures_.

### <a name="save-the-features-using-_fnengineerfeatures_"></a>Salvare le caratteristiche usando _fnEngineerFeatures_

1. Esaminare il codice per la funzione T-SQL personalizzata, _fnEngineerFeatures_, incluso nel database di esempio.
  
   È una funzione con valori di tabella che accetta come input più colonne e restituisce una tabella con più colonne di funzionalità.  Lo scopo della funzione è la creazione di un set di funzionalità da usare per la compilazione di un modello. La funzione _fnEngineerFeatures_ chiama la funzione T-SQL _fnCalculateDistance_ creata in precedenza per ottenere la distanza diretta tra i punti di inizio e fine della corsa.
  
   ```sql
   CREATE FUNCTION [dbo].[fnEngineerFeatures] (
   @passenger_count int = 0,
   @trip_distance float = 0,
   @trip_time_in_secs int = 0,
   @pickup_latitude float = 0,
   @pickup_longitude float = 0,
   @dropoff_latitude float = 0,
   @dropoff_longitude float = 0)
   RETURNS TABLE
   AS
     RETURN
     (
     -- Add the SELECT statement with parameter references here
     SELECT
       @passenger_count AS passenger_count,
       @trip_distance AS trip_distance,
       @trip_time_in_secs AS trip_time_in_secs,
       [dbo].[fnCalculateDistance](@pickup_latitude, @pickup_longitude, @dropoff_latitude, @dropoff_longitude) AS direct_distance
     )
   GO
   ```
  
2. Per verificare che la funzione operi in modo corretto è possibile usarla per calcolare la distanza geografica per i viaggi in cui la distanza sul tassametro era pari a 0 ma i punti di inizio e fine corsa erano diversi.
  
   ```sql
       SELECT tipped, fare_amount, passenger_count,(trip_time_in_secs/60) as TripMinutes,
       trip_distance, pickup_datetime, dropoff_datetime,
       dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) AS direct_distance
       FROM nyctaxi_sample
       WHERE pickup_longitude != dropoff_longitude and pickup_latitude != dropoff_latitude and trip_distance = 0
       ORDER BY trip_time_in_secs DESC
   ```
  
   Come si può notare, la distanza indicata dal tassametro non corrisponde sempre alla distanza geografica. È per questo che la progettazione delle caratteristiche è importante.

Nella parte successiva si apprenderà come usare queste funzionalità dei dati per la creazione e il training di un modello di Machine Learning con Python.

## <a name="next-steps"></a>Passaggi successivi

In questo articolo si apprenderà come:

> [!div class="checklist"]
> + Modificare una funzione personalizzata per calcolare la distanza della corsa
> + Salvare le funzionalità usando un'altra funzione personalizzata

> [!div class="nextstepaction"]
> [Esercitazione su Python: Eseguire il training e il salvataggio di un modello Python usando T-SQL](python-taxi-classification-train-model.md)