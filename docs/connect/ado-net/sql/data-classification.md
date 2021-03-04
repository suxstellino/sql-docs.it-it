---
title: Individuazione dati e classificazione in SqlClient
description: Viene descritto come controllare se un database di SQL Server supporta la classificazione dei dati e come accedere alle informazioni di classificazione dei dati tramite un oggetto SqlDataReader.
ms.date: 11/23/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: bac8c09b862e43549ade3a6d98e34fdc97596131
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835768"
---
# <a name="data-discovery-and-classification-in-sqlclient"></a>Individuazione dati e classificazione in SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

[Individuazione dati e classificazione](../../../relational-databases/security/sql-data-discovery-and-classification.md) è un set di servizi avanzati per l'individuazione, la classificazione, l'assegnazione di etichette e la creazione di report per i dati sensibili nei database. SqlClient fornisce un'API che espone le informazioni di classificazione e individuazione dei dati di sola lettura quando l'origine sottostante supporta la funzionalità. È possibile accedere a queste informazioni tramite SqlDataReader.

Microsoft.Data.SqlClient v2.1.0 introduce il supporto per le informazioni `Sensitivity Rank` della classificazione dei dati. `Sensitivity Rank` è un identificatore basato su un set predefinito di valori che definiscono il livello di riservatezza. Può essere usato da altri servizi, ad esempio Advanced Threat Protection, per rilevare le anomalie in base al rango. Le API di classificazione dei dati seguenti sono ora disponibili nello spazio dei nomi Microsoft.Data.SqlClient.DataClassification:

```csharp
// New in Microsoft.Data.SqlClient v2.1.0
public enum SensitivityRank
{
    NOT_DEFINED = -1,
    NONE = 0,
    LOW = 10,
    MEDIUM = 20,
    HIGH = 30,
    CRITICAL = 40
}

public sealed class SensitivityClassification
{
  // Returns the sensitivity rank for the query associated with the active 'SqlDataReader'.
  // New in Microsoft.Data.SqlClient v2.1.0
  public SensitivityRank SensitivityRank;

  // Returns the labels collection for this 'SensitivityClassification' Object
  public ReadOnlyCollection<Label> Labels;

  // Returns the information types collection for this 'SensitivityClassification' Object
  public ReadOnlyCollection<InformationType> InformationTypes;

  // Returns the column sensitivity for this 'SensitivityClassification' Object
  public ReadOnlyCollection<ColumnSensitivity> ColumnSensitivities;
}

public sealed class SensitivityProperty
{
  // Returns the sensitivity rank for this 'SensitivityProperty' Object
  // New in Microsoft.Data.SqlClient v2.1.0
  public SensitivityRank SensitivityRank;

  // Returns the label for this 'SensitivityProperty' Object
  public Label Label;

  // Returns the information type for this 'SensitivityProperty' Object
  public InformationType InformationType;
}

public sealed class Label
{
  // Gets the name for this 'Label' object
  public string Name;

  // Gets the ID for this 'Label' object
  public string Id;
}

public sealed class InformationType
{
  // Gets the name for this 'InformationType' object
  public string Name;

  // Gets the ID for this 'InformationType' object
  public string Id;
}

public sealed class ColumnSensitivity
{
  // Returns the list of sensitivity properties as received from Server for this 'ColumnSensitivity' information      
  public ReadOnlyCollection<SensitivityProperty> SensitivityProperties;
}
```

> [!NOTE]
> Microsoft.Data.SqlClient legge le informazioni `Sensitivity Rank` solo se SQL Server supporta la classificazione dei dati con rango. Per i server usare la versione precedente della classificazione dei dati senza rango, il valore di pertinenza per le query è "NON DEFINITO".

Questa applicazione di esempio mostra come accedere alle proprietà di classificazione dei dati di SqlDataReader.

[!code-csharp [SqlDataReader_DataDiscoveryAndClassification#1](~/../sqlclient/doc/samples/SqlDataReader_DataDiscoveryAndClassification.cs#1)]


**Vedere anche**  

 - [Funzionalità di SQL Server e ADO.NET](sql-server-features-adonet.md)
 - [sys.sensitivity_classifications (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md)
 - [ADD SENSITIVITY CLASSIFICATION](../../../t-sql/statements/add-sensitivity-classification-transact-sql.md)