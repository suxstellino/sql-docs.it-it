---
description: Istruzioni RESTORE - REWINDONLY (Transact-SQL)
title: RESTORE REWINDONLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RESTORE_REWINDONLY_TSQL
- RESTORE REWINDONLY
dev_langs:
- TSQL
helpviewer_keywords:
- closing backup devices
- backup devices [SQL Server], rewinding
- media [SQL Server]
- open back devices
- rewinding backup devices
- RESTORE REWINDONLY statement
ms.assetid: 7f825b40-2264-4608-9809-590d0f09d882
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: fc5e119a9f3bad173ca72dc7b2905a82481fa1ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186610"
---
# <a name="restore-statements---rewindonly-transact-sql"></a>Istruzioni RESTORE - REWINDONLY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Riavvolge e chiude i dispositivi nastro specificati lasciati aperti dalle istruzioni BACKUP o RESTORE eseguite con l'opzione NOREWIND. Questo comando è supportato solo per i dispositivi nastro.  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```syntaxsql
  
RESTORE REWINDONLY   
FROM <backup_device> [ ,...n ]  
[ WITH {UNLOAD | NOUNLOAD}]  
}   
[;]  
  
<backup_device> ::=  
{   
   { logical_backup_device_name |  
      @logical_backup_device_name_var }  
   | TAPE = { 'physical_backup_device_name' |  
       @physical_backup_device_name_var }   
}   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argomenti
 **\<backup_device> ::=** 
  
 Specifica i dispositivi di backup logici o fisici da utilizzare per il ripristino.  
  
 { *logical_backup_device_name* |  **@** _logical\_backup\_device\_name\_var_ }  
 Nome logico, conforme alle regole per gli identificatori, dei dispositivi di backup creati tramite **sp_addumpdevice** da cui viene ripristinato il database. Se indicato in forma di variabile ( **@** _logical\_backup\_device\_name\_var_), il nome del dispositivo di backup può essere specificato come costante stringa ( **@** _logical\_backup\_device\_name\_var_ = _logical\_backup\_device\_name_) o come variabile con tipo di dati stringa di caratteri, ad eccezione dei tipi di dati **ntext** o **text**.  
  
 {DISK | TAPE } **=** { **'** _physical\_backup\_device\_name_ **'**  |  **@** _physical\_backup\_device\_name\_var_ }  
 Consente di ripristinare i backup dal dispositivo disco o nastro specificato. I tipi di dispositivo disco e nastro devono essere specificati con il nome effettivo (ad esempio percorso completo e nome di file) del dispositivo: DISK = 'C:\Programmi\Microsoft SQL Server\MSSQL\BACKUP\Mybackup.bak' o TAPE = '\\\\.\TAPE0'. Se indicato come variabile ( **@** _physical\_backup\_device\_name\_var_), il nome del dispositivo può essere specificato come costante stringa ( **@** _physical\_backup\_device\_name\_var_ = '*physical_backup_device_name*') oppure come variabile con tipo di dati stringa di caratteri, ad eccezione dei tipi di dati **ntext** o **text**.  
  
 Se si utilizza un server di rete avente un nome UNC (che deve contenere il nome del server), specificare un dispositivo disco. Per altre informazioni sull'uso dei nomi UNC, vedere [Dispositivi di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
 Per eseguire un'operazione RESTORE, l'account con il quale si esegue Microsoft SQL Server deve avere l'accesso in lettura al server di rete o al computer remoto.  
  
 *n*  
 Segnaposto che indica la possibilità di specificare più dispositivi di backup e logici. Il numero massimo di dispositivi di backup o logici è **64**.  
  
 Una sequenza di ripristino può richiedere altrettanti dispositivi di backup quanti ne sono stati utilizzati per creare il set di supporti al quale appartengono i backup, a seconda che il ripristino sia online o offline. Il ripristino offline consente il ripristino di un backup utilizzando un numero minore di dispositivi di backup rispetto a quelli utilizzati per creare il backup. Il ripristino online richiede tutti i dispositivi di backup del backup. Non è possibile eseguire il ripristino con un numero inferiore di dispositivi.  
  
 Per altre informazioni, vedere [Dispositivi di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md).  
  
> [!NOTE]  
>  Durante il ripristino di un backup da un set di supporti con mirroring, è possibile specificare solo un mirror singolo per ogni gruppo di supporti. In caso di errori, tuttavia, la presenza di altri mirror, consente una risoluzione rapida di alcuni problemi di ripristino. È possibile sostituire un volume di un supporto danneggiato con il volume corrispondente da un altro mirror. Si noti che per i ripristini offline è possibile ripristinare da un numero inferiore di dispositivi rispetto a un gruppo di dispositivi, ma ogni gruppo viene elaborato soltanto una volta.  
  
 **Opzioni WITH**  
  
 UNLOAD  
 Specifica che il nastro viene riavvolto ed espulso automaticamente al termine dell'operazione RESTORE. Per impostazione predefinita l'opzione UNLOAD viene impostata all'avvio di una nuova sessione utente e rimane attiva fino a quando non si specifica NOUNLOAD. Viene utilizzata solo per i dispositivi nastro. Se per l'operazione RESTORE non viene utilizzata un dispositivo nastro, questa opzione viene ignorata.  
  
 NOUNLOAD  
 Specifica che il nastro non viene scaricato automaticamente dall'unità dopo un'operazione RESTORE. L'opzione NOUNLOAD rimane attiva fino a quando non si specifica UNLOAD.  
  
## <a name="general-remarks"></a>Osservazioni generali  
 RESTORE REWINDONLY è un'alternativa a RESTORE LABELONLY FROM TAPE = \<name> WITH REWIND. È possibile ottenere un elenco di unità nastro aperte dalla DMV [sys.dm_io_backup_tapes](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md).  
  
## <a name="security"></a>Sicurezza  
  
### <a name="permissions"></a>Autorizzazioni  
 Qualsiasi utente può utilizzare RESTORE REWINDONLY.  
  
## <a name="see-also"></a>Vedere anche  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Set di supporti, gruppi di supporti e set di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Informazioni sulla cronologia e sull'intestazione del backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  

