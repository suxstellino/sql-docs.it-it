---
description: Disabilitare un vincolo CHECK per la replica
title: Disabilitare un vincolo CHECK per la replica | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- CHECK constraints, disabling
- constraints [SQL Server], disabling
- disabling constraints
- constraints [SQL Server], check
ms.assetid: af98fc70-24dd-4bd3-a0a3-f701dfa67b2c
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7b137ba8f3fc98b5892f0154f9dd54d3bd8b3791
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179744"
---
# <a name="disable-check-constraints-for-replication"></a>Disabilitare un vincolo CHECK per la replica
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  È possibile disabilitare i vincoli CHECK in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. I vincoli CHECK possono essere espressamente disabilitati per la replica e ciò può essere utile quando si pubblicano dati da una versione precedente di [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Se una tabella viene pubblicata usando la replica, i vincoli CHECK vengono disabilitati automaticamente per le operazioni eseguite dagli agenti di replica. Quando un agente di replica esegue un accodamento, aggiornamento o una eliminazione a un sottoscrittore, il vincolo non viene controllato; se invece un utente esegue un accodamento, un aggiornamento o una eliminazione, il vincolo viene controllato. Il vincolo viene disabilitato per l'agente di replica in quanto esso è già stato controllato nel server di pubblicazione quando i dati sono stati inseriti, aggiornati o eliminati. Per altre informazioni, vedere [Specificare le opzioni dello schema](../../relational-databases/replication/publish/specify-schema-options.md).  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessario disporre dell'autorizzazione ALTER per la tabella.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-disable-a-check-constraint-for-replication"></a>Per disabilitare un vincolo CHECK per la replica  
  
1.  In **Esplora oggetti** espandere la tabella contenente il vincolo CHECK che si desidera modificare, quindi espandere la cartella **Vincoli** .  
  
2.  Fare clic con il pulsante destro del mouse sul vincolo CHECK da modificare, quindi scegliere **Modifica**.  
  
3.  Nella finestra di dialogo **Verifica vincoli** , in **Progettazione tabelle** selezionare un valore **No** per **Attiva per replica**.  
  
4.  Fare clic su **Close**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-disable-a-check-constraint-for-replication"></a>Per disabilitare un vincolo CHECK per la replica  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. Nell'esempio viene creata una tabella con una colonna IDENTITY e un vincolo CHECK sulla tabella. Nell'esempio il vincolo viene quindi eliminato e ricreato specificando la clausola NOT FOR REPLICATION.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    CREATE TABLE dbo.doc_exd (column_a int IDENTITY (1,1)   
    CONSTRAINT exd_check CHECK (column_a > 1))   
  
    ALTER TABLE dbo.doc_exd   
    DROP CONSTRAINT exd_check;   
    GO  
    ALTER TABLE dbo.doc_exd    
    ADD CONSTRAINT exd_check CHECK NOT FOR REPLICATION (column_a > 1);  
    ```  
  
 Per altre informazioni, vedere [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
###  <a name="TsqlExample"></a>   
## <a name="see-also"></a>Vedere anche  
 [Specificare le opzioni dello schema](../../relational-databases/replication/publish/specify-schema-options.md)  
  
  
