---
title: Gestire account di accesso e processi dopo il failover del mirror
description: Informazioni su come gestire account di accesso e processi dopo aver effettuato il failover del database con mirroring dal database primario a quello secondario.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- role switching [SQL Server]
ms.assetid: fc2fc949-746f-40c7-b5d4-3fd51ccfbd7b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1e410b42c5e53b479f28febee1ccd716d68437d8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354012"
---
# <a name="management-of-logins-and-jobs-after-role-switching-sql-server"></a>Gestione di account di accesso e di processi dopo un cambio di ruolo (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Quando si distribuisce una soluzione di recupero di emergenza o a disponibilità elevata per un database di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , è importante riprodurre le informazioni più significative archiviate per il database nei database **master** o **msdb** . In genere, tra queste informazioni sono inclusi i processi del database primario/principale e gli account di accesso di utenti o processi necessari per la connessione al database. È consigliabile duplicare queste informazioni in qualsiasi istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in cui viene ospitato un database secondario/mirror. Se possibile, dopo il cambio di ruolo, è opportuno riprodurre le informazioni a livello di programmazione nel nuovo database primario/principale.  
  
## <a name="logins"></a>Logins  
 In tutte le istanze del server in cui viene ospitata una copia del database, è consigliabile riprodurre gli account di accesso con l'autorizzazione ad accedere al database principale. Quando il ruolo primario/principale cambia, solo gli utenti con account di accesso presenti nella nuova istanza del server primario/principale possono accedere al nuovo database primario/principale. Gli utenti i cui account di accesso non sono definiti nella nuova istanza del server primario/principale sono orfani e non possono accedere al database.  
  
 Se un utente è orfano, creare l'account di accesso nella nuova istanza del server primario/principale ed eseguire [sp_change_users_login](../../relational-databases/system-stored-procedures/sp-change-users-login-transact-sql.md). Per altre informazioni, vedere [Risolvere i problemi relativi agli utenti isolati &#40;SQL Server&#41;](../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md).  
  
###  <a name="logins-of-applications-that-use-sql-server-authentication-or-a-local-windows-login"></a><a name="SSauthentication"></a> Account di accesso di applicazioni in cui viene utilizzata l'autenticazione di SQL Server o un account di accesso di Windows locale  
 Se in un'applicazione viene utilizzata l'autenticazione di SQL Server o un account di accesso di Windows locale, i SID non corrispondenti possono impedire la risoluzione in un'istanza remota di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]da parte dell'account di accesso dell'applicazione. In caso di SID non corrispondenti, l'account di accesso diventa un utente orfano nell'istanza del server remoto. Questo problema si può verificare quando tramite un'applicazione si effettua la connessione a un database di log shipping o con mirroring dopo un failover o a un database Sottoscrittore di replica inizializzato da un backup.  
  
 Per evitare questo problema, è consigliabile intraprendere misure preventive quando si configura un'applicazione di questo tipo per utilizzare un database ospitato da un'istanza remota di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La prevenzione comporta il trasferimento degli account di accesso e delle password dall'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] all'istanza remota di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni su come evitare questo problema, vedere l'articolo della Knowledge Base 918992 relativo alla[modalità di trasferimento degli account di accesso e delle password tra le istanze di SQL Server](https://support.microsoft.com/kb/918992/).  
  
> [!NOTE]  
>  Questo problema influisce sugli account di Windows locali in computer diversi. Tuttavia, non si verifica in caso di account di dominio, dal momento che il SID è identico in ogni computer.  
  
 Per altre informazioni, vedere la pagina relativa agli [utenti orfani con log shipping e mirroring del database](/archive/blogs/sqlserverfaq/orphaned-users-with-database-mirroring-and-log-shipping) (blog del motore di database).  
  
## <a name="jobs"></a>Processi  
 I processi, ad esempio quelli di backup, richiedono una particolare attenzione. Dopo un cambio di ruolo, in genere il proprietario del database o l'amministratore di sistema deve ricreare i processi per il nuovo database primario/principale.  
  
 Se l'istanza del server primario/principale precedente è disponibile, è consigliabile eliminare i processi originali nell'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]in questione. Si noti che i processi nel database mirror attuale non vengono eseguiti poiché il database si trova nello stato RESTORING e, pertanto, non è disponibile.  
  
> [!NOTE]  
>  È possibile che istanze differenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] siano configurate in modo diverso, ad esempio con lettere di unità differenti. I processi di ogni partner devono supportare eventuali differenze di questo tipo.  
  
## <a name="see-also"></a>Vedere anche  
 [Gestire i metadati quando si rende disponibile un database in un'altra istanza del server &#40;SQL Server&#41;](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md)   
 [Risolvere i problemi relativi agli utenti isolati &#40;SQL Server&#41;](../../sql-server/failover-clusters/troubleshoot-orphaned-users-sql-server.md)  
  
