---
title: Eseguire l'installazione al prompt dei comandi
description: Eseguire il programma di installazione di SQL Server dalla riga di comando per aggiungere Machine Learning Services con Python ed R a un'istanza del motore di database di SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 01/07/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 9b0bf3444ab09cbbbb4aa8eca225dfa9964898c3
ms.sourcegitcommit: d681796e8c012eca2d9629d3b816749e9f50f868
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2021
ms.locfileid: "98005326"
---
# <a name="install-sql-server-machine-learning-services-with-r-and-python-from-the-command-line"></a>Installare SQL Server Machine Learning Services con R e Python dalla riga di comando
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Questo articolo contiene le istruzioni per installare [Machine Learning Services per SQL Server](../sql-server-machine-learning-services.md) con Python ed R da una riga di comando.

È possibile specificare l'interazione automatica, di base o completa con l'interfaccia utente del programma di installazione. Questo articolo è un'integrazione dell'articolo [Installare SQL Server dal prompt dei comandi](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md) in cui sono illustrati i parametri specifici dei componenti R e Python per il Machine Learning.

## <a name="pre-install-checklist"></a>Elenco di controllo preliminare all'installazione

+ Eseguire i comandi da un prompt dei comandi con privilegi elevati. 

+ Per le installazioni nel database è necessaria un'istanza del motore di database. Non è possibile installare solo le funzionalità di R o Python, ma è possibile [aggiungerle in modo incrementale a un'istanza esistente](#add-existing). Se si vuole limitare l'installazione a R e Python senza il motore di database, installare il [server autonomo](#shared-feature).

+ Non eseguire l'installazione in un cluster di failover. Questo requisito dipende dal fatto che il meccanismo di sicurezza usato per isolare i processi R e Python non è compatibile con un ambiente di cluster di failover Windows Server.

+ Non eseguire l'installazione in un controller di dominio. La parte del programma di installazione relativa a Machine Learning Services avrà esito negativo.

+ Evitare di installare nello stesso computer istanze autonome e nel database. Un server autonomo entrerebbe in competizione per le stesse risorse, con effetti negativi sulle prestazioni di entrambe le installazioni.

## <a name="command-line-arguments"></a>Argomenti della riga di comando

L'argomento **/FEATURES** e quelli relativi all'accettazione delle condizioni di licenza sono obbligatori. 

In caso di installazione dal prompt dei comandi, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è supportata la modalità non interattiva completa tramite il parametro **/Q** o la modalità non interattiva semplice tramite il parametro **/QS**. L'opzione **/QS** indica semplicemente lo stato di avanzamento, non accetta alcun input e non consente di visualizzare eventuali messaggi di errore. Il parametro **/QS** è supportato solo quando viene specificato **/Action=install**.

::: moniker range="=sql-server-2016"
| Argomenti | Descrizione |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | Installa la versione nel database: SQL Server R Services (In-Database).  |
| /FEATURES = SQL_SHARED_MR | Installa la funzionalità R per la versione autonoma: SQL Server R Server (Standalone). Un server autonomo è una "funzionalità condivisa" non associata a un'istanza del motore di database.|
| /IACCEPTROPENLICENSETERMS  | Indica che sono state accettate le condizioni di licenza per l'uso dei componenti R open source. |
| /IACCEPTPYTHONLICENSETERMS | Indica che sono state accettate le condizioni di licenza per l'uso dei componenti Python. |
| /IACCEPTSQLSERVERLICENSETERMS | Indica che sono state accettate le condizioni di licenza per l'uso di SQL Server.|
| /MRCACHEDIRECTORY | Per l'installazione offline, imposta la cartella contenente i file CAB dei componenti R. |
::: moniker-end

::: moniker range="=sql-server-2017"
| Argomenti | Descrizione |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | Installa la versione nel database: SQL Server Machine Learning Services (In-Database).  |
| /FEATURES = SQL_INST_MR | Associare questo argomento ad AdvancedAnalytics. Installa la funzionalità R (In-Database), inclusi Microsoft R Open e i pacchetti R proprietari. |
| /FEATURES = SQL_INST_MPY | Associare questo argomento ad AdvancedAnalytics. Installa la funzionalità Python (In-Database), inclusi Anaconda e i pacchetti Python proprietari. |
| /FEATURES = SQL_SHARED_MR | Installa la funzionalità R per la versione autonoma: SQL Server Machine Learning Server (Standalone). Un server autonomo è una "funzionalità condivisa" non associata a un'istanza del motore di database.|
| /FEATURES = SQL_SHARED_MPY | Installa la funzionalità Python per la versione autonoma: SQL Server Machine Learning Server (Standalone). Un server autonomo è una "funzionalità condivisa" non associata a un'istanza del motore di database.|
| /IACCEPTROPENLICENSETERMS  | Indica che sono state accettate le condizioni di licenza per l'uso dei componenti R open source. |
| /IACCEPTPYTHONLICENSETERMS | Indica che sono state accettate le condizioni di licenza per l'uso dei componenti Python. |
| /IACCEPTSQLSERVERLICENSETERMS | Indica che sono state accettate le condizioni di licenza per l'uso di SQL Server.|
| /MRCACHEDIRECTORY | Per l'installazione offline, imposta la cartella contenente i file CAB dei componenti R. |
| /MPYCACHEDIRECTORY | Riservato per utilizzi futuri. Usare %TEMP% per archiviare i file CAB dei componenti Python per l'installazione in computer privi di connessione Internet. |
::: moniker-end

::: moniker range=">=sql-server-ver15"
| Argomenti | Descrizione |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | Installa la versione nel database: SQL Server Machine Learning Services (In-Database).  |
| /FEATURES = SQL_INST_MR | Associare questo argomento ad AdvancedAnalytics. Installa la funzionalità R (In-Database), inclusi Microsoft R Open e i pacchetti R proprietari. |
| /FEATURES = SQL_INST_MPY | Associare questo argomento ad AdvancedAnalytics. Installa la funzionalità Python (In-Database), inclusi Anaconda e i pacchetti Python proprietari. |
| /FEATURES = SQL_INST_MJAVA | Associare questo argomento ad AdvancedAnalytics. Installa la funzionalità Java (In-Database), incluso Open JRE. Si applica a [Estensione del linguaggio Java di SQL Server](../../language-extensions/install/windows-java.md).|
| /FEATURES = SQL_SHARED_MR | Installa la funzionalità R per la versione autonoma: SQL Server Machine Learning Server (Standalone). Un server autonomo è una "funzionalità condivisa" non associata a un'istanza del motore di database.|
| /FEATURES = SQL_SHARED_MPY | Installa la funzionalità Python per la versione autonoma: SQL Server Machine Learning Server (Standalone). Un server autonomo è una "funzionalità condivisa" non associata a un'istanza del motore di database.|
| /IACCEPTROPENLICENSETERMS  | Indica che sono state accettate le condizioni di licenza per l'uso dei componenti R open source. |
| /IACCEPTPYTHONLICENSETERMS | Indica che sono state accettate le condizioni di licenza per l'uso dei componenti Python. |
| /IACCEPTSQLSERVERLICENSETERMS | Indica che sono state accettate le condizioni di licenza per l'uso di SQL Server.|
| /MRCACHEDIRECTORY | Per l'installazione offline, imposta la cartella contenente i file CAB dei componenti R. |
| /MPYCACHEDIRECTORY | Riservato per utilizzi futuri. Usare %TEMP% per archiviare i file CAB dei componenti Python per l'installazione in computer privi di connessione Internet. |
::: moniker-end

## <a name="in-database-instance-installations"></a><a name="indb"></a> Installazioni di istanze nel database

Le funzionalità di analisi nel database sono disponibili per le istanze del motore di database, che sono quindi necessarie per aggiungere la funzionalità **AdvancedAnalytics** all'installazione. È possibile installare un'istanza del motore di database insieme alla funzionalità di analisi avanzata oppure [aggiungere questa funzionalità a un'istanza esistente](#add-existing). 

Per visualizzare le informazioni sullo stato di avanzamento senza messaggi interattivi sullo schermo, usare l'argomento /qs.

> [!IMPORTANT]
> Dopo l'installazione, restano da eseguire due passaggi di configurazione aggiuntivi. L'integrazione non è completata fino a quando queste attività non sono state eseguite. Per istruzioni, vedere [Configurazione successiva all'installazione](#post-install).

::: moniker range=">=sql-server-2017"
### <a name="sql-server-machine-learning-services-database-engine-advanced-analytics-with-python-and-r"></a>SQL Server Machine Learning Services: motore di database e funzionalità di analisi avanzata con Python e R

Per un'installazione simultanea dell'istanza del motore di database, specificare il nome dell'istanza e un account di accesso Administrator (Windows). Includere le funzionalità per l'installazione dei componenti di base e dei linguaggi e quelle per l'accettazione di tutte le condizioni di licenza.

```cmd
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<Windows-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS
```

Di seguito è riportato lo stesso comando, ma con un account di accesso SQL Server in un motore di database che usa l'autenticazione mista.

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY
/INSTANCENAME=MSSQLSERVER /SECURITYMODE=SQL /SAPWD="%password%" /SQLSYSADMINACCOUNTS="<sql-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS
```

Di seguito è riportato un esempio con solo Python per mostrare che è possibile aggiungere un solo linguaggio omettendo una funzionalità.

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MPY 
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<username>" 
/IACCEPTSQLSERVERLICENSETERMS  /IACCEPTPYTHONLICENSETERMS
```
::: moniker-end

::: moniker range="=sql-server-2016"
### <a name="sql-server-r-services-database-engine-and-advanced-analytics-with-r"></a>SQL Server R Services: motore di database e funzionalità di analisi avanzata con R

Per un'installazione simultanea dell'istanza del motore di database, specificare il nome dell'istanza e un account di accesso Administrator (Windows). Includere le funzionalità per l'installazione dei componenti di base e dei linguaggi e quelle per l'accettazione di tutte le condizioni di licenza.

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<Windows-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS 
```
::: moniker-end

## <a name="post-install-configuration-required"></a><a name="post-install"></a> Configurazione successiva all'installazione (obbligatoria)

Si applica solo alle installazioni nel database.

Al termine dell'installazione, saranno disponibili un'istanza del motore di database con R e Python, i pacchetti Microsoft per R e Python, Microsoft R Open, Anaconda, strumenti, esempi e script che fanno parte della distribuzione. 

Per completare l'installazione è necessario eseguire altre due attività:


::: moniker range=">=sql-server-2017"
1. Riavviare il servizio del motore di database.

1. SQL Server Machine Learning Services: abilitare gli script esterni prima di poter usare la funzionalità. Come passaggio successivo, seguire le istruzioni riportate in [Installare SQL Server Machine Learning Services (In-Database)](sql-machine-learning-services-windows-install.md). 
::: moniker-end

::: moniker range="=sql-server-2016"
1. Riavviare il servizio del motore di database.

1. SQL Server R Services: abilitare gli script esterni prima di poter usare la funzionalità. Come passaggio successivo, seguire le istruzioni riportate in [Installare SQL Server R Services (In-Database)](sql-r-services-windows-install.md). 
::: moniker-end

## <a name="add-advanced-analytics-to-an-existing-database-engine-instance"></a><a name="add-existing"></a> Aggiungere la funzionalità di analisi avanzata a un'istanza del motore di database esistente

Quando si aggiunge la funzionalità di analisi avanzata nel database a un'istanza del motore di database esistente, specificare il nome dell'istanza. Se, ad esempio, in precedenza è stato installato un motore di database SQL Server 2017 o versione successiva insieme a Python, è possibile usare questo comando per aggiungere R.

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQL_INST_MR /INSTANCENAME=MSSQLSERVER 
/IACCEPTSQLSERVERLICENSETERMS  /IACCEPTROPENLICENSETERMS
```

## <a name="silent-install"></a><a name="silent"></a> Installazione invisibile all'utente

Nell'installazione invisibile all'utente viene omessa la verifica dei percorsi dei file con estensione cab. Per questo motivo, è necessario specificare il percorso in cui devono essere decompressi questi file. Per Python, i file CAB devono trovarsi in %TEMP*. Per R, è possibile impostare il percorso usando la directory temp.
 
```cmd  
Setup.exe /q /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY 
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS 
/MRCACHEDIRECTORY=%temp% 
```

## <a name="standalone-server-installations"></a><a name="shared-feature"></a> Installazioni del server autonomo

Un server autonomo è una "funzionalità condivisa" non associata a un'istanza del motore di database. Gli esempi seguenti mostrano la sintassi valida per l'installazione del server autonomo.

::: moniker range=">=sql-server-2017"
SQL Server Machine Learning Server supporta Python e R in un server autonomo:

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQL_SHARED_MR,SQL_SHARED_MPY  
/IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS /IACCEPTSQLSERVERLICENSETERMS
```
::: moniker-end
::: moniker range="=sql-server-2016"
SQL Server R Server supporta solo R:

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQL_SHARED_MR 
/IACCEPTROPENLICENSETERMS /IACCEPTSQLSERVERLICENSETERMS
```
::: moniker-end

Al termine dell'installazione, saranno disponibili un server, pacchetti Microsoft, distribuzioni open source di R e Python, strumenti, esempi e script che fanno parte della distribuzione. 

Per aprire una finestra della console R, passare a `\Program files\Microsoft SQL Server\150 (or 140/130)\R_SERVER\bin\x64` e fare doppio clic su **RGui.exe**. Non si ha familiarità con R? Provare questa esercitazione: [Comandi R di base e funzioni RevoScaleR: 25 esempi comuni](/machine-learning-server/r/tutorial-r-to-revoscaler).

Per aprire un comando Python, passare a `\Program files\Microsoft SQL Server\150 (or 140)\PYTHON_SERVER\bin\x64` e fare doppio clic su **python.exe**.

## <a name="next-steps"></a>Passaggi successivi

Gli sviluppatori Python possono apprendere come usare Python con SQL Server seguendo queste esercitazioni:

+ [Esercitazione su Python: Stimare il noleggio di sci con la regressione lineare in Machine Learning Services per SQL Server](../tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Esercitazione su Python: Categorizzazione dei clienti tramite clustering K-Means con Machine Learning Services per SQL Server](../tutorials/python-clustering-model.md)

Gli sviluppatori R possono iniziare alcuni semplici esempi e con le nozioni di base sul funzionamento di R con SQL Server. Per il passaggio successivo, vedere i collegamenti seguenti:

+ [Avvio rapido: Eseguire R in T-SQL](../tutorials/quickstart-r-create-script.md)
+ [Esercitazione: Analisi nel database per sviluppatori R](../tutorials/r-taxi-classification-introduction.md)
