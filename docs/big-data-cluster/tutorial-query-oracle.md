---
title: Eseguire query su dati esterni in Oracle
titleSuffix: SQL Server big data clusters
description: Questa esercitazione illustra come eseguire una query su dati Oracle da un cluster Big Data di SQL Server 2019. Si creerà una tabella esterna con i dati di Oracle e si eseguirà quindi una query.
author: MikeRayMSFT
ms.author: dacoelho
ms.reviewer: mikeray
ms.date: 10/01/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 670ab5046c1904fee16f6c0271c49e2a4ecd72fe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045636"
---
# <a name="tutorial-query-oracle-from-sql-server-big-data-cluster"></a>Esercitazione: Eseguire una query in Oracle da un cluster Big Data di SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Questa esercitazione illustra come eseguire una query su dati Oracle da un cluster Big Data di SQL Server 2019. Per eseguire questa esercitazione, è necessario avere accesso a un server Oracle. È necessario un account utente Oracle con privilegi di lettura sull'oggetto esterno. L'autenticazione utente proxy Oracle è supportata. In caso contrario, questa esercitazione può dare un'idea del funzionamento della virtualizzazione dei dati per origini dati esterne nel cluster Big Data di SQL Server.

In questa esercitazione verranno illustrate le procedure per:

> [!div class="checklist"]
> * Creare una tabella esterna per i dati in un database Oracle esterno.
> * Unire questi dati con dati di valore elevato nell'istanza master.

> [!TIP]
> Se si preferisce, è possibile scaricare ed eseguire uno script per i comandi descritti in questa esercitazione. Per istruzioni, vedere [Esempi di virtualizzazione di dati](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/data-virtualization) in GitHub.

## <a name="prerequisites"></a><a id="prereqs"></a> Prerequisiti

- [Strumenti per Big Data](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **Estensione di SQL Server 2019**
- [Caricare dati di esempio nel cluster Big Data](tutorial-load-sample-data.md)

## <a name="create-an-oracle-table"></a>Creare una tabella Oracle

Questa procedura consente di creare in Oracle una tabella semplice denominata `INVENTORY`.

1. Connettersi a un'istanza di Oracle e al database che si vuole usare per questa esercitazione.

1. Eseguire l'istruzione seguente per creare la tabella `INVENTORY`:

   ```sql
    CREATE TABLE "INVENTORY"
    (
        "INV_DATE" NUMBER(10,0) NOT NULL,
        "INV_ITEM" NUMBER(10,0) NOT NULL,
        "INV_WAREHOUSE" NUMBER(10,0) NOT NULL,
        "INV_QUANTITY_ON_HAND" NUMBER(10,0)
    );

    CREATE INDEX INV_ITEM ON HR.INVENTORY(INV_ITEM);
    ```

1. Importare nella tabella il contenuto del file **inventory.csv**. Questo file è stato creato a partire dagli script di creazione di esempio disponibili nella sezione [Prerequisiti](#prereqs).

## <a name="create-an-external-data-source"></a>Creare un'origine dati esterna

Il primo passaggio prevede la creazione di un'origine dati esterna in grado di accedere al server Oracle.

1. In Azure Data Studio connettersi all'istanza master di SQL Server del cluster Big Data. Per altre informazioni, vedere [Connettersi all'istanza master di SQL Server](connect-to-big-data-cluster.md#master).

1. Fare doppio clic sulla connessione nella finestra **Server** per visualizzare il dashboard del server per l'istanza master di SQL Server. Selezionare **Nuova query**.

   ![Query dell'istanza master di SQL Server](./media/tutorial-query-oracle/sql-server-master-instance-query.png)

1. Eseguire il comando Transact-SQL seguente per modificare il contesto nel database **Sales** dell'istanza master.

   ```sql
   USE Sales
   GO
   ```

1. Creare le credenziali con ambito database per connettersi al server Oracle. Fornire le credenziali appropriate al server Oracle nell'istruzione seguente.

   ```sql
   CREATE DATABASE SCOPED CREDENTIAL [OracleCredential]
   WITH IDENTITY = '<oracle_user,nvarchar(100),SYSTEM>', SECRET = '<oracle_user_password,nvarchar(100),manager>';
   ```

1. Creare un'origine dati esterna che punti al server Oracle.

   ```sql
   CREATE EXTERNAL DATA SOURCE [OracleSalesSrvr]
   WITH (LOCATION = 'oracle://<oracle_server,nvarchar(100)>',CREDENTIAL = [OracleCredential]);
   ```

### <a name="optional-oracle-proxy-authentication"></a>Facoltativo: Autenticazione proxy Oracle

Oracle supporta l'autenticazione proxy per fornire il controllo di accesso con granularità fine. Un utente proxy si connette al database Oracle usando le proprie credenziali e rappresenta un altro utente nel database. 

Un utente proxy può essere configurato in modo che abbia accesso limitato rispetto all'utente rappresentato. Ad esempio, è possibile consentire a un utente proxy di connettersi usando un ruolo del database specifico dell'utente rappresentato. L'identità dell'utente che si connette al database Oracle tramite l'utente proxy viene mantenuta nella connessione, anche se più utenti si connettono tramite l'autenticazione proxy. Questo consente a Oracle di applicare il controllo di accesso e di controllare le azioni intraprese per conto dell'utente effettivo.

Se lo scenario richiede l'uso di un utente proxy Oracle, __sostituire i passaggi 4 e 5 precedenti con i seguenti__.

4. Creare le credenziali con ambito database per connettersi al server Oracle. Specificare le credenziali di utente proxy Oracle appropriate al server Oracle nell'istruzione seguente.

   ```sql
   CREATE DATABASE SCOPED CREDENTIAL [OracleProxyCredential]
   WITH IDENTITY = '<oracle_proxy_user,nvarchar(100),SYSTEM>', SECRET = '<oracle_proxy_user_password,nvarchar(100),manager>';
   ```

5. Creare un'origine dati esterna che punti al server Oracle.

   ```sql
   CREATE EXTERNAL DATA SOURCE [OracleSalesSrvr]
   WITH (LOCATION = 'oracle://<oracle_server,nvarchar(100)>',
   CONNECTION_OPTIONS = 'ImpersonateUser=% CURRENT_USER',
   CREDENTIAL = [OracleProxyCredential]);
   ```

## <a name="create-an-external-table"></a>Creare una tabella esterna

Successivamente, creare una tabella esterna denominata **iventory_ora** sulla base della tabella `INVENTORY` presente nel server Oracle.

```sql
CREATE EXTERNAL TABLE [inventory_ora]
    ([inv_date] DECIMAL(10,0) NOT NULL, [inv_item] DECIMAL(10,0) NOT NULL,
    [inv_warehouse] DECIMAL(10,0) NOT NULL, [inv_quantity_on_hand] DECIMAL(10,0))
WITH (DATA_SOURCE=[OracleSalesSrvr],
        LOCATION='<oracle_service_name,nvarchar(30),xe>.<oracle_schema,nvarchar(128),HR>.<oracle_table,nvarchar(128),INVENTORY>');
```

> [!NOTE]
> I nomi delle tabelle e delle colonne useranno l'identificatore delimitato da ANSI SQL durante l'esecuzione di query su dati Oracle. Per i nomi delle variabili viene quindi fatta distinzione tra maiuscole e minuscole. Nella definizione della tabella esterna è importante specificare il nome rispettando esattamente le lettere minuscole e maiuscole dei nomi delle tabelle e delle colonne nei metadati Oracle.

## <a name="query-the-data"></a>Eseguire una query sui dati

Eseguire la query seguente per creare un join tra i dati della `iventory_ora`tabella esterna e le tabelle nel database `Sales` locale.

```sql
SELECT TOP(100) w.w_warehouse_name, i.inv_item, SUM(i.inv_quantity_on_hand) as total_quantity
  FROM [inventory_ora] as i
  JOIN item as it
    ON it.i_item_sk = i.inv_item
  JOIN warehouse as w
    ON w.w_warehouse_sk = i.inv_warehouse
 WHERE it.i_category = 'Books' and i.inv_item BETWEEN 1 and 18000 --> get items within specific range
 GROUP BY w.w_warehouse_name, i.inv_item;
```

## <a name="clean-up"></a>Eseguire la pulizia

Usare il comando seguente per rimuovere gli oggetti di database creati in questa esercitazione.

```sql
DROP EXTERNAL TABLE [inventory_ora];
DROP EXTERNAL DATA SOURCE [OracleSalesSrvr] ;
DROP DATABASE SCOPED CREDENTIAL [OracleCredential];
```

## <a name="next-steps"></a>Passaggi successivi

Informazioni su come inserire dati nel pool di dati:
> [!div class="nextstepaction"]
> [Caricare dati nel pool di dati](tutorial-data-pool-ingest-sql.md)
