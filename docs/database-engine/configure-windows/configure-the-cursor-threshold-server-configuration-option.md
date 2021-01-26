---
title: Configurare l'opzione di configurazione del server cursor threshold | Microsoft Docs
description: Informazioni sull'opzione cursor threshold. Scoprire in che modo il valore di questa opzione determina se SQL Server genera i keyset del cursore in modo asincrono e su come configurarla.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- cursor threshold option
ms.assetid: 189f2067-c6c4-48bd-9bd9-65f6b2021c12
author: markingmyname
ms.author: maghan
ms.openlocfilehash: de30cb4bcf672fdf47ab926a28680baa1d9b1067
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783285"
---
# <a name="configure-the-cursor-threshold-server-configuration-option"></a>Configurare l'opzione di configurazione del server cursor threshold
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questo argomento si illustra come configurare l'opzione di configurazione del server **cursor threshold** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Con l'opzione **cursor threshold** è possibile specificare il numero delle righe del set di cursori in corrispondenza del quale i keyset del cursore vengono generati in modo asincrono. Quando i cursori generano un keyset per un set di risultati, Query Optimizer produce una stima del numero di righe che verranno restituite per tale set di risultati. Se il numero stimato di righe restituite supera la soglia, il cursore viene generato in modo asincrono ed è pertanto possibile recuperare le righe dal cursore durante il popolamento del cursore stesso. In caso contrario, il cursore viene generato in modo sincrono e la query attende che vengano restituite tutte le righe.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   **Per configurare l'opzione cursor threshold tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Completamento:**  [Dopo la configurazione dell'opzione cursor threshold](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] non supporta la generazione asincrona di cursori [!INCLUDE[tsql](../../includes/tsql-md.md)] gestiti da keyset o statici. [!INCLUDE[tsql](../../includes/tsql-md.md)] , ad esempio OPEN o FETCH, vengono eseguite in batch. Non è quindi necessario generare i cursori [!INCLUDE[tsql](../../includes/tsql-md.md)] in modo asincrono. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supporta ancora i cursori API del server statici o gestiti da keyset asincroni nei casi in cui l'istruzione OPEN a bassa latenza costituisce un problema, a causa dei round trip del client per ogni operazione del cursore.  
  
-   L'accuratezza con cui Query Optimizer è in grado di stimare il numero di righe in un keyset dipende dal livello di aggiornamento delle statistiche relative a tutte le tabelle del cursore.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Questa opzione è avanzata e la relativa modifica è riservata ad amministratori di database esperti o a professionisti dotati di certificazione per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Se si imposta **cursor threshold** su -1, tutti i keyset vengono generati in modo sincrono, a vantaggio dei set di cursori di dimensioni ridotte. Se si imposta **cursor threshold** su 0, tutti i keyset dei cursori vengono generati in modo asincrono. Se si impostano altri valori, Query Optimizer verifica il numero di righe previste nel set di cursori. Se tale numero supera il valore impostato per **cursor threshold**, il keyset viene compilato in modo asincrono. Non impostare **cursor threshold** su un valore troppo basso, in quanto è opportuno che i set di risultati di dimensioni ridotte vengano compilati in modo sincrono.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Le autorizzazioni di esecuzione per **sp_configure** senza alcun parametro o solo con il primo parametro vengono assegnate per impostazione predefinita a tutti gli utenti. Per eseguire **sp_configure** con entrambi i parametri per la modifica di un'opzione di configurazione o per l'esecuzione dell'istruzione RECONFIGURE, a un utente deve essere concessa l'autorizzazione a livello di server ALTER SETTINGS. L'autorizzazione ALTER SETTINGS è assegnata implicitamente ai ruoli predefiniti del server **sysadmin** e **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-configure-the-cursor-threshold-option"></a>Per configurare l'opzione cursor threshold  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse su un server e scegliere **Proprietà**.  
  
2.  Fare clic sul nodo **Avanzate** .  
  
3.  In **Varie** specificare il valore desiderato per l'opzione **Cursor Threshold** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-configure-the-cursor-threshold-option"></a>Per configurare l'opzione cursor threshold  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. Questo esempio mostra come usare [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) per impostare l'opzione `cursor threshold` su `0` in modo che i keyset del cursore vengano generati in modo asincrono.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1 ;  
GO  
RECONFIGURE  
GO  
EXEC sp_configure 'cursor threshold', 0 ;  
GO  
RECONFIGURE  
GO  
  
```  
  
 Per altre informazioni, vedere [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)sia installato il servizio WMI.  
  
##  <a name="follow-up-after-you-configure-the-cursor-threshold-option"></a><a name="FollowUp"></a> Completamento: Dopo la configurazione dell'opzione cursor threshold  
 L'impostazione diventa effettiva immediatamente senza dover riavviare il server.  
  
## <a name="see-also"></a>Vedere anche  
 [@@CURSOR_ROWS &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-rows-transact-sql.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)  
  
  
