---
title: Installare pacchetti Python con sqlmlutils
description: Informazioni su come usare lo strumento pip di Python per installare nuovi pacchetti Python in un'istanza di Machine Learning Services per SQL Server.
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/26/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 9468ec6b2a196e2fa75935fb603385faf9f2a958
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273042"
---
# <a name="install-python-packages-with-sqlmlutils"></a>Installare pacchetti Python con sqlmlutils

[!INCLUDE [SQL Server 2019 SQL MI](../../includes/applies-to-version/sqlserver2019-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Questo articolo descrive come usare le funzioni del pacchetto [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils) per installare nuovi pacchetti Python in un'istanza di [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md) e in [cluster Big Data](../../big-data-cluster/machine-learning-services.md). I pacchetti installati possono essere usati negli script Python in esecuzione nel database usando l'istruzione T-SQL [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
Questo articolo descrive come usare le funzioni del pacchetto [**sqlmlutils**](https://github.com/Microsoft/sqlmlutils) per installare nuovi pacchetti Python in un'istanza di [Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview). I pacchetti installati possono essere usati negli script Python in esecuzione nel database usando l'istruzione T-SQL [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).
::: moniker-end

Per altre informazioni sulla posizione dei pacchetti e sui percorsi di installazione, vedere [Ottenere informazioni sui pacchetti Python](../package-management/python-package-information.md).

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!NOTE]
> Il pacchetto **sqlmlutils** descritto in questo articolo viene usato per aggiungere pacchetti Python a SQL Server 2019 o versione successiva. Per SQL Server 2017 e versioni precedenti, vedere [Installare i pacchetti con gli strumenti Python](./install-python-packages-standard-tools.md?view=sql-server-2017&preserve-view=true).
::: moniker-end

## <a name="prerequisites"></a>Prerequisiti

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
+ È necessario che sia installato [Machine Learning Services per SQL Server](../install/sql-machine-learning-services-windows-install.md) con l'opzione relativa al linguaggio Python.
::: moniker-end

+ Installare [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) nel computer client usato per connettersi a SQL Server. È possibile usare altri strumenti di query o gestione di database, ma in questo articolo si presuppone che venga usato Azure Data Studio.

+ Installare il kernel Python in Azure Data Studio. È anche possibile installare e usare Python dalla riga di comando e usare un ambiente di sviluppo Python alternativo come [Visual Studio Code](https://code.visualstudio.com/download) con l'[estensione Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

  La versione di Python nel computer client deve corrispondere alla versione di Python nel server e i pacchetti installati devono essere conformi alla versione di Python in uso.
  Per informazioni sulla versione di Python inclusa con ogni versione di SQL Server, vedere [Versioni di Python e R](../sql-server-machine-learning-services.md#versions).
  
  Per verificare la versione di Python in una particolare istanza di SQL Server, usare il comando T-SQL seguente.

  ```sql
  EXECUTE sp_execute_external_script
    @language = N'Python',
    @script = N'
  import sys
  print(sys.version)
  '
  ```

### <a name="other-considerations"></a>Altre considerazioni

+ La libreria di pacchetti Python si trova nella cartella Programmi dell'istanza di SQL Server e, per impostazione predefinita, è necessario disporre delle autorizzazioni di amministratore per eseguire installazioni in questa cartella. Per altre informazioni, vedere [Percorso della libreria dei pacchetti](../package-management/python-package-information.md#default-python-library-location).

+ L'installazione del pacchetto è specifica dell'istanza di SQL, del database e dell'utente indicati nelle informazioni di connessione specificate per **sqlmlutils**. Per usare il pacchetto in più istanze o database SQL oppure per utenti diversi, è necessario installare il pacchetto per ognuno di essi. L'eccezione è che se il pacchetto viene installato da un membro di `dbo`, il pacchetto è *pubblico* ed è condiviso con tutti gli utenti. Se un utente installa una versione più recente di un pacchetto pubblico, tale pacchetto non sarà interessato, ma l'utente avrà accesso alla versione più recente.

+ Prima di aggiungere un pacchetto, valutare se è adatto all'ambiente di SQL Server.

  + È consigliabile usare Python all'interno nel database per le attività che traggono vantaggio da una stretta integrazione con il motore di database, ad esempio Machine Learning, piuttosto che per le attività che eseguono semplicemente query sul database.

  + Se si aggiungono pacchetti che richiedono un utilizzo elevato delle risorse di calcolo del server, possono verificarsi problemi di prestazioni.

  + In un ambiente di SQL Server con protezione avanzata, è consigliabile evitare i tipi di pacchetti seguenti:
    + Pacchetti che richiedono l'accesso alla rete
    + Pacchetti che richiedono l'accesso al file system con privilegi elevati
    + Pacchetti usati per lo sviluppo Web o altre attività che non traggono vantaggio dall'esecuzione all'interno di SQL Server

  + Il pacchetto Python **tensorflow** non può essere installato usando sqlmlutils. Per altre informazioni e per una soluzione alternativa, vedere [Problemi noti in Machine Learning Services per SQL Server](../troubleshooting/known-issues-for-sql-server-machine-learning-services.md#9-cannot-install-tensorflow-package-using-sqlmlutils).

## <a name="install-sqlmlutils-on-the-client-computer"></a>Installare sqlmlutils nel computer client

Prima di usare **sqlmlutils**, è necessario installarlo nel computer client usato per connettersi a SQL Server.

### <a name="in-azure-data-studio"></a>In Azure Data Studio

Se si prevede di usare **sqlmlutils** in Azure Data Studio, è possibile eseguire l'installazione usando la funzionalità Gestisci pacchetti in un notebook del kernel Python.

1. In un [notebook del kernel Python in Azure Data Studio](../../azure-data-studio/notebooks/notebooks-python-kernel.md) fare clic su **Gestisci pacchetti**.
1. Fare clic su **Aggiungi nuovo**.
1. Immettere "sqlmlutils" nel campo **Search Pip packages** (Cerca pacchetti Pip) e fare clic su **Cerca**.
1. In **Versione del pacchetto** selezionare la versione da installare (è consigliabile scegliere la versione più recente).
1. Fare clic su **Installa** e quindi su **Chiudi**.

### <a name="from-python-command-line"></a>Dalla riga di comando Python

Se si prevede di usare **sqlmlutils** da un ambiente di sviluppo integrato (IDE) o da un prompt dei comandi Python, è possibile installare sqlmlutils con un semplice comando **pip**:

```console
pip install sqlmlutils
```

È anche possibile installare **sqlmlutils** da un file ZIP:

1. Assicurarsi che **pip** sia installato. Per altre informazioni, vedere [Installazione di pip](https://pip.pypa.io/en/stable/installing/).
1. Scaricare la versione più recente del file ZIP di **sqlmlutils** da https://github.com/Microsoft/sqlmlutils/tree/master/Python/dist nel computer client. Non decomprimere il file.
1. Aprire un **prompt dei comandi** ed eseguire i comandi seguenti per installare il pacchetto **sqlmlutils**. Sostituire il percorso completo del file ZIP di **sqlmlutils** scaricato. In questo esempio si presuppone che il file scaricato sia `c:\temp\sqlmlutils-1.0.0.zip`.
   ```console
   pip install --upgrade --upgrade-strategy only-if-needed c:\temp\sqlmlutils-1.0.0.zip
   ```

## <a name="add-a-python-package-on-sql-server"></a>Aggiungere un pacchetto Python in SQL Server

Usando **sqlmlutils**, è possibile aggiungere pacchetti Python a un'istanza di SQL. È quindi possibile usare tali pacchetti nel codice Python in esecuzione nell'istanza di SQL. **sqlmlutils** usa [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) per installare il pacchetto e ognuna delle relative dipendenze.

Nell'esempio seguente si aggiungerà il pacchetto [text-tools](https://pypi.org/project/text-tools/) in SQL Server.

### <a name="add-the-package-online"></a>Aggiungere il pacchetto online

Se il computer client usato per connettersi a SQL Server dispone di accesso a Internet, è possibile usare **sqlmlutils** per trovare il pacchetto **text-tools** e le eventuali dipendenze su Internet, quindi installare il pacchetto in un'istanza di SQL Server in modalità remota.

::: moniker range=">=sql-server-ver15"

1. Nel computer client aprire **Python** o un ambiente per Python.

1. Usare i comandi seguenti per installare il pacchetto **text-tools**. Usare le proprie informazioni di connessione al database di SQL Server. Se si usa l'autenticazione di Windows, i parametri `uid` e `pwd` non sono necessari.

::: moniker-end

::: moniker range=">=sql-server-linux-ver15||=azuresqldb-mi-current"

1. Nel computer client aprire **Python** o un ambiente per Python.

1. Usare i comandi seguenti per installare il pacchetto **text-tools**. Sostituire le informazioni di connessione del database di SQL Server personalizzate.

::: moniker-end

   ```python
   import sqlmlutils
   connection = sqlmlutils.ConnectionInfo(server="server", database="database", uid="username", pwd="password")
   sqlmlutils.SQLPackageManager(connection).install("text-tools")
   ```

### <a name="add-the-package-offline"></a>Aggiungere il pacchetto offline

Se il computer client usato per connettersi a SQL Server non dispone di accesso a Internet, è possibile usare **pip** in un computer con accesso a Internet per scaricare il pacchetto e gli eventuali pacchetti dipendenti in una cartella locale. Copiare quindi la cartella nel computer client in cui è possibile installare il pacchetto offline.

#### <a name="on-a-computer-with-internet-access"></a>In un computer con accesso a Internet

1. Aprire un **prompt dei comandi** ed eseguire il comando seguente per creare una cartella locale contenente il pacchetto **text-tools**. Questo esempio crea la cartella `c:\temp\text-tools`.

   ```console
   pip download text-tools -d c:\temp\text-tools
   ```

1. Copiare la cartella `text-tools` nel computer client. Nell'esempio seguente si presuppone che sia stata copiata in `c:\temp\packages\text-tools`.

#### <a name="on-the-client-computer"></a>Nel computer client

Usare **sqlmlutils** per installare ogni pacchetto (file WHL) presente nella cartella locale creata da **pip**. L'ordine di installazione dei pacchetti non è importante.

In questo esempio, **text-tools** non ha dipendenze e pertanto nella cartella `text-tools` è presente un solo file da installare. Al contrario, un pacchetto come **scikit-plot** ha 11 dipendenze e quindi nella cartella si troveranno 12 file da installare singolarmente, ovvero il pacchetto **scikit-plot** e gli 11 pacchetti dipendenti.

::: moniker range=">=sql-server-ver15"

Eseguire lo script Python seguente. Usare il percorso e il nome effettivi del pacchetto e le proprie informazioni di connessione al database di SQL Server. Se si usa l'autenticazione di Windows, i parametri `uid` e `pwd` non sono necessari. Ripetere l'istruzione `sqlmlutils.SQLPackageManager` per ogni file di pacchetto nella cartella.

::: moniker-end

::: moniker range=">=sql-server-linux-ver15||=azuresqldb-mi-current"

Eseguire lo script Python seguente. Usare il percorso e il nome effettivi del pacchetto e le proprie informazioni di connessione al database di SQL Server. Ripetere l'istruzione `sqlmlutils.SQLPackageManager` per ogni file di pacchetto nella cartella.

::: moniker-end

```python
import sqlmlutils
connection = sqlmlutils.ConnectionInfo(server="yourserver", database="yourdatabase", uid="username", pwd="password"))
sqlmlutils.SQLPackageManager(connection).install("text_tools-1.0.0-py3-none-any.whl")
```

## <a name="use-the-package"></a>Usare il pacchetto

È ora possibile usare il pacchetto in uno script Python in SQL Server. Ad esempio:

```sql
EXECUTE sp_execute_external_script
  @language = N'Python',
  @script = N'
from text_tools.finders import find_best_string
corpus = "Lorem Ipsum text"
query = "Ipsum"
first_match = find_best_string(query, corpus)
print(first_match)
  '
```

## <a name="remove-the-package-from-sql-server"></a>Rimuovere il pacchetto da SQL Server

Se si vuole rimuovere il pacchetto **text-tools**, usare il comando Python seguente nel computer client, con la stessa variabile di connessione definita in precedenza.

```python
sqlmlutils.SQLPackageManager(connection).uninstall("text-tools")
```

## <a name="more-sqlmlutils-functions"></a>Altre funzioni sqlmlutils

Il pacchetto **sqlmlutils** contiene una serie di funzioni per la gestione dei pacchetti Python e per la creazione, la gestione e l'esecuzione di stored procedure e query in un'istanza di SQL Server. Per informazioni dettagliate, vedere il [file README di sqlmlutils Python](https://github.com/microsoft/sqlmlutils/tree/master/Python).

Per informazioni su qualsiasi funzione di **sqlmlutils**, usare la funzione **help** di Python. Ad esempio:

```Python
import sqlmlutils
help(SQLPackageManager.install)
```

## <a name="next-steps"></a>Passaggi successivi

+ Per informazioni sui pacchetti Python installati in Machine Learning Services per SQL Server, vedere [Ottenere informazioni sui pacchetti Python](../package-management/python-package-information.md).

+ Per informazioni sull'installazione dei pacchetti R in Machine Learning Services per SQL Server, vedere [Installare nuovi pacchetti R in SQL Server](install-additional-r-packages-on-sql-server.md).
