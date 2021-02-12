---
title: Usare T-SQL (CREATE EXTERNAL LIBRARY) per installare i pacchetti R
description: Aggiungere nuovi pacchetti R a SQL Server 2016 R Services o a Machine Learning Services (In-Database) di SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2017
ms.openlocfilehash: 058cd3a573abfa427b1e71ac08ac66c4b0e17932
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074352"
---
# <a name="use-t-sql-create-external-library-to-install-r-packages-on-sql-server"></a>Usare T-SQL (CREATE EXTERNAL LIBRARY) per installare i pacchetti R in SQL Server
[!INCLUDE [SQL Server 2017 only](../../includes/applies-to-version/sqlserver2017-only.md)]

Questo articolo illustra come installare nuovi pacchetti R in un'istanza di SQL Server in cui è abilitato Machine Learning. È possibile scegliere tra più approcci. L'uso di T-SQL è consigliabile agli amministratori di server che non hanno familiarità con R.

L'istruzione [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) consente di aggiungere un pacchetto o un set di pacchetti a un'istanza o a un database specifico senza eseguire direttamente il codice R o Python. Questo metodo richiede tuttavia la preparazione del pacchetto e autorizzazioni aggiuntive per il database.

+ Tutti i pacchetti devono essere disponibili come file compresso locale, anziché essere scaricati su richiesta da Internet.

+ Tutte le dipendenze devono essere identificate in base al nome e alla versione e devono essere incluse nel file ZIP. L'istruzione avrà esito negativo se i pacchetti necessari non sono disponibili, incluse le dipendenze dei pacchetti downstream. 

+ È necessario essere un utente **db_owner** o disporre dell'autorizzazione CREATE EXTERNAL LIBRARY in un ruolo del database. Per informazioni dettagliate, vedere [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md).

## <a name="download-packages-in-archive-format"></a>Scaricare i pacchetti in formato archivio

Se si installa un singolo pacchetto, scaricarlo in formato compresso.

A causa delle dipendenze, generalmente vengono installati più pacchetti. Quando un pacchetto richiede altri pacchetti, è necessario verificare che tutti siano accessibili tra loro durante l'installazione. È consigliabile [creare un repository locale](create-a-local-package-repository-using-minicran.md) usando [miniCRAN](https://andrie.github.io/miniCRAN/), per assemblare una raccolta completa di pacchetti, e [igraph](https://igraph.org/r/) per analizzare le dipendenze dei pacchetti. Se si installa la versione errata di un pacchetto o si omette una dipendenza del pacchetto, può succedere che l'istruzione CREATE EXTERNAL LIBRARY non venga eseguita correttamente. 

## <a name="copy-the-file-to-a-local-folder"></a>Copiare il file in una cartella locale

Copiare il file compresso contenente tutti i pacchetti in una cartella locale nel server. Se non si ha accesso al file system nel server, è anche possibile passare un pacchetto completo come variabile, usando un formato binario. Per altre informazioni, vedere [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md).

## <a name="run-the-statement-to-upload-packages"></a>Eseguire l'istruzione per caricare i pacchetti

Aprire una finestra **Query** usando un account con privilegi amministrativi.

Eseguire l'istruzione T-SQL `CREATE EXTERNAL LIBRARY` per caricare la raccolta di pacchetti compressi nel database.

L'istruzione seguente ad esempio definisce come origine del pacchetto un repository miniCRAN contenente il pacchetto **randomForest** e le relative dipendenze. 

```sql
CREATE EXTERNAL LIBRARY [randomForest]
FROM (CONTENT = 'C:\Temp\Rpackages\randomForest_4.6-12.zip')
WITH (LANGUAGE = 'R');
```

Non è possibile usare un nome arbitrario. Il nome della libreria esterna deve avere lo stesso nome che si prevede di usare quando si carica o chiama il pacchetto.

## <a name="verify-package-installation"></a>Verificare l'installazione del pacchetto

Se la libreria viene creata correttamente, è possibile eseguire il pacchetto in SQL Server chiamandolo all'interno di una stored procedure.
    
```sql
EXEC sp_execute_external_script
@language =N'R',
@script=N'library(randomForest)'
```

## <a name="see-also"></a>Vedere anche

+ [Ottenere informazioni sui pacchetti R](r-package-information.md)
+ [Esercitazioni di R](../tutorials/r-tutorials.md)