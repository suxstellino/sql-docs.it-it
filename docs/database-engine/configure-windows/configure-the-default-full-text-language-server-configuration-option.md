---
title: Configurare l'opzione di configurazione del server default full-text language | Microsoft Docs
description: Informazioni sull'opzione default full-text language. Scoprire come configurarla per specificare la lingua predefinita usata da SQL Server per gli indici full-text.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- languages [full-text search]
- default full-text language option
ms.assetid: 0fa8785b-0830-4a52-aff5-fcf8268b72fc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 45e05505444d00607cae875b8dfd8bd07bf9190e
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783242"
---
# <a name="configure-the-default-full-text-language-server-configuration-option"></a>Configurare l'opzione di configurazione del server default full-text language
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Questo argomento descrive come configurare l'opzione di configurazione del server **default full-text language** in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'opzione **default full-text language** consente di specificare il valore relativo alla lingua predefinita per gli indici full-text. L'analisi linguistica viene eseguita su tutti i dati abilitati per l'indicizzazione full-text e dipende dalla lingua dei dati. Il valore predefinito per questa opzione corrisponde alla lingua impostata per il server. Per le versioni localizzate di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il programma di installazione di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] imposta l'opzione **default full-text language** sulla lingua del server, se esiste una corrispondenza appropriata. Per le versioni non localizzate di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l'opzione **default full-text language** è impostata sull'inglese.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Indicazioni](#Recommendations)  
  
     [Sicurezza](#Security)  
  
-   **Per configurare l'opzione default full-text language tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Completamento:**  [Dopo la configurazione dell'opzione default full-text language](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Il valore dell'opzione **default full-text language** viene usato in un indice full-text quando per una colonna non viene specificata alcuna lingua tramite l'opzione LANGUAGE **termine_lingua** nell'istruzione CREATE FULLTEXT INDEX o ALTER FULLTEXT INDEX. Se l'opzione default full-text language non è supportata o il pacchetto di analisi linguistica non è disponibile, l'operazione CREATE o ALTER non verrà eseguita e in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verrà restituito un messaggio di errore in cui viene indicato che la lingua specificata non è valida.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Indicazioni  
  
-   Questa opzione è avanzata e la relativa modifica è riservata ad amministratori di database esperti o a professionisti dotati di certificazione per [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Per l'opzione **default full-text language** è richiesto un valore LCID. Per un elenco di LCID supportati e delle lingue corrispondenti, vedere [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md). Potrebbero essere disponibili altre lingue, ad esempio da fornitori di software indipendenti. Se non viene rilevato alcun sottolinguaggio specifico, il motore di ricerca full-text passerà automaticamente alla lingua principale.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Le autorizzazioni di esecuzione per **sp_configure** senza alcun parametro o solo con il primo parametro vengono assegnate per impostazione predefinita a tutti gli utenti. Per eseguire **sp_configure** con entrambi i parametri per la modifica di un'opzione di configurazione o per l'esecuzione dell'istruzione RECONFIGURE, a un utente deve essere concessa l'autorizzazione a livello di server ALTER SETTINGS. L'autorizzazione ALTER SETTINGS è assegnata implicitamente ai ruoli predefiniti del server **sysadmin** e **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-configure-the-default-full-text-language-option"></a>Per configurare l'opzione default full-text language  
  
1.  In Esplora oggetti fare clic con il pulsante destro del mouse su un server e scegliere **Proprietà**.  
  
2.  Fare clic sul nodo **Avanzate** .  
  
3.  In Varie usare l'opzione **Lingua predefinita full-text** per specificare il valore relativo alla lingua predefinita per le colonne con indicizzazione full-text.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-configure-the-default-full-text-language-option"></a>Per configurare l'opzione default full-text language  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. Questo esempio illustra come usare [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) per impostare il valore dell'opzione `default full-text` sull'olandese (`1043`).  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1 ;  
GO  
RECONFIGURE  
GO  
EXEC sp_configure 'default full-text language', 1043 ;  
GO  
RECONFIGURE  
GO  
  
```  
  
 Per altre informazioni, vedere [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)sia installato il servizio WMI.  
  
##  <a name="follow-up-after-you-configure-the-default-full-text-language-option"></a><a name="FollowUp"></a> Completamento: Dopo la configurazione dell'opzione default full-text language  
 L'impostazione diventa effettiva immediatamente senza dover riavviare il server.  
  
## <a name="see-also"></a>Vedere anche  
 [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Opzioni di configurazione del server &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
  
