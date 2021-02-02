---
title: Concedere autorizzazioni per una stored procedure | Microsoft Docs
description: Informazioni su come concedere le autorizzazioni per una stored procedure in SQL Server 2019 (15.x) tramite SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], permissions
ms.assetid: a7d15816-a788-4099-ad91-dc4b26618299
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3b41f60b80dcb13d9e61b854666e3298b2b295cf
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250297"
---
# <a name="grant-permissions-on-a-stored-procedure"></a>Concedere autorizzazioni per una stored procedure
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  In questo argomento viene illustrato come concedere autorizzazioni per una stored procedure in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizzando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Le autorizzazioni possono essere concesse a un utente, a un ruolo del database o a un ruolo applicazione nel database.  
  
 **Contenuto dell'articolo**  
  
-   **Prima di iniziare:**  
  
     [Limitazioni e restrizioni](#Restrictions)  
  
     [Sicurezza](#Security)  
  
-   **Per concedere autorizzazioni per una stored procedure utilizzando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Prima di iniziare  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitazioni e restrizioni  
  
-   Non è possibile utilizzare [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] per concedere autorizzazioni per stored procedure o funzioni di sistema. Utilizzare invece [GRANT - autorizzazioni per oggetti](../../t-sql/statements/grant-object-permissions-transact-sql.md) .  
  
###  <a name="security"></a><a name="Security"></a> Sicurezza  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorizzazioni  
 L'utente che concede le autorizzazioni (o l'entità specificata con l'opzione AS) deve disporre della relativa autorizzazione con GRANT OPTION oppure di un'autorizzazione di livello superiore che include l'autorizzazione che viene concessa. È richiesta l'autorizzazione ALTER per lo schema a cui appartiene la stored procedure oppure l'autorizzazione CONTROL per la stored procedure. Per altre informazioni, vedere [GRANT - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilizzo di SQL Server Management Studio  
  
#### <a name="to-grant-permissions-on-a-stored-procedure"></a>Per concedere autorizzazioni per una stored procedure  
  
1.  In Esplora oggetti connettersi a un'istanza del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , quindi espanderla.  
  
2.  Espandere **Database**, espandere il database a cui appartiene la stored procedure, quindi espandere **Programmabilità**.  
  
3.  Espandere **Stored procedure**, fare clic con il pulsante destro del mouse sulla procedura per cui concedere autorizzazioni e quindi scegliere **Proprietà**.  
  
4.  Da **Proprietà stored procedure** selezionare la pagina **Autorizzazioni** .  
  
5.  Per concedere autorizzazioni a un utente, a un ruolo del database o a un ruolo applicazione, fare clic su **Cerca**.  
  
6.  In **Selezione utenti o ruoli** fare clic su **Tipi di oggetti** per aggiungere o cancellare gli utenti e i ruoli desiderati.  
  
7.  Fare clic su **Sfoglia** per visualizzare l'elenco di utenti o ruoli. Selezionare gli utenti o i ruoli a cui concedere le autorizzazioni.  
  
8.  Nella griglia **Autorizzazioni esplicite** selezionare le autorizzazioni da concedere all'utente o al ruolo specificato. Per una descrizione delle autorizzazioni, vedere [Autorizzazioni &#40;Motore di database&#41;](../../relational-databases/security/permissions-database-engine.md).  

 Selezionando **Concedi** al beneficiario verrà assegnata l'autorizzazione specificata. Se si seleziona **Autorizza alla concessione di autorizzazioni** al beneficiario verrà inoltre consentito di concedere l'autorizzazione specificata ad altre entità.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Uso di Transact-SQL  
  
#### <a name="to-grant-permissions-on-a-stored-procedure"></a>Per concedere autorizzazioni per una stored procedure  
  
1.  Connettersi al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dalla barra Standard fare clic su **Nuova query**.  
  
3.  Copiare e incollare l'esempio seguente nella finestra Query, quindi fare clic su **Esegui**. Nell'esempio viene concessa l'autorizzazione `EXECUTE` per la stored procedure `HumanResources.uspUpdateEmployeeHireInfo` a un ruolo applicazione denominato `Recruiting11`.  
  
```sql  
USE AdventureWorks2012;   
GRANT EXECUTE ON OBJECT::HumanResources.uspUpdateEmployeeHireInfo  
    TO Recruiting11;  
GO  
```  
  
## <a name="see-also"></a>Vedere anche  
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [GRANT - autorizzazioni per oggetti &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)   
 [Creazione di una stored procedure](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [Modificare una stored procedure](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)   
 [Eliminare una stored procedure](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)   
 [Rinominare una stored procedure](../../relational-databases/stored-procedures/rename-a-stored-procedure.md)  
  
  
