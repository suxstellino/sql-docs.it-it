---
title: Installare SQL Server con l'archiviazione su condivisione file SMB | Microsoft Docs
description: In SQL Server, i database di sistema e i database utente del motore di database possono essere installati con il file server SMB (Server Message Block) come opzione di archiviazione.
ms.custom: ''
ms.date: 09/05/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 8b7810b2-637e-46a3-9fe1-d055898ba639
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 9c0ed977deed5b4eb031406368f3d010062da7df
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536360"
---
# <a name="install-sql-server-with-smb-fileshare-storage"></a>Installare SQL Server con l'archiviazione su condivisione file SMB

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

A partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]i database di sistema (Master, Model, MSDB e TempDB) e i database utente di [!INCLUDE[ssDE](../../includes/ssde-md.md)] possono essere installati con il file server SMB (Server Message Block) come opzione di archiviazione. Questa condizione è valida per le installazioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autonome e per le installazioni del cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Filestream non è attualmente supportato in una condivisione file SMB.  
  
## <a name="installation-considerations"></a>Considerazioni sull'installazione  
  
### <a name="smb-fileshare-formats"></a>Formati di condivisione file SMB:  
 Quando si specifica la condivisione file SMB, sono supportati i seguenti formati di percorso UNC (Universal Naming Convention) per database autonomi e dell'istanza del cluster di failover:  
  
-   \\\NomeServer\NomeCondivisione\  
  
-   \\\NomeServer\NomeCondivisione  
  
 Per altre informazioni su Universal Naming Convention, vedere [UNC](/openspecs/windows_protocols/ms-dtyp/62e862f4-2a51-452e-8eeb-dc4ff5ee33cc).  
  
 Il percorso UNC del loopback (percorso UNC il cui nome del server è localhost, 127.0.0.1 o il nome del computer locale) non è supportato. In un caso speciale, non è supportato neanche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui viene utilizzato il cluster di file server ospitato nello stesso nodo eseguito da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per evitare questa situazione, è consigliabile che [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e il cluster di file server vengano creati in cluster Windows separati.  
  
 I formati di percorso UNC riportati di seguito non sono supportati:  
  
-   Percorso del loopback, ad esempio \\\localhost\\..\ o \\\127.0.0.1\\...\  
  
-   Condivisioni amministrative, ad esempio \\\nomeserver\x$  
  
-   Altri formati di percorso UNC come \\\\?\x:\  
  
-   Unità di rete su cui è stato eseguito il mapping  
  
### <a name="supported-data-definition-language-ddl-statements"></a>Istruzioni DDL (Data Definition Language) supportate  
 Le istruzioni DDL di [!INCLUDE[tsql](../../includes/tsql-md.md)] e le stored procedure del motore di database seguenti supportano le condivisioni file SMB:  
  
1.  [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)  
  
2.  [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
  
3.  [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
4.  [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)  
  
### <a name="installation-options"></a>Opzione di installazione  
  
-   Nella scheda "Directory dati" della pagina "Configurazione del motore di database" del programma di installazione, impostare il parametro "Directory radice dati" su "\\\fileserver1\share1\".  
  
-   Nell'installazione del prompt dei comandi, specificare "/INSTALLSQLDATADIR" come "\\\fileserver1\share1\".  
  
     Di seguito è riportata la sintassi di esempio per installare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in un server autonomo utilizzando l'opzione di condivisione file SMB:  
  
    ```  
    Setup.exe /q /ACTION=Install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="<StrongPassword>" /SQLSYSADMINACCOUNTS="<DomainName\UserName>" /AGTSVCACCOUNT="<DomainName\UserName>" /AGTSVCPASSWORD="<StrongPassword>" /INSTALLSQLDATADIR="\\FileServer\Share1\" /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
     Per installare un'istanza del cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a nodo singolo con il [!INCLUDE[ssDE](../../includes/ssde-md.md)] e [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)](istanza predefinita):  
  
    ```  
    setup.exe /q /ACTION=InstallFailoverCluster /InstanceName=MSSQLSERVER /INDICATEPROGRESS /ASSYSADMINACCOUNTS="<DomainName\UserName>" /ASDATADIR=<Drive>:\OLAP\Data /ASLOGDIR=<Drive>:\OLAP\Log /ASBACKUPDIR=<Drive>:\OLAP\Backup /ASCONFIGDIR=<Drive>:\OLAP\Config /ASTEMPDIR=<Drive>:\OLAP\Temp /FAILOVERCLUSTERDISKS="<Cluster Disk Resource Name - for example, 'Disk S:'" /FAILOVERCLUSTERNETWORKNAME="<Insert Network Name>" /FAILOVERCLUSTERIPADDRESSES="IPv4;xx.xxx.xx.xx;Cluster Network;xxx.xxx.xxx.x" /FAILOVERCLUSTERGROUP="MSSQLSERVER" /Features=AS,SQL /ASSVCACCOUNT="<DomainName\UserName>" /ASSVCPASSWORD="xxxxxxxxxxx" /AGTSVCACCOUNT="<DomainName\UserName>" /AGTSVCPASSWORD="xxxxxxxxxxx" /INSTALLSQLDATADIR="\\FileServer\Share1\" /SQLCOLLATION="SQL_Latin1_General_CP1_CS_AS" /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="xxxxxxxxxxx" /SQLSYSADMINACCOUNTS="<DomainName\UserName> /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
     Per altre informazioni sull'uso di varie opzioni per i parametri della riga di comando in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], vedere [Installare SQL Server 2016 dal prompt dei comandi](./install-sql-server-from-the-command-prompt.md).  
  
## <a name="operating-system-considerations-smb-protocol-vs-ssnoversion"></a>Considerazioni sul sistema operativo (protocollo SMB e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])  
 Sistemi operativi Windows diversi dispongono di versioni diverse del protocollo SMB e la versione del protocollo SMB è trasparente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È possibile individuare i vantaggi delle varie versioni del protocollo SMB in relazione a [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
|Sistema operativo|Versione del protocollo SMB2|Vantaggi per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|----------------------|---------------------------|-------------------------------------------|  
|[!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] SP 2|2.0|Prestazioni migliorate rispetto a versioni precedenti di SMB.<br /><br /> Durabilità, che consente il recupero da problemi di rete temporanei.|  
|[!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)] SP1, incluso Server Core|2.1|Supporto per MTU di grandi dimensioni che consente il trasferimento di grandi quantità di dati, ad esempio per le operazioni di backup e di ripristino di SQL. Questa funzionalità deve essere abilitata dall'utente. Per altre informazioni su come abilitare questa funzionalità, vedere [What's New in SMB](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff625695(v=ws.10)) (https://go.microsoft.com/fwlink/?LinkID=237319) (Novità di SMB).<br /><br /> Significativi miglioramenti nelle prestazioni, in particolare per i carichi di lavoro di tipo OLTP di SQL. Questi miglioramenti delle prestazioni richiedono l'applicazione di un hotfix. Per altre informazioni sull'hotfix, vedere [questo articolo](https://mskb.pkisolutions.com/kb/2536493) (https://mskb.pkisolutions.com/kb/2536493).|  
|[!INCLUDE[win8srv](../../includes/win8srv-md.md)], incluso Server Core|3.0|Supporto per il failover trasparente delle condivisioni file che garantisce tempi di inattività ridotti a zero senza interventi dell'amministratore del DBA SQL o del file server nelle configurazioni di cluster di file server.<br /><br /> Supporto per IO che utilizzano più interfacce di rete simultaneamente e tolleranza agli errori dell'interfaccia di rete.<br /><br /> Supporto per interfacce di rete con funzionalità RDMA.<br /><br /> Per altre informazioni su queste funzionalità e su Server Message Block, vedere [Server Message Block overview](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831795(v=ws.11)) (https://go.microsoft.com/fwlink/?LinkId=253174) (Panoramica di SMB (Server Message Block)).<br /><br /> Supporto per File server di scalabilità orizzontale (SoFS) con disponibilità continua.|  
|[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2, incluso Server Core|3.2|Supporto per il failover trasparente delle condivisioni file che garantisce tempi di inattività ridotti a zero senza interventi dell'amministratore del DBA SQL o del file server nelle configurazioni di cluster di file server.<br /><br /> Supporto per IO che utilizzano più interfacce di rete simultaneamente e tolleranza agli errori dell'interfaccia di rete tramite SMB multicanale.<br /><br /> Supporto per interfacce di rete con funzionalità RDMA tramite SMB diretto.<br /><br /> Per altre informazioni su queste funzionalità e su Server Message Block, vedere [Server Message Block overview](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831795(v=ws.11)) (https://go.microsoft.com/fwlink/?LinkId=253174) (Panoramica di SMB (Server Message Block)).<br /><br /> Supporto per File server di scalabilità orizzontale (SoFS) con disponibilità continua.<br /><br /> Ottimizzato per piccole operazioni I/O di lettura/scrittura casuali comuni per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] OLTP.<br /><br /> L'unità massima di trasmissione (MTU) è abilitata per impostazione predefinita per migliorare significativamente le prestazioni nei trasferimenti sequenziali di grandi dimensioni come il ripristino o il backup del database e il data warehouse di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
  
## <a name="security-considerations"></a>Considerazioni relative alla sicurezza  
  
-   Gli account dei servizi [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent devono disporre di autorizzazioni di condivisione di controllo completo e di autorizzazioni NTFS sulle cartelle condivise SMB. L'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] può essere un account di dominio o di sistema quando si utilizza un file server SMB. Per altre informazioni sulle autorizzazioni NTFS e di condivisione, vedere [Autorizzazioni NTFS e di condivisione in un file server](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754178(v=ws.11)) (https://go.microsoft.com/fwlink/?LinkId=245535) .  
  
    > [!NOTE]  
    >  Le autorizzazioni di condivisione Controllo completo e le autorizzazioni NTFS sulle cartelle condivise SMB devono essere limitate a: account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent e utenti di Windows con ruoli di amministratore del server.  
  
     Si consiglia per utilizzare un account di dominio come account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se l'account di sistema viene usato come account del servizio, concedere le autorizzazioni per l'account del computer nel formato: \<*domain_name*>\\<*nome_computer*>\*$*.  
  
    > [!NOTE]  
    >  Durante l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], è necessario specificare l'account di dominio come account del servizio se la condivisione file SMB viene specificata come opzione di archiviazione. Con le condivisioni file SMB, è possibile specificare l'account di sistema come account del servizio solo dopo l'installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
    >   
    >  Gli account virtuali non possono essere autenticati a un percorso remoto. Per tutti gli account virtuali viene usata l'autorizzazione dell'account del computer. Eseguire il provisioning dell'account del computer nel formato \<*domain_name*>\\<*nome_computer*>\*$*.  
  
-   L'account utilizzato per installare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deve disporre di autorizzazioni FULL CONTROL sulla cartella della condivisione file SMB utilizzata come directory dei dati o su qualsiasi altra cartella dei dati (directory del database utente, directory del log del database utente, directory TempDB, directory del log TempDB, directory di backup) durante l'Installazione del cluster.  
  
-   È necessario che all'account utilizzato per installare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] venga concesso il privilegio SeSecurityPrivilege nel file server SMB. Per concedere tale privilegio, utilizzare la console Criteri di sicurezza locale nel file server per aggiungere l'account di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ai criteri Gestione file registro di controllo e di sicurezza. Questa impostazione è disponibile nella sezione Assegnazione diritti utente in Criteri locali nella console Criteri di sicurezza locale.  
  
## <a name="known-issues"></a>Problemi noti  
  
-   Dopo aver scollegato un database di [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] che si trova in un archivio collegato alla rete, è possibile che si verifichino problemi di autorizzazione del database durante il tentativo di ricollegare il database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Per altre informazioni, vedere [Errore 5120](../../relational-databases/errors-events/mssqlserver-5120-database-engine-error.md).
  
-   Se la condivisione file SMB viene utilizzata come opzione di archiviazione per un'istanza cluster di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], per impostazione predefinita il log di diagnostica del cluster di failover di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non può essere scritto nella condivisione file perché la DLL risorse di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non dispone di autorizzazioni di lettura/scrittura in una condivisione file. Per risolvere il problema, tentare uno dei metodi seguenti:  
  
    1.  Concedere autorizzazioni di lettura/scrittura in una condivisione file a tutti gli oggetti computer del cluster.  
  
    2.  Impostare il percorso dei log di diagnostica su un percorso file locale. Vedere l'esempio seguente:  
  
        ```sql  
        ALTER SERVER CONFIGURATION  
        SET DIAGNOSTICS LOG PATH = 'C:\logs';  
        ```  
  
## <a name="see-also"></a>Vedere anche  
 [Pianificazione di un'installazione di SQL Server](../../sql-server/install/planning-a-sql-server-installation.md)   
 [Configurare account di servizio e autorizzazioni di Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)  
  
