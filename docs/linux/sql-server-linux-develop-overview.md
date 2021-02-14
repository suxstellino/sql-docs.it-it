---
title: Sviluppare applicazioni per SQL Server in Linux
description: È possibile creare applicazioni che si connettono e usano SQL Server in Linux da un'ampia gamma di linguaggi di programmazione e framework Web diffusi.
author: VanMSFT
ms.author: vanto
ms.date: 11/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 758cb738-b018-465b-9ab0-59a24b892e66
ms.openlocfilehash: 01655b948afa721365b7ea7adebd1e22f8f6f851
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273082"
---
# <a name="how-to-get-started-developing-applications-for-sql-server-on-linux"></a>Come iniziare a sviluppare applicazioni per SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

È possibile creare applicazioni che si connettono e usano SQL Server in Linux da un'ampia gamma di linguaggi di programmazione, ad esempio C#, Java, Node.js, PHP, Python, Ruby e C++. È anche possibile usare framework Web e ORM (Object Relational Mapping) di ampia diffusione.

> [!VIDEO https://channel9.msdn.com/events/Connect/2017/T153/player]

> [!TIP]
> Queste stesse opzioni di sviluppo consentono anche di specificare SQL Server come destinazione in altre piattaforme. È possibile specificare come destinazione delle applicazioni SQL Server in esecuzione in locale o nel cloud, in Linux, Windows o Docker in macOS. In alternativa, è possibile specificare come destinazione il database SQL di Azure e Azure Synapse Analytics.

## <a name="try-the-tutorials"></a>Esercitazioni

Il modo migliore per iniziare a compilare applicazioni con SQL Server consiste nel provare di persona.

- Passare alle [esercitazioni introduttive](https://aka.ms/sqldev).
- Selezionare il linguaggio e la piattaforma di sviluppo.
- Provare gli esempi di codice.

> [!TIP]
> Se si vuole sviluppare per SQL Server in Docker, vedere le esercitazioni per **macOS**.

## <a name="create-new-applications"></a>Creare nuove applicazioni

Se si sta creando una nuova applicazione, vedere l'elenco delle [librerie di connettività](sql-server-linux-develop-connectivity-libraries.md) per un riepilogo dei connettori e dei framework più diffusi disponibili per diversi linguaggi di programmazione.

## <a name="use-existing-applications"></a>Usare applicazioni esistenti

Se si ha un'applicazione di database esistente, è possibile modificare semplicemente la stringa di connessione di questa specificando SQL Server in Linux come destinazione. Assicurarsi di leggere le informazioni sui [problemi noti](sql-server-linux-release-notes.md) di SQL Server in Linux.

## <a name="use-existing-sql-tools-on-windows-with-sql-server-on-linux"></a>Usare strumenti SQL in Windows con SQL Server in Linux

Gli strumenti attualmente eseguiti in Windows, ad esempio SSMS, SSDT e PowerShell, funzionano anche con SQL Server in Linux. Anche se non funzionano in modo nativo in Linux, consentono comunque di gestire istanze remote di SQL Server in Linux. 

Per altre informazioni, vedere gli argomenti seguenti:

- [SQL Server Management Studio (SSMS)](sql-server-linux-manage-ssms.md)
- [SQL Server Data Tools (SSDT)](sql-server-linux-develop-use-ssdt.md)
- [SQL PowerShell](sql-server-linux-manage-powershell.md)

> [!Note]
> Per un'esperienza ottimale, assicurarsi di usare le versioni più recenti di questi strumenti.

## <a name="use-new-sql-tools-for-linux"></a>Usare i nuovi strumenti SQL per Linux

È possibile usare la nuova [estensione mssql](https://aka.ms/mssql-marketplace) per [Visual Studio Code](https://code.visualstudio.com) in Linux, macOS e Windows. Per una procedura dettagliata, vedere l'esercitazione seguente:

- [Usare Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)

È anche possibile usare i nuovi strumenti da riga di comando nativi per Linux. Questi strumenti sono:

- [sqlcmd](../tools/sqlcmd-utility.md)
- [bcp](sql-server-linux-migrate-bcp.md)
- [mssql-conf](sql-server-linux-configure-mssql-conf.md)

## <a name="next-steps"></a>Passaggi successivi

Per iniziare, installare SQL Server in Linux usando una delle guide di avvio rapido seguenti:

- [Eseguire l'installazione in Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Eseguire l'installazione in SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Eseguire l'installazione in Ubuntu](quickstart-install-connect-ubuntu.md)
- [Esecuzione in Docker](quickstart-install-connect-ubuntu.md)