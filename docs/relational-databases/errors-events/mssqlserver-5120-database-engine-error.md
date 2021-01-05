---
description: MSSQLSERVER_5120
title: MSSQLSERVER_5120
ms.custom: ''
ms.date: 07/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 5120 (Database Engine error)
ms.assetid: ''
author: PijoCoder
ms.author: mathoma
ms.openlocfilehash: 331c35a269d63f9f71c3d0fce59485bd08b88df4
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2020
ms.locfileid: "97638823"
---
# <a name="mssqlserver_5120"></a>MSSQLSERVER_5120
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Dettagli  
  
| Attributo | valore |  
| :-------- | :---- |  
|Nome prodotto|SQL Server|  
|ID evento|5120|  
|Origine evento|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nome simbolico|DSK_FCB_FAILURE|  
|Testo del messaggio|Errore di tabella: Impossibile aprire il file fisico "%.*ls". Errore %d: "%ls" del sistema operativo.|  
  
## <a name="explanation"></a>Spiegazione  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non è riuscito ad aprire un file di database.  L'errore del sistema operativo visualizzato nel messaggio indica motivi più specifici all'origine dell'errore. Questo errore può essere visualizzato insieme ad altri errori, ad esempio [17204](mssqlserver-17204-database-engine-error.md) o [17207](mssqlserver-17207-database-engine-error.md).
  
## <a name="user-action"></a>Azione utente  
  
  Trovare e correggere l'errore del sistema operativo, quindi provare di nuovo l'operazione. Diversi stati consentono a Microsoft di circoscrivere l'area del prodotto in cui si è verificato l'errore. 
  
### <a name="access-is-denied"></a>Accesso negato 
Se si riceve l'errore del sistema operativo `Access is Denied` = 5, prendere in considerazione questi metodi:
   -  Controllare le autorizzazioni impostate per il file esaminando le proprietà del file in Esplora risorse. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa i gruppi di Windows per eseguire il provisioning del controllo di accesso sulle varie risorse di file. Verificare che il gruppo appropriato [con nomi come SQLServerMSSQLUser$ComputerName$MSSQLSERVER o SQLServerMSSQLUser$ComputerName$InstanceName] abbia le autorizzazioni necessarie per il file di database indicato nel messaggio di errore. Per altre informazioni, vedere [Configurare le autorizzazioni del file system per l'accesso al motore di database](/previous-versions/sql/2014/database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access?view=sql-server-2014&preserve-view=true). Verificare che il gruppo di Windows includa l'account di avvio del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o il SID del servizio.
   -  Esaminare l'account utente in cui è attualmente in esecuzione il servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. È possibile ottenere queste informazioni tramite Gestione attività di Windows. Cercare il valore "User Name" (Nome utente) nel file eseguibile "sqlservr.exe". Se è stato modificato di recente l'account del servizio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tenere presente che il metodo supportato per questa operazione è l'uso dell'utilità [Gestione configurazione SQL Server](../sql-server-configuration-manager.md). 
   -  A seconda del tipo di operazione (apertura dei database durante l'avvio del server, collegamento di un database, ripristino del database e così via), l'account usato per la rappresentazione e l'accesso al file di database può variare. Vedere l'argomento [Sicurezza dei dati e dei file di log](/previous-versions/sql/sql-server-2008-r2/ms189128(v=sql.105)) per informazioni sulle interazioni tra operazioni, autorizzazioni e account. Usare uno strumento come [Process Monitor](/sysinternals/downloads/procmon) di Windows SysInternals per determinare se l'accesso ai file avviene nel contesto di sicurezza dell'account di avvio del servizio dell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (o SID del servizio) o di un account rappresentato.

      Se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rappresenta le credenziali utente dell'accesso che esegue l'operazione ALTER DATABASE o CREATE DATABASE, si noteranno ad esempio le informazioni seguenti nello strumento Process Monitor:
      
        ```
        Date & Time:      3/27/2010 8:26:08 PM
        Event Class:        File System
        Operation:          CreateFile
        Result:                ACCESS DENIED
        Path:                  C:\Program Files\Microsoft SQL Server\MSSQL13.SQL2016\MSSQL\DATA\attach_test.mdf
        TID:                   4288
        Duration:             0.0000366
        Desired Access:Generic Read/Write
        Disposition:        Open
        Options:            Synchronous IO Non-Alert, Non-Directory File, Open No Recall
        Attributes:          N
        ShareMode:       Read
        AllocationSize:   n/a
        Impersonating: DomainName\UserName
        ```
  
  
### <a name="attaching-files-that-reside-on-a-network-attached-storage"></a>associazione di file che risiedono in un Network-attached storage (NAS)  
Se non è possibile ricollegare un database che risiede in un Network-attached storage (NAS), è possibile che nel registro applicazioni venga registrato un messaggio simile al seguente.

`Msg 5120, Level 16, State 101, Line 1 Unable to open the physical file "\\servername\sharename\filename.mdf". Operating system error 5: (Access is denied.).`

Questo problema si verifica perché [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reimposta le autorizzazioni del file quando il database viene scollegato. Quando si tenta di riconnettere il database, si verifica un errore a causa di autorizzazioni di condivisione limitate.

Per risolvere il problema, seguire questa procedura:
1. usare l'opzione di avvio -T per avviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Usare questa opzione di avvio per attivare il flag di traccia 1802 in [Gestione configurazione SQL Server](../sql-server-configuration-manager.md) (per informazioni su 1802 vedere [Flag di traccia](../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md)). Per altre informazioni sulle modalità di modifica dei parametri di avvio, vedere [Opzioni di avvio del servizio del motore di database](../../database-engine/configure-windows/database-engine-service-startup-options.md).

2. Usare il comando seguente per scollegare il database.
   ```sql
    exec sp_detach_db DatabaseName
    go 
   ```

3. Usare il comando seguente per riconnettere il database.
   ```sql
   exec sp_attach_db DatabaseName, '\\Network-attached storage_Path\DatabaseMDFFile.mdf', '\\Network-attached storage_Path\DatabaseLDFFile.ldf'
   go
   ```
