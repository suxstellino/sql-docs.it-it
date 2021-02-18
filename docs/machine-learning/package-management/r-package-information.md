---
title: Ottenere informazioni sui pacchetti R
description: Istruzioni su come ottenere informazioni sui pacchetti R installati in Machine Learning Services per SQL Server e R Services per SQL Server.
ms.custom: ''
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/27/2020
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 3669b46fa88e8d59fbb3eeb5ff91bc8bb9542ce7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352534"
---
# <a name="get-r-package-information"></a>Ottenere informazioni sui pacchetti R

[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Questo articolo descrive come ottenere informazioni sui pacchetti R installati in [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md) e in [cluster Big Data](../../big-data-cluster/machine-learning-services.md). Gli script R di esempio mostrano come elencare le informazioni sui pacchetti, ad esempio il percorso di installazione e la versione.
::: moniker-end
::: moniker range="<=sql-server-2017"
Questo articolo descrive come ottenere informazioni sui pacchetti R installati in [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md). Gli script R di esempio mostrano come elencare le informazioni sui pacchetti, ad esempio il percorso di installazione e la versione.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Questo articolo descrive come ottenere informazioni sui pacchetti R installati in [Machine Learning Services per Istanza gestita di SQL di Azure](/azure/azure-sql/managed-instance/machine-learning-services-overview). Gli script R di esempio mostrano come elencare le informazioni sui pacchetti, ad esempio il percorso di installazione e la versione.
::: moniker-end

## <a name="default-r-library-location"></a>Percorso della libreria R predefinita

Quando si installa Machine Learning con SQL Server, viene creata una singola libreria di pacchetti a livello di istanza per ogni linguaggio installato. In Windows la libreria dell'istanza è una cartella protetta registrata con SQL Server.

Qualsiasi script eseguito nel database in SQL Server deve caricare le funzioni dalla libreria dell'istanza. SQL Server non può accedere ai pacchetti installati in altre librerie. Questo vale anche per i client remoti. Qualsiasi script R in esecuzione nel contesto di calcolo del server può usare solo i pacchetti installati nella libreria dell'istanza.
Per assicurare la protezione delle risorse del server, la libreria dell'istanza predefinita può essere modificata solo da un amministratore del computer.

::: moniker range="=sql-server-2016"
Il percorso predefinito dei file binari per R è il seguente:

`C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\library`

In questo esempio si presuppone che venga usata l'istanza di SQL predefinita, ovvero MSSQLSERVER. Se SQL Server è installato come istanza definita dall'utente, in alternativa viene usato il nome specificato.
::: moniker-end

::: moniker range="=sql-server-2017"
Il percorso predefinito dei file binari per R è il seguente:

`C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library`

In questo esempio si presuppone che venga usata l'istanza di SQL predefinita, ovvero MSSQLSERVER. Se SQL Server è installato come istanza definita dall'utente, in alternativa viene usato il nome specificato.
::: moniker-end

::: moniker range=">=sql-server-ver15"
Il percorso predefinito dei file binari per R è il seguente:

`C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES\library`

In questo esempio si presuppone che venga usata l'istanza di SQL predefinita, ovvero MSSQLSERVER. Se SQL Server è installato come istanza definita dall'utente, in alternativa viene usato il nome specificato.
::: moniker-end

Eseguire l'istruzione seguente per verificare la libreria di pacchetti R predefinita per l'istanza corrente:

```sql
EXECUTE sp_execute_external_script  
  @language = N'R',
  @script = N'OutputDataSet <- data.frame(.libPaths());'
WITH RESULT SETS (([DefaultLibraryName] VARCHAR(MAX) NOT NULL));
GO
```

## <a name="default-microsoft-r-packages"></a>Pacchetti Microsoft R predefiniti

::: moniker range="=sql-server-2016"

Con R Services per SQL Server vengono installati i pacchetti Microsoft R seguenti.

|Pacchetti | Versione | Descrizione |
|---------|---------|-------------|
| [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler)  | 8.0.3 | Usato per contesti di calcolo remoti, streaming, esecuzione parallela di funzioni rx per l'importazione e la trasformazione dei dati, modellazione, visualizzazione e analisi. |
| [sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils) | 1.0.0 | Usato per includere script R nelle stored procedure. |

::: moniker-end

::: moniker range="=sql-server-2017"

Quando si seleziona la funzionalità R durante l'installazione di Machine Learning Services per SQL Server, vengono installati i pacchetti Microsoft R seguenti.

|Pacchetti | Versione | Descrizione |
|---------|---------|-------------|
| [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler)  | 9.2 | Usato per contesti di calcolo remoti, streaming, esecuzione parallela di funzioni rx per l'importazione e la trasformazione dei dati, modellazione, visualizzazione e analisi. |
| [sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils) | 1.0.0 | Usato per includere script R nelle stored procedure. |
| [MicrosoftML](/r-server/r-reference/microsoftml/microsoftml-package)| 1.4.0 | Aggiunge algoritmi di Machine Learning in R. | 
| [olapR](/machine-learning-server/r-reference/olapr/olapr) | 1.0.0 | Usato per la scrittura di istruzioni MDX in R. |

::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"

Quando si seleziona la funzionalità R durante l'installazione di Machine Learning Services per SQL Server, vengono installati i pacchetti Microsoft R seguenti.

|Pacchetti | Versione | Descrizione |
|---------|---------|-------------|
| [RevoScaleR](/r-server/r-reference/revoscaler/revoscaler)  | 9.4.7 | Usato per contesti di calcolo remoti, streaming, esecuzione parallela di funzioni rx per l'importazione e la trasformazione dei dati, modellazione, visualizzazione e analisi. |
| [sqlrutils](/machine-learning-server/r-reference/sqlrutils/sqlrutils) | 1.0.0 | Usato per includere script R nelle stored procedure. |
| [MicrosoftML](/r-server/r-reference/microsoftml/microsoftml-package)| 9.4.7 | Aggiunge algoritmi di Machine Learning in R. |
| [olapR](/machine-learning-server/r-reference/olapr/olapr) | 1.0.0 | Usato per la scrittura di istruzioni MDX in R. |

::: moniker-end

### <a name="component-upgrades"></a>Aggiornamenti dei componenti

Per impostazione predefinita, i pacchetti R vengono aggiornati tramite i Service Pack e gli aggiornamenti cumulativi. I pacchetti aggiuntivi e gli aggiornamenti completi della versione dei componenti di base di R sono possibili solo tramite aggiornamenti del prodotto oppure tramite il binding del supporto R con Microsoft Machine Learning Server.

::: moniker range="=sql-server-2016"
È inoltre possibile aggiungere pacchetti MicrosoftML e olapR a un'istanza di SQL Server tramite un aggiornamento dei componenti.
::: moniker-end

Per altre informazioni, vedere [Aggiornare i componenti R e Python in SQL Server](../install/upgrade-r-and-python.md).

## <a name="default-open-source-r-packages"></a>Pacchetti R open source predefiniti

Il supporto per R include la distribuzione di R open source. In questo modo è possibile chiamare le funzioni R di base e installare altri pacchetti open source e di terze parti. Il supporto per il linguaggio R include le funzionalità di base, ad esempio **base**, **stats**, **utils** e altre ancora. Un'installazione di base di R include anche numerosi set di dati di esempio e strumenti R standard come **RGui** (un editor interattivo leggero) e **RTerm** (un prompt dei comandi R).

La distribuzione di R open source inclusa nell'installazione è [Microsoft R Open (MRO)](https://mran.microsoft.com/open). MRO aggiunge valore alle funzioni R di base includendo pacchetti open source aggiuntivi come [Intel Math Kernel Library](https://en.wikipedia.org/wiki/Math_Kernel_Library).

Per informazioni sulla versione di R inclusa con ogni versione di SQL Server, vedere [Versioni di Python e R](../sql-server-machine-learning-services.md#versions).

> [!IMPORTANT]
> Non si dovrebbe mai sovrascrivere manualmente la versione di R installata dal programma di installazione di SQL Server con le versioni più recenti sul Web. I pacchetti Microsoft per R sono basati su versioni specifiche di R. La modifica dell'installazione può rendere instabile questa distribuzione.

## <a name="list-all-installed-r-packages"></a>Elenco di tutti i pacchetti R installati

L'esempio seguente usa la funzione R `installed.packages()` in una stored procedure di [!INCLUDE[tsql](../../includes/tsql-md.md)] per visualizzare un elenco dei pacchetti R installati nella libreria R_SERVICES per l'istanza SQL corrente. Questo script restituisce i campi relativi al nome e alla versione del pacchetto nel file DESCRIPTION.

```sql
EXECUTE sp_execute_external_script
  @language=N'R',
@script = N'str(OutputDataSet);
packagematrix <- installed.packages();
Name <- packagematrix[,1];
Version <- packagematrix[,3];
OutputDataSet <- data.frame(Name, Version);',
@input_data_1 = N'
  '
WITH RESULT SETS ((PackageName nvarchar(250), PackageVersion nvarchar(max) ))
```

Per altre informazioni sui campi facoltativi e predefiniti per il file DESCRIPTION del pacchetto R, vedere [https://cran.r-project.org](https://cran.r-project.org/doc/manuals/R-exts.html#The-DESCRIPTION-file).

## <a name="find-a-single-r-package"></a>Trovare un singolo pacchetto R

Se è stato installato un pacchetto R e si vuole verificare che il pacchetto sia disponibile per una determinata istanza di SQL Server, è possibile eseguire una stored procedure per caricare il pacchetto e restituire messaggi.

L'istruzione seguente, ad esempio, esegue la ricerca del pacchetto [glue](https://cran.r-project.org/web/packages/glue/) e lo carica, se disponibile.
Se il pacchetto non viene trovato o non può essere caricato, viene restituito un errore.

```sql
EXECUTE sp_execute_external_script  
  @language =N'R',
  @script=N'
require("glue")
'
```

Per altre informazioni sul pacchetto, vedere la funzione `packageDescription`.
L'istruzione seguente restituisce informazioni per il pacchetto **MicrosoftML**.

```sql
EXECUTE sp_execute_external_script
  @language = N'R',
  @script = N'
print(packageDescription("MicrosoftML"))
'
```

## <a name="next-steps"></a>Passaggi successivi

::: moniker range="<=sql-server-2017"
+ [Installare pacchetti con gli strumenti R](install-r-packages-standard-tools.md)
::: moniker-end
::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=azuresqldb-mi-current"
+ [Installare nuovi pacchetti R con sqlmlutils](install-additional-r-packages-on-sql-server.md)
::: moniker-end