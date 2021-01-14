---
title: Inserire i dati di una tabella SQL in un dataframe Pandas Python
titleSuffix: SQL machine learning
description: Informazioni su come leggere i dati di una tabella SQL e inserirli in un dataframe Pandas usando Python.
author: dphansen
ms.author: davidph
ms.date: 07/23/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: 3acd5153599c6237117e52b483247033ebf983ec
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091629"
---
# <a name="insert-data-from-a-sql-table-into-a-python-pandas-dataframe"></a>Inserire i dati di una tabella SQL in un dataframe Pandas Python
[!INCLUDE[SQL Server SQL DB SQL MI](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

Questo articolo descrive come inserire i dati SQL in un dataframe [Pandas](https://pandas.pydata.org/) usando il pacchetto [pyodbc](../../connect/python/pyodbc/python-sql-driver-pyodbc.md) in Python. È possibile usare le righe e le colonne di dati presenti all'interno del dataframe per un'ulteriore esplorazione dei dati.

## <a name="prerequisites"></a>Prerequisiti

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

* [Ripristinare il database di esempio](../../samples/adventureworks-install-configure.md) per ottenere i dati di esempio usati in questo articolo.

## <a name="verify-restored-database"></a>Verificare il database ripristinato

È possibile verificare che il database ripristinato sia disponibile eseguendo una query sulla tabella **Person.CountryRegion**:

```sql
USE AdventureWorks;
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

## <a name="insert-data"></a>Inserire i dati

Usare lo script seguente per selezionare i dati dalla tabella Person.CountryRegion e inserirli in un dataframe. Modificare le variabili della stringa di connessione "server", "database", "username" e "password" per la connessione a SQL.

Per creare un nuovo notebook:

1. In Azure Data Studio selezionare **File** e quindi **Nuovo notebook**.
2. Nel notebook selezionare il kernel **Python3** e quindi **+Codice**.
3. Incollare il codice nel notebook e selezionare **Esegui tutti**.

```python
import pyodbc
import pandas as pd
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'servername' 
database = 'AdventureWorks' 
username = 'yourusername' 
password = 'databasename'  
cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
# select 26 rows from SQL table to insert in dataframe.
query = "SELECT [CountryRegionCode], [Name] FROM Person.CountryRegion;"
df = pd.read_sql(query, cnxn)
print(df.head(26))
```

**Output**

Il comando `print` nello script precedente consente di visualizzare le righe di dati del dataframe `pandas` `df`.

```text
CountryRegionCode                 Name
0                 AF          Afghanistan
1                 AL              Albania
2                 DZ              Algeria
3                 AS       American Samoa
4                 AD              Andorra
5                 AO               Angola
6                 AI             Anguilla
7                 AQ           Antarctica
8                 AG  Antigua and Barbuda
9                 AR            Argentina
10                AM              Armenia
11                AW                Aruba
12                AU            Australia
13                AT              Austria
14                AZ           Azerbaijan
15                BS         Bahamas, The
16                BH              Bahrain
17                BD           Bangladesh
18                BB             Barbados
19                BY              Belarus
20                BE              Belgium
21                BZ               Belize
22                BJ                Benin
23                BM              Bermuda
24                BT               Bhutan
25                BO              Bolivia
```

## <a name="next-steps"></a>Passaggi successivi

+ [Inserire un dataframe Python in SQL](../data-exploration/python-dataframe-sql-server.md)