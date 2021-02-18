---
title: SQL Server PowerShell
description: Informazioni sui due moduli SQL Server PowerShell, SqlServer e SQLPS, che includono i provider e i cmdlet di PowerShell.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 89b70725-bbe7-4ffe-a27d-2a40005a97e7
author: markingmyname
ms.author: maghan
ms.reviewer: matteot
ms.custom: ''
ms.date: 06/11/2020
ms.openlocfilehash: d8b03ae1e20acfefd673717858d57c894c619437
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350151"
---
# <a name="sql-server-powershell"></a>SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

**[Installare SQL Server PowerShell](download-sql-server-ps-module.md)**

Esistono due moduli SQL Server PowerShell: **[SqlServer](/powershell/module/sqlserver)** e **[SQLPS](/powershell/module/sqlps)** .

**SqlServer** è il modulo corrente di PowerShell da usare.

**SQLPS** è incluso nell'installazione di SQL Server (per compatibilità con le versioni precedenti), ma non viene più aggiornato.

Il modulo **SqlServer** contiene le versioni aggiornate dei cmdlet di **SQLPS** e include nuovi cmdlet per il supporto delle funzionalità SQL più recenti.

Le versioni precedenti del modulo **SqlServer** *erano* incluse in [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md), ma solo con le versioni 16.x di SSMS.

Per usare PowerShell con SSMS versione 17.0 e successive, installare il modulo **SqlServer** da [PowerShell Gallery](https://www.powershellgallery.com/packages/SqlServer).

È anche possibile usare [PowerShell con Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md).

**Perché il modulo è cambiato da SQLPS a SqlServer?**

Per inviare gli aggiornamenti di SQL PowerShell, è stato necessario modificare l'identità del modulo SQL PowerShell, nonché il wrapper noto come *SQLPS.exe*. A causa di questa modifica, sono ora presenti due moduli SQL PowerShell: **SqlServer** e **SQLPS**.  

**Aggiornare gli script di PowerShell se si importa il modulo SQLPS.**

Se si hanno script di PowerShell script che eseguono `Import-Module -Name SQLPS` e se si vogliono sfruttare la nuova funzionalità del provider e i nuovi cmdlet, è necessario modificarli in `Import-Module -Name SqlServer`. Il nuovo modulo viene installato nella cartella `%ProgramFiles%\WindowsPowerShell\Modules\SqlServer`. Pertanto, non è necessario aggiornare la variabile $env:PSModulePath. Se si hanno script che usano una versione di terze parti o community di un modulo denominato **SqlServer**, usare il parametro Prefix per evitare conflitti di nome.

È consigliabile avviare lo script con *Import-Module SQLServer* per evitare problemi di istanze affiancate se il modulo SQLPS viene installato nello stesso computer.

Questa sezione si applica agli script eseguiti da PowerShell e non da SQL Agent. Il nuovo modulo può essere usato con i passaggi del processo di SQL Agent tramite [#NOSQLPS](#sql-server-agent).

## <a name="sql-server-powershell-components"></a>Componenti di PowerShell di SQL Server

Il modulo **SqlServer** include:

- [Provider PowerShell](/powershell/module/microsoft.powershell.core/about/about_providers), che forniscono un semplice meccanismo di navigazione analogo ai percorsi del file system. È possibile creare percorsi simili a quelli del file system, in cui l'unità è associata a un modello a oggetti di gestione di SQL Server e i nodi sono basati sulle classi del modello a oggetti. È quindi possibile usare comandi comuni come **cd** e **dir** per un'esplorazione dei percorsi simile all'esplorazione delle cartelle in una finestra del prompt dei comandi. È possibile usare altri comandi, ad esempio **ren** o **del**, per eseguire azioni sui nodi nel percorso.

- Un set di cmdlet che supportano azioni come l'esecuzione di uno script **sqlcmd** contenente istruzioni Transact-SQL o XQuery.  

- I cmdlet e il provider AS, che in precedenza erano installati separatamente.

## <a name="sql-server-versions"></a>Versioni di SQL Server

I cmdlet SQL PowerShell possono essere usati per gestire le istanze del database SQL di Azure, di Azure Synapse Analytics e di tutti i [prodotti SQL Server supportati](https://support.microsoft.com/lifecycle/search/1044).

## <a name="sql-server-identifiers-that-contain-characters-not-supported-in-powershell-paths"></a>Identificatori SQL Server che contengono caratteri non supportati nei percorsi di PowerShell

I cmdlet **Encode-Sqlname** e **Decode-Sqlname** consentono di specificare identificatori SQL Server che contengono caratteri non supportati nei percorsi di Windows PowerShell. Per altre informazioni, vedere [Identificatori di SQL Server in PowerShell](sql-server-identifiers-in-powershell.md).

Usare il cmdlet **Convert-UrnToPath** per convertire un URN (Uniform Resource Name) di un oggetto motore di database in un percorso del provider SQL Server PowerShell. Per altre informazioni, vedere [Conversione di URN in percorsi di provider di SQL Server](/powershell/module/sqlserver/Convert-UrnToPath).
  
## <a name="query-expressions-and-unique-resource-names"></a>Espressioni di query e Unique Resource Name  

Le espressioni di query sono stringhe che usano una sintassi analoga a XPath per specificare un set di criteri che enumera uno o più oggetti in una gerarchia del modello a oggetti. L'URN (Unique Resource Name) è un tipo specifico di stringa di espressione di query che identifica un singolo oggetto in modo univoco. Per altre informazioni, vedere [Espressioni di query e Uniform Resource Name](query-expressions-and-uniform-resource-names.md).

## <a name="sql-server-agent"></a>SQL Server Agent

Non sono state apportate modifiche al modulo usato da SQL Server Agent. Di conseguenza, i processi di SQL Server Agent, che hanno passaggi di processo di tipo PowerShell, usano il modulo SQLPS. Per altre informazioni, vedere [Come eseguire PowerShell con SQL Server Agent](run-windows-powershell-steps-in-sql-server-agent.md). A partire da SQL Server 2019, tuttavia, è possibile disabilitare SQLPS. A tale scopo, nella prima riga di un passaggio di processo di tipo PowerShell è possibile aggiungere `#NOSQLPS` per impedire a SQL Agent di caricare automaticamente il modulo SQLPS. Quando si esegue questa operazione, il processo di SQL Agent esegue la versione di PowerShell installata nel computer e quindi è possibile usare qualsiasi altro modulo di PowerShell desiderato.

Se si vuole usare il modulo **SqlServer** nel passaggio del processo di SQL Agent, inserire il codice nelle prime due righe dello script.

```powershell
#NOSQLPS
Import-Module -Name SqlServer
```

## <a name="cmdlet-reference"></a>Informazioni di riferimento sui cmdlet

- [Cmdlet di SqlServer](/powershell/module/sqlserver)
- [Cmdlet di SQLPS](/powershell/module/sqlps)

## <a name="next-steps"></a>Passaggi successivi

- [Scaricare il modulo PowerShell di SQL Server](download-sql-server-ps-module.md)
- [Cmdlet di SQL Server PowerShell](/powershell/module/sqlserver)
- [Usare PowerShell con Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md)