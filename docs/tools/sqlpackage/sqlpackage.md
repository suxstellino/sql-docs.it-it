---
title: SqlPackage.exe
description: Informazioni su come automatizzare le attività di sviluppo di database con SqlPackage.exe. È possibile visualizzare esempi, oltre a parametri, proprietà e variabili SQLCMD disponibili.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 11/4/2020
ms.openlocfilehash: 16c4200b70647c08ddee6b531acc3227d2942ad2
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577836"
---
# <a name="sqlpackageexe"></a>SqlPackage.exe

**SqlPackage.exe** è un'utilità della riga di comando che automatizza le attività di sviluppo di database seguenti:  
  
- [Versione](#version): restituisce il numero di build dell'applicazione SqlPackage.  Aggiunto nella versione 18.6.

- [Extract](sqlpackage-extract.md): crea un file snapshot del database con estensione dacpac da un database SQL Server o SQL di Azure attivo.  
  
- [Publish](sqlpackage-publish.md): aggiorna in modo incrementale uno schema di database affinché corrisponda allo schema di un file di origine con estensione dacpac. Se il database non esiste nel server, viene creato durante l'operazione di pubblicazione. In caso contrario, verrà aggiornato un database esistente.  
  
- [Export](sqlpackage-export.md): esporta un database attivo, inclusi lo schema del database e i dati utente, da un database SQL Server o SQL di Azure in un pacchetto BACPAC (file con estensione bacpac).  
  
- [Import](sqlpackage-import.md): importa i dati di tabella e dello schema da un pacchetto BACPAC in un nuovo database utente in un'istanza di database SQL Server o SQL di Azure.  
  
- [DeployReport](sqlpackage-deploy-drift-report.md): crea un report XML delle modifiche che verrebbero effettuate da un'azione Publish.  
  
- [DriftReport](sqlpackage-deploy-drift-report.md): crea un report XML delle modifiche apportate a un database registrato dall'ultima registrazione.  
  
- [Script](sqlpackage-script.md): crea uno script di aggiornamento incrementale Transact-SQL che aggiorna lo schema di una destinazione affinché corrisponda allo schema di un'origine.  
  
La riga di comando di **SqlPackage.exe** consente di specificare queste azioni insieme alle proprietà e ai parametri specifici di ogni azione.  

**[Scaricare la versione più recente](sqlpackage-download.md)** . Per informazioni dettagliate sulla versione più recente, vedere le [note sulla versione](release-notes-sqlpackage.md).
  
## <a name="command-line-syntax"></a>Sintassi della riga di comando

**SqlPackage.exe** avvia le azioni specificate usando i parametri, le proprietà e le variabili SQLCMD indicati nella riga di comando.  
  
```
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

### <a name="usage-examples"></a>Esempi di utilizzo

**Generare un confronto tra database usando i file con estensione dacpac con l'output di uno script SQL**

Per iniziare, creare un file con estensione dacpac delle modifiche più recenti al database:

```
sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_current_version.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```
 
Creare un file con estensione dacpac della destinazione del database (senza modifiche):

 ```
 sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```

Creare uno script SQL che genera le differenze dei due file dacpac:

```
sqlpackage.exe /Action:Script /SourceFile:"C:\sqlpackageoutput\output_current_version.dacpac" /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /TargetDatabaseName:"Contoso.Database" /OutputPath:"C:\sqlpackageoutput\output.sql"
 ```


## <a name="version"></a>Versione

Visualizza la versione di sqlpackage come numero di build.  Può essere usato nei prompt interattivi, nonché nelle [pipeline automatizzate](sqlpackage-pipelines.md).

```
sqlpackage.exe /Version
 ```


## <a name="exit-codes"></a>Codici di uscita

Comandi che restituiscono i codici di uscita seguenti:

- 0 = esito positivo
- diverso da zero = esito negativo


## <a name="next-steps"></a>Passaggi successivi

- Altre informazioni su [Extract di SqlPackage](sqlpackage-extract.md)
- Altre informazioni su [Publish di SqlPackage](sqlpackage-publish.md)
- Altre informazioni su [Export di SqlPackage](sqlpackage-export.md)
- Altre informazioni su [Import di SqlPackage](sqlpackage-import.md)
