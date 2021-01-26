---
title: Configurare l'opzione di configurazione del server recovery interval | Microsoft Docs
description: Informazioni sull'opzione recovery interval. Scoprire come il suo valore influisca sulla frequenza con cui SQL Server emette checkpoint automatici per un database.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- restoring recovery interval [SQL Server]
- checkpoints [SQL Server]
- recovery interval option [SQL Server]
- default recovery interval option
- time [SQL Server], recovery interval
- interval for recovery [SQL Server]
- maximum number of minutes per database recovery
- recovery [SQL Server], recovery interval option
ms.assetid: e4734b3b-8fbe-4b65-9c48-91b5a3dd18e1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ac6d7f368a42e8a6c81fadbf151760b5bdfc8b53
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783158"
---
# <a name="configure-the-recovery-interval-server-configuration-option"></a>Configurare l'opzione di configurazione del server recovery interval
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questo argomento si illustra come configurare l'opzione di configurazione del server **recovery interval** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Con l'opzione **recovery interval** è possibile definire un limite superiore di tempo da impiegare per il recupero di un database. [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] usa il valore specificato per questa opzione per determinare approssimativamente la frequenza di generazione dei [checkpoint automatici](../../relational-databases/logs/database-checkpoints-sql-server.md) in un database specifico.  
  
 Il valore predefinito di recovery-interval è 0. In questo modo, tramite il [!INCLUDE[ssDE](../../includes/ssde-md.md)] è possibile configurare automaticamente l'intervallo di recupero. In genere, con l'intervallo di recupero predefinito vengono generati checkpoint automatici circa una volta al minuto per i database attivi e in un tempo di recupero inferiore al minuto. I valori superiori indicano il tempo di recupero massimo approssimativo, in minuti. Ad esempio, impostando l'intervallo di recupero su 3, il tempo di recupero massimo risulterà di circa 3 minuti.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   **Per configurare l'opzione di configurazione del server recovery interval mediante**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Completamento:**  [Dopo la configurazione dell'opzione recovery interval](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   L'intervallo di recupero influisce solo sui database in cui viene utilizzato il tempo di recupero di riferimento predefinito (0). Per ignorare l'intervallo di recupero del server in un database, configurare un tempo di recupero di riferimento non predefinito nel database. Per altre informazioni, vedere [Modificare il tempo di recupero di riferimento di un database &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Questa opzione è avanzata e la relativa modifica è riservata ad amministratori di database esperti o a professionisti dotati di certificazione per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   In genere, è consigliabile mantenere l'intervallo di recupero a 0, a meno che non si verifichino problemi di prestazioni. Se si decide di aumentare l'impostazione dell'intervallo di recupero, è consigliabile aumentarla gradualmente di piccoli incrementi e valutare l'effetto di ogni aumento incrementale sulle prestazioni del recupero.  
  
-   Se si usa **sp_configure** per impostare il valore dell'opzione **recovery interval** su un valore maggiore di 60 (minuti), specificare RECONFIGURE WITH OVERRIDE. Con WITH OVERRIDE è possibile disabilitare la verifica del valore di configurazione, in particolare per valori non validi o non consigliati.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Le autorizzazioni di esecuzione per **sp_configure** senza alcun parametro o solo con il primo parametro vengono assegnate per impostazione predefinita a tutti gli utenti. Per eseguire **sp_configure** con entrambi i parametri per la modifica di un'opzione di configurazione o per l'esecuzione dell'istruzione RECONFIGURE, a un utente deve essere concessa l'autorizzazione a livello di server ALTER SETTINGS. L'autorizzazione ALTER SETTINGS è assegnata implicitamente ai ruoli predefiniti del server **sysadmin** e **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
 **Per impostare l'intervallo di recupero**  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse sull'istanza del server e selezionare **Proprietà**.  
  
2.  Fare clic sul nodo **Impostazioni database** .  
  
3.  In **Recupero**, nella casella **Intervallo di recupero (minuti)** , digitare o selezionare un valore compreso tra 0 e 32767 per impostare il numero massimo di minuti impiegato da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] per il recupero di ogni database all'avvio. L'impostazione predefinita è 0, che rappresenta la configurazione automatica di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ciò equivale a un tempo di recupero inferiore a un minuto e all'impostazione di checkpoint a intervalli di circa un minuto per i database attivi.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-set-the-recovery-interval"></a>Per impostare l'intervallo di recupero  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. Questo esempio illustra come usare [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) per impostare il valore dell'opzione `recovery interval` su `3` minuti.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'recovery interval', 3 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 Per altre informazioni, vedere [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)sia installato il servizio WMI.  
  
##  <a name="follow-up-after-you-configure-the-recovery-internal-option"></a><a name="FollowUp"></a> Completamento: Dopo la configurazione dell'opzione recovery interval  
 L'impostazione diventa effettiva immediatamente senza dover riavviare il server.  
  
## <a name="see-also"></a>Vedere anche  
 [Modificare il tempo di recupero di riferimento di un database &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md)   
 [Checkpoint di database &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md)   
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Opzione di configurazione del server show advanced options](../../database-engine/configure-windows/show-advanced-options-server-configuration-option.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)  
  
  
