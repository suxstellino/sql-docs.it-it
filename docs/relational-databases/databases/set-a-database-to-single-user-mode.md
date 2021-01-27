---
description: Impostare un database in modalità utente singolo
title: Impostare un database in modalità utente singolo | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- single-user mode [SQL Server], database option
ms.assetid: fb5254eb-b635-4b39-8361-136fd36f2b1f
author: stevestein
ms.author: sstein
ms.openlocfilehash: 68d806e83c389c256737da8054a788697f0bd773
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2021
ms.locfileid: "98812984"
---
# <a name="set-a-database-to-single-user-mode"></a>Impostare un database in modalità utente singolo
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento si descrive come impostare un database definito dall'utente in modalità utente singolo in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Questa modalità consente l'accesso a un solo utente alla volta e viene in genere utilizzata per azioni di manutenzione.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Prerequisiti](#Prerequisites)  
  
     [Sicurezza](#Security)  
  
-   **Per impostare un database in modalità utente singolo utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Se altri utenti sono connessi al database nel momento in cui viene impostata la modalità utente singolo, le relative connessioni verranno chiuse senza preavviso.  
  
-   Il database rimane in modalità utente singolo anche se l'utente che ha impostato l'opzione si disconnette. A questo punto, un altro utente (ma solo uno) potrà connettersi al database.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Prerequisiti  
  
-   Prima di impostare il database in modalità SINGLE_USER, verificare che l'opzione AUTO_UPDATE_STATISTICS_ASYNC sia impostata su OFF. Se l'opzione è impostata su ON, tramite il thread in background utilizzato per aggiornare le statistiche viene stabilita una connessione con il database che non sarà quindi accessibile in modalità utente singolo. Per altre informazioni, vedere [Opzioni ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È richiesta l'autorizzazione ALTER per il database.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-set-a-database-to-single-user-mode"></a>Per impostare un database in modalità utente singolo  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], quindi espandere questa istanza.  
  
2.  Fare clic con il pulsante destro del mouse sul database di cui modificare l'impostazione e quindi scegliere **Proprietà**.  
  
3.  Nella finestra di dialogo **Proprietà database** fare clic sulla pagina **Opzioni** .  
  
4.  Selezionare **Single** nell'opzione **Limitazione accesso**.  
  
5.  Se altri utenti sono connessi al database, verrà visualizzato il messaggio **Connessioni aperte** . Per modificare la proprietà e chiudere tutte le altre connessioni, fare clic su **Sì**.  
  
 Tramite questa procedura, è inoltre possibile impostare l'accesso Multiple o Restricted per il database. Per altre informazioni sulle opzioni di limitazione dell'accesso, vedere [Proprietà del database &#40;pagina Opzioni&#41;](../../relational-databases/databases/database-properties-options-page.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-set-a-database-to-single-user-mode"></a>Per impostare un database in modalità utente singolo  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. In questo esempio si imposta il database in modalità `SINGLE_USER` in modo da ottenere l'accesso esclusivo. Nell'esempio lo stato del database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] viene quindi impostato su `READ_ONLY` e viene ripristinato l'accesso al database per tutti gli utenti. L'opzione di chiusura `WITH ROLLBACK IMMEDIATE` è specificata nella prima istruzione `ALTER DATABASE` . Questa operazione comporterà il rollback di tutte le transazioni incomplete e l'immediata interruzione di qualsiasi altra connessione al database [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
 [!code-sql[DatabaseDDL#AlterDatabase8](../../relational-databases/databases/codesnippet/tsql/set-a-database-to-single_1.sql)]  
  
## <a name="see-also"></a>Vedere anche  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
  
  
