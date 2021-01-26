---
title: Visualizzare o configurare l'opzione di configurazione del server backup compression default | Microsoft Docs
description: Informazioni sull'opzione "backup compression default". Scoprire come determina se SQL Server crea backup compressi per impostazione predefinita e come impostarla.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], backup compression default option
- backup compression [SQL Server], backup compression default Option
ms.assetid: 23029395-3e93-4c29-b7d6-e5a47a3526ff
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ae3f7070ade723af31ed191d76dd274c4137a115
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783568"
---
# <a name="view-or-configure-the-backup-compression-default-server-configuration-option"></a>Visualizzare o configurare l'opzione di configurazione del server backup compression default
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questo argomento illustra come visualizzare o configurare l'opzione di configurazione del server **Valore predefinito di compressione backup** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oppure [!INCLUDE[tsql](../../includes/tsql-md.md)]. Con l'opzione **backup compression default** è possibile determinare se nell'istanza del server vengono creati backup compressi per impostazione predefinita. Quando è installato [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l'opzione **Valore predefinito di compressione backup** è disattivata.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   **Per visualizzare o configurare l'opzione backup compression default tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Completamento:**  [Dopo la configurazione dell'opzione backup compression default](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   La compressione dei backup non è disponibile in tutte le edizioni di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Per altre informazioni, vedere [Funzionalità supportate dalle edizioni di SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   Per impostazione predefinita, la compressione aumenta significativamente l'utilizzo della CPU, il che può avere un impatto negativo sulle operazioni simultanee. Potrebbe pertanto essere necessario creare backup compressi con priorità bassa in una sessione in cui l'utilizzo della CPU è limitato da [Resource Governor](../../relational-databases/resource-governor/resource-governor.md). Per ulteriori informazioni, vedere [Utilizzo di Resource Governor per limitare l'utilizzo della CPU da parte della compressione dei backup &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Quando si crea un singolo backup, si imposta una configurazione di log shipping o si crea un piano di manutenzione, è possibile eseguire l'override dell'impostazione predefinita a livello del server.  
  
-   La compressione dei backup è supportata sia per i dispositivi di backup su disco sia per quelli su nastro.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Le autorizzazioni di esecuzione per **sp_configure** senza alcun parametro o solo con il primo parametro vengono assegnate per impostazione predefinita a tutti gli utenti. Per eseguire **sp_configure** con entrambi i parametri per la modifica di un'opzione di configurazione o per l'esecuzione dell'istruzione RECONFIGURE, a un utente deve essere concessa l'autorizzazione a livello di server ALTER SETTINGS. L'autorizzazione ALTER SETTINGS è assegnata implicitamente ai ruoli predefiniti del server **sysadmin** e **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-view-or-configure-the-backup-compression-default-option"></a>Per visualizzare o configurare l'opzione backup compression default  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse su un server e scegliere **Proprietà**.  
  
2.  Fare clic sul nodo **Impostazioni database** .  
  
3.  Sotto **Backup e ripristino**, **Comprimi backup** viene visualizzata l'impostazione corrente dell'opzione **backup compression default** . Questa impostazione determina l'impostazione predefinita a livello di server per la compressione di backup, come riportato di seguito:  
  
    -   Se la casella **Comprimi backup** è vuota, per impostazione predefinita i nuovi backup non sono compressi.  
  
    -   Se la casella **Comprimi backup** è selezionata, per impostazione predefinita i nuovi backup vengono compressi.  
  
     Se si è un membro del ruolo predefinito del server **sysadmin** o **serveradmin** , è anche possibile modificare l'impostazione predefinita facendo clic sulla casella **Comprimi backup** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-view-the-backup-compression-default-option"></a>Per visualizzare l'opzione backup compression default  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. In questo esempio vengono eseguite query sulla vista del catalogo [sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) per determinare il valore per `backup compression default`. Il valore 0 indica che la compressione dei backup è disabilitata, mentre il valore 1 che è abilitata.  
  
```sql  
SELECT value   
FROM sys.configurations   
WHERE name = 'backup compression default' ;  
GO  
```  
  
#### <a name="to-configure-the-backup-compression-default-option"></a>Per configurare l'opzione backup compression default  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. In questo esempio viene illustrato come usare [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) per configurare l'istanza del server per creare backup compressi per impostazione predefinita.  
  
```sql  
EXEC sp_configure 'backup compression default', 1 ;  
RECONFIGURE;  
GO 
```  
  
 Per altre informazioni, vedere [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)sia installato il servizio WMI.  
  
##  <a name="follow-up-after-you-configure-the-backup-compression-default-option"></a><a name="FollowUp"></a> Completamento: Dopo la configurazione dell'opzione backup compression default  
 L'impostazione diventa effettiva immediatamente senza dover riavviare il server.  
  
## <a name="see-also"></a>Vedere anche  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Panoramica del backup &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)  
  
  

