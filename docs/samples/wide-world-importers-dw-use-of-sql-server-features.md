---
title: Funzionalità principali del database DW WideWorldImporters
description: Informazioni sul modo in cui il database WideWorldImportersDW presenta le funzionalità chiave di SQL Server adatte per il data warehousing e l'analisi.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 07/01/2020
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||>=aps-pdw-2016||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: ad1c38ab7c479497c50e84b305fe23d1441501d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056961"
---
# <a name="wideworldimportersdw-use-of-sql-server-features-and-capabilities"></a>Uso di WideWorldImportersDW delle funzionalità e delle funzionalità di SQL Server
[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]
WideWorldImportersDW è stato progettato per presentare molte delle principali funzionalità di SQL Server che sono adatte per il data warehousing e l'analisi. Di seguito è riportato un elenco di funzionalità e funzionalità di SQL Server e una descrizione del modo in cui vengono usate in WideWorldImportersDW.

## <a name="polybase"></a>PolyBase

[Si applica a SQL Server (2016 e versioni successive)]

La polibase viene usata per combinare le informazioni di vendita di WideWorldImportersDW con un set di dati pubblico sui dati demografici per comprendere quali città possono essere utili per un'ulteriore espansione delle vendite.

Per abilitare l'utilizzo di polibase nel database di esempio, assicurarsi che sia installato ed eseguire le stored procedure seguenti nel database:

```sql
EXECUTE [Application].[Configuration_ApplyPolyBase]
```

Verrà creata una tabella esterna `dbo.CityPopulationStatistics` che fa riferimento a un set di dati pubblico che contiene i dati della popolazione per le città nell'Stati Uniti, ospitati nell'archiviazione BLOB di Azure. Si consiglia di esaminare il codice nel stored procedure per comprendere il processo di configurazione. Se si vuole ospitare i propri dati nell'archivio BLOB di Azure e mantenerli protetti dall'accesso pubblico generale, è necessario eseguire passaggi di configurazione aggiuntivi. La query seguente restituisce i dati dal set di dati esterno:

```sql
SELECT
        CityID, StateProvinceCode, CityName,
        YearNumber, LatestRecordedPopulation
    FROM
        dbo.CityPopulationStatistics;
```

Per comprendere quali città possono essere di interesse per un'ulteriore espansione, la query seguente esamina il tasso di crescita delle città e restituisce le prime 100 città più grandi con una crescita significativa e in cui Wide World Importers non dispone di una presenza di vendite. La query include un join tra la tabella remota `dbo.CityPopulationStatistics` e la tabella locale `Dimension.City` e un filtro che interessa la tabella locale `Fact.Sales` .

```sql
WITH PotentialCities
AS
(
    SELECT cps.CityName,
            cps.StateProvinceCode,
            MAX(cps.LatestRecordedPopulation) AS PopulationIn2016,
            (MAX(cps.LatestRecordedPopulation) - MIN(cps.LatestRecordedPopulation)) * 100.0
                / MIN(cps.LatestRecordedPopulation) AS GrowthRate
    FROM dbo.CityPopulationStatistics AS cps
    WHERE cps.LatestRecordedPopulation IS NOT NULL
    AND cps.LatestRecordedPopulation <> 0
    GROUP BY cps.CityName, cps.StateProvinceCode
),
InterestingCities
AS
(
    SELECT DISTINCT pc.CityName,
                    pc.StateProvinceCode,
                    pc.PopulationIn2016,
                    FLOOR(pc.GrowthRate) AS GrowthRate
    FROM PotentialCities AS pc
    INNER JOIN Dimension.City AS c
    ON pc.CityName = c.City
    WHERE GrowthRate > 2.0
    AND NOT EXISTS (SELECT 1 FROM Fact.Sale AS s WHERE s.[City Key] = c.[City Key])
)
SELECT TOP(100) CityName, StateProvinceCode, PopulationIn2016, GrowthRate
FROM InterestingCities
ORDER BY PopulationIn2016 DESC;
```

## <a name="clustered-columnstore-indexes"></a>Indici columnstore cluster

(Versione completa dell'esempio)

Gli indici columnstore cluster (CCI) vengono usati con tutte le tabelle dei fatti, per ridurre il footprint di archiviazione e migliorare le prestazioni di esecuzione delle query. Con l'uso di CCI, l'archiviazione di base per le tabelle dei fatti usa la compressione di colonna.

Gli indici non cluster vengono usati sopra l'indice columnstore cluster per semplificare i vincoli PRIMARY KEY e Foreign Key. Questi vincoli sono stati aggiunti senza alcuna attenzione. il processo ETL genera i dati dal database WideWorldImporters, che presenta vincoli per applicare l'integrità. La rimozione dei vincoli di chiave primaria ed esterna e i relativi indici di supporto ridurrebbe il footprint di archiviazione delle tabelle dei fatti.

**Dimensioni dei dati**

Le dimensioni dei dati del database di esempio sono limitate, per facilitare il download e l'installazione dell'esempio. Tuttavia, per vedere i vantaggi effettivi delle prestazioni degli indici columnstore, è consigliabile usare un set di dati di dimensioni maggiori.

È possibile eseguire l'istruzione seguente per aumentare le dimensioni della `Fact.Sales` tabella inserendo altre 12 milioni righe di dati di esempio. Queste righe vengono inserite per l'anno 2012, in modo che non esistano interferenze con il processo ETL.

```sql
    EXECUTE [Application].[Configuration_PopulateLargeSaleTable]
```

L'esecuzione di questa istruzione potrebbe richiedere circa 5 minuti. Per inserire più di 12 milioni righe, passare il numero desiderato di righe da inserire come parametro a questo stored procedure.

Per confrontare le prestazioni delle query con e senza columnstore, è possibile eliminare e/o ricreare l'indice columnstore cluster.

Per eliminare l'indice:

```sql
 DROP INDEX [CCX_Fact_Order] ON [Fact].[Order]
```

Per ricreare:

```sql
CREATE CLUSTERED COLUMNSTORE INDEX [CCX_Fact_Order] ON [Fact].[Order]
```

## <a name="partitioning"></a>Partizionamento

(Versione completa dell'esempio)

Le dimensioni dei dati in un data warehouse possono aumentare notevolmente. È pertanto consigliabile utilizzare il partizionamento per gestire l'archiviazione delle tabelle di grandi dimensioni nel database.

Tutte le tabelle dei fatti più grandi vengono partizionate in base all'anno. L'unica eccezione è `Fact.Stock Holdings` , che non è basata sulla data e ha una dimensione dei dati limitata rispetto alle altre tabelle dei fatti.

La funzione di partizione utilizzata per tutte le tabelle partizionate è `PF_Date` e lo schema di partizione utilizzato è `PS_Date` .

## <a name="in-memory-oltp"></a>OLTP in memoria

(Versione completa dell'esempio)

WideWorldImportersDW utilizza SCHEMA_ONLY tabelle ottimizzate per la memoria per le tabelle di staging. Tutte le `Integration.` * `_Staging` tabelle sono SCHEMA_ONLY tabelle ottimizzate per la memoria.

Il vantaggio di SCHEMA_ONLY tabelle è che non vengono registrate e non richiedono l'accesso al disco. Ciò migliora le prestazioni del processo ETL. Poiché queste tabelle non vengono registrate, il relativo contenuto viene perso se si verifica un errore. Tuttavia, l'origine dati è ancora disponibile, pertanto il processo ETL può essere semplicemente riavviato se si verifica un errore.
