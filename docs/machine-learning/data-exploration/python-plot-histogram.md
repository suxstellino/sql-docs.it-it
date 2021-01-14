---
title: Tracciare un istogramma per l'esplorazione dei dati con Python
titleSuffix: SQL machine learning
description: Informazioni su come creare un istogramma per visualizzare i dati usando Python.
author: dphansen
ms.author: davidph
ms.date: 07/14/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: 466abc8687a7f325d216a6b5b6545bc516b2b34d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101991"
---
# <a name="plot-histograms-in-python"></a>Tracciare istogrammi in Python 
[!INCLUDE[SQL Server SQL DB SQL MI](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

Questo articolo descrive come tracciare un grafico dei dati usando il pacchetto Python [pandas'.hist()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.hist.html). Un database SQL è l'origine usata per visualizzare gli intervalli di dati dell'istogramma con valori consecutivi non sovrapposti.

## <a name="prerequisites"></a>Prerequisiti:

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
* [SQL Server per Windows](../../database-engine/install-windows/install-sql-server.md) o [per Linux](../../linux/sql-server-linux-overview.md)
::: moniker-end

::: moniker range="=azuresqldb-current"
* [Database SQL di Azure](/azure/sql-database/sql-database-get-started-portal)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
* [Istanza gestita di database SQL di Azure](/azure/azure-sql/managed-instance/instance-create-quickstart)

* [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) per il ripristino del database di esempio in Istanza gestita di SQL di Azure.
::: moniker-end

* Azure Data Studio. Per eseguire l'installazione, vedere [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md).

* [Ripristinare il database DW di esempio](../../samples/adventureworks-install-configure.md) per ottenere i dati di esempio usati in questo articolo.

## <a name="verify-restored-database"></a>Verificare il database ripristinato

È possibile verificare che il database ripristinato sia disponibile eseguendo una query sulla tabella **Person.CountryRegion**:
```sql
USE AdventureWorksDW;
SELECT * FROM Person.CountryRegion;
```
  
## <a name="install-python-packages"></a>Installare i pacchetti Python

[Scaricare e installare Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md).

Installare i pacchetti Python seguenti:
  * pyodbc
  * pandas

  Per installare questi pacchetti:

  1. Nel notebook di Azure Data Studio selezionare **Gestisci pacchetti**.
  2. Nel riquadro **Gestisci pacchetti** selezionare la scheda **Aggiungi nuovo**.
  3. Per ognuno dei seguenti pacchetti immettere il nome del pacchetto, fare clic su **Cerca**, quindi fare clic su **Installa**.

## <a name="plot-histogram"></a>Tracciare un istogramma

I dati distribuiti visualizzati nell'istogramma sono basati su una query SQL di AdventureWorksDW. L'istogramma visualizza i dati e la frequenza dei valori dei dati. Modificare le variabili della stringa di connessione "server", "database", "username" e "password" per la connessione al database SQL.

Per creare un nuovo notebook:

1. In Azure Data Studio selezionare **File** e quindi **Nuovo notebook**.
2. Nel notebook selezionare il kernel **Python3** e quindi **+Codice**.
3. Incollare il codice nel notebook e selezionare **Esegui tutti**.

```python
import pyodbc 
import pandas as plt
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'servername' 
database = 'AdventureWorksDW' 
username = 'yourusername' 
password = 'databasename'  
cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
sql = "SELECT DATEDIFF(year, c.BirthDate, GETDATE()) AS Age FROM [dbo].[FactInternetSales] s INNER JOIN dbo.DimCustomer c ON s.CustomerKey = c.CustomerKey"
df = pd.read_sql(sql, cnxn)
df.hist(bins=10)
```

Il grafico mostrerà la distribuzione dell'età dei clienti nella tabella FactInternetSales.

![Istogramma Pandas](./media/python-histogram.png)