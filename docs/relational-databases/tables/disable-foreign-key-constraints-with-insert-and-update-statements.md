---
description: Disabilitazione di vincoli di chiave esterna con le istruzioni INSERT e UPDATE
title: Disabilitare i vincoli di chiave esterna con le istruzioni INSERT e UPDATE
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- constraints [SQL Server], foreign keys
- foreign keys [SQL Server], disabling constraints
- disabling constraints
- UPDATE statement [SQL Server], foreign key constraints
- INSERT statement [SQL Server], foreign key constraints
ms.assetid: 029168d7-085e-4b13-9b86-5644b67c6e24
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 575ab9ae04fc18414426791c957acc76cd9ca428
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179730"
---
# <a name="disable-foreign-key-constraints-with-insert-and-update-statements"></a>Disabilitazione di vincoli di chiave esterna con le istruzioni INSERT e UPDATE
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  È possibile disabilitare un vincolo di chiave esterna durante le transazioni INSERT e UPDATE in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] tramite [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Usare questa opzione se si è sicuri che i nuovi dati non violeranno il vincolo esistente o se il vincolo si applica solo ai dati già presenti nel database.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per disabilitare un vincolo di chiave esterna per le istruzioni INSERT e UPDATE tramite:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
 Dopo aver disabilitato questi vincoli, gli inserimenti o gli aggiornamenti successivi della colonna non saranno convalidati in base alle condizioni del vincolo.  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 È necessario disporre dell'autorizzazione ALTER per la tabella.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Con SQL Server Management Studio  
  
#### <a name="to-disable-a-foreign-key-constraint-for-insert-and-update-statements"></a>Per disabilitare un vincolo di chiave esterna per le istruzioni INSERT e UPDATE  
  
1.  In **Esplora oggetti** espandere la tabella contenente il vincolo, quindi espandere la cartella **Chiavi** .  
  
2.  Fare clic con il pulsante destro del mouse sul vincolo e selezionare **Modifica**.  
  
3.  Nella griglia in **Progettazione tabelle** fare clic su **Attiva vincolo della chiave esterna** , quindi scegliere **No** nel menu a discesa.  
  
4.  Fare clic su **Close**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-disable-a-foreign-key-constraint-for-insert-and-update-statements"></a>Per disabilitare un vincolo di chiave esterna per le istruzioni INSERT e UPDATE  
  
1.  In **Esplora oggetti** connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Sulla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare gli esempi seguenti nella finestra Query, quindi fare clic su **Esegui**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    ALTER TABLE Purchasing.PurchaseOrderHeader  
    NOCHECK CONSTRAINT FK_PurchaseOrderHeader_Employee_EmployeeID;  
    GO  
    ```  
  
 Per altre informazioni, vedere [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
###  <a name="TsqlExample"></a>  
