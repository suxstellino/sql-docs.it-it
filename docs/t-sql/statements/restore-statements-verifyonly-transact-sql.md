---
description: Istruzioni RESTORE - VERIFYONLY (Transact-SQL)
title: RESTORE VERIFYONLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- VERIFYONLY
- RESTORE VERIFYONLY
- VERIFYONLY_TSQL
- RESTORE_VERIFYONLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE VERIFYONLY statement
- backups [SQL Server], verifying
- verifying backups
- checking backups
ms.assetid: cba3b6a0-b48e-4c94-812b-5b3cbb408bd6
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: b76a49d34d0603d62ee7160bb2f4ef9c32d482a7
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237803"
---
# <a name="restore-statements---verifyonly-transact-sql"></a>Istruzioni RESTORE - VERIFYONLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  Verifica il backup senza eseguirne il ripristino e controlla che il set di backup sia completo e che l'intero backup sia leggibile. Non verifica tuttavia la struttura dei dati contenuti nei volumi di backup. In [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l'istruzione RESTORE VERIFYONLY è stata migliorata in modo tale da consentire controlli aggiuntivi sui dati e aumentare quindi la probabilità di rilevare errori, allo scopo di essere quanto più possibile vicini ad una vera e propria operazione di ripristino. Per ulteriori informazioni, vedere la sezione Osservazioni.  

 Se il backup è valido, il [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] restituisce un messaggio di operazione riuscita.  
  
> [!NOTE]  
>  Per le descrizioni degli argomenti, vedere [Argomenti RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
RESTORE VERIFYONLY  
FROM <backup_device> [ ,...n ]  
[ WITH    
 {  
   LOADHISTORY   
  
--Restore Operation Option  
 | MOVE 'logical_file_name_in_backup' TO 'operating_system_file_name'   
          [ ,...n ]   
  
--Backup Set Options  
 | FILE = { backup_set_file_number | @backup_set_file_number }   
 | PASSWORD = { password | @password_variable }   
  
--Media Set Options  
 | MEDIANAME = { media_name | @media_name_variable }   
 | MEDIAPASSWORD = { mediapassword | @mediapassword_variable }  
  
--Error Management Options  
 | { CHECKSUM | NO_CHECKSUM }   
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }  
  
--Monitoring Options  
 | STATS [ = percentage ]   
  
--Tape Options  
 | { REWIND | NOREWIND }   
 | { UNLOAD | NOUNLOAD }    
 } [ ,...n ]  
]  
[;]  
  
<backup_device> ::=  
{   
   { logical_backup_device_name |  
      @logical_backup_device_name_var }  
   | { DISK | TAPE | URL } = { 'physical_backup_device_name' |  
       @physical_backup_device_name_var }   
}  
  
```  
 > [!NOTE] 
> URL è il formato usato per specificare il percorso e il nome file per Archiviazione BLOB di Microsoft Azure ed è supportato a partire da [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2. Anche se l'archiviazione di Microsoft Azure è un servizio, l'implementazione è simile a quella per dischi e nastri, in modo da consentire un'esperienza di ripristino coerente e trasparente per tutti e tre i dispositivi.
 
## <a name="arguments"></a>Argomenti  
 Per le descrizioni degli argomenti RESTORE VERIFYONLY, vedere [Argomenti RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
## <a name="general-remarks"></a>Osservazioni generali  
 Il set di supporti o di backup deve contenere un minimo di informazioni corrette affinché possa essere interpretato come MTF (Microsoft Tape Format). In caso contrario, l'esecuzione dell'istruzione RESTORE VERIFYONLY viene arrestata e indica che il formato del backup non è valido.  
  
 I controlli eseguiti da RESTORE VERIFYONLY includono:  
  
-   Verifica che il set di backup sia completo e che tutti i volumi siano leggibili.  
  
-   Verifica di alcuni campi di intestazione delle pagine di database, ad esempio l'ID della pagina (come se stesse per scrivere i dati).  
  
-   Verifica del checksum (se presente sui supporti).  
  
-   Verifica dello spazio disponibile sui dispositivi di destinazione.  
  
> [!NOTE]  
>  RESTORE VERIFYONLY non funziona su uno snapshot del database. Per verificare uno snapshot del database prima di una operazione di ripristino, è possibile eseguire DBCC CHECKDB.  
  
> [!NOTE]  
>  Con i backup di snapshot, RESTORE VERIFYONLY conferma l'esistenza degli snapshot nei percorsi specificati nel file di backup. I backup di snapshot sono una nuova funzionalità di [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]. Per altre informazioni sul backup di snapshot, vedere [Backup di snapshot di file per i file di database in Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
## <a name="security"></a>Sicurezza  
 Per un'operazione di backup è possibile specificare password per un set di supporti o un set di backup oppure per entrambi. Se è stata impostata una password per un set di supporti o un set di backup, la password o le password corrette devono essere specificate nell'istruzione RESTORE. Queste password impediscono operazioni di ripristino non autorizzate e l'aggiunta non autorizzata di set di backup ai supporti tramite gli strumenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Tuttavia, la password non impedisce la sovrascrittura dei supporti tramite l'opzione FORMAT dell'istruzione BACKUP.  
  
> [!IMPORTANT]  
>  Il livello di protezione garantito da questa password è vulnerabile. Lo scopo è impedire un ripristino non corretto da parte di utenti autorizzati o non autorizzati mediante gli strumenti di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Non impedisce la lettura dei dati di backup eseguita con altri mezzi o la sostituzione della password. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]Per ottenere un livello di protezione adeguato dei backup è consigliabile archiviare i nastri di backup in un luogo sicuro oppure eseguire il backup in file su disco protetti da elenchi di controllo di accesso (ACL) appropriati. Gli elenchi di controllo di accesso devono essere impostati a livello della directory radice in cui vengono creati i backup.  
  
### <a name="permissions"></a>Autorizzazioni  
 A partire da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], per ottenere informazioni su un set o dispositivo di backup è necessario disporre dell'autorizzazione CREATE DATABASE. Per altre informazioni, vedere [GRANT - autorizzazioni per database &#40;Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
 
## <a name="examples"></a>Esempi  
 L'esempio seguente verifica il backup dal disco.
  
```sql  
RESTORE VERIFYONLY FROM DISK = 'D:\AdventureWorks.bak';
GO
```  
  
## <a name="see-also"></a>Vedere anche  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Set di supporti, gruppi di supporti e set di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Informazioni sulla cronologia e sull'intestazione del backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  
