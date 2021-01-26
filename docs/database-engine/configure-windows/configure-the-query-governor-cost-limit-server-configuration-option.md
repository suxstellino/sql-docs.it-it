---
title: Configurare l'opzione di configurazione del server query governor cost limit | Microsoft Docs
description: Informazioni sull'opzione query governor cost limit. Informazioni su come usare questa opzione per limitare l'esecuzione delle query.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- queries [SQL Server], time to execute
- query governor cost limit option [SQL Server]
- time [SQL Server], query run time
ms.assetid: e7b8f084-1052-4133-959b-cebf4add790f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7004a0a7b7322fc939b5df21d40c389dc4a3dc6a
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783664"
---
# <a name="configure-the-query-governor-cost-limit-server-configuration-option"></a>Configurare l'opzione di configurazione del server query governor cost limit
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In questo argomento si illustra come configurare l'opzione di configurazione del server **query governor cost limit** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'opzione relativa al limite di costo specifica un limite superiore per il costo stimato consentito per l'esecuzione di una query specifica. Il costo della query è un valore astratto determinato da Query Optimizer in base ai requisiti stimati per l'esecuzione, tra cui tempo di CPU, memoria e I/O su disco. Equivale al tempo trascorso, in secondi, stimato per l'esecuzione di una query in una configurazione hardware specifica. Questo valore astratto non equivale al tempo necessario per completare una query nell'istanza in esecuzione. Deve essere considerato come una misura relativa. Il valore predefinito per questa opzione è 0, cioè Query Governor viene impostato su OFF. Se si imposta il valore su 0, tutte le query verranno eseguite senza limitazione di tempo. Se si specifica un valore diverso da zero e positivo, tramite Query Governor non sarà possibile l'esecuzione delle query il cui costo stimato supera quel valore.   
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   **Per configurare l'opzione query governor cost limit utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Completamento:**  [Dopo la configurazione dell'opzione query governor cost limit](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Questa opzione è avanzata e la relativa modifica è riservata ad amministratori di database esperti o a professionisti dotati di certificazione per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Per modificare il valore di Query Governor Cost Limit a livello di singola connessione, usare l'istruzione [SET QUERY_GOVERNOR_COST_LIMIT](../../t-sql/statements/set-query-governor-cost-limit-transact-sql.md) .  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Le autorizzazioni di esecuzione per **sp_configure** senza alcun parametro o solo con il primo parametro vengono assegnate per impostazione predefinita a tutti gli utenti. Per eseguire **sp_configure** con entrambi i parametri per la modifica di un'opzione di configurazione o per l'esecuzione dell'istruzione RECONFIGURE, a un utente deve essere concessa l'autorizzazione a livello di server ALTER SETTINGS. L'autorizzazione ALTER SETTINGS è assegnata implicitamente ai ruoli predefiniti del server **sysadmin** e **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-configure-the-query-governor-cost-limit-option"></a>Per configurare l'opzione query governor cost limit  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse su un server e scegliere **Proprietà**.  
  
2.  Fare clic sulla pagina **Connessioni** .  
  
3.  Selezionare o deselezionare la casella di controllo **Usa Query Governor per evitare query con esecuzione prolungata** .  
  
     Se si seleziona questa casella di controllo, nella casella sottostante immettere un valore positivo che verrà usato da Query Governor per impedire l'esecuzione di qualsiasi query con costo stimato superiore a quel valore.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-configure-the-query-governor-cost-limit-option"></a>Per configurare l'opzione query governor cost limit  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. Questo esempio illustra come usare [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) per impostare il valore dell'opzione `query governor cost limit` su un limite superiore pari a `120` per il costo stimato della query.
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'query governor cost limit', 120 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 Per altre informazioni, vedere [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)sia installato il servizio WMI.  
  
##  <a name="follow-up-after-you-configure-the-query-governor-cost-limit-option"></a><a name="FollowUp"></a> Completamento: Dopo la configurazione dell'opzione query governor cost limit  
 L'impostazione diventa effettiva immediatamente senza dover riavviare il server.  
  
## <a name="see-also"></a>Vedere anche  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [SET QUERY_GOVERNOR_COST_LIMIT &#40;Transact-SQL&#41;](../../t-sql/statements/set-query-governor-cost-limit-transact-sql.md)   
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
