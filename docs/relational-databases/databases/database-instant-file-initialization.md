---
title: Inizializzazione immediata dei file di database
description: Informazioni sull'inizializzazione immediata dei file e su come abilitarla nel database di SQL Server.
ms.custom: contperf-fy20q4
ms.date: 07/24/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- initializing files [SQL Server]
- instant file initialization [SQL Server]
- fast file initialization [SQL Server]
- file initialization [SQL Server]
- IFI [SQL Server]
- database instant file initialization [SQL Server]
ms.assetid: 1ad468f5-4f75-480b-aac6-0b01b048bd67
author: stevestein
ms.author: sstein
ms.openlocfilehash: 529949c2bf37526eba6d9042322a658f89880a9d
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237369"
---
# <a name="database-instant-file-initialization"></a>Inizializzazione immediata dei file di database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Questo articolo contiene informazioni sull'inizializzazione immediata dei file e su come abilitarla per velocizzare la crescita dei file del database di SQL Server.  

Per impostazione predefinita, i file di dati e di log vengono inizializzati per sovrascrivere eventuali dati esistenti rimasti nel disco in seguito all'eliminazione precedente di file. I file di dati e di log vengono prima di tutto inizializzati azzerando i file (riempiendoli con zeri) quando si eseguono queste operazioni:  
  
- Creare un database.  
- Aggiungere dati o file di log a un database esistente.  
- Aumento delle dimensioni di un file esistente (incluse operazioni di aumento automatico delle dimensioni).  
- Ripristino di un database o un filegroup.  

In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'inizializzazione immediata dei file consente un'esecuzione più rapida delle operazioni sui file indicate in precedenza, poiché recupera lo spazio su disco usato senza riempire lo spazio con zeri. Il contenuto del disco viene invece sovrascritto via via che nuovi dati vengono scritti nei file. Per i file di log non è possibile eseguire l'inizializzazione immediata.


## <a name="enable-instant-file-initialization"></a>Abilitare l'inizializzazione immediata dei file

L'inizializzazione immediata dei file è disponibile solo se all'account di avvio del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è stato concesso il diritto *SE_MANAGE_VOLUME_NAME*. I membri del gruppo Administrator di Windows dispongono di questo diritto e possono concederlo ad altri utenti aggiungendoli ai criteri di sicurezza **Esecuzione attività di manutenzione volume** .  
> [!IMPORTANT]
> L'uso di alcune funzionalità, ad esempio [Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md), può impedire l'inizializzazione immediata dei file.  

> [!NOTE]
> A partire da [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], è possibile concedere questa autorizzazione all'account del servizio al momento dell'installazione, durante la configurazione. <br><br>Se si utilizza il [prompt dei comandi installare](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md), aggiungere l'argomento /SQLSVCINSTANTFILEINIT o selezionare la casella *Grant Perform Volume Maintenance Task privilege to SQL Server Database Engine Service* (Concedi il privilegio di esecuzione attività di manutenzione volume al servizio motore di database di SQL Server) nell'[installazione guidata](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md).
  
Per concedere l'autorizzazione `Perform volume maintenance tasks` a un account:  
  
1.  Nel computer in cui verrà creato il file di dati aprire l'applicazione **Criteri di sicurezza locali** (`secpol.msc`).  
  
1.  Nel riquadro sinistro espandere **Criteri locali**, quindi fare clic su **Assegnazione diritti utente**.  
  
1.  Nel riquadro destro fare doppio clic su **Esecuzione attività di manutenzione volume**.  
  
1.  Fare clic su **Aggiungi utente o gruppo** e aggiungere l'account che esegue il servizio SQL Server.  
  
1.  Fare clic su **Applica**, quindi chiudere tutte le finestre di dialogo di **Criteri di sicurezza locali** .  

1. Riavviare il servizio SQL Server.

1. Controllare il log degli errori di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] all'avvio.
   
  
    **Si applica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4, [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 e [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] e versioni successive).
    1. Se all'account di avvio del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] viene concesso *SE_MANAGE_VOLUME_NAME*, viene registrato un messaggio informativo simile al seguente:

        `Database Instant File Initialization: enabled. For security and performance considerations see the topic 'Database Instant File Initialization' in SQL Server Books Online. This is an informational message only. No user action is required.`

    1. Se all'account di avvio del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **non** viene concesso *SE_MANAGE_VOLUME_NAME*, viene registrato un messaggio informativo simile al seguente:

        `Database Instant File Initialization: disabled. For security and performance considerations see the topic 'Database Instant File Initialization' in SQL Server Books Online. This is an informational message only. No user action is required.`
    > [!NOTE]
    > Si può anche usare la colonna *instant_file_initialization_enabled* della DMV [sys.dm server_services](../../relational-databases/system-dynamic-management-views/sys-dm-server-services-transact-sql.md) per stabilire se è abilitata l'inizializzazione immediata dei file.

## <a name="security-considerations"></a>Considerazioni relative alla sicurezza

È consigliabile abilitare l'inizializzazione immediata dei file perché i vantaggi possono superare i rischi per la sicurezza.

Quando si usa l'inizializzazione immediata dei file, il contenuto del disco eliminato viene sovrascritto solo quando vengono scritti nuovi dati nei file. Per questo motivo, il contenuto eliminato potrebbe essere accessibile a utenti o servizi non autorizzati, finché non vengono eseguite altre scritture dati in tale area specifica del file di dati.

Finché il file di database è collegato all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], questo rischio di diffusione di informazioni è ridotto dall'elenco di controllo di accesso discrezionale (DACL) per il file. Questo elenco consente l'accesso al file solo all'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e all'amministratore locale. Quando il file viene scollegato, tuttavia, diventa accessibile a un utente o a un servizio privo del diritto *SE_MANAGE_VOLUME_NAME*.

Considerazioni di questo tipo sono valide quando:

* *Viene eseguito il backup del database.* Se il file di backup non è protetto con un elenco di controllo di accesso discrezionale (DACL) appropriato, il contenuto eliminato può diventare disponibile a un utente o a un servizio non autorizzato.  

* *Un file viene incrementato usando l'inizializzazione immediata dei file*. Un amministratore SQL Server può potenzialmente accedere ai contenuti delle pagine non elaborati e visualizzare i contenuti precedentemente eliminati.

* *I file di database sono ospitati in una rete di archiviazione*. È anche possibile che la rete di archiviazione visualizzi sempre le pagine nuove come pagine preinizializzate e non sia necessario che il sistema operativo ne esegua nuovamente l'inizializzazione.

Se la potenziale diffusione del contenuto eliminato rappresenta un problema, è consigliabile procedere con una o con entrambe le azioni seguenti:  
  
- Assicurarsi sempre che i file di dati e i file di backup scollegati presentino elenchi di controllo di accesso discrezionale (DACL) restrittivi.  
- Disabilitare l'inizializzazione immediata dei file per l'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].    A tale scopo, revocare *SE_MANAGE_VOLUME_NAME* dall'account di avvio del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
    
    > [!NOTE]
    > La disabilitazione aumenterà i tempi di allocazione per i file di dati e ha effetto solo sui file che sono stati creati o le cui dimensioni sono aumentate dopo la revoca del diritto utente.
  
### <a name="se_manage_volume_name-user-right"></a>Diritto utente SE_MANAGE_VOLUME_NAME

Il privilegio utente *SE_MANAGE_VOLUME_NAME* può essere assegnato in **Strumenti di amministrazione Windows**, applet **Criteri di sicurezza locali**. In **Criteri locali** selezionare **Assegnazione diritti utente** e modificare la proprietà **Esecuzione di attività di manutenzione volume**.

## <a name="performance-considerations"></a>Considerazioni sulle prestazioni

Il processo di inizializzazione dei file di database scrive zeri nelle nuove aree del file in fase di inizializzazione. La durata di questo processo dipende dalle dimensioni della parte del file inizializzata e dal tempo di risposta e dalla capacità del sistema di archiviazione. Se l'inizializzazione richiede molto tempo, è possibile che vengano visualizzati i seguenti messaggi registrati nel log degli errori di SQL Server e nel registro applicazioni.

```
Msg 5144
Autogrow of file '%.*ls' in database '%.*ls' was cancelled by user or timed out after %d milliseconds.  Use ALTER DATABASE to set a smaller FILEGROWTH value for this file or to explicitly set a new file size.
```

```
Msg 5145
Autogrow of file '%.*ls' in database '%.*ls' took %d milliseconds.  Consider using ALTER DATABASE to set a smaller FILEGROWTH for this file.
```

Un aumento automatico esteso di un file di log di database e/o di un file di log delle transazioni può causare problemi di prestazioni delle query. Ciò è dovuto al fatto che un'operazione che richiede l'aumento automatico di un file verrà mantenuta in risorse quali blocchi o latch per la durata dell'operazione di aumento delle dimensioni dei file. Possono verificarsi lunghe attese nei latch per le pagine di allocazione. L'operazione che richiede l'aumento automatico esteso mostrerà un tipo di attesa PREEMPTIVE_OS_WRITEFILEGATHER.





## <a name="see-also"></a>Vedere anche  
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)