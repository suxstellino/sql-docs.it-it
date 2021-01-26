---
title: Configurare l'opzione di configurazione del server remote access | Microsoft Docs
description: Informazioni sulle alternative all'opzione remote access deprecata. Scoprire altre fonti di informazioni per la risoluzione dei problemi relativi alle connessioni di SQL Server.
ms.custom: ''
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- remote servers [SQL Server], stored procedure execution
ms.assetid: f5de748d-1c55-4714-9661-38fe62e5095f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: afe6721bef097dfa941b5578b6d4287fc370c3f2
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783131"
---
# <a name="configure-the-remote-access-server-configuration-option"></a>Configurare l'opzione di configurazione del server remote access
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questo argomento riguarda la funzionalità "Accesso remoto". Questa opzione di configurazione è una funzionalità di comunicazione da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] poco nota e deprecata, che probabilmente non è opportuno usare. Se si è arrivati a questa pagina cercando una soluzione per i problemi di connessione a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vedere invece uno degli argomenti seguenti:  
  
-   [Esercitazione: Introduzione al motore di database](../../relational-databases/tutorial-getting-started-with-the-database-engine.md)  
  
-   [Accesso a SQL Server](../../database-engine/configure-windows/logging-in-to-sql-server.md)  
  
-   [Connettersi a SQL Server se gli amministratori di sistema sono bloccati](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)  
  
-   [Connessione a un server registrato &#40;SQL Server Management Studio&#41;](../../ssms/register-servers/connect-to-a-registered-server-sql-server-management-studio.md)  
  
-   [Connessione ai componenti di SQL Server da SQL Server Management Studio](../../ssms/f1-help/connect-to-any-sql-server-component-from-sql-server-management-studio.md)  
  
-   [Connessione al Motore di database tramite sqlcmd](../../ssms/scripting/sqlcmd-connect-to-the-database-engine.md)  
  
-   [Risoluzione dei problemi relativi alla connessione al motore di database di SQL Server](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)  
  
 I programmatori potrebbero essere interessati agli argomenti seguenti:  
  
-   [Procedura: Connettersi a SQL Server con l'autenticazione SQL in ASP.NET 2.0](/previous-versions/msp-n-p/ff648340(v=pandp.10))  
  
-   [Connessione a un'istanza di SQL Server](../../relational-databases/server-management-objects-smo/create-program/connecting-to-an-instance-of-sql-server.md)  
  
-   [Procedura: Creare connessioni a database di SQL Server](/previous-versions/visualstudio/visual-studio-2008/s4yys16a(v=vs.90))  
  
 **Il corpo principale dell'argomento inizia qui.**  
  
 In questo argomento si illustra come configurare l'opzione di configurazione del server **remote access** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Con l'opzione **remote access** è possibile controllare l'esecuzione di stored procedure da server remoti o locali in cui sono in esecuzione istanze di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Il valore predefinito dell'opzione è 1. In questo modo si ottiene l'autorizzazione a eseguire stored procedure locali da server remoti o stored procedure remote dal server locale. Per impedire l'esecuzione di stored procedure locali da un server remoto o di stored procedure remote nel server locale, impostare l'opzione su 0.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Usare [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) in alternativa.
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per configurare l'opzione remote access utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Completamento:**  [Dopo la configurazione dell'opzione remote access](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   L'opzione **remote access** si applica solo ai server aggiunti usando [sp_addserver](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)e viene inclusa per ragioni di compatibilità con le versioni precedenti.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Le autorizzazioni di esecuzione per **sp_configure** senza alcun parametro o solo con il primo parametro vengono assegnate per impostazione predefinita a tutti gli utenti. Per eseguire **sp_configure** con entrambi i parametri per la modifica di un'opzione di configurazione o per l'esecuzione dell'istruzione RECONFIGURE, a un utente deve essere concessa l'autorizzazione a livello di server ALTER SETTINGS. L'autorizzazione ALTER SETTINGS è assegnata implicitamente ai ruoli predefiniti del server **sysadmin** e **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-configure-the-remote-access-option"></a>Per configurare l'opzione remote access  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse su un server e scegliere **Proprietà**.  
  
2.  Fare clic sul nodo **Connessioni** .  
  
3.  In **Connessioni remote** selezionare o deselezionare la casella di controllo **Consenti connessioni remote al server** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-configure-the-remote-access-option"></a>Per configurare l'opzione remote access  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. Questo esempio illustra come usare [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) per impostare il valore dell'opzione `remote access` su `0`.  
  
```sql  
EXEC sp_configure 'remote access', 0 ;  
GO  
RECONFIGURE ;  
GO  
  
```  
  
 Per altre informazioni, vedere [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)sia installato il servizio WMI.  
  
##  <a name="follow-up-after-you-configure-the-remote-access-option"></a><a name="FollowUp"></a> Completamento: Dopo la configurazione dell'opzione remote access  
 Questa impostazione ha effetto solo dopo il riavvio di SQL Server.  
  
## <a name="see-also"></a>Vedere anche  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
