---
title: Inserire un dataframe Python in una tabella SQL
titleSuffix: SQL machine learning
description: Come inserire dati di un dataframe in una tabella SQL.
author: dphansen
ms.author: davidph
ms.date: 07/23/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: 8d27ae33ab283209725c119ed6d96c4ed511b4ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349117"
---
# <a name="insert-python-dataframe-into-sql-table"></a>Inserire un dataframe Python in una tabella SQL
[!INCLUDE[SQL Server SQL DB SQL MI](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

Questo articolo descrive come inserire un dataframe [Pandas](https://pandas.pydata.org/) in un database SQL usando il pacchetto [pyodbc](../../connect/python/pyodbc/python-sql-driver-pyodbc.md) in Python.

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

È possibile verificare che il database ripristinato sia disponibile eseguendo una query sulla tabella **HumanResources.Department**:

```sql
USE AdventureWorks;
SELECT * FROM HumanResources.Department;
```

## <a name="install-python-packages"></a>Installare i pacchetti Python

* [Scaricare e installare Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md)

Installare i pacchetti Python seguenti:
  * pyodbc
  * pandas

  Per installare questi pacchetti:

  1. Nel notebook di Azure Data Studio selezionare **Gestisci pacchetti**.
  2. Nel riquadro **Gestisci pacchetti** selezionare la scheda **Aggiungi nuovo**.
  3. Per ognuno dei seguenti pacchetti immettere il nome del pacchetto, fare clic su **Cerca**, quindi fare clic su **Installa**.

## <a name="connect-to-sql-server-using-azure-data-studio"></a>Connettersi a SQL Server usando Azure Data Studio

[Connettersi tramite Azure Data Studio](../../azure-data-studio/quickstart-sql-server.md).

1. Connettersi al database AdventureWorks per creare la nuova tabella, HumanResources.DepartmentTest. La tabella SQL verrà usata per l'inserimento del dataframe.

```sql
CREATE TABLE [HumanResources].[DepartmentTest](
[DepartmentID] [smallint] NOT NULL,
[Name] [dbo].[Name] NOT NULL,
[GroupName] [dbo].[Name] NOT NULL
)
GO
```

## <a name="create-csv-file"></a>Creare un file CSV

Copiare il testo e salvare il file come department.csv per il dataframe.

```text
DepartmentID,Name,GroupName,
1,Engineering,Research and Development,
2,Tool Design,Research and Development,
3,Sales,Sales and Marketing,
4,Marketing,Sales and Marketing,
5,Purchasing,Inventory Management,
6,Research and Development,Research and Development,
7,Production,Manufacturing,
8,Production Control,Manufacturing,
9,Human Resources,Executive General and Administration,
10,Finance,Executive General and Administration,
11,Information Services,Executive General and Administration,
12,Document Control,Quality Assurance,
13,Quality Assurance,Quality Assurance,
14,Facilities and Maintenance,Executive General and Administration,
15,Shipping and Receiving,Inventory Management,
16,Executive,Executive General and Administration
```

## <a name="connect-to-sql-using-python"></a>Connettersi a SQL usando Python

1. Modificare le variabili della stringa di connessione "server", "database", "username" e "password" per la connessione al database SQL.

2. Modificare il percorso per il file CSV.

## <a name="load-dataframe-from-csv-file"></a>Caricare il dataframe dal file CSV

Usare il pacchetto `pandas` Python per creare un dataframe e caricare il file CSV. Connettersi a SQL per caricare il dataframe nella nuova tabella SQL, HumanResources.DepartmentTest.

Per creare un nuovo notebook:

1. In Azure Data Studio selezionare **File** e quindi **Nuovo notebook**.
2. Nel notebook selezionare il kernel **Python3** e quindi **+Codice**.
3. Incollare il codice nel notebook e selezionare **Esegui tutti**.

 ```Python
import pyodbc
import pandas as pd
# insert data from csv file into dataframe.
# working directory for csv file: type "pwd" in Azure Data Studio or Linux
# working directory in Windows c:\users\username
df = pd.read_csv("c:\\user\\username\department.csv")
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'yourservername' 
database = 'AdventureWorks' 
username = 'username' 
password = 'yourpassword' 
cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
# Insert Dataframe into SQL Server:
for index, row in df.iterrows():
     cursor.execute("INSERT INTO HumanResources.DepartmentTest (DepartmentID,Name,GroupName) values(?,?,?)", row.DepartmentID, row.Name, row.GroupName)
cnxn.commit()
cursor.close()
```

## <a name="confirm-row-count-in-sql"></a>Verificare il numero di righe in SQL

Eseguire l'istruzione SQL per verificare che la tabella sia stata caricata correttamente con i dati del dataframe.

```sql
SELECT count(*) from HumanResources.DepartmentTest;
```

Risultati

```bash
(No column name)
16
```

## <a name="next-steps"></a>Passaggi successivi

+ [Tracciare un istogramma per l'esplorazione dei dati con Python](../data-exploration/python-plot-histogram.md)