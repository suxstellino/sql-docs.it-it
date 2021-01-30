---
title: Sottoscrivere una categoria di criteri o annullarne la sottoscrizione per un database
description: Descrivere come sottoscrivere una categoria di criteri o annullarne la sottoscrizione per un database per la gestione basata su criteri usando SQL Server Management Studio e Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.dmf.groupsubscription.f1
ms.assetid: d2c31769-7098-428e-ad9c-ef56541b7c52
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: deb83a831430dd6ad57bf50eca26b9e30c49f22c
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076213"
---
# <a name="subscribe-or-unsubscribe-a-database--to-a-policy-category"></a>Sottoscrivere una categoria di criteri o annullarne la sottoscrizione per un database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Questo argomento illustra come sottoscrivere una categoria di criteri o annullarne la sottoscrizione per un database in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] usando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Sicurezza](#Security)  
  
-   **Per sottoscrivere una categoria di criteri o annullarne la sottoscrizione per un database utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 Richiede l'appartenenza al ruolo predefinito del database db_owner.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
  
#### <a name="to-subscribe-or-unsubscribe-a-database-to-a-policy-category"></a>Per sottoscrivere una categoria di criteri o annullarne la sottoscrizione per un database  
  
1.  In **Esplora oggetti** fare clic sul segno più per espandere il server contenente il database in cui si desidera gestire le sottoscrizioni di categorie.  
  
2.  Fare clic sul segno più per espandere la cartella **Database** .  
  
3.  Fare clic con il pulsante destro del mouse sul database dove si desidera gestire sottoscrizioni di categorie, scegliere **Criteri** e selezionare **Categorie**.  
  
     Nella finestra di dialogo **Categorie** sono disponibili le opzioni seguenti:  
  
     Colonna Espandi  
     Fare clic su questa opzione per espandere una categoria di criteri. Verranno elencati tutti i criteri inclusi nella categoria.  
  
     **Nome**  
     Nome della categoria di criteri.  
  
     **Sottoscritto**  
     Indica se la destinazione ha sottoscritto la categoria di criteri. Se questa casella di controllo è disabilitata, la categoria di criteri è impostata su **Imponi sottoscrizioni di database**, ovvero la categoria di criteri si applica a tutti i database nel server.  
  
     **Criteri**  
     Quando i gruppi di criteri sono espansi, vengono visualizzati i criteri inclusi nella categoria di criteri.  
  
     **Enabled**  
     Indica se i criteri sono abilitati o disabilitati.  
  
     **Modalità di esecuzione**  
     Visualizza la modalità di esecuzione dei criteri.  
  
     **History**  
     Fare clic sul collegamento ipertestuale Visualizza cronologia per aprire il Visualizzatore file di log ed esaminare la cronologia dei criteri.  
  
4.  Per sottoscrivere una categoria Gestione basata su criteri, selezionare la casella di controllo della categoria nella colonna **Sottoscritto** . Per annullare la sottoscrizione da una categoria, deselezionare la casella di controllo.  
  
5.  Al termine, fare clic su **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Con Transact-SQL  
  
#### <a name="to-subscribe-a-database-to-a-policy-category"></a>Per sottoscrivere una categoria di criteri per un database  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    -- Adds a subscription to the 'Finance' policy category for the AdventureWorks2012 database.  
    EXEC sys.sp_syspolicy_subscribe_to_policy_category @policy_category = N'Finance';  
    GO  
    ```  
  
 Per altre informazioni, vedere [sp_syspolicy_subscribe_to_policy_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-subscribe-to-policy-category-transact-sql.md).  
  
#### <a name="to-unsubscribe-a-database-to-a-policy-category"></a>Per annullare la sottoscrizione di una categoria di criteri per un database  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    -- Deletes a subscription to the 'Finance' policy category for the AdventureWorks2012 database.  
    EXEC sys.sp_syspolicy_unsubscribe_from_policy_category @policy_category = N'Finance';  
    GO  
    ```  
  
 Per altre informazioni, vedere [sp_syspolicy_unsubscribe_from_policy_category &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syspolicy-unsubscribe-from-policy-category-transact-sql.md).  
  
  
