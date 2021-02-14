---
title: Scollegare un database | Microsoft Docs
description: Informazioni su come scollegare un database in SQL Server usando SQL Server Management Studio o Transact-SQL. I file possono essere ricollegati o collegati a un altro server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.swb.detachdatabase.f1
helpviewer_keywords:
- database detaching [SQL Server]
- detaching databases [SQL Server]
ms.assetid: f63d4107-13e4-4bfe-922d-5e4f712e472d
author: stevestein
ms.author: sstein
ms.openlocfilehash: c20fa84b3921cdd76db17d1657ba6c6f8ae0be86
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350909"
---
# <a name="detach-a-database"></a>Scollegare un database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In questo argomento viene descritto come scollegare un database in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. I file scollegati non vengono eliminati e possono essere ricollegati tramite CREATE DATABASE con l'opzione FOR ATTACH o FOR ATTACH_REBUILD_LOG. È possibile spostare e quindi collegare tali file in un altro server.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per scollegare un database utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
 Per un elenco delle limitazioni e restrizioni, vedere [Collegamento e scollegamento di un database &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md).  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Richiede l'appartenenza al ruolo predefinito del database db_owner.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-detach-a-database"></a>Per scollegare un database  
  
1.  In Esplora oggetti di [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] connettersi all'istanza del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] e quindi espanderla.  
  
2.  Espandere **Database** e selezionare il nome del database utente che si desidera scollegare.  
  
3.  Fare clic con il pulsante destro del mouse sul nome di database, scegliere **Attività** e quindi fare clic su **Scollega**. Verrà visualizzata la finestra di dialogo **Scollega database** .  
  
     **Database da scollegare**  
     Consente di visualizzare i database da scollegare.  
  
     **Nome database**  
     Consente di visualizzare il nome del database da scollegare.  
  
     **Interrompi connessioni**  
     Consente di interrompere le connessioni al database specificato.  
  
    > [!NOTE]  
    >  Non è possibile scollegare un database con connessioni attive.  
  
     **Aggiorna statistiche**  
     Per impostazione predefinita, con l'operazione di scollegamento è possibile mantenere eventuali statistiche di ottimizzazione non aggiornate prima di scollegare il database. Per aggiornare le statistiche di ottimizzazione esistenti, fare clic su questa casella di controllo.  
  
     **Mantieni cataloghi full-text**  
     Per impostazione predefinita, con l'operazione di scollegamento è possibile mantenere eventuali cataloghi full-text associati al database. Per rimuoverli, deselezionare la casella di controllo **Mantieni cataloghi full-text** . Questa opzione è visualizzata solo quando si aggiorna un database da [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
     **Status**  
     Consente di visualizzare uno degli stati seguenti: **Pronto** o **Non pronto**.  
  
     **Messaggio**  
     Nella colonna **Messaggio** possono essere visualizzate informazioni sul database simili alle seguenti:  
  
    -   Quando un database è coinvolto nella replica, lo **Stato** è **Non pronto** e nella colonna **Messaggio** viene visualizzato **Database replicato**.  
  
    -   Quando per un database esistono una o più connessioni attive, il valore di **Stato** è **Non pronto** e la colonna **Messaggio** visualizza _<number_of_active_connections>_ **Connessioni attive**, ad esempio: **Connessioni attive: 1**. Prima di poter scollegare il database è necessario disconnettere tutte le connessioni attive selezionando **Interrompi connessioni**.  
  
     Per ottenere ulteriori informazioni su un messaggio, fare clic sul testo del collegamento ipertestuale per aprire Monitoraggio attività.  
  
4.  Quando si è pronti per scollegare il database, fare clic su **OK**.  
  
> [!NOTE]  
>  Il database scollegato rimarrà visibile nel nodo **Database** di Esplora oggetti fino all'aggiornamento della vista. È possibile aggiornare la vista in qualsiasi momento: Fare clic nel riquadro Esplora oggetti, scegliere **Vista** dalla barra dei menu, quindi **Aggiorna**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-detach-a-database"></a>Per scollegare un database  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. Nell'esempio seguente viene scollegato il database AdventureWorks2012 con skipchecks impostato su true.  
  
```  
EXEC sp_detach_db 'AdventureWorks2012', 'true';  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Collegamento e scollegamento di un database &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [sp_detach_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)  
  
  
