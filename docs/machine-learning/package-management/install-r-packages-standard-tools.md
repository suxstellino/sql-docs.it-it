---
title: Installare i pacchetti con gli strumenti R
description: Informazioni su come usare gli strumenti R standard per installare nuovi pacchetti R in un'istanza di Machine Learning Services di SQL Server o SQL Server R Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: e54df6240cf1bf4dcaee7813eaf614a84c31a0f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082622"
---
# <a name="install-packages-with-r-tools"></a>Installare i pacchetti con gli strumenti R

[!INCLUDE [SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

Questo articolo descrive come usare gli strumenti R standard per installare nuovi pacchetti R in un'istanza di Machine Learning Services di SQL Server o SQL Server R Services. È possibile installare i pacchetti in un'istanza di SQL Server che dispone di una connessione Internet, ma anche in una isolata da Internet.

Oltre agli strumenti R standard, è possibile installare i pacchetti R usando le soluzioni seguenti:

+ [RevoScaleR](install-r-packages-with-revoscaler.md)
::: moniker range="=sql-server-2017"
+ [T-SQL](install-r-packages-with-tsql.md) (CREATE EXTERNAL LIBRARY)
::: moniker-end

## <a name="general-considerations"></a>Considerazioni generali

+ Il codice R eseguito in SQL Server può usare solo i pacchetti installati nella libreria dell'istanza predefinita. SQL Server non è in grado di caricare pacchetti da librerie esterne, anche se si trovano nello stesso computer,
incluse le librerie R installate con altri prodotti Microsoft.

+ La libreria di pacchetti R si trova nella cartella Programmi dell'istanza di SQL Server e, per impostazione predefinita, è necessario disporre delle autorizzazioni di amministratore per eseguire installazioni in questa cartella. Per altre informazioni, vedere [Percorso della libreria dei pacchetti](../package-management/r-package-information.md#default-r-library-location).

  ::: moniker range="=sql-server-2017"
  Gli utenti non amministratori possono installare i pacchetti usando RevoScaleR 9.0.1 e versioni successive oppure CREATE EXTERNAL LIBRARY. L'utente **dbo_owner** o un utente con autorizzazione CREATE EXTERNAL LIBRARY può installare i pacchetti R nel database corrente. Per altre informazioni, vedere:
  + [Usare RevoScaleR per installare i pacchetti R](install-r-packages-with-revoscaler.md)
  + [Usare T-SQL (CREATE EXTERNAL LIBRARY) per installare i pacchetti R in SQL Server](install-r-packages-with-tsql.md)
  ::: moniker-end

  ::: moniker range="=sql-server-2016"
  Gli utenti non amministratori possono installare i pacchetti usando RevoScaleR 9.0.1 o versioni successive. L'utente **dbo_owner** può installare i pacchetti R nel database corrente. Per altre informazioni, vedere [Usare RevoScaleR per installare i pacchetti R](install-r-packages-with-revoscaler.md).
  ::: moniker-end

+ In un ambiente di SQL Server con protezione avanzata, è consigliabile evitare i tipi di pacchetti seguenti:
  + Pacchetti che richiedono l'accesso alla rete
  + Pacchetti che richiedono l'accesso al file system con privilegi elevati
  + Pacchetti usati per lo sviluppo Web o altre attività che non traggono vantaggio dall'esecuzione all'interno di SQL Server

## <a name="online-installation-with-internet-access"></a>Installazione online (con accesso a Internet)

Se l'istanza di SQL Server ha accesso a Internet, è possibile usare gli strumenti standard di installazione dei pacchetti per installare i pacchetti R.

1. Determinare il percorso della libreria dell'istanza (vedere [Ottenere informazioni sui pacchetti R](../package-management/r-package-information.md)) e passare alla cartella in cui sono installati gli strumenti R.

   ::: moniker range="=sql-server-2016"
   Il percorso predefinito per l'istanza predefinita di SQL Server 2017 è ad esempio il seguente:

   `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   Il percorso predefinito per l'istanza predefinita di SQL Server 2017 è ad esempio il seguente:

   `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

1. Eseguire **R** o **Rgui** come amministratore da questa cartella.

1. Eseguire il comando R `install.packages` e specificare il nome del pacchetto. Se il pacchetto ha dipendenze, il programma di installazione scarica automaticamente le dipendenze e le installa.

Se si dispone di più istanze affiancate di SQL Server, eseguire l'installazione separatamente per ogni istanza in cui si vuole usare il pacchetto. I pacchetti non possono essere condivisi tra istanze.

## <a name="offline-installation-no-internet-access"></a><a name = "bkmk_offlineInstall"></a> Installazione offline (senza accesso a Internet)

I server che ospitano i database di produzione spesso non hanno una connessione Internet. Per installare i pacchetti R in tale ambiente, scaricare e preparare i pacchetti e le dipendenze in anticipo come file compressi, quindi copiare i file in una cartella nel server. A questo punto i pacchetti possono essere installati offline.

Identificare tutte le dipendenze è un'operazione complicata. Per R, è consigliabile usare [**miniCRAN**](https://andrie.github.io/miniCRAN/) per creare un repository locale.
**miniCRAN** accetta un elenco di pacchetti da installare, analizza le dipendenze e raccoglie tutti i file compressi necessari. Crea poi un singolo repository che è possibile copiare nell'istanza di SQL Server isolata. Anche il pacchetto [**igraph**](https://igraph.org/r/) è utile per analizzare le dipendenze dei pacchetti.

Per altre informazioni, vedere [Creare un repository di pacchetti R locale usando miniCRAN](create-a-local-package-repository-using-minicran.md).

Quando il file ZIP si trova nell'istanza di SQL Server, è possibile installarlo usando gli strumenti R standard nel server.

1. Determinare il percorso della libreria dell'istanza (vedere [Ottenere informazioni sui pacchetti R](../package-management/r-package-information.md)) e passare alla cartella in cui sono installati gli strumenti R. 

   ::: moniker range="=sql-server-2016"
   Il percorso predefinito per l'istanza predefinita di SQL Server 2017 è ad esempio il seguente:

   `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   Il percorso predefinito per l'istanza predefinita di SQL Server 2017 è ad esempio il seguente:

   `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

1. Eseguire **R** o **Rgui** come amministratore da questa cartella.

1. Eseguire il comando R `install.packages` e specificare il nome del pacchetto o del repository e il percorso dei file compressi. Ad esempio:

   ```R
   install.packages("C:\\Temp\\Downloaded packages\\mynewpackage.zip", repos=NULL)
   ```

   Questo comando estrae il pacchetto R `mynewpackage` dal file compresso locale e installa il pacchetto. Se il pacchetto ha dipendenze, il programma di installazione verifica la presenza di pacchetti esistenti nella libreria. Se è stato creato un repository che include le dipendenze, il programma di installazione installa anche i pacchetti necessari.

   > [!NOTE]
   > Se i pacchetti necessari non sono presenti nella libreria di istanze e non è possibile trovarli nei file compressi, l'installazione del pacchetto di destinazione ha esito negativo.

In alternativa a **miniCRAN**, è possibile eseguire questi passaggi manualmente:

1. Identificare tutte le dipendenze del pacchetto.
1. Controllare se nel server sono già installati i pacchetti necessari. Se il pacchetto è installato, verificare che la versione sia corretta.
1. Scaricare il pacchetto e tutte le dipendenze in un computer separato con accesso a Internet.
1. Inserire il pacchetto e le dipendenze in un singolo archivio di pacchetti.
1. Comprimere l'archivio se non è già in un formato compresso.
1. Spostare i file in una cartella accessibile dal server.
1. Eseguire un comando di installazione o un'istruzione DDL supportati per installare il pacchetto nella libreria dell'istanza.

## <a name="see-also"></a>Vedere anche

+ [Ottenere informazioni sui pacchetti R](r-package-information.md)
+ [Suggerimenti per l'uso di pacchetti R](tips-for-using-r-packages.md)
+ [Esercitazioni sul linguaggio R per SQL Server](../tutorials/r-tutorials.md)