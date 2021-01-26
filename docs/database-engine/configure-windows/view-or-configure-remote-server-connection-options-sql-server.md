---
title: Visualizzare o configurare le opzioni di connessione al server remoto (SQL Server) | Microsoft Docs
description: Informazioni su come visualizzare o configurare le opzioni di connessione al server remoto a livello di server. A questo scopo è possibile usare SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- remote servers [SQL Server], connection options
- servers [SQL Server], remote
- connections [SQL Server], remote servers
ms.assetid: 356d3e6b-8514-4bd2-a683-9de147949b2b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7505c7ce47f0804f2c9cf83951a5d47847edf18b
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783028"
---
# <a name="view-or-configure-remote-server-connection-options-sql-server"></a>Visualizzazione o configurare le opzioni di connessione al server remoto (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come visualizzare e configurare a livello di server le opzioni di connessione al server remoto in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Sicurezza](#Security)  
  
-   **Per visualizzazione o configurazione le opzioni di connessione al server remoto utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Completamento:**  [dopo la configurazione delle opzioni di connessione al server remoto](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 L'esecuzione di **sp_serveroption** richiede l'autorizzazione ALTER ANY LINKED SERVER nel server.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-view-or-configure-remote-server-connection-options"></a>Per visualizzare o configurare le opzioni di connessione al server remoto  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse su un server, quindi scegliere **Proprietà**.  
  
2.  Nella finestra di dialogo **Proprietà - SQL Server - \<**_server_name_**>** , fare clic su **Connessioni**.  
  
3.  Nella pagina **Connessioni** esaminare le impostazioni relative a **Connessioni remote** e, se necessario, modificarle.  
  
4.  Ripetere i passaggi da 1 a 3 nell'altro server della coppia di server remoti.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-view-remote-server-connection-options"></a>Per visualizzare le opzioni di connessione al server remoto  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. In questo esempio viene usato [sp_helpserver](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md) per restituire le informazioni su tutti i server remoti.  
  
```sql  
USE master;  
GO  
EXEC sp_helpserver ;  
```  
  
#### <a name="to-configure-remote-server-connection-options"></a>Per configurare le opzioni di connessione al server remoto  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. In questo esempio viene illustrato come usare [sp_serveroption](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md) per configurare un server remoto. Nell'esempio viene configurata la compatibilità delle regole di confronto tra un server remoto corrispondente a un'altra istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], `SEATTLE3`e l'istanza locale di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
USE master;  
EXEC sp_serveroption 'SEATTLE3', 'collation compatible', 'true';  
```  
  
##  <a name="follow-up-after-you-configure-remote-server-connection-options"></a><a name="FollowUp"></a> Completamento: dopo la configurazione delle opzioni di connessione al server remoto  
 Per poter rendere effettiva l'impostazione, è necessario arrestare e riavviare il server remoto.  
  
## <a name="see-also"></a>Vedere anche  
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [Server remoti](../../database-engine/configure-windows/remote-servers.md)   
 [Server collegati &#40;Motore di database&#41;](../../relational-databases/linked-servers/linked-servers-database-engine.md)   
 [sp_linkedservers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)  
  
  
