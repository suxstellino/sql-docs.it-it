---
title: Eseguire una copia bulk dei dati in SQL Server in Linux
description: Questo articolo descrive l'utilità bcp. Usare bcp per importare un numero elevato di righe in tabelle di SQL Server oppure per esportare dati da tabelle di SQL Server a file di dati.
author: VanMSFT
ms.author: vanto
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 7b93d0d7-7946-4b78-b33a-57d6307cdfa9
ms.openlocfilehash: c27e4095854abd409336c97ac5b8458bf6f62894
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345617"
---
# <a name="bulk-copy-data-with-bcp-to-sql-server-on-linux"></a>Eseguire una copia bulk dei dati con bcp in SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Questo articolo illustra come usare l'utilità della riga di comando [bcp](../tools/bcp-utility.md) per eseguire una copia bulk dei dati tra un'istanza di SQL Server in Linux e un file di dati in un formato specificato dall'utente.

È possibile usare `bcp` per importare un numero elevato di righe in tabelle di SQL Server oppure per esportare dati da tabelle di SQL Server a file di dati. A eccezione del caso in cui venga usata con l'opzione queryout, `bcp` non richiede alcuna conoscenza di Transact-SQL. L'utilità della riga di comando `bcp` può essere usata con Microsoft SQL Server in esecuzione in locale o nel cloud, in Linux, Windows o Docker e nel database SQL di Azure e in Azure Synapse Analytics.

Questo articolo illustra come:
- Importare i dati in una tabella usando il comando `bcp in`
- Esportare i dati da una tabella usando il comando `bcp out`

## <a name="install-the-sql-server-command-line-tools"></a>Installare gli strumenti da riga di comando di SQL Server

`bcp` fa parte degli strumenti da riga di comando di SQL Server, che non vengono installati automaticamente con SQL Server in Linux. Se gli strumenti da riga di comando di SQL Server non sono già stati installati nel computer Linux, è necessario installarli. Per altre informazioni su come installare gli strumenti, selezionare la distribuzione di Linux dall'elenco seguente:

- [Red Hat Enterprise Linux (RHEL)](sql-server-linux-setup-tools.md#RHEL)
- [Ubuntu](sql-server-linux-setup-tools.md#ubuntu)
- [SUSE Linux Enterprise Server (SLES)](sql-server-linux-setup-tools.md#SLES)

## <a name="import-data-with-bcp"></a>Importare i dati con bcp

In questa esercitazione si creano un database e una tabella di esempio nell'istanza di SQL Server locale (**localhost**) e quindi si usa `bcp` per caricare dati nella tabella di esempio da un file di testo su disco.

### <a name="create-a-sample-database-and-table"></a>Creare un database e una tabella di esempio

Si inizierà con la creazione di un database di esempio con una semplice tabella usata nella parte restante di questa esercitazione.

1. Nel computer Linux aprire un terminale di comando.

2. Copiare e incollare i comandi seguenti nella finestra del terminale. Questi comandi usano l'utilità della riga di comando **sqlcmd** per creare un database di esempio (**BcpSampleDB**) e una tabella (**TestEmployees**) nell'istanza di SQL Server locale (**localhost**). Ricordarsi di sostituire `username` e `<your_password>`, se necessario, prima di eseguire i comandi.

Creare il database **BcpSampleDB**:
```bash 
sqlcmd -S localhost -U sa -P <your_password> -Q "CREATE DATABASE BcpSampleDB;"
```
Creare la tabella **TestEmployees** nel database **BcpSampleDB**:
```bash 
sqlcmd -S localhost -U sa -P <your_password> -d BcpSampleDB -Q "CREATE TABLE TestEmployees (Id INT IDENTITY(1,1) NOT NULL PRIMARY KEY, Name NVARCHAR(50), Location NVARCHAR(50));"
```
### <a name="create-the-source-data-file"></a>Creare il file di dati di origine
Copiare e incollare il comando seguente nella finestra del terminale. Si usa il comando `cat` predefinito per creare un file di dati di esempio con tre record. Salvare il file nella home directory come **~/test_data.txt**. I campi nei record sono delimitati da una virgola.

```bash
cat > ~/test_data.txt << EOF
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
EOF
```

È possibile verificare che il file di dati sia stato creato correttamente eseguendo il comando seguente nella finestra del terminale:
```bash 
cat ~/test_data.txt
```

Nella finestra del terminale dovrebbe essere visualizzato quanto segue:
```bash
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
```

### <a name="import-data-from-the-source-data-file"></a>Importa i dati dal file di dati di origine
Copiare e incollare i comandi seguenti nella finestra del terminale. Questo comando usa `bcp` per connettersi all'istanza di SQL Server locale (**localhost**) e importare i dati dal file di dati ( **~/test_data.txt**) nella tabella (**TestEmployees**) nel database (**BcpSampleDB**). Ricordarsi di sostituire il nome utente e `<your_password>`, se necessario, prima di eseguire i comandi.

```bash 
bcp TestEmployees in ~/test_data.txt -S localhost -U sa -P <your_password> -d BcpSampleDB -c -t  ','
```

Ecco una breve panoramica dei parametri della riga di comando usati con `bcp` in questo esempio:
- `-S`: specifica l'istanza di SQL Server alla quale connettersi
- `-U`: specifica l'ID di accesso usato per connettersi SQL Server
- `-P`: specifica la password per l'ID di accesso
- `-d`: specifica il database al quale connettersi
- `-c`: esegue le operazioni usando un tipo di dati carattere
- `-t`: specifica il carattere di terminazione del campo. Per i record nel file di dati viene usato `comma` come carattere di terminazione del campo

> [!NOTE]
> In questo esempio non viene specificato un carattere di terminazione della riga personalizzato. Le righe nel file di dati di testo sono state terminate correttamente con `newline` quando è stato usato il comando `cat` per creare il file di dati in precedenza.

È possibile verificare che i dati siano stati importati correttamente eseguendo il comando seguente nella finestra del terminale. Ricordarsi di sostituire `username` e `<your_password>`, se necessario, prima di eseguire il comando.
```bash 
sqlcmd -S localhost -d BcpSampleDB -U sa -P <your_password> -I -Q "SELECT * FROM TestEmployees;"
```

Verranno visualizzati i risultati seguenti:
```bash
Id          Name                Location
----------- ------------------- -------------------
          1 Jared               Australia
          2 Nikita              India
          3 Tom                 Germany

(3 rows affected)
```

## <a name="export-data-with-bcp"></a>Esportare i dati con bcp

In questa esercitazione si usa `bcp` per esportare i dati dalla tabella di esempio creata in precedenza a un nuovo file di dati.

Copiare e incollare i comandi seguenti nella finestra del terminale. Questi comandi usano l'utilità della riga di comando `bcp` per esportare i dati dalla tabella **TestEmployees** nel database **BcpSampleDB** a un nuovo file di dati denominato **~/test_export.txt**.  Ricordarsi di sostituire il nome utente e `<your_password>`, se necessario, prima di eseguire il comando.

```bash 
bcp TestEmployees out ~/test_export.txt -S localhost -U sa -P <your_password> -d BcpSampleDB -c -t ','
```

È possibile verificare che il file di dati sia stato esportato correttamente eseguendo il comando seguente nella finestra del terminale:
```bash 
cat ~/test_export.txt
```

Nella finestra del terminale dovrebbe essere visualizzato quanto segue:
```
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
```

## <a name="see-also"></a>Vedere anche
- [utilità bcp](../tools/bcp-utility.md)
- [Formati di dati per la compatibilità quando si usa bcp](../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)
- [Importare dati per operazioni bulk usando BULK INSERT](../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)
- [BULK INSERT (Transact-SQL)](../t-sql/statements/bulk-insert-transact-sql.md)
