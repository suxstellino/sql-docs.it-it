---
description: Configurare un server di destinazione
title: Configurare un server di destinazione
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.tsxwiz.complete.f1
- sql13.ag.tsxwiz.cover.f1
- sql13.ag.tsxwiz.credentials.f1
- sql13.ag.tsxwiz.msx.f1
helpviewer_keywords:
- Target Server Wizard
- SQL Server Agent jobs, target servers
- target servers [SQL Server], creating
ms.assetid: 13aabe2d-67fe-4c67-8d49-2928dd705b7a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 06/03/2020
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 93e9a3247cdbb06d20b531e6dcdb3a383dc14529
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251052"
---
# <a name="make-a-target-server"></a>Configurare un server di destinazione
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Istanza gestita di SQL di Azure](/azure/sql-database/sql-database-managed-instance) sono attualmente supportate la maggior parte delle funzionalità di SQL Server Agent, ma non tutte. Per informazioni dettagliate, vedere [Differenze T-SQL tra Istanza gestita di SQL di Azure e SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

In questo argomento viene descritto come configurare un server di destinazione in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o SQL Server Management Objects (SMO).  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Prima di iniziare  
  
### <a name="security"></a><a name="Security"></a>Sicurezza  
I processi distribuiti con passaggi associati a un proxy vengono eseguiti nel contesto dell'account proxy nel server di destinazione. Verificare che siano soddisfatte le condizioni seguenti, per assicurare che i passaggi di processo associati a un proxy vengano scaricati dal server master a quello di destinazione:  
  
-   La sottochiave del Registro di sistema del server master **\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<&#42;nome_istanza&#42;>\SQL Server Agent\AllowDownloadedJobsToMatchProxyName** (REG_DWORD) è impostata su 1 (true). Per impostazione predefinita, questa sottochiave è impostata su 0 (False).  
  
-   Nel server di destinazione deve esistere un account proxy con lo stesso nome dell'account proxy del server master utilizzato per l'esecuzione del passaggio del processo.  
  
Se si verificano errori nel download dei passaggi dei processi che usano account proxy dal server master a quello di destinazione, è possibile controllare se nella colonna **error_message** della tabella **sysdownloadlist** nel database **msdb** sono presenti i messaggi di errore seguenti:  
  
-   "Per questo passaggio del processo è necessario un account proxy, ma l'individuazione dei proxy è disabilitata nel server di destinazione."  
  
    Per risolvere il problema, impostare la sottochiave del Registro di sistema **AllowDownloadedJobsToMatchProxyName** su 1.  
  
-   "Impossibile trovare il proxy."  
  
    Per risolvere il problema, verificare che nel server di destinazione sia disponibile un account proxy con lo stesso nome dell'account proxy del server master utilizzato per l'esecuzione del passaggio di processo.  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorizzazioni  
Le autorizzazioni di esecuzione per questa procedura vengono assegnate per impostazione predefinita ai membri del ruolo predefinito del server **sysadmin** .  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilizzo di SQL Server Management Studio  
  
#### <a name="to-make-a-target-server"></a>Per configurare un server di destinazione  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)], quindi espandere questa istanza.  
  
2.  Fare clic con il pulsante destro del mouse su **SQL Server Agent**, scegliere **Amministrazione multiserver** e fare clic su **Imposta come server di destinazione**. **Configurazione guidata server di destinazione** consente di eseguire in modo semplificato i passaggi necessari per configurare un server di destinazione.  
  
3.  Nella pagina **Selezione server master** selezionare il server master dal quale il server di destinazione corrente riceverà i processi.  
  
    **Seleziona server**  
    Tramite questa opzione è possibile connettersi al server master.  
  
    **Descrizione del server**  
    Consente di digitare una descrizione del server di destinazione corrente. Tale descrizione verrà caricata sul server master dal server di destinazione.  
  
4.  Nella pagina **Credenziali account di accesso al server master** creare un nuovo account di accesso al server di destinazione, se necessario.  
  
    **Crea un nuovo account di accesso se necessario e assegna i diritti per il server MSX**  
    Tramite questa opzione è possibile creare un nuovo account di accesso nel server di destinazione se l'account di accesso specificato non esiste già.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilizzo di Transact-SQL  
  
#### <a name="to-make-a-target-server"></a>Per configurare un server di destinazione  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. In questo esempio il server corrente viene integrato nel server master AdventureWorks1. L'ubicazione del server corrente è Building 21, Room 309, Rack 5.  
  
    ```sql
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_msx_enlist N'AdventureWorks1',   
        N'Building 21, Room 309, Rack 5' ;   
    GO
    ```  
  
    Per altre informazioni, vedere [sp_msx_enlist (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-msx-enlist-transact-sql.md).  
  
## <a name="see-also"></a>Vedere anche  
[Amministrazione automatizzata in un'organizzazione](../../ssms/agent/automated-administration-across-an-enterprise.md)  
