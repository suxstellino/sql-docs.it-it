---
title: Domande frequenti su SQL Server in Linux
description: Questo articolo fornisce le risposte alle domande frequenti su SQL Server in esecuzione in Linux.
author: VanMSFT
ms.author: vanto
ms.date: 10/23/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: dc5d54c3d64d43eb4b68a6762e3bd024587937ab
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094651"
---
# <a name="sql-server-on-linux-frequently-asked-questions-faq"></a>Domande frequenti su SQL Server in Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Le sezioni seguenti contengono domande e risposte comuni per SQL Server in esecuzione in Linux.

## <a name="general-questions"></a>Domande generali

1. **Quali piattaforme Linux sono supportate?**

   SQL Server è attualmente supportato in Red Hat Enterprise Server, SUSE Linux Enterprise Server e Ubuntu. È supportata anche l'esecuzione in un contenitore con Docker. Per le informazioni più recenti sulle versioni supportate, vedere [Piattaforme supportate](sql-server-linux-setup.md#supportedplatforms).

1. **SQL Server in Linux può essere usato anche in altre piattaforme**?

   SQL Server è testato e supportato in Linux per le distribuzioni elencate in precedenza. Le altre distribuzioni Linux sono strettamente correlate e potrebbero riuscire a eseguire SQL Server. CentOS, ad esempio, è strettamente correlato a Red Hat Enterprise Server. Se tuttavia si sceglie di installare SQL Server in un sistema operativo non supportato, vedere la sezione **Criteri di supporto** di [Criteri di supporto tecnico per Microsoft SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server) per comprendere le implicazioni per il supporto. Si noti anche che alcune distribuzioni Linux gestite dalla community non ricevono supporto in modo formale se il problema è dovuto al sistema operativo sottostante.

1. **SQL Server in Linux è uguale alla versione per Windows?**

   Il motore di database principale per SQL Server è lo stesso sia in Linux che in Windows. Tuttavia, alcune funzionalità non sono attualmente supportate in Linux. Per un elenco delle funzionalità non supportate in Linux, vedere [Funzionalità e servizi non supportati](sql-server-linux-editions-and-components-2019.md#Unsupported). Vedere anche [Problemi noti](sql-server-linux-release-notes.md#known-issues). Se non diversamente specificato in questi elenchi, le altre funzionalità e servizi di SQL Server sono supportati in Linux.

1. **Quali sono i criteri di supporto per SQL Server?**

   Per informazioni sui criteri di supporto, vedere [Criteri di supporto tecnico per Microsoft SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server).

1. **Si ha familiarità con SQL Server per Windows. Sono disponibili risorse per imparare a usare SQL Server in Linux?**

   Gli [argomenti di avvio rapido](sql-server-linux-setup.md#platforms) forniscono istruzioni dettagliate per installare SQL Server in Linux ed eseguire query di Transact-SQL. In altre esercitazioni sono disponibili istruzioni aggiuntive sull'uso di SQL Server in Linux. Per un elenco di suggerimenti di terze parti, vedere l'[elenco di suggerimenti su SQL Server in Linux di MSSQLTIPS](https://www.mssqltips.com/sql-server-tip-category/226/sql-server-on-linux/).

## <a name="licensing"></a>Gestione delle licenze

1. **Come funzionano le licenze in Linux?**

   SQL Server viene concesso in licenza nello stesso modo per Windows e Linux. Si ottiene infatti la licenza per SQL Server che può essere usata nella piattaforma preferita. Per altre informazioni, vedere [Come ottenere una licenza per SQL Server](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

1. **Quale edizione di SQL Server scegliere quando l'acquisto è già stato effettuato?**

   Quando si esegue l'installazione di mssql-conf, vengono visualizzate le opzioni seguenti:
   
   ```
   Choose an edition of SQL Server:
      1. Evaluation (free, no production use rights, 180-day limit)
      2. Developer (free, no production use rights)
      3. Express (free)
      4. Web (PAID)
      5. Standard (PAID)
      6. Enterprise (PAID)
      7. Enterprise Core (PAID)
      8. I bought a license through a retail sales channel and have a product key to enter.
   ```
     
   Se la licenza è stata ottenuta tramite contratti multilicenza nell'ambito di un contratto Enterprise o tramite l'abbonamento a MSDN, è necessario selezionare le opzioni da 4 a 7. Questo passaggio non richiede di immettere la licenza, ma è necessario aver acquistato in precedenza la licenza appropriata per la configurazione. Se è stata acquistata l'edizione Standard tramite un canale di vendita al dettaglio, selezionare l'opzione 8. Questa opzione richiede di immettere una chiave. 

1. **Come verificare la versione installata e l'edizione di SQL Server in Linux?**

   Connettersi all'istanza di SQL Server con uno strumento client, ad esempio **sqlcmd**, **mssql-cli** o Visual Studio Code. Eseguire quindi la query Transact-SQL seguente per verificare la versione e l'edizione di SQL Server in esecuzione: 

   ```sql
   SELECT @@VERSION
   SELECT SERVERPROPERTY('Edition')
   ```

## <a name="installation"></a>Installazione

1. **Come installare SQL Server nei server Linux?**

   Microsoft gestisce i repository dei pacchetti per l'installazione di SQL Server e supporta l'installazione tramite utilità di gestione pacchetti nativi, ad esempio yum, zypper e apt. Per un'installazione rapida, vedere uno degli [argomenti di avvio rapido](sql-server-linux-setup.md#platforms).

1. **È possibile installare SQL Server nel sottosistema Linux per Windows 10?**

   No. Linux in esecuzione in Windows 10 non è attualmente una piattaforma supportata per SQL Server e gli strumenti correlati.

1. **Quali file system Linux possono essere usati da SQL Server per i file di dati?**

   SQL Server in Linux supporta attualmente ext4 e XFS. Il supporto per gli altri file system verrà aggiunto in futuro, se necessario.

1. **È possibile scaricare i pacchetti di installazione per installare SQL Server offline?**

   Sì. Per altre informazioni, vedere i collegamenti ai download dei pacchetti nelle [Note sulla versione](sql-server-linux-release-notes.md). Vedere anche le [istruzioni per le installazioni offline](sql-server-linux-setup.md#offline).

1. **È possibile eseguire un'installazione automatica di SQL Server in Linux?**

   Sì. Per informazioni sull'installazione automatica, vedere [Linee guida per l'installazione di SQL Server in Linux](sql-server-linux-setup.md#unattended). Vedere gli script di esempio per [Red Hat](sample-unattended-install-redhat.md), [SUSE Linux Enterprise Server](sample-unattended-install-suse.md) e [Ubuntu](sample-unattended-install-ubuntu.md). È anche possibile vedere [questo script di esempio](/archive/blogs/sqlcat/unattended-install-and-configuration-for-sql-server-2017-on-linux) creato dal team di consulenza clienti di SQL Server.

## <a name="tools"></a>Strumenti

1. **È possibile usare il client SQL Server Management Studio in Windows per accedere a SQL Server in Linux?**

   Sì, per accedere a SQL Server in Linux, è possibile usare tutti gli strumenti esistenti eseguiti in Windows, inclusi gli strumenti di Microsoft, ad esempio SQL Server Management Studio (SSMS), SQL Server Data Tools (SSDT) e OSS, e strumenti di terze parti.

1. **È disponibile uno strumento come SSMS eseguito in Linux?**

   Il nuovo Azure Data Studio è uno strumento multipiattaforma per la gestione di SQL Server. Per altre informazioni, vedere [Informazioni su Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md).

1. **I comandi come sqlcmd e bcp sono disponibili in Linux?**

   Sì, [sqlcmd e bcp](sql-server-linux-setup-tools.md) sono disponibili in modalità nativa in Linux, macOS e Windows. Usare anche il nuovo strumento da riga di comando [mssql-scripter](https://github.com/Microsoft/mssql-scripter) in Linux, macOS o Windows per generare script T-SQL per il database SQL ovunque sia in esecuzione. Vedere anche la versione di anteprima per [mssql-cli](https://blogs.technet.microsoft.com/dataplatforminsider/2017/12/12/try-mssql-cli-a-new-interactive-command-line-tool-for-sql-server/).

1. **È possibile visualizzare Monitoraggio attività quando si è connessi tramite SSMS in Windows per un'istanza in esecuzione in Linux?**

   Sì, è possibile usare SSMS in Windows per connettersi in remoto e usare strumenti/funzionalità come i comandi di Monitoraggio attività in un'istanza di Linux.

1. **Quali strumenti sono disponibili per monitorare le prestazioni di SQL Server in Linux?**

   È possibile usare le [viste a gestione dinamica di sistema](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) per raccogliere diversi tipi di informazioni su SQL Server, incluse informazioni sui processi Linux. È possibile usare [Query Store](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md) per migliorare le prestazioni delle query. Altri strumenti, ad esempio [Performance Dashboard](/archive/blogs/sql_server_team/new-in-ssms-performance-dashboard-built-in), funzionano in modalità remota in SQL Server Management Studio (SSMS) da Windows.

   > [!TIP]
   > Un modo per migliorare le prestazioni consiste nel configurare correttamente il sistema operativo Linux e l'istanza di SQL Server. Per altre informazioni, vedere [Procedure consigliate per le prestazioni e linee guida per la configurazione per SQL Server in Linux](sql-server-linux-performance-best-practices.md).

## <a name="administration"></a>Amministrazione

1. **Microsoft ha creato un'app come Gestione configurazione SQL Server in Linux?**

   Sì, è disponibile uno strumento di configurazione per SQL Server in Linux: [mssql-conf](sql-server-linux-configure-mssql-conf.md).

1. **SQL Server in Linux supporta più istanze nello stesso host?**

   Per avere più istanze distinte, è consigliabile eseguire più contenitori in un host. Questa operazione può essere eseguita facilmente con Docker, ma ogni contenitore deve essere in ascolto su una porta diversa. Per altre informazioni, vedere [Eseguire più contenitori di SQL Server](./sql-server-linux-docker-container-deployment.md#multiple).

1. **L'autenticazione di Active Directory è supportata in Linux?**

   Sì. Per altre informazioni, vedere [Usare l'autenticazione di Active Directory con SQL Server in Linux](sql-server-linux-active-directory-authentication.md).

1. **Always On e il clustering sono supportati in Linux?**

   Il clustering di failover e la disponibilità elevata in Linux sono possibili grazie a Pacemaker in Linux. Per altre informazioni, vedere [Continuità aziendale e recupero del database - SQL Server in Linux](sql-server-linux-business-continuity-dr.md).

1. **È possibile configurare la replica da Linux a Windows e viceversa?**

   È possibile usare le repliche con scalabilità in lettura tra Windows e Linux per la replica di dati unidirezionale.

1. **È possibile eseguire la migrazione di database esistenti in versioni precedenti di SQL Server da Windows a Linux?**

   Sì, esistono [diversi metodi](sql-server-linux-migrate-overview.md) per eseguire questa operazione.

1. **È possibile eseguire la migrazione dei dati da Oracle e altri motori di database a SQL Server in Linux?**

   Sì. SSMA supporta la migrazione da diversi tipi di motori di database: Microsoft Access, DB2, MySQL, Oracle e SAP ASE (in precedenza SAP Sybase ASE). Per un esempio dell'uso di SSMA, vedere [Eseguire la migrazione di uno schema Oracle a SQL Server in Linux con SQL Server Migration Assistant](../ssma/oracle/sql-server-linux-convert-from-oracle.md?toc=/sql/toc/toc.json).

1. **Quali autorizzazioni sono necessarie per i file di SQL Server?**

   Tutti i file nella cartella di file `/var/opt/mssql` devono essere di proprietà dell'utente **mssql** e appartenere al gruppo **mssql**. Sia l'utente che il gruppo **mssql** devono avere autorizzazioni di lettura/scrittura per tutti i file e le directory. Si notino gli scenari speciali seguenti riguardanti le autorizzazioni per file e directory:

   * Le autorizzazioni per il proprietario e il gruppo mssql sono necessarie per le condivisioni di rete montate che vengono usate per archiviare i file di SQL Server.
   * Se si inseriscono file di database o backup in una directory non predefinita, è necessario impostare anche le autorizzazioni per tale directory.
   * Se si modifica l'opzione umask per la radice predefinita da 0022, la configurazione di SQL Server dopo l'installazione non riesce. È quindi necessario applicare manualmente le autorizzazioni necessarie all'account di avvio di SQL Server.

1. **È possibile modificare la proprietà dei file e delle directory di SQL Server rispetto all'account e al gruppo mssql installati?**

   La modifica della proprietà della directory e dei file di SQL Server rispetto all'installazione predefinita non è supportata. L'account e il gruppo mssql vengono usati in modo specifico per SQL Server e non hanno accesso interattivo.
   
 1. **I collegamenti simbolici sono supportati per le directory dei dati e dei log di SQL Server?** 
    
    No, i collegamenti simbolici non sono supportati per le directory dei dati e dei log di SQL Server. Per modificare le directory dei dati e dei log predefinite, vedere [Modificare il percorso predefinito della directory dei dati o dei log](sql-server-linux-configure-mssql-conf.md#datadir).
    
[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]