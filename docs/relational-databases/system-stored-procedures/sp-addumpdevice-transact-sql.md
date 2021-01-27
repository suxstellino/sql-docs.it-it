---
description: sp_addumpdevice (Transact-SQL)
title: sp_addumpdevice (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_addumpdevice_TSQL
- sp_addumpdevice
dev_langs:
- TSQL
helpviewer_keywords:
- backup devices [SQL Server], defining
- sp_addumpdevice
ms.assetid: c2d2ae49-0808-46d8-8444-db69a69d0ec3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f0a05127f4f6ddcd70fbb00cc5ae3bd2d22fe152
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813306"
---
# <a name="sp_addumpdevice-transact-sql"></a>sp_addumpdevice (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
**Si applica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (da [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] alla [versione corrente](/troubleshoot/sql/general/determine-version-edition-update-level)).  

Viene aggiunto un dispositivo di backup a un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icona di collegamento a un argomento](../../database-engine/configure-windows/media/topic-link.gif "Icona di collegamento a un argomento") [Convenzioni della sintassi Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintassi  
  
```  
  
sp_addumpdevice [ @devtype = ] 'device_type'   
    , [ @logicalname = ] 'logical_name'   
    , [ @physicalname = ] 'physical_name'  
      [ , { [ @cntrltype = ] controller_type |  
          [ @devstatus = ] 'device_status' }  
      ]  
```  
  
## <a name="arguments"></a>Argomenti  
`[ @devtype = ] 'device_type'` Tipo di dispositivo di backup. *device_type* è di tipo **varchar (20)** e non prevede alcun valore predefinito. i possibili valori sono i seguenti.  
  
|Valore|Descrizione|  
|-----------|-----------------|  
|**disco**|File del disco rigido impostato come dispositivo di backup.|  
|**nastro**|Qualsiasi dispositivo nastro supportato da [!INCLUDE[msCoName](../../includes/msconame-md.md)].<br /><br /> Nota: Il supporto per i dispositivi di backup su nastro verrà rimosso in una versione futura di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evitare di usare questa funzionalità in un nuovo progetto di sviluppo e prevedere interventi di modifica nelle applicazioni in cui è attualmente implementata.|  
  
`[ @logicalname = ] 'logical_name'` Nome logico del dispositivo di backup utilizzato nelle istruzioni BACKUP e RESTOre. *logical_name* è di **tipo sysname** e non prevede alcun valore predefinito e non può essere null.  
  
`[ @physicalname = ] 'physical_name'` È il nome fisico del dispositivo di backup. I nomi fisici devono essere conformi alle regole per i nomi di file del sistema operativo o alle convenzioni di denominazione universali per i dispositivi di rete e devono includere un percorso completo. *physical_name* è di **tipo nvarchar (260)** e non prevede alcun valore predefinito e non può essere null.  
  
 Quando si crea un dispositivo di backup in un percorso di rete remoto, assicurarsi che all'account specificato per l'avvio di [!INCLUDE[ssDE](../../includes/ssde-md.md)] siano associate le autorizzazioni di scrittura necessarie nel computer remoto.  
  
 Se si aggiunge un dispositivo nastro, questo parametro deve corrispondere al nome fisico assegnato al dispositivo nastro locale da Windows. ad esempio, **\\ \\ .\tape0** per il primo dispositivo nastro nel computer. Il dispositivo nastro deve essere collegato al computer server. Non può pertanto essere utilizzato in remoto. I nomi contenenti caratteri non alfanumerici devono essere racchiusi tra virgolette.  
  
> [!NOTE]  
>  Questa procedura consente di immettere nel catalogo il nome fisico specificato ma non di accedere o creare il dispositivo.  
  
`[ @cntrltype = ] 'controller_type'` Obsoleto. Se specificato, questo parametro viene ignorato. È supportato solo per motivi di compatibilità con le versioni precedenti. I nuovi utilizzi di **sp_addumpdevice** devono omettere questo parametro.  
  
`[ @devstatus = ] 'device_status'` Obsoleto. Se specificato, questo parametro viene ignorato. È supportato solo per motivi di compatibilità con le versioni precedenti. I nuovi utilizzi di **sp_addumpdevice** devono omettere questo parametro.  
  
## <a name="return-code-values"></a>Valori del codice restituito  
 0 (operazione completata) o 1 (operazione non riuscita)  
  
## <a name="result-sets"></a>Set di risultati  
 nessuno  
  
## <a name="remarks"></a>Osservazioni  
 **sp_addumpdevice** aggiunge un dispositivo di backup alla vista del catalogo **sys.backup_devices** . È possibile includere riferimenti logici al dispositivo nelle istruzioni BACKUP e RESTORE. **sp_addumpdevice** non esegue alcun accesso al dispositivo fisico. L'accesso al dispositivo specificato avviene solo quando viene eseguita un'istruzione BACKUP o RESTORE. La creazione di un dispositivo di backup logico consente di semplificare le istruzioni BACKUP e RESTORE. L'indicazione del nome di dispositivo costituisce infatti un'alternativa all'utilizzo della clausola "TAPE =" o "DISK =" per specificare il percorso del dispositivo.  
  
 Eventuali problemi correlati alla proprietà e alle autorizzazioni possono interferire con l'utilizzo di dispositivi di backup su disco o su file. Assicurarsi che all'account di Windows utilizzato per l'avvio di [!INCLUDE[ssDE](../../includes/ssde-md.md)] siano associate le autorizzazioni per i file appropriate.  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] supporta i backup su dispositivi nastro supportati da Windows. Per ulteriori informazioni sui dispositivi nastro supportati da Windows, vedere l'elenco di compatibilità hardware di Windows. Per visualizzare i dispositivi nastro disponibili nel computer, utilizzare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 Utilizzare solo i tipi di nastro consigliati dal produttore per l'unità nastro in uso. Se si utilizzano unità DAT (Digital Audio Tape), utilizzare nastri DAT per computer (Digital Data Storage, DDS).  
  
 Impossibile eseguire **sp_addumpdevice** all'interno di una transazione.  
  
 Per eliminare un dispositivo, usare [sp_dropdevice](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md) o[SQL Server Management Studio](../../relational-databases/backup-restore/delete-a-backup-device-sql-server.md).  
  
## <a name="permissions"></a>Autorizzazioni  
 È richiesta l'appartenenza al ruolo predefinito del server **diskadmin** .  
  
 Richiede l'autorizzazione di scrittura sul disco.  
  
## <a name="examples"></a>Esempi  
  
### <a name="a-adding-a-disk-dump-device"></a>R. Aggiunta di un dispositivo di dump su disco  
 Nell'esempio seguente viene aggiunto il dispositivo di backup su disco `mydiskdump` con nome fisico `c:\dump\dump1.bak`.  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'disk', 'mydiskdump', 'c:\dump\dump1.bak';  
```  
  
### <a name="b-adding-a-network-disk-backup-device"></a>B. Aggiunta di un dispositivo di backup su disco di rete  
 Nell'esempio seguente viene illustrata l'aggiunta di un dispositivo di backup su disco remoto chiamato `networkdevice`. All'account utilizzato per l'avvio di [!INCLUDE[ssDE](../../includes/ssde-md.md)] devono essere associate le autorizzazioni per tale file remoto (`\\<servername>\<sharename>\<path>\<filename>.bak`).  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'disk', 'networkdevice',  
    '\\<servername>\<sharename>\<path>\<filename>.bak';  
```  
  
### <a name="c-adding-a-tape-backup-device"></a>C. Aggiunta di un dispositivo di backup su nastro  
 Nell'esempio seguente viene aggiunto il dispositivo `tapedump1` con nome fisico `\\.\tape0`.  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'tape', 'tapedump1', '\\.\tape0';  
```  
  
### <a name="d-backing-up-to-a-logical-backup-device"></a>D. Backup in un dispositivo di backup logico  
 Nell'esempio seguente viene creato in un dispositivo di backup logico, `AdvWorksData`, per un file del disco di backup. Nell'esempio viene quindi eseguito il backup del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] nel dispositivo di backup logico.  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'disk', 'AdvWorksData',   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\BACKUP\AdvWorksData.bak';  
GO  
BACKUP DATABASE AdventureWorks2012   
 TO AdvWorksData  
   WITH FORMAT;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Dispositivi di backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Definire un dispositivo di backup logico per un file su disco &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)   
 [Definizione di un dispositivo di backup logico per un'unità nastro &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [sp_dropdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md)   
 [sys.backup_devices &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md)   
 [Stored procedure di sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
