---
title: Configurare l'opzione di configurazione del server user connections | Microsoft Docs
description: Informazioni sull'opzione "user connections". Scoprire come può essere utile per evitare il sovraccarico di un'istanza di SQL Server con un numero eccessivo di connessioni simultanee.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- simultaneous connections [SQL Server]
- user connections option [SQL Server]
- users [SQL Server], simultaneous connections
- maximum number of simultaneous user connections
- connections [SQL Server], simultaneous
ms.assetid: 53beee6e-59fe-4276-9abb-8f1cec2a3508
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c5a6e6b2a47151a967980b6e75766bdec39e88ae
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673655"
---
# <a name="configure-the-user-connections-server-configuration-option"></a>Configurare l'opzione di configurazione del server user connections
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  In questo argomento si illustra come impostare l'opzione di configurazione del server **user connections** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Con l'opzione **user connections** è possibile specificare il numero massimo di connessioni utente simultanee permesse in un'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il numero effettivo di connessioni utente consentite dipende inoltre dalla versione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizzata nonché dai limiti delle applicazioni e dei componenti hardware. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] è consentito un massimo di 32.767 connessioni utente. Poiché **user connections** è un'opzione dinamica a configurazione automatica, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] regola automaticamente il numero massimo di connessioni utente come necessario, fino al valore massimo consentito. Se, ad esempio, sono connessi solo 10 utenti, vengono allocati 10 oggetti connessione utente. Nella maggior parte dei casi, non è necessario modificare il valore dell'opzione. Il valore predefinito è 0, che indica che è consentito il numero massimo di connessioni utente (32.767).  
  
 Per determinare il numero massimo di connessioni utente consentito dal sistema, è possibile eseguire [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) oppure eseguire una query sulla vista del catalogo [sys.configuration](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) .  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   **Per configurare l'opzione user connections utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Completamento:**  [Dopo la configurazione dell'opzione user connections](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Questa opzione è avanzata e la relativa modifica è riservata ad amministratori di database esperti o a professionisti dotati di certificazione per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Tramite l'opzione **user connections** è possibile evitare il sovraccarico del server con un numero eccessivo di connessioni simultanee. In base ai requisiti di sistema e ai requisiti degli utenti è possibile stimare il numero di connessioni necessarie. In un sistema con molti utenti, ad esempio, non tutti gli utenti richiedono una connessione univoca, ma le connessioni possono essere condivise tra gli utenti. Per gli utenti che eseguono applicazioni OLE DB è necessaria una connessione per ogni oggetto connessione aperto, per gli utenti che eseguono applicazioni ODBC è necessaria una connessione per ogni handle di connessione attivo nell'applicazione, mentre per gli utenti che eseguono applicazioni DB-Library è necessaria una connessione per ogni processo avviato che chiama la funzione **dbopen** di DB-Library.  
  
    > [!IMPORTANT]  
    >  Se è necessario utilizzare questa opzione, evitare di impostare un valore troppo elevato. Ogni connessione comporta indipendentemente dal fatto che venga utilizzata o meno. Se si supera il numero massimo consentito di connessioni utente, viene visualizzato un messaggio di errore e non saranno consentite ulteriori connessioni fino a quando una di quelle correnti non tornerà disponibile.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Le autorizzazioni di esecuzione per **sp_configure** senza alcun parametro o solo con il primo parametro vengono assegnate per impostazione predefinita a tutti gli utenti. Per eseguire **sp_configure** con entrambi i parametri per la modifica di un'opzione di configurazione o per l'esecuzione dell'istruzione RECONFIGURE, a un utente deve essere concessa l'autorizzazione a livello di server ALTER SETTINGS. L'autorizzazione ALTER SETTINGS è assegnata implicitamente ai ruoli predefiniti del server **sysadmin** e **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-configure-the-user-connections-option"></a>Per configurare l'opzione user connections  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse su un server, quindi scegliere **Proprietà**.  
  
2.  Fare clic sul nodo **Connessioni** .  
  
3.  Nella casella **Numero massimo di connessioni simultanee** in **Connessioni** digitare o selezionare un valore da 0 a 32767 per impostare il numero massimo di utenti a cui è consentito connettersi simultaneamente all'istanza di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
4.  Riavviare [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-configure-the-user-connections-option"></a>Per configurare l'opzione user connections  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. Questo esempio illustra come usare [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) per configurare il valore dell'opzione `user connections` per utenti `325` .  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'user connections', 325 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 Per altre informazioni, vedere [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)sia installato il servizio WMI.  
  
##  <a name="follow-up-after-you-configure-the-user-connections-option"></a><a name="FollowUp"></a> Completamento: Dopo la configurazione dell'opzione user connections  
 Per poter rendere effettiva l'impostazione, è necessario riavviare l'istanza di SQL Server.  
  
## <a name="see-also"></a>Vedere anche  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
